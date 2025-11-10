---
dg-publish: true
---

>[!note]- Trareas urgentes para traslado
>blue

Zonas que se necesitan habilitar:

**Master Suite**
- *Dormitorio*
- *Baño*
- *Vestidor*

**Área servicio**
- *Dormitorio*
- *Patio*
- *Despensa*
- *Cocineta*

**Piscina**

```dataviewjs
// =============================================
// CONTEO GENERAL TOTAL - JavaScript version
// =============================================
const tasks = dv.pages('"Registro/Diario"')
    .file.tasks
    .where(t => t.section?.subpath === "Zapata" && 
                !t.completed && 
                t.text.includes("@") &&
                t.text.includes("#urgente"));  // ← Filtro añadido

dv.paragraph(`**TOTAL:** ${tasks.length} tareas pendientes con @mención de #urgente`);
```

>[!note]- Trareas urgentes por encargado
>blue


```dataview
// =============================================
// **Funcional 100%**
//QUERY: BUSCADOR DE MENCIONES EN TAREAS [[Zapata]]
// Propósito: Localizar tareas pendientes con @menciones 
// en la sección específica de [[Zapata]] del diario
// =============================================
// Buscar tareas en todo el vault//
TASK
// Especificar la carpeta donde buscar - en este caso el diario//
FROM "Registro/Diario"
// Filtrar: solo tareas en la sección "Zapata" que no están completadas//
WHERE meta(section).subpath = "Zapata" AND !completed
// Expandir los enlaces salientes de cada tarea para poder analizarlos//
FLATTEN outlinks as link
// Filtrar solo tareas que contienen el carácter "@" en su texto//
WHERE contains(text, "@") AND contains(text, "#urgente") 
FLATTEN replace(text,"#urgente", "") as cleantext
// Agrupar los resultados por el nombre del archivo de destino del enlace//
GROUP BY link.file.name
// Ordenar alfabéticamente por el nombre del archivo//
SORT link.file.name asc
```

---
>[!abstract]- Tareas Por Contratistas

```dataviewjs
// =============================================
// CONTEO GENERAL TOTAL - JavaScript version
// =============================================
const tasks = dv.pages('"Registro/Diario"')
    .file.tasks
    .where(t => t.section?.subpath === "Zapata" && 
                !t.completed && 
                t.text.includes("@"));

dv.paragraph(`**TOTAL:** ${tasks.length} tareas pendientes con @menciones`);
```

>[!abstract]- Todas las Tareas Por Contratistas
// =============================================
// **Funcional 100%**
//QUERY: BUSCADOR DE MENCIONES EN TAREAS ROCABADO
// Propósito: Localizar tareas pendientes con @menciones 
// en la sección específica de Zapata del diario
// =============================================
// Buscar tareas en todo el vault//
TASK
// Especificar la carpeta donde buscar - en este caso el diario//
FROM "Registro/Diario"
// Filtrar: solo tareas en la sección "Zapata" que no están completadas//
WHERE meta(section).subpath = "Zapata" AND !completed
// Expandir los enlaces salientes de cada tarea para poder analizarlos//
FLATTEN outlinks as link
// Filtrar solo tareas que contienen el carácter "@" en su texto//
WHERE contains(text, "@")
// Agrupar los resultados por el nombre del archivo de destino del enlace//
GROUP BY link.file.name
// Ordenar alfabéticamente por el nombre del archivo//
SORT link.file.name asc

>[!failure]- Tareas Por Fechas
task
from "Registro/Diario"   
sort file.name
where contains(tags, "#Obra/Zapata") and !completed
group by file.link 

>[!success]- Tareas Completados
