# Capítulo VII: DevOps Practices

En este capítulo se describe cómo se aplicaron prácticas de DevOps para asegurar un ciclo de desarrollo más controlado, repetible y seguro. La solución Budgetly fue organizada con un flujo de trabajo que permite validar cambios antes de su integración, empaquetar artefactos de despliegue y publicar nuevas versiones con menor intervención manual.

La estrategia implementada se apoyó principalmente en GitHub Actions, ramas `develop` y `main`, uso de secretos para credenciales y mecanismos de despliegue automáticos para el Frontend y el Back-End.

## 7.1. Continuous Integration

La Integración Continua se aplicó a los dos repositorios principales de la solución: `Budgetly-FrontEnd` y `Budgetly-BackEnd`. En ambos casos, el proceso comienza con la apertura o actualización de un Pull Request y termina con la validación automática del código mediante compilación y pruebas.

Este enfoque permitió detectar errores de integración en etapas tempranas, reducir retrabajos y asegurar que cada cambio llegue con un nivel mínimo de calidad antes de pasar a producción.

### 7.1.1. Tools and Practices

Para implementar CI se utilizaron las siguientes herramientas y prácticas:

* **GitHub Actions**: se empleó como motor de automatización para ejecutar los pipelines de validación en cada Pull Request y en los eventos de `push` a `main`.
* **GitFlow**: se mantuvo la estrategia de ramas `main`, `develop` y ramas de trabajo tipo feature para aislar cambios antes de integrarlos.
* **Branch protection**: se recomendó proteger la rama `main` para exigir checks verdes antes del merge.
* **Secrets de GitHub**: se usaron secretos para credenciales de publicación y despliegue, evitando exponer información sensible en el repositorio.

#### Frontend CI

El Frontend fue desarrollado con **Vue 3 + Vite**, por lo que la validación automática se implementó con los comandos estándar de Node.js:

* `npm ci` para instalar dependencias de forma reproducible.
* `npm run build` para generar la compilación de producción.

El workflow del Frontend se configuró para ejecutarse cuando se abre un Pull Request hacia `main`. De esta manera, cada cambio de la rama `develop` se verifica antes de promoverlo a producción.

Además, esta validación automática resulta especialmente útil en proyectos con múltiples componentes visuales, ya que permite confirmar que las vistas, estilos y módulos principales compilan correctamente antes de fusionar el código.

Se puede observar el resultado de este proceso en la siguiente evidencia del pipeline de integración continua del Frontend:

<p align="center">
	<img src="../assets/chapter-07/frontend-build.png" alt="Frontend CI build" />
</p>

#### Back-End CI

El Back-End fue desarrollado con **.NET 9**, por lo que el pipeline de CI ejecuta las siguientes etapas:

* `dotnet restore Backend.sln` para restaurar dependencias.
* `dotnet build Backend.sln --configuration Release --no-restore` para compilar la solución.
* `dotnet test Backend.sln --no-build --verbosity normal` para ejecutar pruebas de la solución.
* `dotnet publish com.split.backend/com.split.backend.csproj -c Release -o publish` para generar el artefacto de despliegue.

Además de la compilación, se generó un artefacto intermedio para su uso posterior en los flujos de despliegue.

Con ello se garantizó que la solución del servidor mantuviera coherencia entre dependencias, compilación y empaquetado, evitando publicar versiones incompletas o con errores de integración.

La evidencia de este flujo para el Back-End se muestra a continuación:

<p align="center">
	<img src="../assets/chapter-07/backend-build.png" alt="Backend CI build" />
</p>

### 7.1.2. Build & Test Suite Pipeline Components

Los componentes principales del pipeline de CI fueron los siguientes:

* **Checkout**: descarga del código fuente del repositorio.
* **Setup runtime**: instalación del entorno requerido, como Node.js para el Frontend o .NET 9 para el Back-End.
* **Restore/Install**: restauración de dependencias según el stack tecnológico.
* **Build**: compilación de la solución o del proyecto frontend.
* **Test**: validación automática del comportamiento esperado.
* **Artifact generation**: publicación de la salida compilada para su posterior uso en despliegue.

