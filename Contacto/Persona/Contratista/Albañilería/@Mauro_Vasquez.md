---
Nombre: Mauro Vasquez
Tipo: Contratista
Contacto: https://wa.me//59175363918
tags:
  - Construcción/Albañilería
---

### [[Rocabado]]

```dataview
task
from "Registro/Diario"   
WHERE meta(section).subpath = "Rocabado" AND !completed
WHERE contains(text, "[[@Mauro_Vasquez]]")
GROUP BY file.link
sort file.name ASC
```
