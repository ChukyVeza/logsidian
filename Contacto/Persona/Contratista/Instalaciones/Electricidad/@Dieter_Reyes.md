---
Nombre: Dieter Reyes Antelo
Oficio: Contratista
Contacto: https://wa.me//59173186861
tags:
  - Construcción/Instalación/Electricidad
dg-publish: true
---
### #Obra/Zapata 
```dataview
task
from "Registro/Diario"   
WHERE contains(file.tags, "#Obra/Zapata") AND contains(text, "[[@Dieter_Reyes]]") and !completed
group by file.link
sort file.name ASC
```
