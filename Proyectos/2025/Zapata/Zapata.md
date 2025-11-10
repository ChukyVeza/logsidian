---
Nombre: "[[Zapata]]"
Tipo: Proyecto
Inicio: 2025-04-01
Fin: 2025-11-15
Contacto: "[[@Sofía_Zapata]]"
---

- Digital Garden
  https://logsidian-git-main-chukyvezas-projects.vercel.app/pendientes/obras-pendientes/25-obra-zapata


#Obra/Zapata
```dataview
// =============================================
// **Funcional 100%**
//QUERY: BUSCADOR DE MENCIONES EN TAREAS ZAPATA
// Propósito: Localizar tareas pendientes con @menciones 
// en la sección específica de Rocabado del diario
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
```


### Script original y funcional
```
//Funcional 100%//
task 
//Buscar las tareas en las notas diarias//
from "Registro/Diario" 
//Que tengan la etiqueta #Obra/Zapata y estén sin completar//
WHERE contains(file.tags,"#Obra/Zapata") and !completed and outlinks
//Todos los enlaces volver texto//
flatten outlinks as link
//Los enlaces que contengan "@" Y NO sea el enlace "@Vpay"// 
WHERE contains(text, "@") AND !contains(["[[@VPay]]", "[[@Studio Industrial]]", "[[@Alsevic]]", "@Mauro_Vasquez"], link.file.name)
//Agrupar por enlace//
group by link.file.name
//Ordenar por nombre// 
sort link.file.name asc
```


## Script con el total de tareas encontradas:29 | Grupos:14 {DeepSeek}


```dataviewjs
// Configuración: Carpeta donde están las notas diarias
const folder = "Registro/Diario";

// Función recursiva para obtener todas las tareas y subtareas
function getAllTasks(page) {
    const allTasks = [];
    
    function processList(list) {
        if (list.task && !list.completed) {
            // Verificar si la tarea contiene #Obra/Zapata en cualquier parte
            if (list.text.includes("#Obra/Zapata")) {
                allTasks.push(list);
            }
        }
        
        // Procesar subtareas recursivamente
        if (list.subtasks) {
            for (const subtask of list.subtasks) {
                processList(subtask);
            }
        }
    }
    
    for (const list of page.file.lists) {
        processList(list);
    }
    
    return allTasks;
}

// Obtener todas las páginas de la carpeta
const pages = dv.pages(`"${folder}"`);

// Recopilar todas las tareas y subtareas no completadas que contengan "#Obra/Zapata"
const tasks = [];
for (const page of pages) {
    const pageTasks = getAllTasks(page);
    tasks.push(...pageTasks);
}

// Extraer enlaces del texto de cada tarea y agrupar (solo los que contienen @)
const groups = {};

for (const task of tasks) {
    // Método más robusto para encontrar enlaces
    // Buscar patrones de enlaces de Obsidian: [[...]] o @...
    const linkPattern = /(\[\[[^\]]+\]\]|@[^\s\]]+)/g;
    const matches = task.text.match(linkPattern);
    
    let foundValidLink = false;
    
    if (matches && matches.length > 0) {
        // Filtrar solo enlaces que contienen "@"
        const filteredMatches = matches.filter(match => 
            match.includes('@') && !match.includes('#')
        );
        
        if (filteredMatches.length > 0) {
            // Usar el primer enlace con "@" encontrado para agrupar
            const key = filteredMatches[0].replace('[[', '').replace(']]', '');
            if (!groups[key]) {
                groups[key] = [];
            }
            groups[key].push(task);
            foundValidLink = true;
        }
    }
    
    if (!foundValidLink) {
        // Si no encontramos enlaces con @, categorizar
        const hasObraZapata = task.text.includes("#Obra/Zapata");
        const key = hasObraZapata ? "Tareas madre (Sin asignar)" : "Otras tareas";
        
        if (!groups[key]) {
            groups[key] = [];
        }
        groups[key].push(task);
    }
}

// Ordenar los grupos alfabéticamente por la clave
const sortedGroups = Object.entries(groups).sort(([keyA], [keyB]) => 
    keyA.localeCompare(keyB)
);

// Mostrar las tareas agrupadas por enlace
for (const [key, groupTasks] of sortedGroups) {
    if (groupTasks.length > 0) {
        dv.header(3, key);
        dv.taskList(groupTasks);
    }
}

// Mostrar estadísticas
if (tasks.length === 0) {
    dv.paragraph("No se encontraron tareas pendientes con #Obra/Zapata");
} else {
    dv.paragraph(`**Total de tareas encontradas:** ${tasks.length} | **Grupos:** ${Object.keys(groups).length}`);
}
```

