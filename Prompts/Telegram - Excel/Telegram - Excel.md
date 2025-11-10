---
tags:
  - IAs
  - Prompts
---
## 1
Los datos adjuntos corresponden a un registro diario de gastos personales y de obras.
Generar una tabla con las siguientes columnas.
- **Fecha**
- **Hora**
- **Monto** (positivo para ingresos, negativo para egresos y "±" para transferencias)
- **Etiquetas (#)** (cada etiqueta en una columna separada)
- **Personas mencionadas (@)** (cada persona en una columna separada)
- **Descripción**
Evaluar y completar con cuantas columnas sean necesarios, no hay límites para la cantidad de columnas
La moneda es el Boliviano (Bs.)
Ordenar la tabla de manera que las columnas contengan un solo tipo de registro.
Exportar la tabla a Excel.

## 2
Realiza una tabla con TODOS los datos del siguiente registro de gastos diarios y ordénalo de la siguiente manera
Considerar cada día del registro como una fila de la tabla e incluye las siguientes columnas
- **Fecha** 
- **Hora** 
- **Monto** (positivo para ingresos, negativo para egresos y "±" para transferencias) 
- **Etiqueta1 (#), Etiqueta2 (#), Etiqueta3 (#)** separar y ordenar las etiquetas
- **Personas (@)** 
- **Descripción** 
Exporta la tabla de excel

## 3
Corregido por Chat GPT:
Genera una tabla en Excel utilizando los datos de un registro de gastos diarios. La tabla debe cumplir con las siguientes especificaciones:

1. Cada día del registro debe ser una fila en la tabla.
    
2. Las columnas deben organizarse en el siguiente orden:
    
    - **Fecha**: Fecha del registro.
    - **Hora**: Hora del registro.
    - **Monto**: Ingresos deben tener valores positivos, egresos valores negativos, y transferencias deben representarse con "±".
    - **Etiqueta1 (#), Etiqueta2 (#), Etiqueta3 (#)**: Las etiquetas deben separarse y asignarse a las columnas correspondientes, ordenadas alfabéticamente si es necesario.
    - **Personas (@)**: Personas involucradas, listadas con el símbolo `@`.
    - **Descripción**: Texto descriptivo del gasto o ingreso.
3. Exporta el resultado como un archivo de Excel.
---
## 4 Funciona 100%
```
**Genera una tabla a partir de los datos del archivo utilizando las siguientes columnas y criterios:**

1. **ID:** Identificador único de cada mensaje.
2. **Fecha:** Fecha del mensaje en formato ISO 8601.
3. **Hora:** Hora del mensaje en formato ISO 8601.
4. **Monto:** Valores numéricos con el siguiente formato "{:,.2f} Bs.".format(numero)
5. **Justificación:** Mensajes que contengan alguno de estos hashtags que coincida 100% con:
    - #Anticipo
    - #Ingreso
    - #Egreso
    - #Transferencia
    - #Préstamo
    - #Devolución
    - #Fondo_Propio
    - #Gasto
    - #Rendir
    - #Presupuesto
    - #Transferencia
6. **Categoría:** Mensajes que contengan alguno de estos hashtags que coincida 100% con:
    - #Obra
    - #Personal
    - #Varios
7. **Personal:** Mensajes que contengan alguno de estos hashtags que coincida 100% con:
    - #CPVA
    - #HVV
    - #JMVD
    - #JSVC
    - #MVV
8. **Obra:** Mensajes que contengan alguno de estos hashtags que coincida 100% con:
    - #Mantenimiento
    - #Gonzalez
    - #Vasquez
    - #Ichilo
    - #Casa_3PF
    - #Montero-Kiosko
9. **Tipo1:** Mensajes que contengan alguno de estos hashtags que coincida 100% con:
    - #Alimentación
    - #Educación
    - #Electrónica
    - #Entretenimiento
    - #Financiero
    - #Herramienta
    - #Higiene
    - #Impuesto
    - #Ocio
    - #Otro_Ingreso
    - #Regalo
    - #Salud
    - #Servicio_Básico
    - #Sueldo
    - #Suscripción
    - #Transporte
    - #Varios
    - #Vestimenta
    - #Viaje
    - #Vivienda
10. **Tipo2:** Mensajes que contengan alguno de estos hashtags que coincida 100% con:
    - #Administración
    - #Anticipo
    - #Gasto
    - #GG
    - #Herramienta
    - #Material
    - #MO
    - #Planilla_Avance
    - #Presupuesto
    - #Servicio
    - #Transporte
11. **Proveedores:** Mensajes que contengan alguno de estos hashtags que coincida 100% con:
    - #Hipermaxi 
    - #Farmacorp 
    - #Farmacia_Chavez 
    - #Roho 
    - #Ferreteros 
    - #Paradiso_Montesori
12. **Personas:** Mensajes que incluyan menciones con arroba (@) o los siguientes términos que coincida 100% con:
    - Varios
    - @Carlos_Durán
    - @Javier_Sánchez
    - @JC_Herrera
    - #Herramienta
    - #Transferencia
13. **Descripción:** Cualquier otro contenido que no corresponda a las categorías anteriores.
 ```
---
## 5
```
Extrae información de un archivo de texto y genera una tabla en formato Markdown con las siguientes columnas, aplicando los criterios de extracción que se detallan:

*   **ID:** Identificador único de cada mensaje. (Asume que el archivo provee un ID único para cada entrada. Si no lo hace, indica cómo se debe generar o si se debe omitir la columna).
*   **Fecha:** Fecha del mensaje en formato ISO 8601 (AAAA-MM-DD).
*   **Hora:** Hora del mensaje en formato ISO 8601 (HH:MM:SS).
*   **Monto:** Valores numéricos formateados a dos decimales, incluyendo el símbolo "Bs." (Ejemplo: "1,234.56 Bs."). Extrae solo el valor numérico y formatea la salida.
*   **Justificación:** Extrae el primer hashtag encontrado que coincida con la siguiente lista: #Anticipo, #Ingreso, #Egreso, #Transferencia, #Préstamo, #Devolución, #Fondo_Propio, #Gasto, #Rendir, #Presupuesto. Si no se encuentra ninguno, dejar la celda vacía.
*   **Categoría:** Extrae el primer hashtag encontrado que coincida con: #Obra, #Personal, #Varios. Si no se encuentra ninguno, dejar la celda vacía.
*   **Personal:** Extrae el primer hashtag encontrado que coincida con: #CPVA, #HVV, #JMVD, #JSVC, #MVV. Si no se encuentra ninguno, dejar la celda vacía.
*   **Obra (Nombre de la Obra):** Extrae el primer hashtag encontrado que coincida con: #Mantenimiento, #Gonzalez, #Vasquez, #Ichilo, #Casa_3PF, #Montero-Kiosko. Si no se encuentra ninguno, dejar la celda vacía.
*   **Tipo1:** Extrae el primer hashtag encontrado que coincida con: #Alimentación, #Educación, #Electrónica, #Entretenimiento, #Financiero, #Herramienta, #Higiene, #Impuesto, #Ocio, #Otro_Ingreso, #Regalo, #Salud, #Servicio_Básico, #Sueldo, #Suscripción, #Transporte, #Varios, #Vestimenta, #Viaje, #Vivienda. Si no se encuentra ninguno, dejar la celda vacía.
*   **Tipo2:** Extrae el primer hashtag encontrado que coincida con: #Administración, #Anticipo, #Gasto, #GG, #Herramienta, #Material, #MO, #Planilla_Avance, #Presupuesto, #Servicio, #Transporte. Si no se encuentra ninguno, dejar la celda vacía.
*   **Proveedores:** Extrae el primer hashtag encontrado que coincida con: #Hipermaxi, #Farmacorp, #Farmacia_Chavez, #Roho, #Ferreteros, #Paradiso_Montesori. Si no se encuentra ninguno, dejar la celda vacía.
*   **Personas:** Extrae la primera mención con arroba (@) encontrada. Si no hay menciones con arroba, busca y extrae el primer término que coincida con: Varios, @Carlos_Durán, @Javier_Sánchez, @JC_Herrera. Si no se encuentra ninguno, dejar la celda vacía.
*   **Descripción:** Extrae cualquier otro contenido del mensaje que no haya sido capturado por las columnas anteriores.

**Consideraciones Adicionales:**

*   **Formato del Archivo:** Especifica el formato del archivo de entrada (ej. texto plano, CSV, JSON). Esto es crucial. Ejemplo: "Extrae información de un archivo de texto plano..." o "Extrae información de un archivo en formato CSV..."
*   **Manejo de Hashtags Repetidos:** El prompt ahora especifica "el *primer* hashtag encontrado". Esto evita ambigüedades.
*   **Manejo de Datos Faltantes:** Se indica explícitamente qué hacer si no se encuentra un valor para una columna: "dejar la celda vacía".
*   **Formato de Salida:** Se especifica claramente que la tabla debe estar en formato Markdown, lo cual es un formato bien soportado y fácil de interpretar.
*   **Claridad en las Instrucciones:** Se han reformulado varias frases para mayor claridad y precisión. Por ejemplo, se especifica el formato exacto de fecha y hora ISO 8601.
*   **Ejemplo de Formato de Monto:** Se proporciona un ejemplo claro del formato de monto esperado.
*   **Priorización en "Personas":** Se aclara la prioridad: primero buscar menciones con "@", luego los otros términos.

**Ejemplo de cómo se vería la tabla en Markdown:**

|ID|Fecha|Hora|Monto|Justificación|Categoría|...|Descripción|
|---|---|---|---|---|---|---|---|
|1|2024-10-27|10:00:00|1,500.00 Bs.|#Ingreso|#Personal|...|Pago de sueldo.|
|2|2024-10-27|11:30:00|500.00 Bs.|#Gasto|#Obra|...|Compra de materiales #Herramienta.|
```
---
## 6

"The Mother of Prompts"
```
Genera una tabla en formato Markdown con las siguientes columnas, aplicando los criterios de extracción que se detallan:

*   **ID:** Identificador único de cada mensaje. (Asume que el archivo provee un ID único para cada entrada. Si no lo hace, indica cómo se debe generar o si se debe omitir la columna).
*   **Fecha:** Fecha del mensaje en formato ISO 8601 (AAAA-MM-DD).
*   **Hora:** Hora del mensaje en formato ISO 8601 (HH:MM:SS).
*   **Monto:** Valores numéricos formateados a dos decimales, incluyendo el símbolo "Bs." (Ejemplo: "1,234.56 Bs."). Extrae solo el valor numérico y formatea la salida.
*   **Justificación:** Extrae el primer hashtag encontrado que coincida 100% con la siguiente lista: #Anticipo, #Ingreso, #Egreso, #Transferencia, #Préstamo, #Devolución, #Fondo_Propio, #Gasto, #Rendir, #Presupuesto. Si no se encuentra ninguno, dejar la celda vacía.
*   **Categoría:** Extrae el primer hashtag encontrado que coincida 100% con la siguiente lista: #Obra, #Personal, #Varios. Si no se encuentra ninguno, dejar la celda vacía.
*   **Personal:** Extrae el primer hashtag encontrado que coincida 100% con la siguiente lista: #CPVA, #HVV, #JMVD, #JSVC, #MVV. Si no se encuentra ninguno, dejar la celda vacía.
*   **Obra (Nombre de la Obra):** Extrae el primer hashtag encontrado que coincida 100% con la siguiente lista: #Mantenimiento, #Gonzalez, #Vasquez, #Ichilo, #Casa_3PF, #Montero-Kiosko. Si no se encuentra ninguno, dejar la celda vacía.
*   **Tipo1:** Extrae el primer hashtag encontrado que coincida 100% con la siguiente lista: #Alimentación, #Educación, #Electrónica, #Entretenimiento, #Financiero, #Herramienta, #Higiene, #Impuesto, #Ocio, #Otro_Ingreso, #Regalo, #Salud, #Servicio_Básico, #Sueldo, #Suscripción, #Transporte, #Varios, #Vestimenta, #Viaje, #Vivienda. Si no se encuentra ninguno, dejar la celda vacía.
*   **Tipo2:** Extrae el primer hashtag encontrado que coincida 100% con la siguiente lista: #Administración, #Anticipo, #Gasto, #GG, #Herramienta, #Material, #MO, #Planilla_Avance, #Presupuesto, #Servicio, #Transporte. Si no se encuentra ninguno, dejar la celda vacía.
*   **Proveedores:** Extrae el primer hashtag encontrado que coincida 100% con la siguiente lista: #Hipermaxi, #Farmacorp, #Farmacia_Chavez, #Roho, #Ferreteros, #Paradiso_Montesori. Si no se encuentra ninguno, dejar la celda vacía.
*   **Personas:** Extrae la primera mención con arroba (@) encontrada. Si no hay menciones con arroba, busca y extrae el primer término que coincida con: Varios, @Carlos_Durán, @Javier_Sánchez, @JC_Herrera. Si no se encuentra ninguno, dejar la celda vacía.
*   **Descripción:** Extrae cualquier otro contenido del mensaje que no haya sido capturado por las columnas anteriores.

**Consideraciones Adicionales:**

*   **Formato del Archivo:** Especifica el formato del archivo de entrada (ej. texto plano, CSV, JSON). Esto es crucial. Ejemplo: "Extrae información de un archivo de texto plano..." o "Extrae información de un archivo en formato CSV..."
*   **Manejo de Hashtags Repetidos:** El prompt ahora especifica "el *primer* hashtag encontrado". Esto evita ambigüedades.
*   **Manejo de Datos Faltantes:** Se indica explícitamente qué hacer si no se encuentra un valor para una columna: "dejar la celda vacía".
*   **Formato de Salida:** Se especifica claramente que la tabla debe estar en formato Markdown, lo cual es un formato bien soportado y fácil de interpretar.
*   **Claridad en las Instrucciones:** Se han reformulado varias frases para mayor claridad y precisión. Por ejemplo, se especifica el formato exacto de fecha y hora ISO 8601.
*   **Ejemplo de Formato de Monto:** Se proporciona un ejemplo claro del formato de monto esperado.
*   **Priorización en "Personas":** Se aclara la prioridad: primero buscar menciones con "@", luego los otros términos.

**Ejemplo de cómo se vería la tabla en Markdown:**

| ID  | Fecha      | Hora     | Monto        | Justificación | Categoría | ... | Descripción                        |
| --- | ---------- | -------- | ------------ | ------------- | --------- | --- | ---------------------------------- |
| 1   | 2024-10-27 | 10:00:00 | 1,500.00 Bs. | #Ingreso      | #Personal | ... | Pago de sueldo.                    |
| 2   | 2024-10-27 | 11:30:00 | 500.00 Bs.   | #Gasto        | #Obra     | ... | Compra de materiales #Herramienta. |
Exporta a tabla de excel
```
