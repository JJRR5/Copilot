---
mode: 'agent'
tools: ['development-toolset']
---

## ✅ Model: `academy.record`

```python
from odoo import models, fields

class AcademyRecord(models.Model):
    _name = "academy.record"
    _description = "Academy Record"

    name = fields.Char(string="Name", required=True)
    create_date = fields.Datetime(string="Created On", readonly=True)
    write_date = fields.Datetime(string="Last Updated", readonly=True)
    status = fields.Selection(
        selection=[
            ("draft", "Draft"),
            ("confirmed", "Confirmed"),
            ("cancelled", "Cancelled"),
        ],
        string="Status",
        default="draft",
        required=True,
    )

```python
from odoo import http, _
from odoo.http import request
from odoo.addons.portal.controllers.portal import CustomerPortal, pager as portal_pager
from operator import itemgetter
from odoo.tools import groupby as groupbyelem

ACADEMY_RECORD_BASE_URL = "/my/academy-records"

class PortalAcademyRecord(CustomerPortal):
    def _prepare_academy_record_values(self, records, **kwargs):
        return {
            "page_name": "academy_record",
            "records": records,
            "default_url": ACADEMY_RECORD_BASE_URL,
            **kwargs,
        }

    @http.route(
        [ACADEMY_RECORD_BASE_URL, f"{ACADEMY_RECORD_BASE_URL}/page/<int:page>"],
        type='http',
        auth='user',
        website=True
    )
    def portal_my_academy_records(self, page=1, sortby="date", search=None, groupby="status", **kw):
        values = self._prepare_portal_layout_values()
        domain = []

        searchbar_sortings = {
            "date": {"label": _("Newest"), "order": "create_date desc"},
            "name": {"label": _("Name"), "order": "name"},
            "status": {"label": _("Status"), "order": "status"},
        }

        searchbar_inputs = {
            "all": {
                "input": "all",
                "label": _("Search in all"),
                "domain": [
                    "|", "|",
                    ("name", "ilike", search),
                    ("status", "ilike", search),
                    ("id", "ilike", search),
                ],
            },
            "name": {"input": "name", "label": _("Search by name"), "domain": [("name", "ilike", search)]},
            "status": {"input": "status", "label": _("Search by status"), "domain": [("status", "ilike", search)]},
        }

        searchbar_filters = {
            "all": {"label": _("All"), "domain": []},
            "draft": {"label": _("Draft"), "domain": [("status", "=", "draft")]},
            "confirmed": {"label": _("Confirmed"), "domain": [("status", "=", "confirmed")]},
            "cancelled": {"label": _("Cancelled"), "domain": [("status", "=", "cancelled")]},
        }

        searchbar_groupby = {
            "status": {"input": "status", "label": _("Status")},
        }

        order = searchbar_sortings[sortby]["order"]
        domain += searchbar_filters["all"]["domain"]

        if search and search.strip():
            domain += searchbar_inputs["all"]["domain"]

        AcademyRecord = request.env["academy.record"]
        total = AcademyRecord.search_count(domain)

        pager = portal_pager(
            url=ACADEMY_RECORD_BASE_URL,
            total=total,
            page=page,
            step=self._items_per_page,
        )

        records = AcademyRecord.search(domain, order=order, limit=self._items_per_page, offset=pager['offset'])

        grouped_records = (
            [(AcademyRecord.concat(*g), k) for k, g in groupbyelem(records, itemgetter(groupby))]
            if records
            else []
        )

        values.update(self._prepare_academy_record_values(records))
        values.update({
            "grouped_records": grouped_records,
            "pager": pager,
            "searchbar_sortings": searchbar_sortings,
            "searchbar_inputs": searchbar_inputs,
            "searchbar_filters": searchbar_filters,
            "searchbar_groupby": searchbar_groupby,
            "search": search,
            "sortby": sortby,
            "groupby": groupby,
        })

        return request.render("academy_portal.portal_my_academy_records", values)
```

```xml
<odoo>
    <template id="portal_my_academy_records" name="My Academy Records">
        <t t-call="portal.portal_layout">
            <t t-set="breadcrumbs_searchbar" t-value="True" />

            <t t-call="portal.portal_searchbar">
                <t t-set="title">Academy Records</t>
            </t>

            <t t-if="not records">
                <div class="alert alert-warning mt8" role="alert">
                    There are no academy records.
                </div>
            </t>

            <t t-if="records" t-call="portal.portal_table">
                <thead>
                    <tr class="active">
                        <th>Name</th>
                        <th class="text-center">Created On</th>
                        <th class="text-center">Updated On</th>
                        <th class="text-center">Status</th>
                        <th class="text-center">Actions</th>
                    </tr>
                </thead>
                <tbody>
                    <t t-foreach="grouped_records" t-as="record_group">
                        <t t-set="record" t-value="record_group[0]" />
                        <tr t-if="groupby != 'none' and record['status']" class="table-info">
                            <th t-if="groupby == 'status'" colspan="5">
                                <span class="fa fa-fw fa-folder-open" />
                                <t t-out="dict(record.fields_get(['status'])['status']['selection'])[record['status']]" />
                                <span class="float-end">Total: 
                                    <span class="text-muted" t-out="len(record_group)" />
                                </span>
                            </th>
                        </tr>
                        <tr t-foreach="record_group if groupby != 'none' else records" t-as="record">
                            <td>
                                <a t-attf-href="/my/academy-records/{{record.id}}" class="text-decoration-none">
                                    <t t-out="record.name" />
                                </a>
                            </td>
                            <td class="text-center">
                                <span t-field="record.create_date" />
                            </td>
                            <td class="text-center">
                                <span t-field="record.write_date" />
                            </td>
                            <td class="text-center">
                                <span class="badge bg-info" t-out="record.status" />
                            </td>
                            <td class="text-center">
                                <div class="btn-group dropstart">
                                    <button
                                        type="button"
                                        class="btn btn-secondary dropdown-toggle"
                                        data-bs-toggle="dropdown"
                                        aria-expanded="false"
                                    >
                                        <i class="fa fa-cogs" />
                                    </button>
                                    <ul class="dropdown-menu">
                                        <li>
                                            <a class="dropdown-item" href="#">Remove</a>
                                        </li>
                                    </ul>
                                </div>
                            </td>
                        </tr>
                    </t>
                </tbody>
            </t>
        </t>
    </template>
</odoo>
```