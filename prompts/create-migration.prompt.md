---

mode: 'agent'
tools: ['development-toolset']

---

Your task is to automatically create a new migration for an Odoo module. The migration can be a **pre** or **post** migration depending on the user’s choice.

## 🔧 Required Inputs

* `${input:migration_type:select|pre,post}` — Type of migration to create (`pre` or `post`).
* (Optional) `${input:migration_notes:placeholder(Brief description of the migration logic)}` — A short note describing what the migration should do.

## 🛠 Steps to perform

1. **Locate and read** the module’s manifest in `__manifest__.py` at the root of the current workspace folder.

2. **Parse the current version** from the manifest’s `version` field (e.g. `"15.0.1.0"`).

3. **Increment** the last numeric component of the version by 1 (for `15.0.1.0` → `15.0.1.1`).

4. **Update** the `version` field in `__manifest__.py` to the new version.

5. **Determine** the migrations folder path: `${workspaceFolder}/migrations/${old_version}`.

   * Create this folder if it does not exist.

6. **Scaffold** a migration file inside that folder named `${migration_type}-migration.py` with the following content:

   ```python
   import logging
   from odoo import SUPERUSER_ID, api
   from odoo.upgrade import util

   _logger = logging.getLogger(__name__)

   def migrate(cr, version):
       env = api.Environment(cr, SUPERUSER_ID, {})
       # ${input:migration_notes}
       # TODO: implement migration logic here
   ```

7. **Save** the migration file at:

```
${workspaceFolder}/migrations/${new_version}/${migration_type}-migration.py
```

8. **Ensure** the new migration appears in the module’s migration path so Odoo will execute it.

