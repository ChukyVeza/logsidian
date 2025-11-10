---
Nombre: Gary
Oficio: Contratista
Contacto: https://wa.me//59176035837
tags:
  - Construcción/Albañilería/Revestimiento
dg-publish: true
---
### #Obra/Zapata 
```dataview
task
from "Registro/Diario"   
WHERE contains(file.tags, "#Obra/Zapata") AND contains(text, "[[@Gary]]") and !completed
group by file.link
sort file.name ASC
```
