---
dg-publish: true
---

>[!abstract]- Tareas Por Encargados

```dataviewjs
// =============================================
// CONTEO GENERAL TOTAL - JavaScript version
// =============================================
const tasks = dv.pages('"Registro/Diario"')
    .file.tasks
    .where(t => t.section?.subpath === "Hatt" && 
                !t.completed && 
                t.text.includes("@"));

dv.paragraph(`**TOTAL:** ${tasks.length} tareas pendientes con @menciones`);
```

```dataview
// =============================================
// **Funcional 100%**
//QUERY: BUSCADOR DE MENCIONES EN TAREAS HATT
// Propósito: Localizar tareas pendientes con @menciones 
// en la sección específica de Hatt del diario
// =============================================
// Buscar tareas en todo el vault//
TASK
// Especificar la carpeta donde buscar - en este caso el diario//
FROM "Registro/Diario"
// Filtrar: solo tareas en la sección "Hatt" que no están completadas//
WHERE meta(section).subpath = "Hatt" AND !completed
// Expandir los enlaces salientes de cada tarea para poder analizarlos//
FLATTEN outlinks as link
// Filtrar solo tareas que contienen el carácter "@" en su texto//
WHERE contains(text, "@")
// Agrupar los resultados por el nombre del archivo de destino del enlace//
GROUP BY link.file.name
// Ordenar alfabéticamente por el nombre del archivo//
SORT link.file.name asc
```

 > [!todo]- Tareas Por Fechas

```dataview
TASK
FROM "Registro/Diario"
WHERE meta(section).subpath = "Hatt" AND !completed
GROUP BY link
SORT link.file.name asc
```


