### Buscar todos las Citas y Callouts

```dataviewjs
// DVJS: Encontrar todas las citas (blockquote) que usan la sintaxis "> ..."
// Agrupa líneas consecutivas que comienzan con ">" (y líneas vacías intermedias) como una sola cita.

const files = dv.pages('"Registro/Diario"');

for (let file of files) {
  const content = await dv.io.load(file.file.path);
  const lines = content.split('\n');

  let inQuote = false;
  let buffer = [];

  for (let i = 0; i < lines.length; i++) {
    const line = lines[i];
    const trimmed = line.trim();

    // Si la línea comienza con ">", es parte de una cita
    if (/^\s*>/.test(line)) {
      buffer.push(line);
      inQuote = true;
    }
    // Si estamos en una cita y la línea está vacía, también la incluimos (para formato)
    else if (inQuote && trimmed === "") {
      buffer.push(line);
    }
    // Si estamos en una cita y la línea NO es vacía y NO comienza con ">", termina la cita
    else if (inQuote) {
      // Mostrar la cita completa
      dv.paragraph(buffer.join("\n"));
      // Reiniciar
      inQuote = false;
      buffer = [];
      // Esta línea actual NO se procesa (es texto normal)
    }
    // Si no estamos en cita y es texto normal, lo ignoramos
  }

  // Al final del archivo, si quedó una cita sin cerrar
  if (inQuote && buffer.length > 0) {
    dv.paragraph(buffer.join("\n"));
  }
}
```


```dataviewjs
// DVJS: Encontrar todas las citas (blockquote) que usan la sintaxis "> ..."
// Agrupa líneas consecutivas que comienzan con ">" (y líneas vacías intermedias) como una sola cita.
// Muestra un enlace a la nota antes de cada cita.

const files = dv.pages('"Registro/Diario"');

for (let file of files) {
  const content = await dv.io.load(file.file.path);
  const lines = content.split('\n');

  let inQuote = false;
  let buffer = [];

  for (let i = 0; i < lines.length; i++) {
    const line = lines[i];
    const trimmed = line.trim();

    // Si la línea comienza con ">", es parte de una cita
    if (/^\s*>/.test(line)) {
      buffer.push(line);
      inQuote = true;
    }
    // Si estamos en una cita y la línea está vacía, también la incluimos (para formato)
    else if (inQuote && trimmed === "") {
      buffer.push(line);
    }
    // Si estamos en una cita y la línea NO es vacía y NO comienza con ">", termina la cita
    else if (inQuote) {
      // Mostrar el enlace a la nota primero
      dv.paragraph(`[[${file.file.path}|${file.file.name}]]`);
      // Luego la cita completa
      dv.paragraph(buffer.join("\n"));
      // Reiniciar
      inQuote = false;
      buffer = [];
    }
  }

  // Al final del archivo, si quedó una cita sin cerrar
  if (inQuote && buffer.length > 0) {
    dv.paragraph(`[[${file.file.path}|${file.file.name}]]`);
    dv.paragraph(buffer.join("\n"));
  }
}
```



---
### Buscar todos las  citas y callouts con la etiqueta #dato 

```dataviewjs
// DVJS: Mostrar solo las citas (blockquote) que contienen la etiqueta #dato

const files = dv.pages('"Registro/Diario"');

for (let file of files) {
  const content = await dv.io.load(file.file.path);
  const lines = content.split('\n');

  let inQuote = false;
  let buffer = [];

  for (let i = 0; i < lines.length; i++) {
    const line = lines[i];
    const trimmed = line.trim();

    if (/^\s*>/.test(line)) {
      buffer.push(line);
      inQuote = true;
    }
    else if (inQuote && trimmed === "") {
      buffer.push(line);
    }
    else if (inQuote) {
      // Llegamos al final del bloque de cita → verificar si contiene #dato
      const quoteText = buffer.join("\n");
      if (/#dato\b/.test(quoteText)) {
        dv.paragraph(quoteText);
      }
      // Reiniciar
      inQuote = false;
      buffer = [];
    }
    // Líneas fuera de cita se ignoran
  }

  // Al final del archivo, verificar si hay una cita sin cerrar
  if (inQuote && buffer.length > 0) {
    const quoteText = buffer.join("\n");
    if (/#dato\b/.test(quoteText)) {
      dv.paragraph(quoteText);
    }
  }
}
```


