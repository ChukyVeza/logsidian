---
Nombre: Freddy Rodas
Tipo: Albañil
Contacto: https://wa.me//59168813743
tags:
  - Construcción/Albañilería
---

### [[Rocabado]]


```dataview
task
from "Registro/Diario"   
WHERE meta(section).subpath = "Rocabado" AND !completed
WHERE contains(text, "[[@Freddy_Rodas]]")
GROUP BY file.link
sort file.name ASC
```
