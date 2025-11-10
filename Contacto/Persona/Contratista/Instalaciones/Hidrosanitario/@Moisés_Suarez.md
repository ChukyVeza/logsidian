---
Nombre: Moisés Suarez
Oficio: Contratista
Contacto: https://wa.me//59179851767
tags:
  - Construcción/Instalación/Hidrosanitario
dg-publish: false
---
### #Obra 

```dataview
task
from "Registro/Diario"   
WHERE contains(file.tags, "#Obras/Zapata") AND contains(text, "@Moisés_Suarez") and !completed
group by file.link
sort file.name ASC
```