En el caso del Back-End, se incorporó la creación de una imagen Docker para empaquetar la aplicación y facilitar su publicación en el registry de contenedores.

En conjunto, estas etapas forman una cadena de validación que ayuda a confirmar que el código puede ser construido y preparado correctamente en un entorno automatizado, lo que incrementa la confiabilidad del proceso.

## 7.2. Continuous Delivery

La Entrega Continua se definió como el paso siguiente a la validación de CI. Cuando los cambios pasan las revisiones automáticas y el Pull Request es aprobado, el merge hacia `main` dispara la preparación de los artefactos finales para su publicación en los entornos productivos.

En esta etapa se priorizó que la publicación de nuevas versiones fuera consistente y trazable. De esa forma, cada despliegue queda asociado a un commit específico y puede auditarse con facilidad en caso de incidencias.

### 7.2.1. Tools and Practices

Las herramientas utilizadas para esta etapa fueron:

* **GitHub Actions** para orquestar el flujo de entrega.
* **Firebase Hosting** para el despliegue del Frontend estático.
* **GHCR (GitHub Container Registry)** para publicar la imagen Docker del Back-End.
* **Azure Web App** para hospedar el servicio ASP.NET Core del Back-End.
* **Secrets de repositorio** para almacenar credenciales de despliegue como `FIREBASE_SERVICE_ACCOUNT`, `FIREBASE_PROJECT_ID`, `GHCR_PAT` y los secretos de Azure.

En esta etapa, el despliegue no depende de acciones manuales sobre los archivos fuente, sino de eventos de integración en la rama principal.

#### Entrega del Frontend

El Frontend se publica automáticamente en **Firebase Hosting**. El workflow se dispara cuando se realiza `push` sobre `main` y realiza la compilación de producción antes de enviar los archivos estáticos al hosting.

Este mecanismo fue útil para asegurar una publicación rápida del sitio web, manteniendo la misma versión que fue validada previamente en CI.

La publicación exitosa en Firebase Hosting se evidencia en la siguiente captura:

<p align="center">
	<img src="../assets/chapter-07/frontend-deploy.png" alt="Frontend deploy to Firebase Hosting" />
</p>

#### Entrega del Back-End

El Back-End se empaqueta en una imagen Docker y se publica en **GitHub Container Registry**. Este paso permite disponer de una imagen versionada del servicio y, posteriormente, desplegarla al entorno de Azure Web App.

La publicación de la imagen en un registry centralizado facilita el control de versiones, la reutilización en distintos entornos y la recuperación del estado exacto de una entrega anterior.

La evidencia del despliegue y publicación del Back-End se muestra en la siguiente captura:

<p align="center">
	<img src="../assets/chapter-07/backend-deploy.png" alt="Backend deploy to Azure Web App" />
</p>

### 7.2.2. Stages Deployment Pipeline Components

Los componentes de la etapa de entrega se estructuraron de la siguiente forma:

* **Build artifact**: compilación del código fuente y generación del paquete de salida.
* **Containerization**: creación de la imagen Docker para el Back-End.
* **Registry publication**: publicación de la imagen en GHCR.
* **Web hosting deployment**: publicación del Frontend en Firebase Hosting.
* **Application deployment**: despliegue del Back-End en Azure Web App.

En la práctica, el Frontend y el Back-End comparten la misma lógica de promoción: primero se valida en CI sobre `develop` o en Pull Requests, y luego se publica desde `main`.

Gracias a esta secuencia, el equipo pudo mantener un flujo estable entre desarrollo, validación y entrega sin introducir pasos manuales innecesarios.

## 7.3. Continuous Deployment

El Despliegue Continuo se implementó como la automatización completa del paso final hacia producción. Una vez que el código llega a `main`, los pipelines publican automáticamente las nuevas versiones en los entornos configurados.

