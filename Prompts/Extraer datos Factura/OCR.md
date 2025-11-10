## 1

```Python
import cv2
import pytesseract
from PIL import Image
# Configura la ruta de Tesseract si es necesario (dependiendo de tu sistema)
# pytesseract.pytesseract.tesseract_cmd = r'D:\Program Files\Tesseract-OCR\tesseract.exe'
# Configuración de pytesseract
pytesseract.pytesseract.tesseract_cmd = r'D:\Program Files\Tesseract-OCR\tesseract.exe'  # Ajusta la ruta según tu instalación
def extraer_datos_factura(ruta_imagen):
    # Cargar la imagen usando OpenCV
    imagen = cv2.imread(ruta_imagen)
    # Convertir la imagen a escala de grises para mejorar la precisión de OCR
    gris = cv2.cvtColor(imagen, cv2.COLOR_BGR2GRAY)
    # Aplicar un umbral para binarizar la imagen (opcional, dependiendo de la calidad de la imagen)
    _, binaria = cv2.threshold(gris, 150, 255, cv2.THRESH_BINARY)
    # Convertir la imagen binaria a un formato compatible con PIL
    imagen_pil = Image.fromarray(binaria)
    # Extraer texto de la imagen usando pytesseract
    texto_extraido = pytesseract.image_to_string(imagen_pil, lang='spa')
    # Inicializar un diccionario para almacenar los datos extraídos
    datos_factura = {
        "Nombre": None,
        "Sucursal": None,
        "NIT": None,
        "N° Factura": None,
        "Fecha": None,
        "Hora": None,
        "Detalles de los productos": [],
        "Total": None,
        "Cambio": None,
        "Crédito fiscal": None
    }
    # Procesar el texto extraído para encontrar la información relevante
    lineas = texto_extraido.split('\n')
    for linea in lineas:
        # Extraer el nombre del comercio
        if "Nombre" in linea:
            datos_factura["Nombre"] = linea.split(":")[1].strip()
        # Extraer la sucursal
        if "Sucursal" in linea:
            datos_factura["Sucursal"] = linea.split(":")[1].strip()
        # Extraer el NIT
        if "NIT" in linea:
            datos_factura["NIT"] = linea.split(":")[1].strip()
        # Extraer el número de factura
        if "N° Factura" in linea:
            datos_factura["N° Factura"] = linea.split(":")[1].strip()
        # Extraer la fecha
        if "Fecha" in linea:
            datos_factura["Fecha"] = linea.split(":")[1].strip()
        # Extraer la hora
        if "Hora" in linea:
            datos_factura["Hora"] = linea.split(":")[1].strip()
        # Extraer detalles de los productos (asumiendo un formato específico)
        if "Cant" in linea and "Detalle" in linea and "PU" in linea and "Subtotal" in linea:
            # Aquí se asume que los detalles de los productos están en líneas subsiguientes
            # y que siguen un formato específico.
            pass  # Implementar lógica para extraer detalles de productos
        # Extraer el total
        if "Total" in linea:
            datos_factura["Total"] = linea.split(":")[1].strip()
        # Extraer el cambio
        if "Cambio" in linea:
            datos_factura["Cambio"] = linea.split(":")[1].strip()
        # Extraer el crédito fiscal
        if "Crédito fiscal" in linea:
            datos_factura["Crédito fiscal"] = linea.split(":")[1].strip()
    # Retornar los datos extraídos
    return datos_factura
# Ejemplo de uso
ruta_imagen = r"J:\Mi unidad\PYTHON\OCR_Facturas\factura.jpg"
datos = extraer_datos_factura(ruta_imagen)
# Imprimir los datos en formato de tabla
print("Información del comercio:")
print(f"Nombre: {datos['Nombre']}")
print(f"Sucursal: {datos['Sucursal']}")
print(f"NIT: {datos['NIT']}")
print("\nDatos de la factura:")
print(f"N° Factura: {datos['N° Factura']}")
print(f"Fecha: {datos['Fecha']}")
print(f"Hora: {datos['Hora']}")
print("\nDetalles de los productos comprados:")
print("| Cant | Detalle | PU | Subtotal |")
for producto in datos['Detalles de los productos']:
    print(f"| {producto['Cant']} | {producto['Detalle']} | {producto['PU']} | {producto['Subtotal']} |")
print("\nTotales y otros valores relevantes:")
print(f"Total: {datos['Total']}")
print(f"Cambio: {datos['Cambio']}")
print(f"Crédito fiscal: {datos['Crédito fiscal']}")
```

