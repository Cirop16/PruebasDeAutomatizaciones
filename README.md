Pre‑entrega Workflow
Nombre del caso elegido: Webhook → API → IF → Email

Qué dispara el workflow (trigger): El flujo se inicia cuando se recibe un POST en el Webhook con un JSON simple (ejemplo: { "keyword": "cat" }). Este evento activa automáticamente la ejecución del resto de los nodos.

Descripción de cada nodo:

Webhook: recibe la petición externa y entrega el contenido del body al workflow.

HTTP Request: consulta la API pública https://catfact.ninja/fact para obtener un dato aleatorio sobre gatos.

IF: evalúa el texto recibido de la API para decidir si contiene la palabra “cat”.

Set True: normaliza los datos cuando la condición se cumple, guardando fact y status = "match".

Set False: normaliza los datos cuando la condición no se cumple, guardando fact y status = "no_match".

Email (Gmail): envía un correo con el resultado, incluyendo el fact y el estado.

Qué evalúa el condicional y por qué: El nodo IF verifica si el texto (fact) devuelto por la API contiene la palabra "cat". Esto permite diferenciar entre coincidencias relevantes y casos en los que no aparece la palabra, generando dos caminos claros en el workflow.

Cómo configuraste la notificación: Se utilizó el nodo Email con credenciales de Gmail almacenadas en el gestor de n8n. El asunto se genera dinámicamente según el status (match o no_match), y el cuerpo del mensaje incluye el fact y el estado. El tipo de correo se configuró como Plain Text para simplicidad.

Buenas prácticas aplicadas:

Credenciales seguras: se guardaron en el gestor de credenciales de n8n, nunca expuestas en el archivo exportado.

Logs: se verificó cada ejecución en el Execution Log para asegurar trazabilidad.

Idempotencia: cada POST recibido por el Webhook se procesa como ejecución independiente, evitando duplicados.

Normalización de datos: uso de nodos Set para limpiar y estructurar la salida antes de enviarla.
