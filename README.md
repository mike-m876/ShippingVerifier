## 1. ¿Qué problema elegí y por qué?

Elegí la revisión de documentos de embarque: conocimientos de embarque, listas de empaque y facturas comerciales. Hoy alguien tiene que leerlos uno por uno y comparar a mano el número asignado, el peso, el valor declarado, las fechas y todos los campos asociados. Es un trabajo repetitivo donde un error de captura puede detener mercancía o generar problemas en aduana.

Descarté la idea de un chatbot puesto que es lo más común y no demuestra que entiendo cómo se usa la IA dentro de una empresa. Me quedé con este problema por cuatro razones:
1. Es un problema real con resultado medible. 
2. La IA hace trabajo de verdad y no solo conversa. 
3. Se puede demostrar con documentos de ejemplo. 
4. Encaja con lo que enseña la empresa a la que aplico: es un caso de adopción de IA con buenas prácticas, donde la IA lee, el código decide y una persona aprueba.

## 2. ¿Qué le pedí al agente para poder hacer la app?

Trabajé con Claude Code directamente desde mi computadora, utilizando la terminal para desarrollar la aplicación que revisa documentos de embarque. Una vez elegido el problema, le pedí primero desarrollar únicamente el frontend, que incluía carga de archivos, campos editables, validaciones y botones para aprobar y rechazar. Después le solicité crear el backend utilizando la API de OpenAI, implementando un servidor express con la ruta /api/analyze, respuestas en formato JSON estricto y un registro de pedidos de ejemplo. A continuación, le pedí integrar ambas partes en un solo proyecto y generar tres documentos PDF de prueba, utilizando los datos de orders.json. Durante las pruebas, cuando la aplicación mostraba el error de "demasiadas solicitudes", revisamos el problema y encontramos que me faltaban créditos en OpenAI. También se mejoró la identificación de errores según su causa. Más adelante, le pedí guardar los resultados en data/reviews.json para conservar un historial, agregando rutas para consultar y actualizar los registros y una bitácora de actividad.

## 3. ¿Qué escribieron los agentes y qué corregí yo?

**Lo que escribió el agente.** Toda la implementación: la interfaz, el servidor, la llamada a la API con salida estructurada, las siete reglas de validación, los pedidos de ejemplo, los PDF de prueba, el guardado en JSON con su bitácora y las traducciones.

**Lo que decidí y corregí yo.**

Las decisiones de producto fueron mías: descartar el chatbot, escoger este problema, avanzar por etapas, pedir que la app recordara las revisiones y que todo estuviera en español.

También hice la parte que el agente no podía hacer: instalar Node, configurar mi clave en un '.env', cargar saldo en la cuenta de OpenAI y probar con documentos reales. Esas pruebas sacaron a la luz un error de diseño. El código original mostraba el mismo aviso de "demasiadas solicitudes" para dos causas distintas, el límite de uso y la falta de saldo, y por eso no podía saber qué fallaba. Después de revisar la terminal confirmé que era saldo, y solicité añadir una funcionalidad con la cual el servidor ahora distingue los casos de error.

**Límites que dejé tal cual.** 
- La referencia de pedido nunca puede "fallar" por diferencia, porque el pedido esperado se busca usando el mismo número que lee el documento: si ese pedido no existe, la app avisa que "no hay pedido registrado". 
- El botón "Leer de nuevo" solo aparece en documentos subidos en la sesión actual, porque no guardo el archivo original, solo los datos extraídos. 
- Y el archivo JSON sirve para este proyecto, pero con muchos usuarios habría que pasar a una base de datos como MySQL.

## 4. ¿Cómo mitigaría un riesgo de seguridad?

El principal riesgo identificado es la exposición de la clave de API y la falta de protección del servidor. Si alguien obtiene la clave, podría gastar el saldo de la cuenta asociada, y si el servidor no cuenta con autenticación, cualquier persona podría utilizar el análisis o consultar el historial. Para reducir estos riesgos, se implementó el almacenamiento de la clave en un archivo .env, que junto con la carpeta data está excluido de GitHub mediante .gitignore. También se limitaron los archivos aceptados a PDF, PNG y JPG de máximo 15 MB. Como medidas adicionales, se recomienda mantener desactivada la recarga automática de créditos, establecer límites de gasto, implementar autenticación y roles, limitar las solicitudes por usuario, restringir CORS, utilizar HTTPS y un gestor de secretos, así como establecer políticas de conservación del historial y evitar registrar el contenido de los documentos.

El segundo riesgo consiste en las instrucciones ocultas dentro de los documentos, que podrían intentar manipular al modelo para que apruebe un envío incorrectamente. Para mitigarlo, se indicó al modelo que trate el contenido de los documentos únicamente como información y no como instrucciones, además de restringir sus respuestas a un esquema JSON con diez campos definidos. Las validaciones se realizan directamente en el código utilizando los datos del pedido, sin confiar únicamente en las respuestas del modelo. Finalmente, la aprobación o el rechazo definitivo siempre queda en manos de una persona, reduciendo la posibilidad de que una respuesta incorrecta del modelo provoque una decisión equivocada.
