---
Nombre: Erika Martinez Requez
Oficio: Contratista
Contacto: https://wa.me//59175533968
tags:
  - Construcción/Instalación/Gas
dg-publish: true
---
### #Obra/Zapata 
```dataview
task
from "Registro/Diario"   
WHERE contains(file.tags, "#Obra/Zapata") AND contains(text, "[[@Erika_Martinez]]") and !completed
group by file.link
sort file.name ASC
```
