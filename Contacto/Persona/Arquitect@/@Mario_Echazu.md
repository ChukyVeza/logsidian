---
Nombre: Mario Echazu
Oficio: Arquitect@
Contacto: https://wa.me//59176006090
tags:
  - Arquitectura
---

### [[Zapata]] 

```dataview
task
from "Registro/Diario"   
WHERE meta(section).subpath = "Zapata" AND !completed
WHERE contains(text, "[[@Mario_Echazu]]")
GROUP BY file.link
sort file.name ASC
```
