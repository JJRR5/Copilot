---
mode: 'agent'
tools: ['development-toolset']
description: 'Scaffold an Odoo v15+ model class file from a natural-language specification'

---

Your task is to generate a new Odoo model.  

Use [academy-example.prompt.md](./academy-example.prompt.md) as a style reference.


## 💡 What to Expect from `${input:model_spec}`

This will contain a natural language description with some or all of the following:
- The module name (e.g. "academy")
- The model technical name (e.g. "academy.record")
- A human-friendly description (e.g. "Academy Record")
- A list of fields and types (e.g. "Char field 'name', required", "Selection field 'status' with values 'draft' and 'done'")
- Optional Python methods (described as code or text)


## 🧱 Generation Steps

### 1. Parse model information
- Extract:
  - `_name` from the technical model name
  - `_description` from the user description
  - The list of fields and types
  - Any optional methods

### 2. Derive file/class names
- Class name: PascalCase of the last segment in model name (e.g. `AcademyRecord`)
- Filename: `${fileBasenameNoExtension}.py`
- Path: `${fileDirname}/models/${fileBasenameNoExtension}.py`

### 3. Generate model file structure
```python
from odoo import models, fields

class ${ClassName}(models.Model):
    _name = "${full_model_name}"
    _description = "${human_description}"

    # Fields
    <render each field line here>

    # Optional methods
    <render method(s) here if given>
````

### 4. Import the model in the module's `__init__.py`
```python
from . import ${fileBasenameNoExtension}
```

---

## 📁 Output

Save the result to:

```
${fileDirname}/models/${fileBasenameNoExtension}.py
```

---

## ✅ Reference

Use [academy-example.prompt.md](./academy-example.prompt.md) for naming, formatting, and conventions.

