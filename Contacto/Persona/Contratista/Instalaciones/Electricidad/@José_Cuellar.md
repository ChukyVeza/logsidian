---
Nombre: José Cuellar
Oficio: Contratista
Contacto: https://wa.me//59176005423
tags:
  - Construcción/Instalación/Electricidad
---
### #Obra 

```dataview
task
from "Registro/Diario"   
WHERE contains(file.tags, "#Obras/Zapata") AND contains(text, "@José_Cuellar") and !completed
group by file.link
sort file.name ASC
```
