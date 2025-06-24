---
mode: 'agent'
tools: ['development-toolset']
description: 'Generate a portal controller in Odoo for rendering a list view of a given model'
---

Your goal is to generate a new HTTP controller in Odoo (v15+) that displays a list view (tree view) for a specific model inside the portal.

Follow the structure and conventions shown in [academy-example.prompt.md](./academy-example.prompt.md). This example includes the model, the controller, and the template.


### 🔧 Instructions

1. **Ask for the following input** if not already provided:
   - Model name (`_name`)
   - URL base for the portal
   - Field to use for `groupby` (optional)
   - Fields for sorting and filtering

2. **Controller Requirements**:
   - The controller must inherit from `CustomerPortal`.
   - Define a constant at the top like `MODELNAME_BASE_URL`, e.g. `ACADEMY_RECORD_BASE_URL`.
   - Use `portal_pager` for pagination.
   - Use `request.env` to access records.
   - Include `searchbar_sortings`, `searchbar_inputs`, `searchbar_filters`, and `searchbar_groupby`.
   - Return data grouped by a field (e.g. `status`) if provided.
   - Render the view using `request.render()` with a dynamic template path.
   - Include grouping logic using `groupbyelem` and `itemgetter`.

3. **Use These Variables**:
   - `${input:model_name}`
   - `${fileDirname}/controllers/portal_${fileBasenameNoExtension}.py`
   - `${fileBasenameNoExtension}` for naming base

