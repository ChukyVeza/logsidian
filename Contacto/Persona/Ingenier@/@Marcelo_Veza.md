---
Nombre: Juan Marcelo Veza Dorado
Oficio: Ingenier@
Contacto: https://wa.me//59172141920
tags: []
dg-publish: true
---

### **[[Zapata]]** 

```dataview
task
from "Registro/Diario"   
WHERE meta(section).subpath = "Rocabado" AND !completed
WHERE contains(text, "[[@Marcelo_Veza]]")
GROUP BY file.link
sort file.name ASC
```

---
### **[[Rocabado]]** 

```dataview
task
from "Registro/Diario"   
WHERE meta(section).subpath = "Rocabado" AND !completed
WHERE contains(text, "[[@Marcelo_Veza]]")
GROUP BY file.link
sort file.name ASC
```
