---
Nombre: Salma Diana Saavedra Antelo
Oficio: Arquitect@
Contacto: https://wa.me//59170034758
tags:
  - Arquitectura
dg-publish: true
---

### [[Zapata]] 

```dataview
task
from "Registro/Diario"   
WHERE meta(section).subpath = "Zapata" AND !completed
WHERE contains(text, "[[@Salma_Saavedra]]")
GROUP BY file.link
sort file.name ASC
```
