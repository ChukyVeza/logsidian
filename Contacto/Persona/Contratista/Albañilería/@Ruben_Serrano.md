---
Nombre: Rubén Serrano
Tipo: Contratista
Contacto: https://wa.me//59178060812
tags:
  - Construcción/Estructura/Hormigón
dg-publish: true
---
### #Obra/Zapata 
```dataview
task
from "Registro/Diario"   
WHERE contains(file.tags, "#Obra/Zapata") AND contains(text, "[[@Ruben_Serrano]]") and !completed
group by file.link
sort file.name ASC
```
