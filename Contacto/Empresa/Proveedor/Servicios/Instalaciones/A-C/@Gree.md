---
Nombre: Gree
Tipo: Proveedor
Contacto:
tags:
dg-publish: true
---

### [[Zapata]]

```dataview
task
from "Registro/Diario"   
WHERE contains(file.tags, "#Obra/Zapata") AND contains(text, "[[@Gree]]") and !completed
group by file.link
sort file.name ASC
```
