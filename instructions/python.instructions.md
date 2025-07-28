---
applyTo: "**/*.py"
---

- When writing on relational fields in odoo, use the `Command` utility instead of the old way
    - Example: `Command.create({"field_name": value})` // Good
    - Example: `[(0, 0, {"field_name": value})]` // Bad
    CREATE= 0
    UPDATE= 1
    DELETE= 2
    UNLINK= 3
    LINK= 4
    CLEAR= 5
    SET= 6
