---
Nombre: Modesto Flores
Oficio: Contratista
Contacto: https://wa.me//59177309947
tags:
  - Construcción/Estructura/Metálica
---
### #Obra

```dataview
task
from "Registro/Diario"   
WHERE contains(file.tags, "#Obras/Zapata") AND contains(text, "@Modesto_Flores") and !completed
group by file.link
sort file.name ASC
```
