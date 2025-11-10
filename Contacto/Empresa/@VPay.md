---
Nombre: VPay
Tipo: Empresa
Contacto:
tags:
dg-publish: false
---



```dataview
task
from "Registro/Diario"   
WHERE contains(text, "[[@VPay]]") and !completed
group by file.link
sort file.name ASC
```
