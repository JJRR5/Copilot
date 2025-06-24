---

mode: 'agent'
tools: ['development-toolset']
description: 'Generate an Odoo web tour (JavaScript) and corresponding test (Python) based on a provided XML view and a description of the desired flow.'
---

Your task is to generate:

1. **A JavaScript file** that defines an Odoo web tour (using `tour.register(…)`) which walks through the UI elements defined in a given XML view.
2. **A Python test file** (using Odoo’s `HttpCase`) that executes the tour and verifies it succeeds.

The agent must follow these instructions closely:



## 🔧 Required Inputs

* `${input:xml_file_path}` — Path to the XML view file containing the UI elements (buttons, links, inputs) that the tour should interact with. The agent will parse or inspect this file to identify CSS selectors or `t-att` attributes needed for `trigger` values.
* `${input:flow_description}` — A brief narrative of the user flow to test (e.g., “Create a new record, fill in fields, save, and verify a success message”). This allows the agent to map each step in the flow to selectors and actions.


## 📄 JavaScript Tour Generation

1. **Import and setup**

   * Use `/** @odoo-module */` at the top.
   * Import `tour` from `"web_tour.tour"`.

2. **Define the tour name**

   * Derive a slug from the XML file’s name. For example, if `${fileBasenameNoExtension(xml_file_path)}` is `crm_lead_form`, use `"crm_lead_form_tour"`.
   * In `tour.register()`, set `test: true`.
   * If the flow requires starting on a specific URL (e.g., a form view), set `url` accordingly. Otherwise, default to `"/"` or omit `url`.

3. **Generate each step** by reading the flow description and matching it to selectors found in the XML:

   * For each action described in `${input:flow_description}`, identify the corresponding UI element in the XML. Example mappings:

     * “Click on the ‘Save’ button” → scan XML for `button` with `string="Save"` or `data-action="save"`, then set `trigger: "button:contains('Save')"` (or a more specific CSS selector).
     * “Fill in ‘Name’ field” → find `<field name="name">` or input with `name="name"`, then set `trigger: "input[name='name']"`, `run: "text <value>"`.
   * Build a JavaScript array of step objects, each containing:

     * `content`: Short description of what the user sees or should do.
     * `trigger`: CSS selector or text-based selector that targets the element.
     * `run`: One of `"click"`, `"text <value>"`, or a function (e.g., `function () { window.location.reload(); }`) to reload or wait.
     * Optional flags: `in_modal: true/false`, `timeout: <ms>`, etc., for cases where modals or AJAX waits are needed.

4. **Extract repeated steps** into constants if they appear multiple times (e.g., a generic “Save” step as `const saveStep = { … }`).

   * Use that constant wherever applicable in the steps array.

5. **Wrap in `tour.register`**

   ```javascript
   tour.register(
       "<derived_tour_name>",
       { test: true, url: "<starting_url>" },
       [
           /* step objects generated above */
       ]
   );
   ```

   * Ensure the array is properly comma-separated and formatted.

6. **Save the JavaScript file** at:

   ```
   ${fileDirname}/static/tests/tours/${fileBasenameNoExtension(xml_file_path)}_tour.js
   ```



## 🧪 Python Test Generation

1. **Import Required Classes**

   ```python
   from odoo.tests import HttpCase, tagged
   from .common import <YourCommonClass>
   ```

   * If there is a common base test (e.g., `VendorPortalCommon`), import it. Otherwise, use `HttpCase` directly.

2. **Define a Test Class**

   * Annotate with `@tagged("<test_tag>", "post_install", "-at_install")`. Use `<fileBasenameNoExtension(xml_file_path)>_tour` as the `<test_tag>` (e.g., `"crm_lead_form_tour_tests"`).
   * Inherit from `HttpCase` (and any common mixin if applicable).

3. **Implement `setUp`**

   * Call `super().setUp()`.
   * Set up any necessary records or permissions so that the tour can run. For example, assign an admin or portal user if the tour requires login.

4. **Create a Test Method** for each distinct flow or scenario:

   ```python
   def test_01_<flow_slug>(self):
       # Ensure any prerequisites: e.g., login as admin or portal user
       self.start_tour("<starting_url>", "<derived_tour_name>", login=<user.login>)
   ```

   * Use `self.env.ref("<xml_module>.<record_xml_id>")` if you need to reference records.
   * The `<starting_url>` should match the `url` used in the JS tour.
   * The `<derived_tour_name>` must match exactly the string passed to `tour.register`.

5. **Repeat** for multiple flows if described in `${input:flow_description}`.

6. **Save the test file** at:

   ```
   ${fileDirname}/tests/test_${fileBasenameNoExtension(xml_file_path)}_tour.py
   ```

### Modify __manifest__.py
7. **Update `__manifest__.py`** to include the new tour if not already present:

   ```python
      "assets": {
        "web.assets_tests": [
            "${fileDirname}/static/tests/tours/*.js",
        ],
    },
   ```



## ✅ Reference Example

Use the following as a guide when verifying your output. Replace names and selectors based on the actual XML content in `${input:xml_file_path}`:

### Example JS Tour (for `vendor_permissions.xml`)

```javascript
/** @odoo-module */
import tour from "web_tour.tour";

const clickSave = {
    content: "Click the ‘Save’ button",
    trigger: "button:contains('Save')",
    run: "click",
};

tour.register(
    "vendor_permissions_tour",
    {
        test: true,
        url: "/vendor/permissions/form",  // example starting page
    },
    [
        {
            content: "Open the user menu",
            trigger: "#top_menu a[href='#']",
            run: "click",
        },
        {
            content: "Click ‘My Permissions’",
            trigger: "a[href='/vendor_portal/my/permissions']",
            run: "click",
        },
        {
            content: "Change the email to ‘test@test.com’",
            trigger: "input[name='login']",
            run: "text test@test.com",
        },
        clickSave,
        {
            content: "Verify success notification",
            trigger: ".o_notification_body:contains('User details updated successfully')",
        },
    ]
);
```

### Example Python Test (for `vendor_permissions_tour`)

```python
from odoo.tests import HttpCase, tagged
from .common import VendorPortalCommon

@tagged("vendor_permissions_tour_tests", "post_install", "-at_install")
class TestVendorPermissionsTours(HttpCase, VendorPortalCommon):
    def setUp(self):
        super().setUp()
        self.admin_user = self.env.ref("base.user_admin")
        self.portal_user = self.env.ref("base.demo_user0")

    def test_01_update_user_email(self):
        self.admin_user.parent_id = self.portal_user.parent_id
        self.start_tour(
            "/vendor/permissions/form",
            "vendor_permissions_tour",
            login=self.admin_user.login,
        )
```
