---
Nombre:
dg-publish: true
---
### #Obra/Zapata 
```dataview
task
from "Registro/Diario"   
WHERE contains(file.tags, "#Obra/Zapata") AND contains(text, "[[@Javier_Gallardo]]") and !completed
group by file.link
sort file.name ASC
```
