---
tags:
  - IAs
  - Prompts
---
## 1
*Qwen*
"Extrae y organiza la siguiente información de la imagen de la factura: 
Nombre del comercio 
Sucursal 
NIT (Número de Identificación Tributaria) 
Número de factura 
Fecha y hora de emisión 
Detalles de los productos comprados (cantidad, precio unitario, descripción, subtotal) 
Total a pagar 
Cambio 
Crédito fiscal 
Presenta los datos en un formato claro y estructurado."


## 2

*Perplexcity*
¡Por supuesto! Aquí tienes el prompt mejorado siguiendo el criterio que proporcionaste, con un formato claro y consistente tanto en la descripción como en el JSON esperado:

**Prompt mejorado:**

```Markdown
"Analiza la imagen de la factura proporcionada y extrae la información solicitada de manera precisa. Organiza los datos en un formato estructurado y claro, utilizando las etiquetas especificadas para cada campo.

1. Información del comercio:
    - Nombre: <Nombre del comercio>
    - Sucursal: <N° de sucursal>
    - NIT: <Número de identificación tributaria>
        
2. Datos de la factura:
    - N° Factura: <Número de Factura> 
    - Fecha: <día/mes/año>
    - Hora: <hora:minuto:segundo>
        
3. Detalles de los productos comprados:  
    Para cada producto listado, extrae:
    - Cant: <Cantidad>
    - Detalle: <Descripción del producto>
    - PU: <Precio Unitario>
    - Subtotal: <Subtotal>
        
4. Totales y otros valores relevantes:
    - Total: <Total a pagar (monto final)>
    - Cambio: <Cambio (si está especificado)>
    - Crédito fiscal: <Crédito fiscal (si aplica)>

Formato de salida esperado:  
Presenta los datos en formato claro y estructurado de tabla.
Consideraciones adicionales:
- Asegúrate de capturar todos los valores con precisión, incluso si están en diferentes posiciones dentro de la factura o tienen formatos variados.
- Si algún campo no está presente en la factura, indica explícitamente su ausencia con `null`
En la sección "Detalles de los productos" la tabla deberá tener el siguiente estructura:
| Cant | Detalle | PU | Subtotal |
```

---

Ahora transformado todo esto en código:

Para realizar la tarea de analizar una imagen de factura y extraer la información solicitada, necesitarás utilizar bibliotecas de procesamiento de imágenes y reconocimiento óptico de caracteres (OCR). Una opción popular para OCR en Python es `pytesseract`, que se basa en el motor Tesseract-OCR. Además, puedes usar `Pillow` para manejar las imágenes.

A continuación, te proporciono un ejemplo de cómo podrías implementar esto en Python:

### Paso 1: Instalar las dependencias

Primero, asegúrate de tener instaladas las siguientes bibliotecas:

```bash
pip install pytesseract pillow opencv-python pandas
```

