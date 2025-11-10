---
Nombre: VidrAlum
Oficio: Empresa
Contacto:
tags:
  - Construcción/Carpintería/Aluminio
dg-publish: true
---
### #Obra/Zapata 
```dataview
task
from "Registro/Diario"   
WHERE contains(file.tags, "#Obra/Zapata") AND contains(text, "[[@VidrAlum]]") and !completed
group by file.link
sort file.name ASC
```
