---
mode: 'agent'
tools: ['development-toolset']
---
El timesheet es el registro diario del tiempo que se a cada tarea dentro del ecosistema de Odoo en Vauxoo. Permite medir esfuerzo, planificar mejor los proyectos y, en muchos casos, justificar trabajo facturable al cliente.

Sigue estas pautas:

Una entrada por tarea: Si trabajaste en 3 tareas diferentes, haz 3 entradas.

Descripción específica: Explica qué hiciste, por qué lo hiciste y si hubo algo que destacar (problemas, soluciones, decisiones técnicas).

Usa lenguaje técnico cuando aplique: Nombra métodos, modelos, errores o tests modificados.

Incluye referencia a la tarea (T#): Si es una tarea en Odoo (por ejemplo en el módulo account), identifícala con su código.

Evita descripciones genéricas como “avance”, “soporte” o “trabajo en módulo”.

🧾 Ejemplo de Entrada Correcta
    Tarea: T#10724 - Error en validación de facturas de proveedor
    Horas: 1.5
    Descripción:
    Se corrigió el método _check_invoice_date en el modelo account.move que estaba generando validación incorrecta en facturas de proveedor cuando invoice_date_due < invoice_date. Se aplicó fix y se subió MR en rama 15.0-fix-invoice-validation-jdoe. Pendiente revisión QA.

 Tip Final
Piensa en tu TS como una bitácora profesional: útil para ti, para tu equipo, y para dejar trazabilidad de lo que hiciste.

No excedas los 600 caracteres en la descripción para mantener claridad y concisión.