Además, debes instalar Tesseract-OCR en tu sistema. Puedes descargarlo desde [aquí](https://github.com/tesseract-ocr/tesseract).

### Paso 2: Código en Python

```python
import cv2
import pytesseract
import pandas as pd
from PIL import Image

# Configuración de pytesseract
pytesseract.pytesseract.tesseract_cmd = r'C:\Program Files\Tesseract-OCR\tesseract.exe'  # Ajusta la ruta según tu instalación

def extract_text_from_image(image_path):
    # Leer la imagen
    image = cv2.imread(image_path)
    # Convertir a escala de grises
    gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
    # Aplicar umbralización
    _, thresh = cv2.threshold(gray, 0, 255, cv2.THRESH_BINARY_INV + cv2.THRESH_OTSU)
    # Usar pytesseract para extraer texto
    text = pytesseract.image_to_string(thresh, lang='spa')
    return text

def parse_invoice(text):
    data = {
        'Nombre': None,
        'Sucursal': None,
        'NIT': None,
        'N° Factura': None,
        'Fecha': None,
        'Hora': None,
        'Productos': [],
        'Total': None,
        'Cambio': None,
        'Crédito fiscal': None
    }

    lines = text.split('\n')
    for line in lines:
        if "ESTACION DE SERVICIO URBARI SRL" in line:
            data['Nombre'] = "ESTACION DE SERVICIO URBARI SRL"
        elif "No FACTURA:" in line:
            data['N° Factura'] = line.split(':')[-1].strip()
        elif "Fecha:" in line:
            data['Fecha'] = line.split(':')[-1].strip()
        elif "Hora:" in line:
            data['Hora'] = line.split(':')[-1].strip()
        elif "NIT/CI/CEX:" in line:
            data['NIT'] = line.split(':')[-1].strip()
        elif "TOTAL Bs." in line:
            data['Total'] = line.split(' ')[-1].strip()
        elif "Importe base para credito fiscal" in line:
            data['Crédito fiscal'] = line.split(' ')[-1].strip()

        # Detalles de los productos
        if "CANTIDAD DETALLE P.UNITARIO SUBTOTAL" in line:
            product_lines = lines[lines.index(line) + 1:]
            for product_line in product_lines:
                if "TOTAL Bs." in product_line:
                    break
                parts = product_line.split()
                if len(parts) == 4:
                    cant, detalle, pu, subtotal = parts
                    data['Productos'].append({
                        'Cant': cant,
                        'Detalle': detalle,
                        'PU': pu,
                        'Subtotal': subtotal
                    })

    return data

def format_output(data):
    print("1. Información del comercio:")
    print(f"    - Nombre: {data['Nombre']}")
    print(f"    - Sucursal: {data['Sucursal'] or 'null'}")
    print(f"    - NIT: {data['NIT']}")

    print("\n2. Datos de la factura:")
    print(f"    - N° Factura: {data['N° Factura']}")
    print(f"    - Fecha: {data['Fecha']}")
    print(f"    - Hora: {data['Hora']}")

    print("\n3. Detalles de los productos comprados:")
    print("| Cant | Detalle | PU | Subtotal |")
    for product in data['Productos']:
        print(f"| {product['Cant']} | {product['Detalle']} | {product['PU']} | {product['Subtotal']} |")

    print("\n4. Totales y otros valores relevantes:")
    print(f"    - Total: {data['Total']}")
    print(f"    - Cambio: {data['Cambio'] or 'null'}")
    print(f"    - Crédito fiscal: {data['Crédito fiscal']}")

if __name__ == "__main__":
    image_path = 'ruta/a/tu/factura.jpg'  # Reemplaza con la ruta de tu factura
    text = extract_text_from_image(image_path)
    data = parse_invoice(text)
    format_output(data)
```

### Consideraciones adicionales:

- **Ruta de pytesseract**: Asegúrate de ajustar la ruta de `pytesseract.pytesseract.tesseract_cmd` según dónde hayas instalado Tesseract-OCR.
- **Idioma de OCR**: En este ejemplo, se utiliza el idioma español (`lang='spa'`). Si la factura está en otro idioma, ajusta el parámetro `lang` en `image_to_string`.
- **Mejora de precisión**: Dependiendo de la calidad de la imagen, puede ser necesario aplicar más técnicas de preprocesamiento de imágenes para mejorar la precisión del OCR.

Este código proporciona una estructura básica para extraer y organizar la información de una factura. Puede requerir ajustes adicionales según el formato específico de las facturas que estés procesando.

---
Esta otra versión crea un archivo Json

```Python
"Analiza la imagen de la factura proporcionada y extrae la información solicitada de manera precisa. Organiza los datos en un formato estructurado y claro, utilizando las etiquetas especificadas para cada campo.

1. **Información del comercio:**
    
    - Nombre del comercio = `<Nombre>`
        
    - Sucursal (si aplica) = `<Sucursal>`
        
    - NIT (Número de Identificación Tributaria) = `<NIT>`
        
2. **Datos de la factura:**
    
    - Número de factura = `<N° Factura>`
        
    - Fecha de emisión (día/mes/año) = `<Fecha>`
        
    - Hora de emisión (hora:minuto) = `<Hora>`
        
3. **Detalles de los productos comprados:**  
    Para cada producto listado, extrae:
    
    - Cantidad = `<Cant>`
        
    - Descripción del producto o servicio = `<Detalle>`
        
    - Precio unitario = `<PU>`
        
    - Subtotal = `<Subtotal>`
        
4. **Totales y otros valores relevantes:**
    
    - Total a pagar (monto final) = `<Total>`
        
    - Cambio (si está especificado) = `<Cambio>`
        
    - Crédito fiscal (si aplica) = `<Crédito Fiscal>`
        

**Formato de salida esperado:**  
Presenta los datos en formato JSON estructurado como el siguiente ejemplo:

```json
{
  "comercio": {
    "Nombre": "Nombre del Comercio",
    "Sucursal": "Sucursal X",
    "NIT": "123456789"
  },
  "factura": {
    "N° Factura": "000123",
    "Fecha": "23/02/2025",
    "Hora": "11:30 PM"
  },
  "productos": [
    {
      "Cant": 2,
      "Detalle": "Producto A",
      "PU": 10.50,
      "Subtotal": 21.00
    },
    {
      "Cant": 1,
      "Detalle": "Producto B",
      "PU": 15.00,
      "Subtotal": 15.00
    }
  ],
  "totales": {
    "Total": 36.00,
    "Cambio": 4.00,
    "Crédito Fiscal": 3.60
  }
}

**Consideraciones adicionales:**
- Asegúrate de capturar todos los valores con precisión, incluso si están en diferentes posiciones dentro de la factura o tienen formatos variados.
    
- Si algún campo no está presente en la factura, indica explícitamente su ausencia con `null` en el JSON generado.
```


