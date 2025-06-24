---
mode: 'agent'
tools: ['development-toolset']
description: 'Generate standalone tree, form, and search views XML for the Odoo model defined in the current file context. Only list_fields is requested; infer everything else from context.'
---

Your task is to create an XML file defining the **tree**, **form**, **search** views — and the corresponding **act_window** action and **menuitem** — for the Odoo model in the currently open Python file. Do **not** ask the user for module name, model name, or file path—derive them automatically. Only request the list of fields to show in the tree view.


### 🔧 Required Input

- `${input:list_fields}` — comma-separated fields to display in the **tree** view (e.g. `name,create_date,status`).


---

### 🔍 Context Extraction

1. **Model Name**  
   Scan `${file}` for the line `_name = "..."` and capture its value (e.g. `academy.record`).

2. **Module Name**  
   Infer from the path: if `${fileDirname}` ends in `/models`, take its parent folder name.


### 🏗️ Generation Steps

1. **Wrap with `<odoo>` root**  
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<odoo>
  <!-- tree, form, search records go here -->
</odoo>
````

2. **Tree view**

   ```xml
   <record id="view_${model_alias}_tree" model="ir.ui.view">
     <field name="name">${model_alias}.tree</field>
     <field name="model">${model_name}</field>
     <field name="arch" type="xml">
       <tree>
         <!-- one <field> per list_fields -->
         <field name="[each field]" />
       </tree>
     </field>
   </record>
   ```

3. **Form view**

   * Show the same fields inside a single `<group>`

   ```xml
   <record id="view_${model_alias}_form" model="ir.ui.view">
     <field name="name">${model_alias}.form</field>
     <field name="model">${model_name}</field>
     <field name="arch" type="xml">
       <form>
         <sheet>
           <group>
             <!-- render each list_fields -->
             <field name="[each field]" />
           </group>
         </sheet>
       </form>
     </field>
   </record>
   ```

4. **Search view**

   * Include the first three fields from list\_fields (or all if ≤3)

   ```xml
   <record id="view_${model_alias}_search" model="ir.ui.view">
     <field name="name">${model_alias}.search</field>
     <field name="model">${model_name}</field>
     <field name="arch" type="xml">
       <search>
         <!-- pick first up to 3 of list_fields -->
         <field name="[each field]" />
       </search>
     </field>
   </record>
   ```

5. **Action (ir.actions.act\_window)**

   ```xml
   <record id="action_${model_alias}_tree" model="ir.actions.act_window">
     <field name="name">${model_alias.replace('_',' ').title()}</field>
     <field name="res_model">${model_name}</field>
     <field name="view_mode">tree,form</field>
     <field name="view_id" ref="view_${model_alias}_tree"/>
   </record>
   ```

6. **Menuitem**

   ```xml
   <menuitem id="menu_${model_alias}_tree"
             name="${model_alias.replace('_',' ').title()}"
             parent="menu_${module_name}_main"
             action="action_${model_alias}_tree"
             sequence="10"/>
   ```

   * If `menu_${module_name}_main` doesn’t exist, user can adjust the `parent` manually.



7. **Update the module's `__manifest__.py`**

   * Add the new XML file to the `data` list:
   ```python
   'data': [
       # ... other data files
       'views/${fileBasenameNoExtension}.xml',
   ],
   ```