---
Creado:
  - "{{date}} {{time}}"
tags:
  - "#Log/MonthLylog"
---

# 📆 Mes  {mm/yyyy}

### ✅ Servicios
- [ ] Teléfono #MV
- [ ] Internet #MV
- [ ] Salud #MV/HVV  
- [ ] Salud #MV/MVV 
- [ ] Agua #MV
- [ ] Luz #MV

### ✅ Tareas
```dataview
//Definimos que es una búsqueda por tareas
task
//De las notas en la carpeta diario
FROM "Registro/Diario"
where contains(file.name, "yyyy-mm") and !completed
sort file.name
group by file.link 
```

