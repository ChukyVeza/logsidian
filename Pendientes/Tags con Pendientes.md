
## Tags con Pendientes sin (#Semana)

```dataview
LIST WITHOUT ID tag
FROM "Registro/Diario/2025"
FLATTEN file.tags AS tag
WHERE length(file.tasks) > 0
AND any(file.tasks, (t) => !t.completed)
AND !contains(lower(tag), lower("semana"))
GROUP BY tag
SORT tag ASC
```

## Tags con pendientes (Completo)

```dataview
LIST WITHOUT ID tag
FROM "Registro/Diario/2025"
FLATTEN file.tags AS tag
WHERE length(file.tasks) > 0
AND any(file.tasks, (t) => !t.completed)
AND !contains(lower(tag), lower("semana"))
GROUP BY tag
SORT tag ASC
```
