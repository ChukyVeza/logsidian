---
Nombre: Sin Asignar
Oficio:
Contacto:
tags:
dg-publish: true
---
### #Obra/Zapata 
```dataview
task
from "Registro/Diario"   
WHERE contains(file.tags, "#Obra/Zapata") AND contains(text, "[[@Sin_Asignar]]") and !completed
group by file.link
sort file.name ASC
```
