>[!failure]- SIN COMPLETAR

```dataview
task
from "Registro/Diario"   
sort file.name
where contains(tags, "#Obras/Zapata") and !completed
group by file.link 
```


>[!success]- COMPLETADOS


```dataview
task
from "Registro/Diario"   
sort file.name
where contains(tags, "#Obras/Zapata") and completed
group by file.link 
```

>[!failure]- Ejemplo
>```
tasks
group by file.link
from "Registro/Diario"   
sort file.name
where contains(tags, "#Obras/Zapata") and !completed

```dataview
TABLE WITHOUT ID 
  join(rows.file.link, " | ") as "Notas",
  join(rows.text, " | ") as "Tareas"
FROM "Registro/Diario"
FLATTEN file.tasks AS T
WHERE contains(T.tags, "#Obra/Rocabado") 
  AND !T.completed
  AND regexmatch("\[\[@[^\]]+\]\]", T.text)
FLATTEN regexextract(T.text, "\[\[(@[^\]]+)\]\]") AS asignado
GROUP BY asignado
SORT asignado ASC
```