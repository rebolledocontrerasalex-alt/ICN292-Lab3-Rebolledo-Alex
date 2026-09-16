# ICN292-Lab3-Rebolledo-Alex
ICN292 — Laboratorio 3: Triage automático de devoluciones (AndesHogar SpA)

Autor: Álex Rebolledo Contreras RUT (sin dígito verificador): [COMPLETAR — ej: 20.145.876] · S (semilla, últimos 3 dígitos) = 500 Fecha: [COMPLETAR fecha de entrega] Curso: ICN292 — Sistemas de Información para la Gestión, Universidad Técnica Federico Santa María Repositorio: https://github.com/rebolledocontrerasalex-alt/ICN292-Lab3-Rebolledo-Alex

Parámetros de la semilla personal
Parámetro	Cálculo	Valor
S	Tres últimos dígitos del RUT, sin dígito verificador	500
U (umbral de monto)	30000 + 1000 × (S mod 50)	$30.000
D (plazo máximo)	7 + 7 × (S mod 4)	7 días
Contenido de este repositorio
ICN292-Lab3-Rebolledo-Alex.pdf — informe en PDF.
ICN292-Lab3-Rebolledo-Alex.docx — informe en Word.
ICN292-Lab3-Rebolledo-Alex-triage.json — workflow de triage (Parte A, 12 nodos, sin código).
ICN292-Lab3-Rebolledo-Alex-emisor.json — workflow que simula el envío de las 15 solicitudes de prueba.
ICN292-Lab3-Rebolledo-Alex-resumen.json — workflow programado de resumen diario (Parte B).
Cómo reproducir cada archivo
Workflows de n8n (.json)
Abrir una instancia de n8n (propia o self-hosted).
Menú del workflow (···) → Import from File → seleccionar el .json correspondiente.
triage: configurar una credencial de Google Sheets en el nodo Registro - Google Sheets y reemplazar el documentId por el ID de tu propio Google Sheet (debe tener una hoja llamada Registro con los encabezados: id_solicitud, sku, monto, dias_desde_compra, estado_producto, ruta, motivo, U, D, monto_uf, email_cliente, timestamp, uf_error). Publicar el workflow para activar el webhook (/webhook/triage-devoluciones).
emisor: apunta por defecto a la Production URL del webhook del triage. Ejecutar manualmente (Trigger Manual) para mandar las 15 solicitudes de prueba, espaciadas 5 segundos entre sí (para evitar colisiones de escritura concurrente en Google Sheets, ver informe).
resumen: configurar la misma credencial y el mismo Sheet ID en el nodo Google Sheets - Leer Registro. Corre automáticamente todos los días a las 20:00 hora de Santiago (cron 0 20 * * *), o se puede ejecutar manualmente para probar.
Informe

Abrir directamente ICN292-Lab3-Rebolledo-Alex.pdf o ICN292-Lab3-Rebolledo-Alex.docx.

Resumen de resultados
Parte A: las 15 solicitudes (+ 3 de control) se clasificaron correctamente: 7 RECHAZO, 6 REVISION, 2 APROBACION.
Parte B: workflow de resumen probado manualmente, arma correctamente el desglose por ruta, monto total y tasa de aprobación automática del día.
Parte C: se documentan 7 casos —incluyendo dos hallazgos reales no buscados: fallas intermitentes de conexión a mindicador.cl, y pérdida ocasional de filas al escribir concurrentemente en Google Sheets— con evidencia (capturas) en el informe.

Ninguna credencial, token ni contraseña está incluida en los archivos de este repositorio; las credenciales de Google Sheets se gestionan por separado en el gestor de credenciales de n8n.
