---
Nombre: Karla Sofía Zapata
dg-publish: true
---

### [[Zapata]]

```dataview
task
from "Registro/Diario"   
WHERE contains(file.tags, "#Obra/Zapata") AND contains(text, "[[@Sofía_Zapata]]") and !completed
group by file.link
sort file.name ASC
```
