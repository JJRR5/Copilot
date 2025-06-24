---
mode: 'agent'
tools: ['development-toolset']
---

Your task is to generate both the Odoo model class and its corresponding standalone XML views for a single model in one go. Follow these steps in order:

1. **Generate the Model**  
   - Refer to [create-model.prompt.md](./create-model.prompt.md) and execute its instructions to scaffold the Python model file under `models/`.  

2. **Generate the Views**  
   - Once the model file is created, refer to [create-model-views.prompt.md](./create-model-views.prompt.md) to produce the XML views (tree, form, search, action, menuitem).