Este patrón reduce tiempos de publicación y permite que las mejoras aprobadas estén disponibles con mayor rapidez para el usuario final.

### 7.3.1. Tools and Practices

Las prácticas aplicadas para el despliegue continuo fueron:

* **Despliegue automático por rama principal**: el merge hacia `main` inicia la publicación.
* **Separación de responsabilidades**: un workflow valida y otro publica o despliega.
* **Uso de secretos**: se evitó colocar credenciales dentro del código fuente.
* **Versionado por commit**: las imágenes Docker se etiquetan con el hash del commit, lo que facilita identificar exactamente qué versión fue desplegada.

* **Observabilidad básica del flujo**: el historial de ejecuciones en GitHub Actions permite revisar qué etapa falló, cuánto demoró y qué artefacto fue generado.

#### Frontend en producción

El Frontend se despliega en Firebase Hosting con los secretos de servicio configurados en GitHub. El pipeline publica una versión estática consistente y permite validar rápidamente el sitio web final.

Esto asegura que los cambios visuales y funcionales del Frontend puedan publicarse con un proceso uniforme, minimizando diferencias entre el entorno local y el entorno de producción.

#### Back-End en producción

El Back-End se despliega en Azure Web App. Adicionalmente, se publica la imagen Docker en GHCR para conservar una referencia inmutable del estado de la aplicación en producción.

De esta manera, el servicio queda disponible en un entorno administrado por la nube, con una imagen de contenedor que puede reutilizarse o reemplazarse de forma controlada.

### 7.3.2. Production Deployment Pipeline Components

Los componentes finales del pipeline productivo son:

* **Trigger**: `push` a `main` o ejecución manual cuando se requiera.
* **Compile and package**: generación del artefacto de producción.
* **Publish to registry**: publicación de la imagen en GHCR.
* **Deploy to hosting**: publicación del Frontend en Firebase Hosting.
* **Deploy to cloud app service**: despliegue del Back-End en Azure Web App.

Como resultado, la solución Budgetly quedó integrada con una estrategia de CI/CD que cubre validación, empaquetado y despliegue de ambos lados del sistema.

Esta arquitectura de automatización mejora la calidad de entrega, simplifica el mantenimiento y fortalece la trazabilidad del proceso de desarrollo.

## 7.4. Continuous Monitoring

### 7.4.1. Tools and Practices

Para implementar el monitoreo continuo se definió una estrategia basada en las herramientas ya utilizadas durante el ciclo DevOps del proyecto. Esto permitió evitar complejidad innecesaria y aprovechar los registros generados por los servicios de despliegue, automatización y hosting.

Las herramientas y prácticas consideradas fueron las siguientes:

| Herramienta o práctica | Uso dentro del monitoreo |
| --- | --- |
| **GitHub Actions** | Permite revisar el historial de ejecuciones de CI/CD, identificar workflows fallidos, consultar logs por etapa y validar qué commit originó cada ejecución. |
| **Firebase Hosting** | Permite verificar el estado de publicación del Frontend, la versión desplegada y la disponibilidad del sitio web para los usuarios finales. |
| **Azure Web App** | Permite observar el estado operativo del Back-End, revisar logs de aplicación, reinicios del servicio, errores de ejecución y disponibilidad del API. |
| **GitHub Container Registry** | Permite mantener trazabilidad de las imágenes Docker publicadas, sus etiquetas y la versión del Back-End asociada a cada despliegue. |
| **Secrets y variables de entorno** | Permiten mantener credenciales y configuraciones sensibles fuera del código fuente, reduciendo riesgos durante la operación. |
| **Revisión de logs** | Permite analizar errores técnicos, fallos de compilación, problemas de despliegue y respuestas inesperadas del sistema. |

Además de estas herramientas, se establecieron buenas prácticas operativas para que el monitoreo no dependa únicamente de una revisión manual aislada:

* Revisar el estado de los workflows después de cada merge hacia `main`.
* Confirmar que el Frontend publicado en Firebase Hosting corresponda a la última versión validada.
* Verificar que el Back-End responda correctamente después de cada despliegue.
* Mantener etiquetas de imagen Docker asociadas al commit de origen.
* Consultar logs cuando se detecte una falla en producción o en los pipelines.
* Documentar incidencias relevantes para relacionarlas con cambios recientes.

Con estas prácticas, el equipo mantiene una visión básica pero efectiva del comportamiento de Budgetly durante su operación.

### 7.4.2. Monitoring Pipeline Components

El pipeline de monitoreo se estructura como una secuencia de observación posterior al despliegue. Su propósito no es reemplazar los pipelines de CI/CD, sino complementarlos mediante la revisión constante de señales operativas.

Los principales componentes del pipeline de monitoreo son:

* **Event source**: corresponde al evento que inicia la observación, como un `push` a `main`, un despliegue en Firebase Hosting, la publicación de una imagen Docker en GHCR o una actualización del Back-End en Azure Web App.
* **Execution logs**: registros generados por GitHub Actions durante las etapas de instalación, compilación, pruebas, empaquetado, publicación y despliegue.
* **Deployment status**: resultado final de la publicación del Frontend y del Back-End, indicando si el despliegue fue exitoso o si requiere intervención.
* **Application logs**: registros generados por el Back-End durante la ejecución del servicio, incluyendo errores, excepciones o respuestas inesperadas.
* **Availability checks**: validaciones manuales o automatizables para confirmar que la aplicación web y los endpoints principales se encuentren disponibles.
* **Traceability records**: relación entre commit, workflow, artefacto generado, imagen Docker y versión desplegada.
* **Incident review**: análisis posterior de fallas para determinar causa, impacto y acción correctiva.

El flujo propuesto para el monitoreo continuo es el siguiente:

1. Se realiza un cambio en el repositorio y se integra a la rama `main`.
2. GitHub Actions ejecuta el pipeline correspondiente de publicación.
3. El Frontend se despliega en Firebase Hosting y el Back-End se publica mediante la imagen Docker y el servicio en la nube.
4. El equipo revisa el resultado del workflow y los logs generados.
5. Se valida la disponibilidad de la aplicación y de los endpoints principales del API.
6. Si se detecta una falla, se revisan los logs asociados y se identifica el commit o despliegue responsable.
7. Se registra la incidencia y se genera una corrección mediante un nuevo ciclo de desarrollo.

Esta estructura permite que el monitoreo esté conectado directamente con el ciclo DevOps, manteniendo trazabilidad desde el cambio de código hasta el comportamiento observado en producción.

### 7.4.3. Alerting Pipeline Components

El pipeline de alertas tiene como objetivo identificar condiciones anómalas que requieren atención del equipo. Una alerta debe generarse cuando el sistema presenta un fallo técnico, un despliegue incompleto o un comportamiento que pueda afectar la experiencia del usuario.

Para Budgetly, las alertas se organizaron en tres niveles de severidad:

| Nivel | Criterio | Acción esperada |
| --- | --- | --- |
| **Bajo** | Evento que no interrumpe el servicio, pero requiere revisión posterior. Por ejemplo, advertencias en logs o demoras leves en una ejecución. | Registrar la observación y revisarla durante el mantenimiento del proyecto. |
| **Medio** | Falla parcial que puede afectar una funcionalidad específica, como error en un endpoint no crítico o fallo en una etapa secundaria del pipeline. | Revisar logs, asignar responsable y preparar corrección en la siguiente iteración. |
| **Alto** | Falla que impide el despliegue, deja inaccesible el Frontend o afecta endpoints principales del Back-End. | Atender de forma prioritaria, detener promociones adicionales y aplicar corrección inmediata. |

Los componentes del pipeline de alertas son:

* **Alert trigger**: condición que inicia la alerta, como un workflow fallido, error de despliegue, caída del servicio o respuesta incorrecta del API.
* **Alert evaluator**: revisión de la severidad del evento para determinar si se trata de una advertencia, una falla parcial o una incidencia crítica.
* **Context collector**: recopilación de información necesaria para diagnosticar el problema, incluyendo commit, rama, workflow, hora de ejecución, logs y servicio afectado.
* **Responsible assignment**: asignación del incidente al integrante responsable según el componente impactado, ya sea Frontend, Back-End o configuración DevOps.
* **Corrective action**: corrección del problema mediante ajuste de código, configuración, secreto, workflow o infraestructura.
* **Resolution tracking**: verificación de que el nuevo despliegue corrige la falla y no introduce nuevos errores.

Algunos escenarios concretos de alerta para Budgetly son:

* El workflow de Frontend falla durante `npm ci` o `npm run build`.
* El workflow de Back-End falla durante `dotnet restore`, `dotnet build`, `dotnet test` o `dotnet publish`.
* La imagen Docker del Back-End no se publica correctamente en GHCR.
* El despliegue del Back-End en Azure Web App finaliza con error.
* El Frontend desplegado en Firebase Hosting no carga correctamente.
* Un endpoint principal del API responde con error después del despliegue.
* Se detectan errores repetidos en los logs del Back-End.

Este pipeline ayuda a que los errores no queden ocultos en los registros técnicos y puedan convertirse en acciones concretas de mantenimiento.

### 7.4.4. Notification Pipeline Components.

El pipeline de notificaciones define cómo se comunica una alerta al equipo y qué información mínima debe incluirse para facilitar la respuesta. Su propósito es evitar que cada integrante tenga que revisar manualmente todos los servicios y, en su lugar, recibir información clara cuando ocurra un evento relevante.

Los componentes principales del pipeline de notificaciones son:

* **Notification source**: servicio que genera el aviso, como GitHub Actions, Firebase Hosting, Azure Web App o el repositorio de imágenes en GHCR.
* **Notification channel**: medio por el cual el equipo recibe la información. Puede ser GitHub, correo electrónico asociado al repositorio, comentarios en Pull Requests, issues o canales internos de coordinación del equipo.
* **Notification payload**: contenido del mensaje enviado al equipo. Debe incluir el servicio afectado, el estado del evento, la fecha y hora, el enlace al workflow o log, el commit relacionado y una breve descripción del problema.
* **Recipient group**: integrantes que deben recibir la notificación según el tipo de incidencia. Por ejemplo, fallos de interfaz pueden asignarse al equipo Frontend y errores de API al equipo Back-End.
* **Acknowledgement**: confirmación de que un integrante revisó la alerta y asumió la responsabilidad de analizarla.
* **Follow-up**: seguimiento de la corrección mediante issue, commit, Pull Request o nuevo despliegue.

La información mínima recomendada para una notificación de incidencia es la siguiente:

| Campo | Descripción |
| --- | --- |
| **Servicio afectado** | Frontend, Back-End, pipeline de CI/CD, Firebase Hosting, Azure Web App o GHCR. |
| **Tipo de evento** | Error de build, error de test, despliegue fallido, caída del servicio, error de API o advertencia operativa. |
| **Severidad** | Bajo, medio o alto, según el impacto en la operación. |
| **Commit relacionado** | Hash o referencia del commit que originó el pipeline o despliegue. |
| **Responsable inicial** | Integrante encargado de revisar el incidente. |
| **Enlace de evidencia** | URL del workflow, log, despliegue o recurso donde se observa el problema. |
| **Acción requerida** | Revisión, corrección, rollback, nuevo despliegue o documentación de incidencia. |

Un ejemplo de notificación para el equipo sería:

> Se detectó una falla de severidad alta en el despliegue del Back-End. El workflow asociado al commit más reciente en `main` no completó la publicación hacia Azure Web App. Se requiere revisar los logs de GitHub Actions, validar la imagen Docker generada y confirmar la configuración de los secretos de despliegue.

Con este proceso, el equipo puede responder de manera ordenada a los problemas operativos, mantener comunicación clara y cerrar el ciclo de monitoreo mediante acciones correctivas verificables.