## Script con el resumen de tareas relacionadas :29 {DeepSeek}


```dataviewjs
// Configuración: Carpeta donde están las notas diarias
const folder = "Registro/Diario";

// Función recursiva para obtener todas las tareas y subtareas
function getAllTasks(page) {
    const allTasks = [];
    
    function processList(list) {
        // Verificar que la lista tenga las propiedades necesarias
        if (list && list.task !== undefined && !list.completed) {
            allTasks.push(list);
        }
        
        // Procesar subtareas recursivamente
        if (list.subtasks) {
            for (const subtask of list.subtasks) {
                processList(subtask);
            }
        }
    }
    
    if (page.file && page.file.lists) {
        for (const list of page.file.lists) {
            processList(list);
        }
    }
    
    return allTasks;
}

// Función mejorada para extraer enlaces
function extractLinks(text) {
    if (!text) return [];
    
    // Buscar enlaces en formato [[@Nombre]] o [[Nombre]]
    const bracketLinks = text.match(/\[\[([^\]]+)\]\]/g) || [];
    
    // Buscar menciones directas @Nombre (sin corchetes)
    const directMentions = text.match(/(?<!\w)@[^\s\],\[#]+/g) || [];
    
    // Combinar y limpiar
    const allLinks = [...bracketLinks, ...directMentions]
        .map(link => {
            // Limpiar enlaces con corchetes
            if (link.startsWith('[[') && link.endsWith(']]')) {
                return link.slice(2, -2);
            }
            return link;
        })
        .filter(link => link && link.includes('@')); // Solo enlaces que contienen @

    return allLinks;
}

// Función para verificar si una tarea está relacionada con #Obra/Zapata
function isRelatedToObraZapata(task) {
    if (!task || !task.text) return false;
    
    // Buscar #Obra/Zapata en la tarea actual
    if (task.text.includes("#Obra/Zapata")) {
        return true;
    }
    
    // Buscar en tareas padre
    let parentTask = task.parent;
    while (parentTask) {
        if (parentTask.text && parentTask.text.includes("#Obra/Zapata")) {
            return true;
        }
        parentTask = parentTask.parent;
    }
    
    return false;
}

// Obtener todas las páginas de la carpeta
const pages = dv.pages(`"${folder}"`);

// Recopilar todas las tareas y subtareas no completadas
const allTasks = [];
for (const page of pages) {
    if (page && page.file) {
        const pageTasks = getAllTasks(page);
        allTasks.push(...pageTasks);
    }
}

// Filtrar solo tareas que están relacionadas con #Obra/Zapata
const tasks = allTasks.filter(task => isRelatedToObraZapata(task));

// Agrupar tareas por enlaces @
const groups = {};

for (const task of tasks) {
    if (!task || !task.text) continue;
    
    const links = extractLinks(task.text);
    
    if (links.length > 0) {
        // Agrupar por cada enlace encontrado
        for (const link of links) {
            if (!link) continue;
            
            if (!groups[link]) {
                groups[link] = [];
            }
            // Evitar duplicados usando una clave única
            const taskKey = task.text + (task.section ? task.section.path : '');
            if (!groups[link].some(t => (t.text + (t.section ? t.section.path : '')) === taskKey)) {
                groups[link].push(task);
            }
        }
    } else {
        // Tareas sin asignaciones @
        const key = "Sin asignaciones @";
        if (!groups[key]) {
            groups[key] = [];
        }
        const taskKey = task.text + (task.section ? task.section.path : '');
        if (!groups[key].some(t => (t.text + (t.section ? t.section.path : '')) === taskKey)) {
            groups[key].push(task);
        }
    }
}

// Ordenar alfabéticamente
const sortedGroups = Object.entries(groups).sort(([keyA], [keyB]) => 
    keyA.localeCompare(keyB)
);

// Mostrar resultados
if (tasks.length === 0) {
    dv.paragraph("No se encontraron tareas pendientes con #Obra/Zapata");
} else {
    for (const [key, groupTasks] of sortedGroups) {
        dv.header(3, key);
        dv.taskList(groupTasks);
    }
    // Estadísticas
    dv.paragraph(`**Total de tareas relacionadas con #Obra/Zapata:** ${tasks.length}`);
}
```