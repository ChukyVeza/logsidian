---
Nombre: Assinco
Tipo: Proveedor
Contacto:
tags:
dg-publish: true
---

### [[Zapata]]

```dataview
task
from "Registro/Diario"   
WHERE contains(file.tags, "#Obra/Zapata") AND contains(text, "[[@Assinco]]") and !completed
group by file.link
sort file.name ASC
```
