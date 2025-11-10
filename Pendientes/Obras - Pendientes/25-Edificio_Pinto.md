# Tareas

>[!abstract]- Por Contratistas

```
//Funcional 100%//
task 
//Buscar las tareas en las notas diarias//
from "Registro/Diario" 
//Que tengan la etiqueta #Obra/Pinto y estén sin completar//
WHERE contains(file.tags,"#Obra/Pinto") and !completed and outlinks
//Todos los enlaces volver texto//
flatten outlinks as link
//Los enlaces que contengan "@" Y NO sea el enlace "@Vpay"// 
WHERE contains(text, "@") AND !contains(["[[@Alejandro_Solíz]]",
"[[@Alsevic]]", "[[@Dieter_Reyes]]", "[[@LeanCruz]]", "[[@Mario_Echazu]]", 
"[[@Mi_Piscina]]", "[[@Ruben_Serrano]]", "[[@Salma_Saavedra]]", 
"[[@Sin_Asignar]]", "[[@Sofía_Zapata]]", "[[@VPay]]", "[[@Abigail_Roca]]"
 ], link.file.name)
//Agrupar por enlace//
group by link.file.name
//Ordenar por nombre// 
sort link.file.name asc
```

>[!failure]- Por Fechas

```dataview
task
from "Registro/Diario"   
sort file.name
where contains(tags, "#Obra/Pinto") and !completed
group by file.link 
```

