---
Nombre: Melissa Barba
Oficio: Ingenier@
Contacto: https://wa.me//59177800650
tags: []
dg-publish: true
---

```dataview
task
from "Registro/Diario/2025"   
WHERE contains(file.tags, "#Obras/Zapata") AND contains(text, "@Melissa_Barba") and !completed
group by file.link
sort file.name ASC
```