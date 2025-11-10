

````
// Configuración: Carpeta donde están las notas diarias
const folder = "Registro/Diario";

// Obtener todas las páginas de la carpeta
const pages = dv.pages(`"${folder}"`);

// Recopilar todas las tareas no completadas que contengan "Rocabado"
const tasks = [];
for (const page of pages) {
    for (const list of page.file.lists) {
        if (list.task && !list.completed && list.text.includes("Rocabado")) {
            tasks.push(list);
        }
    }
}

// Extraer enlaces del texto de cada tarea y agrupar
const groups = {};
for (const task of tasks) {
    // Buscar todos los enlaces en el texto (formato [[...]])
    const matches = task.text.match(/\[\[([^\]]+)\]\]/g);
    if (matches && matches.length > 0) {
        // Usar el primer enlace encontrado para agrupar (o todos si prefieres)
        const key = matches[0]; // Puedes ajustar esto si quieres múltiples enlaces
        if (!groups[key]) {
            groups[key] = [];
        }
        groups[key].push(task);
    } else {
        // Tareas sin enlaces
        const key = "Sin enlace";
        if (!groups[key]) {
            groups[key] = [];
        }
        groups[key].push(task);
    }
}

// Mostrar las tareas agrupadas por enlace
for (const [key, groupTasks] of Object.entries(groups)) {
    dv.header(3, key);
    dv.taskList(groupTasks);
}
```
````