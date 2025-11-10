
### Datos

```dataviewjs
// DVJS: Mostrar bloques con #dato y sus hijos indentados
const folderPath = "Registro/Diario";
const targetTag = "#dato";

const files = dv.pages(`"${folderPath}"`)
  .where(p => p.file.name); // asegura que sea archivo válido

for (let f of files) {
  const content = await dv.io.load(f.file.path);
  const lines = content.split('\n');
  let inDatoContext = false;
  let baseIndent = -1;

  for (let i = 0; i < lines.length; i++) {
    const line = lines[i];
    
    // Verificar si la línea contiene #dato y es un elemento de lista
    if (line.match(/^[\s]*[-*]\s.*#dato\b/) && !line.trim().startsWith('>')) {
      // Encontrado bloque con #dato
      dv.el("div", line, { cls: "block-dato-parent" });
      inDatoContext = true;
      // Calcular nivel de indentación (número de espacios antes del "-")
      baseIndent = line.search(/[-*]/);
    }
    else if (inDatoContext) {
      // Verificar si la línea actual está más indentada que baseIndent
      const currentIndent = line.search(/\S/); // -1 si está vacía
      if (currentIndent > baseIndent && line.trim() !== '' && !line.trim().startsWith('>')) {
        dv.el("div", line, { cls: "block-dato-child" });
      } else {
        // Salir del contexto si la indentación no es mayor o es una línea vacía/fuera
        inDatoContext = false;
        baseIndent = -1;
      }
    }
  }
}
```


```dataviewjs
// DVJS: Mostrar bloques con #dato y sus hijos indentados (versión robusta)
const folderPath = "Registro/Diario";
const targetTag = "#dato";

// Escapar el símbolo # para usarlo en regex
const escapedTag = targetTag.replace(/#/g, '\\#');
// Regex para detectar listas (ordenadas o desordenadas) que contengan #dato
const parentPattern = new RegExp(`^[\\s]*([-*]|\\d+\\.)\\s+.*${escapedTag}\\b`);

const files = dv.pages(`"${folderPath}"`).where(p => p.file.name);

for (let f of files) {
  const content = await dv.io.load(f.file.path);
  const lines = content.split('\n');

  let inDatoContext = false;
  let baseIndent = -1;

  for (let i = 0; i < lines.length; i++) {
    const line = lines[i];
    const trimmed = line.trim();

    // Saltar líneas vacías inmediatas en contexto (pero no salir del contexto)
    if (inDatoContext && trimmed === '') {
      // Opcional: puedes renderizar o ignorar líneas vacías
      // Aquí las ignoramos (no se muestran)
      continue;
    }

    // Detectar un nuevo bloque padre #dato (en cualquier momento)
    if (parentPattern.test(line) && !trimmed.startsWith('>')) {
      // Renderizar como Markdown para soportar enlaces, énfasis, etc.
      dv.paragraph(line, { cls: "block-dato-parent" });
      inDatoContext = true;
      baseIndent = line.search(/[-*]|\d+\./); // posición del inicio de la lista
    }
    // Procesar líneas hijas si estamos en contexto
    else if (inDatoContext) {
      const firstCharIndex = line.search(/\S/); // -1 si vacía (aunque ya las saltamos arriba)

      // Salir del contexto si:
      // - La línea no está indentada más que el padre
      // - Es una cita
      // - Es un bloque de código (comienza con 4+ espacios y no es parte de lista)
      if (
        firstCharIndex === -1 ||
        firstCharIndex <= baseIndent ||
        trimmed.startsWith('>') ||
        (firstCharIndex >= 4 && !/^\s*([-*]|\d+\.)/.test(line))
      ) {
        inDatoContext = false;
        baseIndent = -1;
        // No procesar esta línea; pero permitir que un futuro #dato la reactive
        continue;
      }

      // Si sigue en contexto, renderizar como hijo
      dv.paragraph(line, { cls: "block-dato-child" });
    }
  }
}
```

---

### Datos relevantes (citas)

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

