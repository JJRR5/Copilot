---
mode: 'agent'
tools: ['development-toolset']
description: 'Generate a portal controller and corresponding list view template in Odoo for a specified model'
---

Your task is to generate both a portal controller and its corresponding list view template in Odoo (v15+) for a specified model.

Follow these steps sequentially:

1. **Generate the Controller**:
   - Refer to [create-controller.prompt.md](./create-controller.prompt.md) for detailed instructions.

2. **Generate the List View Template**:
   - Refer to [create-list-template.prompt.md](./create-list-template.prompt.md) for detailed instructions.

**Inputs Required**:
- Model name (e.g., `academy.record`).
- Fields to display in the list view.
- Fields to include in search filters and groupings.
- Desired URL base for the portal (e.g., `/my/academy-records`).

**Output**:
- Controller file: `controllers/portal_<model_name>.py`.
- Template file: `views/portal_<model_name>.xml`.

Ensure consistency between the controller and template, particularly in the use of fields for sorting, filtering, and grouping.