```dataviewjs
// DVJS: Mostrar solo las citas (blockquote) que contienen la etiqueta #dato
// y preceder cada una con un enlace a la nota de origen

const files = dv.pages('"Registro/Diario"');

for (let file of files) {
  const content = await dv.io.load(file.file.path);
  const lines = content.split('\n');

  let inQuote = false;
  let buffer = [];

  for (let i = 0; i < lines.length; i++) {
    const line = lines[i];
    const trimmed = line.trim();

    if (/^\s*>/.test(line)) {
      buffer.push(line);
      inQuote = true;
    }
    else if (inQuote && trimmed === "") {
      buffer.push(line);
    }
    else if (inQuote) {
      // Fin del bloque de cita
      const quoteText = buffer.join("\n");
      if (/#dato\b/.test(quoteText)) {
        // Enlace a la nota primero
        dv.paragraph(`[[${file.file.name}]]`);
        // Luego la cita
        dv.paragraph(quoteText);
      }
      inQuote = false;
      buffer = [];
    }
    // Líneas fuera de cita se ignoran
  }

  // Al final del archivo, verificar cita pendiente
  if (inQuote && buffer.length > 0) {
    const quoteText = buffer.join("\n");
    if (/#dato\b/.test(quoteText)) {
      dv.paragraph(`[[${file.file.name}]]`);
      dv.paragraph(quoteText);
    }
  }
}
```


## ✅ Versión A: Párrafos mejorados (con icono y enlace antes)

```dataviewjs
// DVJS: Citas con #dato + enlace a nota + icono (formato visual limpio)

const files = dv.pages('"Registro/Diario"');

for (let file of files) {
  const content = await dv.io.load(file.file.path);
  const lines = content.split('\n');

  let inQuote = false;
  let buffer = [];

  for (let line of lines) {
    const trimmed = line.trim();

    if (/^\s*>/.test(line)) {
      buffer.push(line);
      inQuote = true;
    }
    else if (inQuote && trimmed === "") {
      buffer.push(line);
    }
    else if (inQuote) {
      const quoteText = buffer.join("\n");
      if (/#dato\b/.test(quoteText)) {
        dv.paragraph(`📌 [[${file.file.name}]]`);
        dv.paragraph(quoteText);
        dv.el("br"); // separador visual ligero
      }
      inQuote = false;
      buffer = [];
    }
  }

  if (inQuote && buffer.length > 0) {
    const quoteText = buffer.join("\n");
    if (/#dato\b/.test(quoteText)) {
      dv.paragraph(`📌 [[${file.file.name}]]`);
      dv.paragraph(quoteText);
      dv.el("br");
    }
  }
}
```


## ✅ Versión B: **Tabla Dataview** (recomendada para análisis)


```dataviewjs
// DVJS: Tabla de citas con #dato — solo columnas "Nota" y "Cita"

const files = dv.pages('"Registro/Diario"');
let results = [];

for (let file of files) {
  const content = await dv.io.load(file.file.path);
  const lines = content.split('\n');

  let inQuote = false;
  let buffer = [];

  for (let line of lines) {
    const trimmed = line.trim();

    if (/^\s*>/.test(line)) {
      buffer.push(line);
      inQuote = true;
    }
    else if (inQuote && trimmed === "") {
      buffer.push(line);
    }
    else if (inQuote) {
      const quoteText = buffer.join("\n");
      if (/#dato\b/.test(quoteText)) {
        results.push({
          nota: `[[${file.file.name}]]`,
          cita: quoteText
        });
      }
      inQuote = false;
      buffer = [];
    }
  }

  // Al final del archivo
  if (inQuote && buffer.length > 0) {
    const quoteText = buffer.join("\n");
    if (/#dato\b/.test(quoteText)) {
      results.push({
        nota: `[[${file.file.name}]]`,
        cita: quoteText
      });
    }
  }
}

// Mostrar tabla con solo "Nota" y "Cita"
dv.table(["Nota", "Cita"], results.map(r => [r.nota, r.cita]));
```







