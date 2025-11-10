### Buscar todos los Callouts

```dataviewjs
// DVJS: Encontrar todos los callouts que usan la sintaxis "> [!..."

const files = dv.pages('"Registro/Diario"');

for (let file of files) {
  const content = await dv.io.load(file.file.path);
  const lines = content.split('\n');

  let inCallout = false;
  let buffer = [];

  for (let line of lines) {
    const trimmed = line.trim();

    // Detecta inicio de callout: "> [!tipo]..."
    if (/^\s*>+\s*\[!\w+/.test(trimmed)) {
      // Mostrar el callout anterior si existe
      if (inCallout && buffer.length > 0) {
        dv.paragraph(buffer.join("\n"));
      }
      // Empezar nuevo callout
      inCallout = true;
      buffer = [line];
    }
    // Dentro de un callout: recoger líneas que continúen con "> "
    else if (inCallout) {
      // Una línea pertenece al callout si:
      // - empieza con ">" (aunque puede tener más niveles: ">>", etc.)
      // - o está vacía (para mantener espaciado)
      if (/^\s*>/.test(line) || trimmed === "") {
        buffer.push(line);
      } else {
        // Fin del callout
        dv.paragraph(buffer.join("\n"));
        inCallout = false;
        buffer = [];
        // No procesamos esta línea (puede ser texto normal o nuevo callout en otra iteración)
      }
    }
  }

  // Al final del archivo
  if (inCallout && buffer.length > 0) {
    dv.paragraph(buffer.join("\n"));
  }
}
```

---
### Buscar todos los callouts con la etiqueta #dato 

```dataviewjs
// DVJS: Mostrar callouts (sin ">") que contengan la etiqueta #dato

const files = dv.pages('"Registro/Diario"');

for (let file of files) {
  const content = await dv.io.load(file.file.path);
  const lines = content.split('\n');

  let inCallout = false;
  let buffer = [];
  let hasDato = false;

  for (let line of lines) {
    const trimmed = line.trim();

    // Detectar inicio de callout: [!tipo] (sin ">")
    if (/^\s*\[!\w+/.test(trimmed)) {
      // Si había un callout anterior, mostrarlo si contiene #dato
      if (inCallout && hasDato && buffer.length > 0) {
        dv.paragraph(buffer.join("\n"));
      }

      // Iniciar nuevo callout
      inCallout = true;
      hasDato = trimmed.includes("#dato");
      buffer = [line];
    }
    // Dentro de un callout
    else if (inCallout) {
      // Verificar si esta línea contiene #dato (por si aparece dentro del cuerpo)
      if (!hasDato && trimmed.includes("#dato")) {
        hasDato = true;
      }

      // ¿Sigue siendo parte del callout?
      // En Obsidian, el contenido de un callout NO lleva ">"; sigue con indentación o líneas normales.
      // Pero técnicamente, cualquier línea que **no sea otro encabezado, lista, o bloque nuevo**
      // y que venga justo después del callout, se considera parte de él hasta que aparece contenido "plano".
      // Sin embargo, Obsidian renderiza TODO lo que sigue (sin indentación especial) como contenido del callout,
      // **hasta que aparece una línea que no pertenece al bloque** (como otro título, lista, etc.).

      // Para simplificar y ser práctico: asumimos que el callout termina cuando:
      // - Aparece una línea vacía **seguida de contenido no indentado**, o
      // - Aparece una línea que claramente es nuevo bloque (como [!, #, -, *, 1., etc.)
      const isNewBlock = /^\s*([#\-*]|\d+\.|\[!)/.test(trimmed) || trimmed === "";

      if (!isNewBlock) {
        buffer.push(line);
      } else {
        // Fin del callout actual
        if (hasDato && buffer.length > 0) {
          dv.paragraph(buffer.join("\n"));
        }
        inCallout = false;
        buffer = [];
        hasDato = false;

        // Esta línea podría ser el inicio de un nuevo callout → no la descartamos
        if (/^\s*\[!\w+/.test(trimmed)) {
          inCallout = true;
          hasDato = trimmed.includes("#dato");
          buffer = [line];
        }
      }
    }
  }

  // Al final del archivo
  if (inCallout && hasDato && buffer.length > 0) {
    dv.paragraph(buffer.join("\n"));
  }
}
```
```dataviewjs
// DVJS: Mostrar SOLO callouts que usan "> [!..." y contienen la etiqueta #dato

const files = dv.pages('"Registro/Diario"');

for (let file of files) {
  const content = await dv.io.load(file.file.path);
  const lines = content.split('\n');

  let inCallout = false;
  let buffer = [];
  let hasDato = false;

  for (let line of lines) {
    const trimmed = line.trim();

    // Detectar inicio de callout: "> [!tipo]..."
    if (/^\s*>+\s*\[!\w+/.test(trimmed)) {
      // Si estábamos en un callout anterior y tenía #dato, mostrarlo
      if (inCallout && hasDato && buffer.length > 0) {
        dv.paragraph(buffer.join("\n"));
      }

      // Iniciar nuevo callout
      inCallout = true;
      hasDato = trimmed.includes("#dato");
      buffer = [line];
    }
    // Seguimos dentro de un callout
    else if (inCallout) {
      // Verificar si esta línea contiene #dato (por si aparece en el cuerpo)
      if (!hasDato && trimmed.includes("#dato")) {
        hasDato = true;
      }

      // ¿La línea pertenece al callout?
      // En la sintaxis de citas con `>`, cada línea del callout debe comenzar con `>`
      if (/^\s*>/.test(line)) {
        buffer.push(line);
      } else {
        // Fin del callout: esta línea no empieza con `>`
        if (hasDato && buffer.length > 0) {
          dv.paragraph(buffer.join("\n"));
        }
        inCallout = false;
        buffer = [];
        hasDato = false;

        // Esta línea podría ser el inicio de otro callout → se evaluará en la próxima iteración
      }
    }
  }

  // Al final del archivo, si quedó un callout con #dato
  if (inCallout && hasDato && buffer.length > 0) {
    dv.paragraph(buffer.join("\n"));
  }
}
```

