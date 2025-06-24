---
mode: 'agent'
tools: ['development-toolset']
description: 'Generate a QWeb template for a portal list view in Odoo for a specific model'
---

Your goal is to generate a QWeb XML template for a list view in the Odoo portal for a specified model.

This view is meant to be rendered by a portal controller, like the one shown in [academy-example.prompt.md](./academy-example.prompt.md). Ensure the template is consistent with the controller's logic.


### 🔧 Instructions

1. Ask for or use the following inputs:
   - Model name
   - List of fields to display (default: `name`, `create_date`, `write_date`, `status`)
   - Field to group by (optional, e.g., `status`)
   - Template ID and name (optional)

2. Structure:
   - Extend `portal.portal_layout`
   - Include the search bar with `t-call="portal.portal_searchbar"` and set `breadcrumbs_searchbar` to `True`
   - Add a fallback message when no records are found
   - Include a table (`portal.portal_table`) with:
     - `<thead>` defining the column headers
     - `<tbody>` rendering each row using `t-foreach`
     - Grouped rows if a `groupby` field is defined

3. Inside each row:
   - Render links to each record using their `id` and `name`
   - Use `t-field` for datetime fields like `create_date` and `write_date`
   - Display a visual badge for status fields if present
   - Include a right-aligned dropdown for "Actions" (can be basic)

4. Use these variables where applicable:
   - `${fileDirname}/views/portal_${fileBasenameNoExtension}.xml`
   - `${input:model_name}`
   - `${input:template_id}`


### 📁 Output File
Save the generated file as:
${fileDirname}/views/portal_${fileBasenameNoExtension}.xml

Ensure that the fields, filters, sortings, and groupby logic match those used in the controller.
