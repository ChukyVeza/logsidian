---
Nombre: Yamil Flores García
Oficio: Ingenier@
Contacto: https://wa.me//59169125644
tags:
  - Ingeniería/Hidrosanitaria
dg-publish: false
---
Correo: floresgarciayamil@gmail.com


```dataview
tasks
from "Registro/Diario/2025"   
WHERE contains(file.tags, "#Obras/Zapata") AND contains(text, "@Adilson_Pérez") and !completed
group by file.link
sort file.name ASC
```