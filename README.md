# ICN292 - Laboratorio 3: Triage automático de devoluciones (AndesHogar SpA)

Autor: Álex Rebolledo Contreras
RUT: 21.500.500-3
Fecha: 16 de septiembre de 2026
Curso: ICN292, Sistemas de Información para la Gestión, Universidad Técnica Federico Santa María
Repositorio: https://github.com/rebolledocontrerasalex-alt/ICN292-Lab3-Rebolledo-Alex

## Parámetros de la semilla personal

S son los tres últimos dígitos del RUT sin dígito verificador, en este caso 500.

U (umbral de monto) se calcula como 30000 + 1000 x (S mod 50), lo que da $30.000.
D (plazo máximo) se calcula como 7 + 7 x (S mod 4), lo que da 7 días.

## Contenido de este repositorio

- ICN292-Lab3-Rebolledo-Alex.pdf: informe en PDF.
- ICN292-Lab3-Rebolledo-Alex.docx: informe en Word.
- ICN292-Lab3-Rebolledo-Alex-triage.json: workflow de triage (Parte A, 12 nodos, sin código).
- ICN292-Lab3-Rebolledo-Alex-emisor.json: workflow que simula el envío de las 15 solicitudes de prueba.
- ICN292-Lab3-Rebolledo-Alex-resumen.json: workflow programado de resumen diario (Parte B).

## Cómo reproducir cada archivo

Para los workflows de n8n (.json), hay que abrir una instancia de n8n propia o self-hosted, ir al menú del workflow y usar la opción Import from File, seleccionando el archivo correspondiente.

En el workflow de triage hay que configurar una credencial de Google Sheets en el nodo Registro - Google Sheets, y reemplazar el documentId por el ID de tu propio Google Sheet. Ese Sheet debe tener una hoja llamada Registro con los encabezados: id_solicitud, sku, monto, dias_desde_compra, estado_producto, ruta, motivo, U, D, monto_uf, email_cliente, timestamp, uf_error. Después hay que publicar el workflow para activar el webhook en /webhook/triage-devoluciones.

El workflow emisor apunta por defecto a la Production URL del webhook del triage. Se ejecuta de forma manual (Trigger Manual) para mandar las 15 solicitudes de prueba, que se envían espaciadas cinco segundos entre sí para evitar colisiones al escribir en Google Sheets (esto se explica con más detalle en el informe).

El workflow de resumen necesita la misma credencial y el mismo Sheet ID en el nodo Google Sheets - Leer Registro. Corre solo todos los días a las 20:00 hora de Santiago, pero también se puede ejecutar manualmente para probarlo.

Para el informe, basta con abrir directamente el PDF o el Word.

## Resumen de resultados

En la Parte A, las 15 solicitudes más las 3 de control se clasificaron correctamente: 7 quedaron en RECHAZO, 6 en REVISION y 2 en APROBACION.

En la Parte B, el workflow de resumen se probó manualmente y arma bien el desglose por ruta, el monto total y la tasa de aprobación automática del día.

En la Parte C se documentan los 7 casos pedidos, incluyendo dos hallazgos reales que no se buscaron a propósito: fallas intermitentes de conexión a mindicador.cl, y pérdida ocasional de alguna fila al escribir de forma concurrente en Google Sheets. Ambos quedan con evidencia (capturas) en el informe.

Los archivos de este repositorio no incluyen ninguna credencial, token ni contraseña. Las credenciales de Google Sheets se manejan aparte, en el gestor de credenciales de n8n.