---

### Buscar todos los callouts con la etiqueta #dato y se encuentren dentro del encabezado ### [[Rocabado]]

```dataviewjs
// DVJS: Mostrar SOLO callouts con "> [!..." que contengan "#dato" y estén dentro de "### [[Rocabado]]"

const files = dv.pages('"Registro/Diario"');

for (let file of files) {
  const content = await dv.io.load(file.file.path);
  const lines = content.split('\n');

  let inRocabadoSection = false;
  let inCallout = false;
  let buffer = [];
  let hasDato = false;

  for (let line of lines) {
    const trimmed = line.trim();

    // Detectar el encabezado EXACTO
    if (trimmed === "### [[Rocabado]]") {
      inRocabadoSection = true;
      continue;
    }

    // Salir de la sección si encontramos otro encabezado de nivel 1–3
    if (inRocabadoSection && /^#{1,3}\s/.test(trimmed)) {
      inRocabadoSection = false;
      inCallout = false;
      buffer = [];
      hasDato = false;
      continue;
    }

    // Solo procesar dentro de la sección
    if (!inRocabadoSection) continue;

    // Detectar INICIO de callout con "> [!..."
    if (/^\s*>+\s*\[!\w+/.test(trimmed)) {
      // Mostrar callout anterior si tenía #dato
      if (inCallout && hasDato && buffer.length > 0) {
        dv.paragraph(buffer.join("\n"));
      }

      // Iniciar nuevo callout
      inCallout = true;
      hasDato = trimmed.includes("#dato");
      buffer = [line];
    }
    // Dentro de un callout: seguir recolectando líneas que empiecen con ">"
    else if (inCallout) {
      // Verificar si esta línea tiene #dato (en el cuerpo)
      if (!hasDato && trimmed.includes("#dato")) {
        hasDato = true;
      }

      // ¿Sigue siendo parte del mismo callout?
      if (/^\s*>/.test(line)) {
        buffer.push(line);
      } else {
        // Fin del callout
        if (hasDato && buffer.length > 0) {
          dv.paragraph(buffer.join("\n"));
        }
        inCallout = false;
        buffer = [];
        hasDato = false;

        // Esta línea podría ser un nuevo callout (se evaluará en la próxima iteración)
      }
    }
  }

  // Al final del archivo: mostrar último callout si aplica
  if (inCallout && hasDato && buffer.length > 0) {
    dv.paragraph(buffer.join("\n"));
  }
}
```

