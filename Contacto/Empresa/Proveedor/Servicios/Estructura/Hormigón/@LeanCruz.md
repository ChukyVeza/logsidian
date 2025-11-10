---
Nombre: LeanCruz
Tipo: Empresa
Contacto: "[[@Adilson_Pérez]]"
tags:
  - Proveedor/Servicio/Hormigón
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
