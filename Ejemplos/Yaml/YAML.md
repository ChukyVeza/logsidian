```Yaml

#Comment: This is a supermarket list using YAML
#Note that - character represents the list
---
food: 
  - vegetables: tomatoes #first list item
  - fruits: #second list item
      citrics: oranges 
      tropical: bananas
      nuts: peanuts
      sweets: raisins
```

```Yaml
# Comentario: Este es un ejemplo de archivo YAML con diversas sintaxis

# 1. Escalares simples (strings, números, booleanos)
nombre: "Juan Pérez"          # String entre comillas dobles
edad: 30                      # Número entero
es_estudiante: false          # Booleano

# 2. Listas (secuencias)
lenguajes_programacion:
  - Python                    # Elemento de la lista
  - JavaScript
  - "C++"

# 3. Diccionarios (mapeos)
direccion:
  calle: "Av. Siempre Viva"
  numero: 123
  ciudad: "Springfield"

# 4. Strings sin comillas
descripcion: Esto es una descripción sin comillas

# 5. Strings multilínea (bloques literales y plegados)
biografia: |
  Juan Pérez es un desarrollador de software.
  Le encanta aprender nuevas tecnologías.
  Este texto respeta los saltos de línea.

notas_adicionales: >
  Esta es una nota adicional.
  Los saltos de línea se convierten en espacios.

# 6. Anclajes y alias
empleo_principal: &principal
  puesto: "Desarrollador Senior"
  empresa: "TechCorp"

empleo_secundario:
  <<: *principal             # Alias que reutiliza el contenido del ancla
  proyecto: "Innovación"

# 7. Tipos de datos específicos
fecha_nacimiento: !!timestamp 1993-05-15T00:00:00Z
colores_favoritos: !!set
  azul: null
  verde: null

# 8. Documentos múltiples
---
documento_extra:
  clave: valor
...
```
