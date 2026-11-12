## 1. ¿Qué problema elegiste y por qué?

El problema seleccionado fue la revisión de documentos de embarque. Este proceso suele realizarse manualmente, lo que implica revisar cada documento individualmente y comparar sus campos para verificar que la información esté completa y sea correcta. Debido al volumen de documentos y al tiempo que requiere esta actividad, existe la posibilidad de cometer errores humanos que pueden ocasionar retrasos en los procesos logísticos e incluso generar conflictos durante los procedimientos aduaneros.
Por este motivo, se propuso desarrollar una aplicación que permita automatizar parte del proceso de revisión mediante inteligencia artificial, facilitando la identificación de inconsistencias y permitiendo que el usuario tome decisiones con mayor rapidez.

## 2. ¿Qué le pediste a Claude Code, Cursor o Codex para que pudieras hacer la app?

El desarrollo se dividió en varias etapas siguiendo un enfoque modular. Inicialmente, se solicitó la creación del frontend, adaptado al contexto de la aplicación, incluyendo funcionalidades para cargar documentos, visualizar y editar campos, realizar validaciones y aprobar o rechazar los documentos revisados.

Posteriormente, se solicitó desarrollar el backend utilizando la API de OpenAI, mediante un servidor Express y la ruta /api/analyze. También se implementó un formato JSON estricto para estructurar las respuestas del modelo y un sistema para almacenar los resultados de las revisiones en un archivo JSON, con el propósito de conservar un historial.

Finalmente, se solicitó integrar el frontend y el backend en un proyecto unificado y generar documentos PDF de prueba para comprobar el funcionamiento de la aplicación. Durante las pruebas se presentó un error relacionado con un exceso de solicitudes, cuyo origen se identificó como la falta de créditos disponibles en la API de OpenAI. A partir de esto, se solicitó mejorar la identificación y diferenciación de los errores según su causa.

## 3. ¿Qué escribieron los agentes y qué corregiste tú?

Claude Code se encargó de generar la implementación de la interfaz de usuario, el backend con las llamadas a la API de OpenAI, las reglas de validación y los documentos de ejemplo utilizados durante las pruebas. También desarrolló el mecanismo para almacenar y consultar el historial de revisiones.

Por mi parte, me encargué de seleccionar el problema que se abordaría, definir las funcionalidades necesarias y dividir el desarrollo en etapas para facilitar su implementación. También realicé la instalación y configuración de Node.js, configuré el acceso a la API de OpenAI desde mi entorno local, incorporé la clave de acceso mediante variables de entorno y, finalmente, gestioné la publicación del proyecto en un repositorio utilizando la terminal.

Durante las pruebas también identifiqué los problemas de funcionamiento, analicé sus posibles causas y solicité los ajustes necesarios para corregirlos.

## 4. ¿Cómo mitigarías un riesgo de seguridad de la app?

Uno de los principales riesgos identificados es la posible filtración de la clave de acceso a la API de OpenAI. Si una persona no autorizada obtiene esta clave, podría utilizarla indebidamente y consumir los créditos disponibles de la cuenta asociada.

Para reducir este riesgo, se implementó el almacenamiento de la clave en un archivo .env, el cual, junto con la carpeta data, fue excluido del repositorio mediante .gitignore, evitando que esta información se publique accidentalmente. Asimismo, se establecieron restricciones para aceptar únicamente archivos PDF, JPG y PNG con un tamaño máximo de 15 MB. Como medidas adicionales, se propone implementar autenticación de usuarios y roles, así como limitar la cantidad de solicitudes que puede realizar cada usuario.

Otro riesgo importante es la presencia de instrucciones maliciosas u ocultas dentro de los documentos, que podrían intentar manipular al modelo para obtener aprobaciones incorrectas. Para mitigar este problema, se indicó al modelo que tratara el contenido de los documentos exclusivamente como información y no como instrucciones. También se restringieron sus respuestas a un esquema JSON con diez campos predefinidos. Las validaciones se realizan tanto mediante el código de la aplicación como con el apoyo del modelo, evitando depender exclusivamente de sus respuestas. Finalmente, la decisión definitiva de aprobar o rechazar un documento permanece bajo la responsabilidad del usuario.
