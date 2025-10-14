---
applyTo: "**"
---

# General Code Generation Guidelines
   **Use always the antagonic socratic method to ask for more information.**  
   - If you need more information, ask questions to clarify the requirements.
   - Do not assume or guess what the user wants.

1. **DRIVE (Don’t Repeat Yourself)**  
   - Do not duplicate logic or code blocks.  
   - If you notice repeated patterns, extract into a helper function or variable.

2. **Early Returns**  
   - Favor guard clauses at the top of functions to handle invalid or edge‐case inputs, then return immediately.  
   - This minimizes nesting and keeps the “happy path” unindented.

3. **Avoid Complex `if-else` Chains**  
   - Do not generate large `if-else` or nested conditionals.  
   - Use early returns, `elif`, or strategy/value maps instead of deep nesting.  

4. **Generate Only What’s Requested**  
   - Do not add imports, helper functions, or features unless the user explicitly asks for them.  
   - If you have ideas for enhancements, leave them as `// TODO: …` or `# TODO: …` inside the code, without implementing.

4. **Keep It Clear and Minimal**  
   - Write concise, focused code.  

5. **Respect Project Style**  
   - Follow existing naming conventions and formatting of the project.  
   - If style isn’t specified, use the idiomatic patterns of the language (PEP8 for Python, standard ESLint style for JavaScript, etc.).

6. **Useful odoo commands**
   - The odoo commands must be run from the odoo folder
   - To start the odoo server:
       ```bash
       ./odoo-bin
       ```
7. **Always generate the code in english unless the user explicitly asks for another language.**  
   - If the user requests code in a specific language, generate it in that language.
       
8. **The commands to execute the odoo tests are:**
   - [-][tag][/module][:class][.method]
   - Example:
       ```bash
         ./odoo-bin -d the_db --test-tags=.method
         ./odoo-bin -d the_db --test-tags=:Class
         ./odoo-bin -d the_db --test-tags=tag
       ```
9. **When coding in odoo follow its patterns**

10. **Be an antagonist**  
    - If the user asks for a feature that is not aligned with the project goals or best practices, politely explain why it may not be a good idea and suggest alternatives.  

11. **Avoid using single letter variable names**  
