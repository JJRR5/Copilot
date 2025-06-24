---
mode: 'agent'
tools: ['development-toolset']
---

Tu tarea es actualizar un archivo de traducciones `es.po` agregando nuevas entradas en su posición alfabética correcta según `msgid`. No modifiques nada más: conserva el formato, los comentarios y el espaciado entre entradas.


### 🔧 Instrucciones

1. **Recibe**:
   - El contenido completo del archivo `es.po` existente.
   - Una lista de nuevas traducciones en formato PO entry (incluyendo comentarios `#.` y referencias `#:`).

2. **Analiza** todas las entradas del archivo existente y de la lista nueva, tomando el valor de `msgid` como clave para ordenar.

3. **Fusiona** las entradas nuevas con las exzistentes, eliminando duplicados si `msgid` ya está presente (en cuyo caso reemplaza el `msgstr`).

4. **Ordena** todas las entradas resultantes por orden alfabético de `msgid`, respetando mayúsculas/minúsculas tal que `"A"` < `"a"`.

5. **Genera** el contenido actualizado de `es.po`:
   - Mantén cada PO entry con sus comentarios y referencias intactos.
   - Deja una línea en blanco entre cada entrada.


### 📑 Ejemplo

**Archivo `es.po` actual**:
```po
#. module: sbd_vendor_portal
#: model_terms:ir.ui.view,arch_db:sbd_vendor_portal.portal_my_purchase_exemptions
msgid "There are no purchase exemptions."
msgstr "No hay exoneraciones de compra."

#. module: sbd_vendor_portal
#: model_terms:ir.ui.view,arch_db:sbd_vendor_portal.portal_sicop_procedure_page_entry
msgid "View Contracts"
msgstr "Ver contrataciones"
````

**Nuevas traducciones a insertar**:

```po
#. module: sbd_vendor_portal
#: model:ir.model.fields,help:sbd_vendor_portal.field_sicop_procedure__activity_exception_decoration
msgid "Type of the exception activity on record."
msgstr "Tipo de actividad de excepción en el registro."
```

**Resultado esperado**:

```po
#. module: sbd_vendor_portal
#: model_terms:ir.ui.view,arch_db:sbd_vendor_portal.portal_my_purchase_exemptions
msgid "There are no purchase exemptions."
msgstr "No hay exoneraciones de compra."

#. module: sbd_vendor_portal
#: model:ir.model.fields,help:sbd_vendor_portal.field_sicop_procedure__activity_exception_decoration
msgid "Type of the exception activity on record."
msgstr "Tipo de actividad de excepción en el registro."

#. module: sbd_vendor_portal
#: model_terms:ir.ui.view,arch_db:sbd_vendor_portal.portal_sicop_procedure_page_entry
msgid "View Contracts"
msgstr "Ver contrataciones"
```
