
### 📌 **Objetivo general**

Buscar, en todas las notas de la carpeta `Registro/Diario`, los **viñetas** (bullet points) que:

- están dentro de una sección titulada exactamente `### [[Rocabado]]`,
- contienen la etiqueta `#dato`,
- **no** son citas (`>`),
- **no** son tareas (`- [ ]` o `- [x]`).

Luego, **muestra esos bullets agrupados por nota**, incluyendo un **enlace al archivo** donde aparecen.

---

### 🔍 **Cómo lo hace, paso a paso**

1. **Busca notas**  
    Revisa todas las notas en la carpeta `"Registro/Diario"` y las ordena por nombre de archivo de más nuevo a más antiguo.
    
2. **Revisa cada nota línea por línea**  
    Usa un bucle para analizar el contenido de cada archivo.
    
3. **Detecta la sección `### [[Rocabado]]`**  
    Solo presta atención a lo que esté **dentro** de esa sección. Si aparece otro encabezado (`#`, `##`, `###`), sale de esa sección.
    
4. **Encuentra bullets con `#dato`**  
    Mientras está dentro de la sección correcta, busca líneas que:
    
    - empiecen con `-` o `*`,
    - contengan la palabra `#dato`,
    - **no** empiecen con `>` (para evitar citas),
    - **no** sean tareas (como `- [ ]`).
5. **Captura el contenido asociado al bullet**  
    Si el bullet tiene líneas posteriores con sangría (es decir, texto que pertenece al mismo punto), las incluye también — pero **se detiene** si:
    
    - aparece una cita (`>`),
    - aparece otra tarea,
    - aparece otro bullet al mismo nivel o superior,
    - o aparece un nuevo encabezado.
6. **Muestra los resultados**  
    Por cada nota que tenga al menos un bullet que cumpla las condiciones:
    
    - pone un **encabezado de nivel 4** con el nombre de la nota (y enlace a ella),
    - muestra cada bullet (con su contenido sangrado, si lo tiene) dentro de un bloque `<div>`.

---

### ✅ **En resumen muy simple**

> “Busca en mis notas diarias los puntos con `#dato` que estén bajo el título `### [[Rocabado]]`, y muéstramelos agrupados por nota, sin incluir citas ni tareas.”

Este script respeta la estructura de sangría y evita mezclar bloques distintos, lo que es útil para mantener la claridad en notas con listas anidadas.

---

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



---

### Eliminar las tareas del query

```dataviewjs
// DVJS: Mostrar bloques con #dato y sus hijos indentados (sin dividir bullets multilínea, excluyendo tareas)
const folderPath = "Registro/Diario";
const targetTag = "#dato";

const files = dv.pages(`"${folderPath}"`)
  .where(p => p.file.name);

for (let f of files) {
  const content = await dv.io.load(f.file.path);
  const lines = content.split('\n');
  let i = 0;

  while (i < lines.length) {
    const line = lines[i];

    // Buscar bullets con #dato, pero no blockquotes ni tareas
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

        if (nextLine.trim() === '') {
          block += '\n' + nextLine;
          i++;
          continue;
        }

        if (nextLine.trim().startsWith('>')) {
          break;
        }

        // Detectar si la línea es una nueva tarea → romper (no incluir)
        if (nextLine.match(/^[\s]*[-*]\s\[[\sx]\]/)) {
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

      dv.el("div", block, { cls: "block-dato-parent" });
    } else {
      i++;
    }
  }
}
```


---

### Adicionar enlaces de notas

```dataviewjs
// DVJS: Mostrar bloques con #dato agrupados por nota, con enlace a la nota, excluyendo tareas y blockquotes
const folderPath = "Registro/Diario";

const files = dv.pages(`"${folderPath}"`)
  .where(p => p.file.name)
  .sort(p => p.file.name, 'desc'); // opcional: orden descendente (más reciente primero)

for (let f of files) {
  const content = await dv.io.load(f.file.path);
  const lines = content.split('\n');
  let i = 0;
  let blocksForThisNote = [];

  while (i < lines.length) {
    const line = lines[i];

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

        if (nextLine.trim() === '') {
          block += '\n' + nextLine;
          i++;
          continue;
        }

        // Detener si encontramos un blockquote, nueva tarea o nuevo bullet del mismo nivel
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
    } else {
      i++;
    }
  }

  // Solo renderizar si hay bloques con #dato en esta nota
  if (blocksForThisNote.length > 0) {
    // Enlace a la nota (antes del contenido)
    dv.header(4, dv.fileLink(f.file.path, false, f.file.name));
    
    // Renderizar cada bloque
    for (let block of blocksForThisNote) {
      dv.el("div", block, { cls: "block-dato-parent" });
    }
  }
}
```

---

### Filtrar encabezado ### [[Rocabado]]

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




---

