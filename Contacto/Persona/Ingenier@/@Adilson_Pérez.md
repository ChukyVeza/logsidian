---
Nombre: Adilson Pérez
Oficio: Ingenier@
Contacto: https://wa.me//59173186861
tags:
  - Construcción/Estructura/Hormigón
dg-publish: true
---
### #Obra/Zapata 
```dataview
task
from "Registro/Diario"   
WHERE contains(file.tags, "#Obra/Zapata") AND contains(text, "[[@LeanCruz]]") and !completed
group by file.link
sort file.name ASC
```
