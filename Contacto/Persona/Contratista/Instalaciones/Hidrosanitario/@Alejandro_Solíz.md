---
Nombre: Alejandro Solíz Mendieta
Oficio: Contratista
Contacto: https://wa.me//59179897649
tags:
  - Construcción/Instalación/Hidrosanitario
dg-publish: true
---
### #Obra/Zapata 
```dataview
task
from "Registro/Diario"   
WHERE contains(file.tags, "#Obra/Zapata") AND contains(text, "[[@Alejandro_Solíz]]") and !completed
group by file.link
sort file.name ASC
```
