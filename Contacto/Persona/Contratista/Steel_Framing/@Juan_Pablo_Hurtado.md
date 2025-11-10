---
Nombre: Juan Pablo Hurtado
Oficio: Contratista
Contacto: https://wa.me//59176340920
tags:
  - Construcción/Steel_Framing/Cielo_Falso
dg-publish: true
---
### #Obra/Zapata 
```dataview
task
from "Registro/Diario"   
WHERE meta(section).subpath = "Zapata" AND !completed
WHERE contains(text, "[[@Juan_Pablo_Hurtado]]")
GROUP BY file.link
sort file.name ASC
```
