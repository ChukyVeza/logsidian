---
Nombre: Carolina Tarabillo Weise
Oficio: Arquitect@
Contacto: https://wa.me//59176092029
tags:
  - Arquitectura
dg-publish: true
---

### [[Rocabado]]

```dataview
task
from "Registro/Diario"   
WHERE meta(section).subpath = "Rocabado" AND !completed
WHERE contains(text, "[[@Carolina_Tarabillo]]")
GROUP BY file.link
sort file.name ASC
```
