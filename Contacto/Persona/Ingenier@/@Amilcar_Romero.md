---
Nombre: Amilcar Fabian Romero Baldivieso
Oficio: Ingenier@
Contacto: https://wa.me//59179503113
tags:
  - Ingeniería/Eléctrica
dg-publish: false
---
Correo: fertapiaserv@gmail.com


```dataview
tasks
from "Registro/Diario/2025"   
WHERE contains(file.tags, "#Obras/Zapata") AND contains(text, "@Adilson_Pérez") and !completed
group by file.link
sort file.name ASC
```