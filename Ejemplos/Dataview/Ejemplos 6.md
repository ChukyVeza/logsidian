### ✅ Tareas x día (100% funcional)
dataview
task
from "Registro/Diario" and #Semana-25/41  
where !completed
sort file.name
group by file.link 


---
dataview
//Definimos que es una búsqueda por tareas
task
//De las notas en la carpeta diario
FROM "Registro/Diario"
where contains(file.name, "2025-04") and !completed
sort file.name
group by file.link 
