---
Nombre: Erwin Aspiazu
Oficio: Ingenier@
Contacto: https://wa.me//59171651777
tags:
  - Proveedor/Climatización
dg-publish: true
aliases: []
---
### #Obra/Zapata 
```dataview
task
from "Registro/Diario"   
WHERE contains(file.tags, "#Obra/Zapata") AND contains(text, "[[@Erwin_Aspiazu]]") and !completed
group by file.link
sort file.name ASC
```
