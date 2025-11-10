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
    .where(t => t.section?.subpath === "Rocabado" && 
                !t.completed && 
                t.text.includes("@"));

dv.paragraph(`**TOTAL:** ${tasks.length} tareas pendientes con @menciones`);
```

```dataview
// =============================================
// **Funcional 100%**
//QUERY: BUSCADOR DE MENCIONES EN TAREAS ROCABADO
// Propósito: Localizar tareas pendientes con @menciones 
// en la sección específica de Rocabado del diario
// =============================================
// Buscar tareas en todo el vault//
TASK
// Especificar la carpeta donde buscar - en este caso el diario//
FROM "Registro/Diario"
// Filtrar: solo tareas en la sección "Rocabado" que no están completadas//
WHERE meta(section).subpath = "Rocabado" AND !completed
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

```data
TASK
FROM "Registro/Diario"
WHERE meta(section).subpath = "Rocabado" AND !completed
GROUP BY link
SORT link.file.name asc
```


>[!abstract]- Pedidos

```dataview
LIST
FROM "Registro/Diario"
WHERE contains(file.lists.text, "pedido")
```

>[!example]- Datos de obra

```dataviewjs
// DVJS: Mostrar bloques con #dato dentro de "### [[Rocabado]]", agrupados por nota, con enlace
const folderPath = "Registro/Diario";
const targetHeader = "### [[Rocabado]]";

const files = dv.pages(`"${folderPath}"`)
  .where(p => p.file.name)
  .sort(p => p.file.name, 'desc');

for (let f of files) {
  const content = await dv.io.load(f.file.path);
  const lines = content.split('\n');
  let i = 0;
  let blocksForThisNote = [];
  let insideTargetSection = false;

  while (i < lines.length) {
    const line = lines[i];

    // Detectar encabezados de nivel H1-H3 (cualquier #, ##, o ###)
    if (line.match(/^#{1,3}\s/)) {
      // Verificar si es exactamente el encabezado objetivo
      if (line.trim() === targetHeader) {
        insideTargetSection = true;
      } else {
        // Cualquier otro H1/H2/H3 cierra la sección actual
        insideTargetSection = false;
      }
      i++;
      continue;
    }

    // Solo procesar si estamos dentro de la sección objetivo
    if (insideTargetSection) {
      // Detectar bullet con #dato, NO blockquote, NO tarea
      if (
        line.match(/^[\s]*[-*]\s.*#dato\b/) &&
        !line.trim().startsWith('>') &&
        !line.match(/^[\s]*[-*]\s\[[\sx]\]/)
      ) {
        const bulletIndent = line.search(/[-*]/);
        let block = line;

        i++;
        while (i < lines.length) {
          const nextLine = lines[i];
          const nextIndent = nextLine.search(/\S/);

          // Si es un encabezado, salimos de la sección
          if (nextLine.match(/^#{1,3}\s/)) {
            insideTargetSection = false;
            break;
          }

          if (nextLine.trim() === '') {
            block += '\n' + nextLine;
            i++;
            continue;
          }

          // Detener si encontramos blockquote, tarea o bullet de mismo/superior nivel
          if (
            nextLine.trim().startsWith('>') ||
            nextLine.match(/^[\s]*[-*]\s\[[\sx]\]/) ||
            (nextIndent !== -1 && nextIndent <= bulletIndent)
          ) {
            break;
          }

          const contentIndent = bulletIndent + 2;
          if (nextIndent >= contentIndent) {
            block += '\n' + nextLine;
            i++;
          } else {
            break;
          }
        }

        blocksForThisNote.push(block);
        continue; // ya incrementamos i dentro del bucle
      }
    }

    i++;
  }

  // Renderizar solo si hay bloques en esta nota
  if (blocksForThisNote.length > 0) {
    dv.header(4, dv.fileLink(f.file.path, false, f.file.name));
    for (let block of blocksForThisNote) {
      dv.el("div", block, { cls: "block-dato-parent" });
    }
  }
}
```

