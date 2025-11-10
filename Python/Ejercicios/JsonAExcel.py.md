
Script para transformar datos de .json a .md

```
import json

import os

  

def procesar_gastos(ruta_json, ruta_salida):

    """

    Procesa un archivo JSON de gastos, genera una tabla Markdown y la guarda.

  

    Args:

        ruta_json: Ruta al archivo JSON de entrada.

        ruta_salida: Ruta donde se guardará el archivo Markdown.

    """

  

    try:

        with open(ruta_json, 'r', encoding='utf-8') as archivo:

            datos = json.load(archivo)

    except FileNotFoundError:

        print(f"Error: Archivo no encontrado en {ruta_json}")

        return

    except json.JSONDecodeError:

        print(f"Error: El archivo en {ruta_json} no es un JSON válido")

        return

  

    mensajes = datos.get('messages', [])  # Maneja el caso de que 'messages' no exista

    tabla_markdown = "| ID  | Fecha       | Hora      | Monto      | Justificación | Categoría | Personal | Obra (Nombre de la Obra) | Tipo1       | Tipo2     | Proveedores | Personas | Descripción                               |\n"

    tabla_markdown += "| --- | ----------- | --------- | ---------- | ------------- | --------- | -------- | ------------------------ | ----------- | --------- | ----------- | -------- | ----------------------------------------- |\n"

  

    hashtags_justificacion = ["#Anticipo", "#Ingreso", "#Egreso", "#Transferencia", "#Préstamo", "#Devolución", "#Fondo_Propio", "#Gasto", "#Rendir", "#Presupuesto"]

    hashtags_categoria = ["#Obra", "#Personal", "#Varios"]

    hashtags_personal = ["#CPVA", "#HVV", "#JMVD", "#JSVC", "#MVV"]

    hashtags_obra = ["#Mantenimiento", "#Gonzalez", "#Vasquez", "#Ichilo", "#Casa_3PF", "#Montero-Kiosko"]

    hashtags_tipo1 = ["#Alimentación", "#Educación", "#Electrónica", "#Entretenimiento", "#Financiero", "#Herramienta", "#Higiene", "#Impuesto", "#Ocio", "#Otro_Ingreso", "#Regalo", "#Salud", "#Servicio_Básico", "#Sueldo", "#Suscripción", "#Transporte", "#Varios", "#Vestimenta", "#Viaje", "#Vivienda"]

    hashtags_tipo2 = ["#Administración", "#Anticipo", "#Gasto", "#GG", "#Herramienta", "#Material", "#MO", "#Planilla_Avance", "#Presupuesto", "#Servicio", "#Transporte"]

    hashtags_proveedores = ["#Hipermaxi", "#Farmacorp", "#Farmacia_Chavez", "#Roho", "#Ferreteros", "#Paradiso_Montesori"]

  
  

    for mensaje in mensajes:

        id_mensaje = mensaje.get('id')

        fecha_str = mensaje.get('date')

        monto_str = mensaje.get('text', [])

        if isinstance(monto_str, list):

            monto_str = "".join([t.get('text', '') if isinstance(t, dict) else t for t in monto_str])

        monto = 0.00

        try:

            monto = float(monto_str.split(" Bs.")[0].replace("-","").replace("±","").strip())

        except (ValueError, IndexError):

          pass

  

        justificacion = ""

        categoria = ""

        personal = ""

        obra = ""

        tipo1 = ""

        tipo2 = ""

        proveedor = ""

        persona = ""

        descripcion = ""

  

        texto_entities = mensaje.get("text_entities", [])

        texto_completo = "".join([entity.get("text") for entity in texto_entities])

  

        for entity in texto_entities:

            if entity.get("type") == "hashtag":

                hashtag = entity.get("text")

                if not justificacion and hashtag in hashtags_justificacion:

                    justificacion = hashtag

                elif not categoria and hashtag in hashtags_categoria:

                    categoria = hashtag

                elif not personal and hashtag in hashtags_personal:

                    personal = hashtag

                elif not obra and hashtag in hashtags_obra:

                    obra = hashtag

                elif not tipo1 and hashtag in hashtags_tipo1:

                    tipo1 = hashtag

                elif not tipo2 and hashtag in hashtags_tipo2:

                    tipo2 = hashtag

                elif not proveedor and hashtag in hashtags_proveedores:

                    proveedor = hashtag

            elif entity.get("type") == "mention":

                if not persona:

                    persona = entity.get("text")

        descripcion = texto_completo.replace(justificacion, "").replace(categoria, "").replace(personal, "").replace(obra, "").replace(tipo1, "").replace(tipo2, "").replace(proveedor, "").replace(persona, "").replace(f"{monto:.2f} Bs.","").replace(f"-{monto:.2f} Bs.","").replace(f"±{monto:.2f} Bs.","").strip()

  

        try:

            fecha, hora = fecha_str.split("T")

            hora = hora.split(".")[0]

        except (ValueError, AttributeError):

            fecha = ""

            hora = ""

  

        tabla_markdown += f"| {id_mensaje} | {fecha} | {hora} | {monto:.2f} Bs. | {justificacion} | {categoria} | {personal} | {obra} | {tipo1} | {tipo2} | {proveedor} | {persona} | {descripcion} |\n"

  

    try:

        with open(ruta_salida + ".md", 'w', encoding='utf-8') as archivo_md:

            archivo_md.write(tabla_markdown)

        print(f"Tabla Markdown guardada en {ruta_salida}.md")

    except Exception as e:

        print(f"Error al guardar el archivo Markdown: {e}")

  

# Ejemplo de uso:

ruta_json = r"J:\Mi unidad\GASTOS\2025\TELEGRAM\SEMANA-03\Gastos_2025-01-15\result.json"

ruta_salida = r"J:\Mi unidad\GASTOS\2025\TELEGRAM\SEMANA-03\Gastos_2025-01-15\result_TABLA" # Nombre del archivo de salida

  

procesar_gastos(ruta_json, ruta_salida)
```