## 2

```Python
import cv2
import pytesseract
from PIL import Image
import re

# Configuración de pytesseract
pytesseract.pytesseract.tesseract_cmd = r'D:\Program Files\Tesseract-OCR\tesseract.exe'  # Ajusta la ruta según tu instalación

def preprocess_image(image_path):
    # Cargar la imagen usando OpenCV
    image = cv2.imread(image_path)
    
    # Convertir la imagen a escala de grises para mejorar la precisión de OCR
    gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
    
    # Aplicar un umbral para binarizar la imagen
    _, binary = cv2.threshold(gray, 150, 255, cv2.THRESH_BINARY)
    
    return binary

def extract_text(image):
    # Convertir la imagen binaria a un formato compatible con PIL
    image_pil = Image.fromarray(image)
    
    # Extraer texto de la imagen usando pytesseract
    text = pytesseract.image_to_string(image_pil, lang='spa')
    
    return text

def parse_generic_invoice(text):
    data = {
        "Empresa": None,
        "NIT": None,
        "Sucursal": None,
        "N° Factura": None,
        "Fecha": None,
        "Detalles de los productos": [],
        "Subtotal": None,
        "Total": None,
        "Crédito fiscal": None
    }
    
    lines = text.split('\n')
    
    for line in lines:
        # Empresa
        if re.search(r'\bS\.A\.', line):
            data["Empresa"] = line.strip()
        
        # NIT
        nit_match = re.search(r'NIT\s*:\s*(\d+)', line)
        if nit_match:
            data["NIT"] = nit_match.group(1)
        
        # Sucursal
        sucursal_match = re.search(r'Sucursal|OFICINA', line)
        if sucursal_match:
            data["Sucursal"] = line.strip()
        
        # N° Factura
        factura_match = re.search(r'Factura|FAC\s*:\s*(\d+)', line)
        if factura_match:
            data["N° Factura"] = factura_match.group(1)
        
        # Fecha
        fecha_match = re.search(r'Fecha\s*:\s*(\d{2}/\d{2}/\d{4})', line)
        if fecha_match:
            data["Fecha"] = fecha_match.group(1)
        
        # Detalles de los productos
        producto_match = re.search(r'(\d+)\s+(.*?)\s+(\d+\.\d{2})', line)
        if producto_match:
            cantidad = producto_match.group(1)
            descripcion = producto_match.group(2)
            precio = producto_match.group(3)
            data["Detalles de los productos"].append({
                "Cantidad": cantidad,
                "Descripcion": descripcion,
                "Precio": precio
            })
        
        # Subtotal
        subtotal_match = re.search(r'Subtotal\s*:\s*(\d+\.\d{2})', line)
        if subtotal_match:
            data["Subtotal"] = subtotal_match.group(1)
        
        # Total
        total_match = re.search(r'Total\s*:\s*(\d+\.\d{2})', line)
        if total_match:
            data["Total"] = total_match.group(1)
        
        # Crédito fiscal
        credito_fiscal_match = re.search(r'Crédito\s*fiscal\s*:\s*(\d+\.\d{2})', line)
        if credito_fiscal_match:
            data["Crédito fiscal"] = credito_fiscal_match.group(1)
    
    return data

def main():
    image_path = r"J:\Mi unidad\PYTHON\OCR_Facturas\factura.jpg"
    processed_image = preprocess_image(image_path)
    extracted_text = extract_text(processed_image)
    parsed_data = parse_generic_invoice(extracted_text)
    
    print("Información del comercio:")
    print(f"Empresa: {parsed_data['Empresa']}")
    print(f"NIT: {parsed_data['NIT']}")
    print(f"Sucursal: {parsed_data['Sucursal']}")
    print("\nDatos de la factura:")
    print(f"N° Factura: {parsed_data['N° Factura']}")
    print(f"Fecha: {parsed_data['Fecha']}")
    print("\nDetalles de los productos comprados:")
    for producto in parsed_data['Detalles de los productos']:
        print(f"Cantidad: {producto['Cantidad']}, Descripción: {producto['Descripcion']}, Precio: {producto['Precio']}")
    print("\nTotales y otros valores relevantes:")
    print(f"Subtotal: {parsed_data['Subtotal']}")
    print(f"Total: {parsed_data['Total']}")
    print(f"Crédito fiscal: {parsed_data['Crédito fiscal']}")

if __name__ == "__main__":
    main()
```