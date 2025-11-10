---
Nombre: Marcelo Luis Sanchez Pinto
Oficio: Ingenier@
Contacto: https://wa.me//59175345096
tags:
  - Ingeniería/Estructural
dg-publish: false
---


```dataview
tasks
from "Registro/Diario/2025"   
WHERE contains(file.tags, "#Obras/Zapata") AND contains(text, "@Adilson_Pérez") and !completed
group by file.link
sort file.name ASC
```