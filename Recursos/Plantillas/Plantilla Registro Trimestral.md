---
trimestre: "{{title}}"
inicio: "{{date:YYYY-MM-DD}}"
fin: "{{date:YYYY-MM-DD:end-of-quarter}}"
estado: Activo
etiquetas: [Trimestre, GTD, Planificación]
---

# Planificación Trimestral: [[{{title}}]]

## 🎯 Objetivos Clave del Trimestre
*Describe 3-5 objetivos principales y medibles para este trimestre.*

1.  **Objetivo 1:** Descripción clara y concisa.
    *   **Métrica:** ¿Cómo medirás el éxito?
2.  **Objetivo 2:** Descripción clara y concisa.
    *   **Métrica:** ¿Cómo medirás el éxito?
3.  **Objetivo 3:** Descripción clara y concisa.
    *   **Métrica:** ¿Cómo medirás el éxito?

## 📂 Proyectos Activos (GTD)
*Aquí se listarán automáticamente todos los proyectos etiquetados con `#proyecto` y que no estén terminados. Cada proyecto debería ser una nota separada en tu carpeta de proyectos.*

```dataview
TABLE estado, progreso, objetivo
FROM #proyecto AND !"00_Plantillas"
WHERE estado != "Completado"
SORT file.name ASC