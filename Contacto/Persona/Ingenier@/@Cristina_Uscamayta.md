---
Nombre: Reaquelin Cristina Uscamayta Ajhuacho
Oficio: Ingenier@
Contacto: https://wa.me//59170256491
tags:
  - Estudio/Suelos
dg-publish: false
---


```dataview
tasks
from "Registro/Diario/2025"   
WHERE contains(file.tags, "#Obras/Zapata") AND contains(text, "@Adilson_Pérez") and !completed
group by file.link
sort file.name ASC
```