---
Nombre: Quilosa
Tipo: Empresa
Contacto: "[[@Juan_Pablo_Carreras]]"
tags:
---
### #Obra/Zapata 
```dataview
task
from "Registro/Diario"   
WHERE contains(file.tags, "#Obra/Zapata") AND contains(text, "[[@Quilosa]]") and !completed
group by file.link
sort file.name ASC
```
