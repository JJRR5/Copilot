---
applyTo: "**/*.xml"
---

- When using t directives avoid generating t tags, combine them with html tags instead.
    - Example: `<t t-out="variable"/>` // Bad
    - Example: `<span t-out="variable"/>` // Good
    - Example: `
        <t t-if="condition">
            <span t-out="variable"/>
        </t>
    ` // Bad
    - Example: `
        <span t-if="condition" t-out="variable"/>
    ` // Good

- Do not use t-esc or t-raw directives, use t-out instead.

- If the line is too long, break it into multiple lines.
    - Example: // Good
        <span 
            t-set="contract_currency" 
            t-value="contract.purchase_order_ids[0].currency_id if contract.purchase_order_ids else False" 
        />
    - Example: // Bad
        <span t-set="contract_currency" t-value="contract.purchase_order_ids[0].currency_id if contract.purchase_order_ids else False" />

- When using t-attf, use the double curly braces syntax.
    - Example: `<span t-attf-class="{{ varable }}"/>` // Good
    - Example: `<span t-attf-class="#{ variable }"/>` // Bad