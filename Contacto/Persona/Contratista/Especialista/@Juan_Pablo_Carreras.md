---
Nombre: Juan Pablo Carreras
Tipo: Contratista
Contacto: https://wa.me//59170351807
tags:
dg-publish: true
---
### #Obra/Zapata 
```dataview
task
from "Registro/Diario"   
WHERE contains(file.tags, "#Obra/Zapata") AND contains(text, "[[@Quilosa]]") and !completed
group by file.link
sort file.name ASC
```