---

### Buscar todos los callouts con la etiqueta #dato y se encuentren dentro del encabezado ### [[Rocabado]]


```dataviewjs
// DVJS: Mostrar citas que contienen #dato y están dentro de "### [[Rocabado]]"

const files = dv.pages('"Registro/Diario"');

for (let file of files) {
  const content = await dv.io.load(file.file.path);
  const lines = content.split('\n');

  let inRocabado = false;
  let inQuote = false;
  let buffer = [];

  for (let i = 0; i < lines.length; i++) {
    const line = lines[i];
    const trimmed = line.trim();

    // Detectar encabezados para controlar el contexto
    if (/^#{1,3}\s/.test(line)) {
      // Es un encabezado de nivel 1, 2 o 3
      if (/^###\s*\[\[Rocabado\]\]/.test(line)) {
        inRocabado = true;
      } else if (/^#{1,3}\s/.test(line)) {
        // Cualquier otro encabezado de nivel 1-3 cierra la sección actual
        inRocabado = false;
      }
    }

    // Procesar cita solo si estamos dentro de Rocabado
    if (inRocabado) {
      if (/^\s*>/.test(line)) {
        buffer.push(line);
        inQuote = true;
      }
      else if (inQuote && trimmed === "") {
        buffer.push(line);
      }
      else if (inQuote) {
        // Fin de la cita: verificar si contiene #dato
        const quoteText = buffer.join("\n");
        if (/#dato\b/.test(quoteText)) {
          dv.paragraph(quoteText);
        }
        inQuote = false;
        buffer = [];
      }
    } else {
      // Si no estamos en Rocabado y estábamos en una cita, reiniciar
      if (inQuote) {
        inQuote = false;
        buffer = [];
      }
    }
  }

  // Al final del archivo, verificar cita pendiente (solo si aún en Rocabado)
  if (inQuote && buffer.length > 0 && inRocabado) {
    const quoteText = buffer.join("\n");
    if (/#dato\b/.test(quoteText)) {
      dv.paragraph(quoteText);
    }
  }
}
```



```dataviewjs
// DVJS: Mostrar enlace a la nota y luego la cita con #dato dentro de "### [[Rocabado]]"

const files = dv.pages('"Registro/Diario"');

for (let file of files) {
  const content = await dv.io.load(file.file.path);
  const lines = content.split('\n');

  let inRocabado = false;
  let inQuote = false;
  let buffer = [];

  for (let i = 0; i < lines.length; i++) {
    const line = lines[i];
    const trimmed = line.trim();

    // Detectar encabezados para controlar el contexto
    if (/^#{1,3}\s/.test(line)) {
      if (/^###\s*\[\[Rocabado\]\]/.test(line)) {
        inRocabado = true;
      } else if (/^#{1,3}\s/.test(line)) {
        inRocabado = false;
      }
    }

    if (inRocabado) {
      if (/^\s*>/.test(line)) {
        buffer.push(line);
        inQuote = true;
      }
      else if (inQuote && trimmed === "") {
        buffer.push(line);
      }
      else if (inQuote) {
        const quoteText = buffer.join("\n");
        if (/#dato\b/.test(quoteText)) {
          // Primero: enlace a la nota
          dv.paragraph(`[[${file.file.name}]]`);
          // Luego: la cita
          dv.paragraph(quoteText);
        }
        inQuote = false;
        buffer = [];
      }
    } else {
      if (inQuote) {
        inQuote = false;
        buffer = [];
      }
    }
  }

  // Al final del archivo
  if (inQuote && buffer.length > 0 && inRocabado) {
    const quoteText = buffer.join("\n");
    if (/#dato\b/.test(quoteText)) {
      dv.paragraph(`— [[${file.file.name}]]`);
      dv.paragraph(quoteText);
    }
  }
}
```





