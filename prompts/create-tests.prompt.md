---
mode: 'agent'
tools: ['development-toolset']
---

Based on the other tests of the current module and codebase implement the tests for the controller methods, if you need to mock some data use the MockRequest from odoo.

- Make sure to cover edge cases and error handling.
- Ensure that the tests are isolated and do not depend on external systems or state.
- Use appropriate assertions to validate the expected outcomes.
- Follow best practices for writing clean and maintainable test code.
- Include setup and teardown methods if necessary to prepare the test environment.
- Only create data when absolutely necessary for the tests, if you are creating a lot of data, consider creating a method to do so or directly data demo files.