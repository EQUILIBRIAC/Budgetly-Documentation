# Capítulo 5: Product Implementation, Validation & Deployment #

## 5.1. Software Configuration Management

En esta sección, se detalla la configuración de la tecnología a usar en el ciclo de vida de desarrollo del proyecto del curso.

### 5.1.1. Software Development Environment Configuration 

En esta sección, se explica los entornos en donde se decidió llevar a cabo el ciclo de vida de desarrollo de los productos de software relacionados al proyecto del curso.

* **Project Management**

  - En el aspecto de gestión y desarrollo del ciclo de vida del proyecto se utilizó la aplicación DISCORD y GOOGLE MEET para las reuniones de grupo en las cuales se conversan sobre temas relacionados a avances y corrección de aspectos del proyecto. Además, para la documentación del proyecto, se utilizó el formato Mark Down en un repositorio de GitHub para el control de versiones del informe.

* **Requirements Management**

  - Para el manejo de los requisitos (historias de usuario, product backlog, sprint backlog) del producto, se utilizó TRELLO la cual es una herramienta ideal para gestionar proyectos. Además, usando esta herramienta, se puede organizar un product backlog, ya que permite estructurar tareas visualmente en un tablero. También puedes crear listas que representen etapas del flujo de trabajo, y en cada lista añadir tarjetas que describan las user stories o tareas individuales. Estas tarjetas permiten detallar información clave, como prioridades, etiquetas de color, descripciones y checklists, facilitando así el seguimiento y la colaboración del equipo.

* **Product UX/UI Design**

  - Para el desarrollo de plantillas de los user persona, de los Impact Maps y los User Journey Maps se utilizó la aplicación UXPRESSIA la cual es una plataforma especializada en la creación de mapas de experiencia del usuario ofreciendo una interfaz enfocada exclusivamente en UX que facilita la estructuración clara y profesional de estos elementos. Destaca por sus plantillas personalizables, la posibilidad de añadir datos reales, imágenes y métricas, y por permitir la colaboración en tiempo real.

  - Para la creación del Lean UX Canvas se utilizó la aplicación de diseño CANVA. Esta aplicación es una herramienta versátil para crear diversos diseños. Canva facilita la colaboración del equipo y la exportación de los proyectos en archivo PNG o PDF, manteniendo el proceso creativo ordenado y atractivo. Para los Journey Mapping, Empathy Mapping, entre otros mapas,  se decidió utilizar Miro. Esta aplicación permite una colaboración en tiempo real entre equipos, ofrece una interfaz visual e intuitiva, y cuenta con plantillas prediseñadas que agilizan el proceso sin perder calidad metodológica.

  - Finalmente, para el desarrollo de interfaces de usuario (wireframes, mockups y prototipos de aplicación) se decidió utilizar FIGMA. Esta es una herramienta que facilita el diseño de interfaces, permitiéndonos trabajar con colores, imágenes, formas, y otros elementos visuales para crear nuestra aplicación. Nos ofrece la posibilidad de probar diversos modelos de dispositivos. Además, esta plataforma será clave en la creación de nuestro prototipo, ya que brinda una simulación interactiva que permite visualizar y experimentar el proyecto desde la perspectiva del usuario.

* **Software Development**

  - Para el desarrollo del producto de software correspondiente al Landing Page, se utilizarán dos aplicaciones, las cuales son GITHUB y JETBRAINS WEBSTORM. La primera ayuda al equipo a gestionar de manera correcta los avances colaborativos del proyecto. Por otro lado, JetBrains WebStorm ayudará a trabajar el proyecto con lenguajes como HTML5, CSS y JavaScript para el desarrollo del landing page.
 
* **Software Testing**

  - Las pruebas de la Landing Page se realizarán mediante uso del navegador web GOOGLE para verificar que el diseño del mismo cumple con aspectos como el diseño responsivo en cualquier dispositivo desde el que se acceda al landing page del proyecto. Además, para visualizar que se han implementado correctamente elementos visuales que deben aparecer en las distintas secciones de la página.

* **Software Deployment**

  - Para los despliegues de la Landing Page se uso el servicio web de GITHUB PAGES, este servicio se especializa en el despliegue de sitios web staticos directamente desde un repositorio creado en GitHub.

### 5.1.2. Source Code Management ###

En esta sección, se describen los medios y esquemas de organización para gestionar de manera efectiva los archivos de proyecto relacionados a Landing Page, Web Services y Frontend Web Applications. En el caso de los repositorios, se usará GitHub para almacenar los archivos. Además, se implementará GitFlow. Esta función de GitHub ayudará al equipo, gracias a las ramas de características de lanzamiento, a poder trabajar paralelamente en el proyecto y a tomar el control de versiones de avance del proyecto.

#### **5.1.2.1. Repositorios**

A continuación, se adjuntan los enlaces para acceder a los repositorios donde se almacenarán los archivos y avances de proyecto relacionados al Landing Page, Front-End y Back-End Application.

* **Landing Page: https://github.com/EQUILIBRIAC/Budgetly-LandingPage**
* **Front End: https://github.com/EQUILIBRIAC/Budgetly-FrontEnd**
* **Back End: https://github.com/EQUILIBRIAC/Budgetly-BackEnd**
* **Mobile App: https://github.com/EQUILIBRIAC/Budgetly-MobileApp**

**5.1.2.2. GitFlow**

Para el desarrollo de este proyecto, GITFLOW ayudará al equipo de desarrollo a gestionar de manera efectiva el proyecto en su ciclo de vida. En general, GITHUB ayudará a facilitar el desarrollo del proyecto para el equipo ya que es más sencillo desarrollar trabajos en equipo en los repositorios de los archivos de proyecto.

##### **5.1.2.2.1. Main Branches**

* **Main Branch**   
  Llamada también rama principal del proyecto, esta es la rama predeterminada del proyecto creado en el repositorio. Esta rama representa el historial del proyecto lo que ayuda a llevar el control de versiones del mismo.
    
* **Develop Branch**  
  Llamada también rama de desarrollo del proyecto. Esta rama es una bifurcación de código original del proyecto para definir nuevos rumbos respecto del proyecto original que servirá para evaluar variaciones del proyecto para su evolución. Además, ayudan a incorporar nuevas funciones al proyecto.

##### **5.1.2.2.2. Supporting Branches**

* **Feature Branch**  
  También llamada rama de característica del proyecto, es una rama de desarrollo que ayuda a incorporar nuevas funciones al proyecto en desarrollo. Además, permite el aislamiento de la función agregada y que varios colaboradores puedan trabajar simultáneamente en dicha funcionalidad.

* **Release Branch**  
  También llamada rama de lanzamiento del proyecto, es una versión de código del proyecto que se usa para empezar un nuevo ciclo de lanzamiento del producto de software. Además, en esta rama se pueden realizar correcciones de errores de la versión pasada del proyecto. Finalmente, una vez terminada con esta rama, se suma a la rama principal del proyecto y se le asigna un nuevo número de versión de proyecto.
    
* **Hotfix Branch**  
  También llamada rama de corrección del proyecto, es una rama que permite dar mantenimiento al código del proyecto. Se utiliza principalmente para arreglar errores en alguna sección del producto de software de manera rápida.

#### **5.1.2.3. Release Versioning Conventions**

Para la nomenclatura de los lanzamientos de la Landing Page, se utilizará Semantic Versioning que consta de tres partes para describir cambios mayores, cambios menores y parches para corrección de bugs, según la siguiente estructura:

* Número principal: Incrementa cuando se realiza un cambio mayor y significativo al proyecto.  
* Número secundario: Incrementa cuando se realiza un cambio menor al proyecto como arreglo de errores o agregación de características.  
* Número terciario: Incrementa cuando se realiza un parche al proyecto como una corrección de bugs o errores visuales.

#### **5.1.2.3. Commits Conventions**

Para los textos de mensajes en los *‘commits’* del proyecto en Git, se utilizará Conventional Commits. Estos son mensajes de confirmación que son fáciles de entender por los colaboradores del proyecto. Finalmente, estos mensajes siguen la siguiente estructura:

<!-- Commits-->
<p align="center">
  <img src="https://i.imgur.com/vfirypa.png" alt="Commits">
</p>

La sección *‘type’* indica el tipo de mensaje de confirmación que se usará. A continuación, la sección *‘description’* indica la descripción que se le agrega al mensaje de confirmación, por ejemplo, una característica agregada. Además, la sección *‘body’* incluye una descripción más detallada del cambio aplicado al proyecto.  
Luego, se tienen distintos tipos de mensajes de confirmación. Por ejemplo, se tiene el mensaje tipo *‘fix’* que incluye una corrección al proyecto. Utilizar este tipo conlleva aumentar el número terciario de la versión del proyecto (por ejemplo, de 1.0.0. a 1.0.1.). Después, utilizar el mensaje de tipo *‘feat’* conlleva agregar una nueva función a la aplicación, por lo tanto, se debe aumentar el número secundario de la versión (por ejemplo, de 1.0.0. a 1.1.0.). Finalmente, si se agrega una sección de tipo ‘BREAKING CHANGE’ indicaría que las versiones anteriores del proyecto dejarán de ser compatibles entre sí, lo que conlleva un cambio significativo y el aumento del número principal de la versión (por ejemplo, de 1.0.0. a 2.0.0.).

### 5.1.3. Source Code Style Guide & Conventions ###

En esta sección, se definen las referencias que se usaron para adoptar estrategias de nomenclatura de elementos de programación en los lenguajes que se usarán para la solución (HTML, CSS, JavaScript, y C\#). En general, la nomenclatura de los archivos y secciones en la programación se hará en inglés.

* **Nomenclatura en HTML:**  
  Para la codificación del proyecto en HTML, se utilizará el artículo *“HTML Style Guide and Coding Conventions”.* Este artículo contiene información útil y necesaria para conocer cómo debe ser la nomenclatura de los diversos aspectos que incluye la programación en HTML como si se debe escribir en minúsculas o mayúsculas las secciones del cuerpo del documento. A continuación se adjunta el enlace para acceder al artículo de referencia: [https://www.w3schools.com/html/html5\_syntax.asp](https://www.w3schools.com/html/html5_syntax.asp)   
  Finalmente, se aplicará el contenido del artículo para la nomenclatura en HTML para la landing page de StockSip a desarrollar.

* **Nomenclatura en CSS:**  
  Para la codificación del proyecto en Cascading Style Sheets (CSS), se utilizará el artículo *“Google HTML/CSS Style Guide”.* Este artículo contiene información útil y necesaria para conocer cómo debe ser la nomenclatura de los diversos aspectos que incluye la programación en CSS como capitalización en código de colores, referencias a imágenes, etc. A continuación se adjunta el enlace para acceder al artículo de referencia: [https://google.github.io/styleguide/htmlcssguide.html](https://google.github.io/styleguide/htmlcssguide.html)   
  Finalmente, se aplicará el contenido del artículo para la nomenclatura en CSS para el estilo de colores que se quiere agregar al landing page de Harmonix a desarrollar.

* **Nomenclatura en JavaScript:**  
  Para la codificación del proyecto en JavaScript, se utilizará el artículo *“Google JavaScript Style Guide”.* Este artículo contiene información útil y necesaria para conocer cómo debe ser la nomenclatura de los diversos aspectos que conforman un proyecto desarrollado en JavaScript, según los lineamientos establecidos por Google.Se trata de la guía de estilo oficial de JavaScript de Google, un documento detallado que establece una serie de convenciones para escribir código JavaScript limpio, coherente y fácil de mantener, especialmente en equipos de trabajo. A continuación se adjunta el enlace para acceder al artículo de referencia: [https://google.github.io/styleguide/jsguide.html](https://google.github.io/styleguide/jsguide.html)  
  Finalmente, se aplicará el contenido del artículo para el Web Services de Harmonix.  
    
* **Nomenclatura en Vue:**  
  Para la codificación del proyecto en Vue, se utilizará el artículo *“Vue Style Guide”.* Este artículo contiene información útil y necesaria para conocer cómo debe ser la nomenclatura de los diversos aspectos que conforman un proyecto desarrollado con Vue.js 2\. Se trata de la guía de estilo oficial de Vue 2, en la cual se detallan las convenciones recomendadas para escribir código claro, consistente y fácil de mantener. Esta guía organiza sus recomendaciones en diferentes niveles de prioridad; reglas esenciales, reglas fuertemente recomendadas, reglas recomendadas, reglas de uso con precaución y reglas estrictamente opcionales. A continuación se adjunta el enlace para acceder al artículo de referencia: [https://v2.vuejs.org/v2/style-guide/?redirect=true](https://v2.vuejs.org/v2/style-guide/?redirect=true)  
  Finalmente, se aplicará el contenido del artículo para el Frontend Applications de Harmonix.  
    
* **Nomenclatura en C\#:**  
  Para la codificación del proyecto en C\#, se utilizará el artículo *“C\# Coding Conventions”.* Este artículo contiene información útil y necesaria para conocer cómo debe ser la nomenclatura de los diversos aspectos que conforman un proyecto desarrollado en C\#, según las convenciones oficiales de codificación establecidas por Microsoft. Se trata de la guía de convenciones de estilo de código para C\# publicada por Microsoft, la cual proporciona una serie de recomendaciones para escribir código claro, coherente y mantenible en aplicaciones .NET. A continuación se adjunta el enlace para acceder al artículo de referencia:  [https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)  
  Finalmente, se aplicará el contenido del artículo para el Web Services de Harmonix.

* **Nomenclatura en ASP.NET:**  
  Para la codificación del proyecto en ASP.NET, se utilizará el artículo *“Microsoft ASP.NET Core Coding Guidelines”.* Este artículo contiene información útil y necesaria para conocer cómo debe ser la nomenclatura y el estilo de los diversos aspectos que conforman un proyecto desarrollado con ASP.NET Core. Se trata de la guía de ingeniería oficial del equipo de ASP.NET Core, en la cual se detallan las convenciones recomendadas para escribir código claro, consistente y fácil de mantener. Esta guía organiza sus recomendaciones en distintos apartados que abarcan desde el formato del código, el uso de tipos y palabras clave, la compatibilidad multiplataforma, hasta el control de cambios en versiones del framework. Cada sección tiene como propósito establecer prácticas que favorezcan la legibilidad, el rendimiento, y la calidad del desarrollo colaborativo a gran escala. A continuación se adjunta el enlace para acceder al artículo de referencia:  [https://github.com/dotnet/aspnetcore/wiki/Engineering-guidelines#codingguidelines](https://github.com/dotnet/aspnetcore/wiki/Engineering-guidelines#codingguidelines)  
  Finalmente, se aplicará el contenido del artículo para el Web Services de Harmonix

* **Nomenclatura para RESTful API:**

  Para la nomenclatura de endpoints a implementar en la aplicación Back-End, se usó el artículo *"REST API URI Naming Conventions and Best Practices".* Este mismo contiene información sobre consejos y buenas prácticas al momento de nombrar correctamente a los endpoints en una aplicación back-end que use el esquema REST. A continuación se adjunta el enlace para acceder al artículo de referencia: [https://restfulapi.net/resource-naming/](https://restfulapi.net/resource-naming/).

* **Nomenclatura en MySQL:**

  Para la nomenclatura de objetos en una base de datos relacional usando MySQL, se usó el artículo *"MYSQL Naming Conventions"* como base para la correcta nomenclatura de tablas y columnas. A continuación, se adjunta el enlace para acceder al artículo de referencia: [(https://medium.com/@centizennationwide/mysql-naming-conventions-e3a6f6219efe)](https://medium.com/@centizennationwide/mysql-naming-conventions-e3a6f6219efe) .


### 5.1.4. Software Deployment Configuration ###

En esta sección, se especifica la configuración para realizar el despliegue de la solución en el repositorio. Para realizar esto, se usó GITHUB PAGES para desplegar el landing page.

## Sitio web estático ##
* **Paso 1: Creación del repositorio**  
  Como primer paso, se debe crear el repositorio en GitHub que será el lugar donde se aloja todo lo relacionado al Landing Page.

<p align="center">
  <img src="https://i.imgur.com/qpvh4Zt.png" alt="Repository creation">
</p>

* **Paso 2: Carga de archivos necesarios**  
  Como segundo paso, se importan todos los archivos necesarios para el desarrollo de la landing page como imágenes, archivos HTML, CSS y JavaScript.

<p align="center">
  <img src="https://i.imgur.com/KC9BrHf.png" alt="File organization">
</p>

* **Paso 3: Preparar el lanzamiento**  
  Como tercer paso, se juntan todas las características del proyecto en una sola para verificar el correcto funcionamiento de cada una. Luego, se envía todo a la rama principal donde se encuentra, por defecto, el proyecto.

<p align="center">
  <img src="https://i.imgur.com/LZxoPlI.png" alt="Master branch">
</p>

* **Paso 4: Desplegar la Landing Page**  
  Como cuarto paso, cuando todo se encuentre en la rama principal, se accede a la sección Configuración del repositorio, luego, se selecciona la opción “GitHub Pages” y se seleccionará la rama principal que es la que se desea desplegar.

<p align="center">
  <img src="https://i.imgur.com/9dbeFRr.png" alt="List of deployments.">
</p>

* **Paso 5: Acceder al Landing Page**  
  Como paso final, el entorno otorgará un enlace para poder acceder al proyecto desplegado.
  
<p align="center">
  <img src="https://imgur.com/MJt1dLo.png" alt="View of the Landing Page">
</p>

## 5.2 Product Implementation & Deployment


### 5.2.1 Sprint Backlogs

|**User Story Id**|**Title**|**Task Id**|**Task Title**|**Description**|**Estimation (Hours)**|**Assigned To**|**Status (To-do/In-Process/To-Review/Done)**|
|---|---|---|---|---|---|---|---|
|US31|Visualizar información general sobre la solución desde la landing page|T1|Información general|Desarrollo de la sección con información introductoria del proyecto|2|Carlos Guimaraes|Done|
|US32|Conocer las funciones principales del sistema|T1|Funciones principales|Diseño y desarrollo de la sección que explica las funcionalidades clave|2|Camila Huamani|Done|
|US33|Explorar beneficios del sistema|T1|Beneficios|Desarrollo de la sección que muestre los beneficios de la solución|2|Angelo Solano|Done|
|US34|Ver ejemplos o simulaciones del funcionamiento|T1|Ejemplos y simulaciones|Implementación de ejemplos visuales del funcionamiento del sistema|3|Martin Gonzales|Done|
|US35|Acceder fácilmente al registro o login|T1|Botones de acceso|Diseño e implementación de botones visibles para registro y login|1|Renzo Uribe|Done|
|TS18|Documentar los pasos de despliegue|T1|Documentación de despliegue|Redacción de la guía para desplegar nuevas versiones|2|Carlos Guimaraes|Done|
|TS19|Habilitar monitoreo básico del sistema|T1|Monitoreo básico|Configuración inicial para logs y monitoreo|3|Camila Huamani|Done|

### 5.2.2 Implemented Landing Page Evidence

Como parte de la revisión del sprint, se presentan las evidencias de ejecución relacionadas con el desarrollo del Landing Page de Budgetly. La implementación se realizó empleando HTML, CSS y JavaScript, asegurando una estructura semántica clara, un diseño visual coherente con las guías de estilo y funcionalidades interactivas que mejoran la experiencia de usuario.

Además de las capturas del código implementado, se incluye un video demostrativo donde se explica y evidencia la navegación lograda durante este sprint, así como el flujo de interacción principal que se validó.

Video del landing page: bit.ly/4nxTsa6  

Link: https://equilibriac.github.io/Budgetly-LandingPage/

- Home:<br>

   ![Alt Text](https://i.imgur.com/ZR6EaIy.png)
<br>

- About Us:<br>

   <p align="center">
  <img src="https://imgur.com/tdh3OY4.png">
  </p>

  <br>
  
- Services: <br>

  ![Alt Text](https://i.imgur.com/tceyH5o.png)
  
  <br>
- How does it work?: <br>

  ![Alt Text](https://i.imgur.com/jASoXbj.png)

  <br>

- Prices: <br>

  ![Alt Text](https://i.imgur.com/YPBkCel.png)

  <br>

- Contact us: <br>

  ![Alt Text](https://i.imgur.com/7rnTMg8.png)

  <br>

### 5.2.3 Implemented Frontend-Web Application Evidence

Se desarrolló e implementó el **Frontend Web Application de Budgetly**, utilizando Vue.js. Se construyeron las principales vistas del sistema, incluyendo dashboards para los distintos tipos de usuario, gestión de hogares, miembros, gastos y contribuciones.

El sistema fue desplegado públicamente, permitiendo validar su funcionamiento y navegación.

- Link: https://budgetly-exp-app.web.app/

**Evidencias de la interfaz:**

- Representative-Dashboard:

<p align="center"> <img src="https://i.imgur.com/O7CAEZi.png"> </p>

- Representative-Households:

<p align="center"> <img src="https://i.imgur.com/DKj8F74.png"> </p>

- Representative-Members:

<p align="center"> <img src="https://i.imgur.com/yKfqLuo.png"> </p>

- Representative-Expenses:

<p align="center"> <img src="https://i.imgur.com/CJfGSuJ.png"> </p>

- Representative-Contributions:

<p align="center"> <img src="https://i.imgur.com/cLadc0H.png"> </p>

- Representative-Configuration:

<p align="center"> <img src="https://i.imgur.com/0L6H7A2.png"> </p>

- Member-HomeState:

<p align="center"> <img src="https://i.imgur.com/ISCzrHc.png"> </p>

- Member-FindHome:

<p align="center"> <img src="https://i.imgur.com/qEaJzkc.png"> </p>

### 5.2.4 Acuerdo de Servicio - SaaS

Esta sección describe el **Acuerdo de Servicio – SaaS (SaaS Agreement)** aplicable a los usuarios de **Budgetly**, estableciendo derechos, obligaciones y restricciones para garantizar el uso transparente y responsable de la plataforma. Su contenido debe publicarse en la landing page dentro de la sección **“Terms and Conditions”**.

#### 5.2.4.1. Información general y aceptación

**Budgetly** es una plataforma digital ofrecida bajo el modelo **Software as a Service (SaaS)**, orientada a la gestión colaborativa de finanzas del hogar mediante registro de usuarios, creación/unión a hogares, registro de gastos y contribuciones.

Al **registrarse, acceder o utilizar** Budgetly, el usuario declara haber leído y aceptado estos Términos y Condiciones. Si el usuario no está de acuerdo, debe abstenerse de usar el servicio.

#### 5.2.4.2. Definiciones

- **Plataforma / Servicio:** Budgetly (Landing Page, Web Application y API asociada).
- **Proveedor:** Startup **Equilibria** (equipo del proyecto Budgetly).
- **Usuario:** Persona que crea una cuenta y utiliza la plataforma.
- **Hogar (Household):** Espacio de colaboración donde se agrupan miembros para administrar gastos y aportes.
- **Representante:** Usuario que crea y gestiona el Hogar (administración general).
- **Miembro:** Usuario que se une a un Hogar y participa en la gestión/consulta según permisos.

#### 5.2.4.3. Elegibilidad y registro

- El usuario declara contar con **capacidad legal** para aceptar este acuerdo.
- El usuario se compromete a proporcionar información **veraz, completa y actualizada** durante el registro y uso del servicio.
- El usuario es responsable de mantener la **confidencialidad de sus credenciales** (correo/contraseña) y de las actividades realizadas desde su cuenta.

#### 5.2.4.4. Uso del servicio (alcance funcional)

Budgetly permite, entre otras funciones:
- Registro e inicio de sesión de usuarios.
- Creación y administración de un Hogar por un Representante.
- Unión de Miembros a un Hogar (por ejemplo, mediante un identificador/ID).
- Registro, consulta y seguimiento de gastos del Hogar.
- Registro y seguimiento de contribuciones/aportes asociados a gastos.

El servicio se ofrece con fines de organización financiera colaborativa y **no constituye asesoría financiera, contable ni legal**.

#### 5.2.4.5. Responsabilidades por rol

**Representante del Hogar**
- Administrar la configuración del hogar, miembros y registros que correspondan.
- Promover un uso adecuado de la plataforma dentro del hogar.
- Verificar que los datos ingresados (gastos, fechas, montos, descripciones) sean correctos.

**Miembro del Hogar**
- Ingresar información correcta cuando corresponda (por ejemplo, aportes o datos solicitados).
- Respetar la dinámica del hogar y las decisiones organizativas acordadas con el Representante y otros miembros.

Budgetly es una herramienta de apoyo: los acuerdos internos del hogar respecto a pagos y responsabilidades se definen entre sus integrantes.

#### 5.2.4.6. Conductas prohibidas (restricciones)

Se prohíbe expresamente:
- Usar la plataforma con fines ilícitos o que vulneren derechos de terceros.
- Intentar acceder sin autorización a hogares ajenos o información de otros usuarios.
- Realizar acciones que afecten la disponibilidad o seguridad del servicio (ataques, explotación de vulnerabilidades, automatizaciones abusivas, etc.).
- Introducir contenido malicioso (malware) o manipular el funcionamiento del sistema.
- Copiar, modificar, distribuir o realizar ingeniería inversa del servicio, salvo autorización expresa del Proveedor.

El Proveedor podrá **suspender o restringir** cuentas ante indicios razonables de incumplimiento.

#### 5.2.4.7. Datos, privacidad y confidencialidad (visión general)

- Los datos ingresados por el usuario (por ejemplo: gastos, aportes, descripciones) se utilizan para la operación del servicio y la experiencia colaborativa del Hogar.
- El usuario entiende que los datos del Hogar podrán ser **visibles para los miembros** del mismo Hogar según el rol y el diseño del sistema.
- Se recomienda no ingresar información altamente sensible innecesaria (por ejemplo, contraseñas, datos de tarjetas, etc.).

*(Nota: Para cumplimiento normativo completo, debe complementarse con una “Privacy Policy” publicada en el website.)*

#### 5.2.4.8. Disponibilidad del servicio y cambios

- El servicio puede presentar interrupciones por mantenimiento, actualizaciones o fallos de infraestructura de terceros (por ejemplo, proveedor cloud).
- El Proveedor puede realizar cambios en la plataforma (mejoras, correcciones, ajustes de interfaz), procurando mantener la continuidad del servicio.

#### 5.2.4.9. Propiedad intelectual

Budgetly, su interfaz, documentación y componentes asociados son propiedad del Proveedor o se utilizan bajo licencias correspondientes. El usuario recibe un derecho de uso **limitado, no exclusivo y revocable** para utilizar la plataforma conforme a este acuerdo.

#### 5.2.4.10. Limitación de responsabilidad

En la máxima medida permitida por la legislación aplicable:
- Budgetly se ofrece **“tal cual”** y **según disponibilidad**.
- El Proveedor no se responsabiliza por decisiones financieras tomadas únicamente en base a la información mostrada si los datos ingresados por los usuarios son erróneos o incompletos.
- El Proveedor no será responsable por daños indirectos, pérdida de datos, pérdida de oportunidades o conflictos entre miembros del hogar derivados del uso de la plataforma.

#### 5.2.4.11. Terminación o suspensión de la cuenta

El usuario puede dejar de usar el servicio en cualquier momento. El Proveedor podrá suspender o limitar el acceso si:
- Se incumplen estos términos,
- Se detecta un riesgo de seguridad,
- Se realizan actividades prohibidas o que afecten al servicio o a terceros.

#### 5.2.4.12. Ley aplicable y jurisdicción (Perú)

Este Acuerdo se rige por las leyes de la **República del Perú**. Cualquier controversia será sometida a los jueces y tribunales competentes del Perú, según corresponda.

#### 5.2.4.13. Canal de contacto

Para consultas, soporte o reportes relacionados al servicio, se habilitará un canal de contacto visible en el website (por ejemplo, formulario “Contact us” o correo oficial del equipo).

### 5.2.5 Implemented Native-Mobile Application Evidence

Se desarrolló e implementó el **Native-Mobile Application de Budgetly**, utilizando Dart Fluttter Se construyeron las principales vistas del sistema, incluyendo dashboards para los distintos tipos de usuario, gestión de hogares, miembros, gastos y contribuciones.

**Evidencias de la interfaz:**

- Register:

<p align="center"> <img src="https://i.imgur.com/KWYGdoj.png"> </p>

- Login:

<p align="center"> <img src="https://i.imgur.com/quzs3bc.png"> </p>
                     
- Representative-Dashboard:

<p align="center"> <img src="https://i.imgur.com/rpN70L3.png"> </p>

- Representative-Households:

<p align="center"> <img src="https://i.imgur.com/iIHCVUN.png"> </p>

- Representative-Members:

<p align="center"> <img src="https://i.imgur.com/ut59ZMU.png"> </p>

- Representative-Contributions:

<p align="center"> <img src="https://i.imgur.com/1rP9rZU.png"> </p>

- Member-Dashboard:

<p align="center"> <img src="https://i.imgur.com/ZFJ9aGL.png"> </p>

- Member-Contributions:

<p align="center"> <img src="https://i.imgur.com/llqYwAq.png"> </p>

- Member-FindHome:

<p align="center"> <img src="https://i.imgur.com/IjxMqij.png"> </p>

- Member-Setting:

<p align="center"> <img src="https://i.imgur.com/5Eie57Q.png"> </p>

### 5.2.6 Implemented RESTful API and/or Serverless Backend Evidence

Se desarrolló el **Backend de Budgetly**, implementando los principales endpoints REST para la gestión del sistema.

Se integraron funcionalidades como:

- Autenticación de usuarios (login y registro con JWT).
- Gestión de usuarios y perfiles.
- Administración de hogares.
- Registro y gestión de gastos (bills).
- Creación y seguimiento de contribuciones.

El backend fue desplegado en la nube mediante Azure, asegurando su disponibilidad pública: https://budgetly-api-dev-dxcfedfvdxeebad5.chilecentral-01.azurewebsites.net/swagger/index.html


<p align="center"> <img src="https://i.imgur.com/UxzJwaz.png"> </p>

### 5.2.7 RESTful API documentation

A continuación, se resumen los principales endpoints implementados en Budgetly:

 <br> **Authentication:**
| **Endpoint**                     | **Acción implementada** | **Método HTTP** | **Parámetros**                    | **Ejemplo Request**                                                | **Ejemplo Response**                                                            |
| -------------------------------- | ----------------------- | --------------- | --------------------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------- |
| `/api/v1/authentication/sign-in` | Iniciar sesión          | POST            | body: `{ email, password }`       | `{ "email": "test@gmail.com", "password": "123456" }`              | `{ "token": "jwt_token", "user": { ... } }`                                     |
| `/api/v1/authentication/sign-up` | Registrar usuario       | POST            | body: `{ name, email, password }` | `{ "name": "Jose", "email": "x@gmail.com", "password": "123456" }` | `{ "id": "uuid", "email": "x@gmail.com", "createdAt": "2025-12-03T00:00:00Z" }` |


 <br> **User:**
| **Endpoint**                                 | **Acción implementada**                  | **Método HTTP** | **Parámetros**                                               | **Ejemplo Request**                                   | **Ejemplo Response**                                       |
| -------------------------------------------- | ---------------------------------------- | --------------- | ------------------------------------------------------------ | ----------------------------------------------------- | ---------------------------------------------------------- |
| `/api/v1/user/user/{id}`                     | Obtener usuario por Id                   | GET             | path: `{ id }`                                               | `GET /api/v1/user/user/1`                             | `{ "id": 1, "name": "Jose", "email": "test@gmail.com" }`   |
| `/api/v1/user`                               | Listar todos los usuarios                | GET             | —                                                            | `GET /api/v1/user`                                    | `[ { "id": 1, "name": "Jose", "email": "..." }, ... ]`     |
| `/api/v1/user/householdid/{mainHouseHoldId}` | Obtener usuarios por Household principal | GET             | path: `{ mainHouseHoldId }`                                  | `GET /api/v1/user/householdid/household-123`          | `[ { "id": 1, "mainHouseHoldId": "household-123" }, ... ]` |
| `/api/v1/user/byemail/{emailAddress}`        | Actualizar usuario por email             | PUT             | path: `{ emailAddress }`, body: parcial `UpdateUserResource` | `PUT /api/v1/user/byemail/test@gmail.com` + body JSON | `{ "id": 1, "email": "test@gmail.com", "updated": true }`  |
| `/api/v1/user/byemail/{email}`               | Eliminar usuario por email               | DELETE          | path: `{ email }`                                            | `DELETE /api/v1/user/byemail/test@gmail.com`          | `{ "deleted": true }`                                      |

<br> **User Income:**
| **Endpoint**                            | **Acción implementada**      | **Método HTTP** | **Parámetros**                                           | **Ejemplo Request**                                                           | **Ejemplo Response**                                                        |
| --------------------------------------- | ---------------------------- | --------------- | -------------------------------------------------------- | ----------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| `/api/v1/user_income`                   | Crear ingreso de usuario     | POST            | body: `{ userId, amount, source, frequency }`            | `{ "userId": 1, "amount": 1500, "source": "Salary", "frequency": "monthly" }` | `{ "id": "income-1", "userId": 1, "amount": 1500 }`                         |
| `/api/v1/user_income/{id}`              | Obtener ingreso por Id       | GET             | path: `{ id }`                                           | `GET /api/v1/user_income/income-1`                                            | `{ "id": "income-1", "userId": 1, "amount": 1500, "frequency": "monthly" }` |
| `/api/v1/user_income/byuserid/{userId}` | Obtener ingresos por usuario | GET             | path: `{ userId }`                                       | `GET /api/v1/user_income/byuserid/1`                                          | `[ { "id": "income-1", "userId": 1, "amount": 1500 }, ... ]`                |
| `/api/v1/user_income/byid/{id}`         | Actualizar ingreso por Id    | PUT             | path: `{ id }`, body: parcial `UpdateUserIncomeResource` | `PUT /api/v1/user_income/byid/income-1` + body JSON                           | `{ "id": "income-1", "updated": true }`                                     |

<br> **Contribution:**

| **Endpoint**                                       | **Acción implementada**              | **Método HTTP** | **Parámetros**                                             | **Ejemplo Request**                                                                  | **Ejemplo Response**                                       |
| -------------------------------------------------- | ------------------------------------ | --------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------------ | ---------------------------------------------------------- |
| `/api/v1/contribution`                             | Listar todas las contribuciones      | GET             | —                                                          | `GET /api/v1/contribution`                                                           | `[ { "id": "contrib-1", "amount": 100 }, ... ]`            |
| `/api/v1/contribution`                             | Crear contribución                   | POST            | body: `{ billId, householdId, amount, description }`       | `{ "billId": "bill-1", "householdId": "hh-1", "amount": 100, "description": "Luz" }` | `{ "id": "contrib-1", "billId": "bill-1", "amount": 100 }` |
| `/api/v1/contribution/{id}`                        | Obtener contribución por Id          | GET             | path: `{ id }`                                             | `GET /api/v1/contribution/contrib-1`                                                 | `{ "id": "contrib-1", "billId": "bill-1", "amount": 100 }` |
| `/api/v1/contribution/{id}`                        | Eliminar contribución                | DELETE          | path: `{ id }`                                             | `DELETE /api/v1/contribution/contrib-1`                                              | `{ "deleted": true }`                                      |
| `/api/v1/contribution/bybillid/{billId}`           | Obtener contribuciones por Bill      | GET             | path: `{ billId }`                                         | `GET /api/v1/contribution/bybillid/bill-1`                                           | `[ { "id": "contrib-1", "billId": "bill-1" }, ... ]`       |
| `/api/v1/contribution/byhouseholdid/{householdId}` | Obtener contribuciones por Household | GET             | path: `{ householdId }`                                    | `GET /api/v1/contribution/byhouseholdid/hh-1`                                        | `[ { "id": "contrib-1", "householdId": "hh-1" }, ... ]`    |
| `/api/v1/contribution/byid/{id}`                   | Actualizar contribución por Id       | PUT             | path: `{ id }`, body: parcial `UpdateContributionResource` | `PUT /api/v1/contribution/byid/contrib-1` + body JSON                                | `{ "id": "contrib-1", "updated": true }`                   |


 <br> **Bills:**

| **Endpoint**                              | **Acción implementada**           | **Método HTTP** | **Parámetros**                                     | **Ejemplo Request**                         | **Ejemplo Response**                                                |
| ----------------------------------------- | --------------------------------- | --------------- | -------------------------------------------------- | ------------------------------------------- | ------------------------------------------------------------------- |
| `/api/v1/bills`                           | Listar todas las facturas (bills) | GET             | —                                                  | `GET /api/v1/bills`                         | `[ { "id": "bill-1", "householdId": "hh-1", "amount": 200 }, ... ]` |
| `/api/v1/bills/byhousehold/{householdId}` | Obtener Bills por Household       | GET             | path: `{ householdId }`                            | `GET /api/v1/bills/byhousehold/hh-1`        | `[ { "id": "bill-1", "householdId": "hh-1" }, ... ]`                |
| `/api/v1/bills/byid/{id}`                 | Actualizar Bill por Id            | PUT             | path: `{ id }`, body: parcial `UpdateBillResource` | `PUT /api/v1/bills/byid/bill-1` + body JSON | `{ "id": "bill-1", "updated": true }`                               |
| `/api/v1/bills/{id}`                      | Eliminar Bill                     | DELETE          | path: `{ id }`                                     | `DELETE /api/v1/bills/bill-1`               | `{ "deleted": true }`                                               |


 <br> **HouseHolds:**

| **Endpoint**              | **Acción implementada**  | **Método HTTP** | **Parámetros**                                            | **Ejemplo Request**                                                                                       | **Ejemplo Response**                                           |
| ------------------------- | ------------------------ | --------------- | --------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| `/api/v1/house_hold/{Id}` | Obtener Household por Id | GET             | path: `{ Id }`                                            | `GET /api/v1/house_hold/hh-1`                                                                             | `{ "id": "hh-1", "name": "Casa Vallejos", "currency": "PEN" }` |
| `/api/v1/house_hold`      | Crear Household          | POST            | body: `{ name, description, representativeId, currency }` | `{ "name": "Depto amigos", "description": "Depto Miraflores", "representativeId": 1, "currency": "PEN" }` | `{ "id": "hh-1", "name": "Depto amigos" }`                     |



<br> **HouseHold Member:**

| **Endpoint**                                           | **Acción implementada**        | **Método HTTP** | **Parámetros**                                                | **Ejemplo Request**                                                 | **Ejemplo Response**                                        |
| ------------------------------------------------------ | ------------------------------ | --------------- | ------------------------------------------------------------- | ------------------------------------------------------------------- | ----------------------------------------------------------- |
| `/api/v1/household_member`                             | Crear miembro de Household     | POST            | body: `{ householdId, userId, isRepresentative }`             | `{ "householdId": "hh-1", "userId": 1, "isRepresentative": false }` | `{ "id": 10, "householdId": "hh-1", "userId": 1 }`          |
| `/api/v1/household_member`                             | Listar todos los miembros      | GET             | —                                                             | `GET /api/v1/household_member`                                      | `[ { "id": 10, "householdId": "hh-1", "userId": 1 }, ... ]` |
| `/api/v1/household_member/{id}`                        | Obtener miembro por Id         | GET             | path: `{ id }`                                                | `GET /api/v1/household_member/10`                                   | `{ "id": 10, "householdId": "hh-1", "userId": 1 }`          |
| `/api/v1/household_member/{id}`                        | Actualizar miembro por Id      | PUT             | path: `{ id }`, body: parcial `UpdateHouseholdMemberResource` | `PUT /api/v1/household_member/10` + body JSON                       | `{ "id": 10, "updated": true }`                             |
| `/api/v1/household_member/{id}`                        | Eliminar miembro               | DELETE          | path: `{ id }`                                                | `DELETE /api/v1/household_member/10`                                | `{ "deleted": true }`                                       |
| `/api/v1/household_member/household/{householdId}`     | Obtener miembros por Household | GET             | path: `{ householdId }`                                       | `GET /api/v1/household_member/household/hh-1`                       | `[ { "id": 10, "householdId": "hh-1" }, ... ]`              |
| `/api/v1/household_member/user/{userId}`               | Obtener miembros por usuario   | GET             | path: `{ userId }`                                            | `GET /api/v1/household_member/user/1`                               | `[ { "id": 10, "userId": 1, "householdId": "hh-1" }, ... ]` |
| `/api/v1/household_member/{id}/promote-representative` | Promover a representante       | POST            | path: `{ id }`                                                | `POST /api/v1/household_member/10/promote-representative`           | `{ "id": 10, "isRepresentative": true }`                    |
| `/api/v1/household_member/{id}/demote-representative`  | Degradar representante         | POST            | path: `{ id }`                                                | `POST /api/v1/household_member/10/demote-representative`            | `{ "id": 10, "isRepresentative": false }`                   |

<br> **Income Allocation:**

| **Endpoint**                                           | **Acción implementada**        | **Método HTTP** | **Parámetros**                                                | **Ejemplo Request**                                                 | **Ejemplo Response**                                        |
| ------------------------------------------------------ | ------------------------------ | --------------- | ------------------------------------------------------------- | ------------------------------------------------------------------- | ----------------------------------------------------------- |
| `/api/v1/household_member`                             | Crear miembro de Household     | POST            | body: `{ householdId, userId, isRepresentative }`             | `{ "householdId": "hh-1", "userId": 1, "isRepresentative": false }` | `{ "id": 10, "householdId": "hh-1", "userId": 1 }`          |
| `/api/v1/household_member`                             | Listar todos los miembros      | GET             | —                                                             | `GET /api/v1/household_member`                                      | `[ { "id": 10, "householdId": "hh-1", "userId": 1 }, ... ]` |
| `/api/v1/household_member/{id}`                        | Obtener miembro por Id         | GET             | path: `{ id }`                                                | `GET /api/v1/household_member/10`                                   | `{ "id": 10, "householdId": "hh-1", "userId": 1 }`          |
| `/api/v1/household_member/{id}`                        | Actualizar miembro por Id      | PUT             | path: `{ id }`, body: parcial `UpdateHouseholdMemberResource` | `PUT /api/v1/household_member/10` + body JSON                       | `{ "id": 10, "updated": true }`                             |
| `/api/v1/household_member/{id}`                        | Eliminar miembro               | DELETE          | path: `{ id }`                                                | `DELETE /api/v1/household_member/10`                                | `{ "deleted": true }`                                       |
| `/api/v1/household_member/household/{householdId}`     | Obtener miembros por Household | GET             | path: `{ householdId }`                                       | `GET /api/v1/household_member/household/hh-1`                       | `[ { "id": 10, "householdId": "hh-1" }, ... ]`              |
| `/api/v1/household_member/user/{userId}`               | Obtener miembros por usuario   | GET             | path: `{ userId }`                                            | `GET /api/v1/household_member/user/1`                               | `[ { "id": 10, "userId": 1, "householdId": "hh-1" }, ... ]` |
| `/api/v1/household_member/{id}/promote-representative` | Promover a representante       | POST            | path: `{ id }`                                                | `POST /api/v1/household_member/10/promote-representative`           | `{ "id": 10, "isRepresentative": true }`                    |
| `/api/v1/household_member/{id}/demote-representative`  | Degradar representante         | POST            | path: `{ id }`                                                | `POST /api/v1/household_member/10/demote-representative`            | `{ "id": 10, "isRepresentative": false }`                   |


 <br> **Settings:**

| **Endpoint**            | **Acción implementada**           | **Método HTTP** | **Parámetros**                                        | **Ejemplo Request**                                                           | **Ejemplo Response**                                                  |
| ----------------------- | --------------------------------- | --------------- | ----------------------------------------------------- | ----------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| `/api/v1/settings`      | Obtener configuración por usuario | GET             | header: `Authorization: Bearer token` (user en token) | `GET /api/v1/settings`                                                        | `{ "id": "set-1", "userId": 1, "language": "es", "currency": "PEN" }` |
| `/api/v1/settings`      | Crear configuración               | POST            | body: `{ userId, language, currency, notifications }` | `{ "userId": 1, "language": "es", "currency": "PEN", "notifications": true }` | `{ "id": "set-1", "userId": 1 }`                                      |
| `/api/v1/settings/{id}` | Actualizar configuración          | PUT             | path: `{ id }`, body: parcial `UpdateSettingResource` | `PUT /api/v1/settings/set-1` + body JSON                                      | `{ "id": "set-1", "updated": true }`                                  |

<br> **Member Contribution:**

| **Endpoint**                                                    | **Acción implementada**                       | **Método HTTP** | **Parámetros**             | **Ejemplo Request**                                          | **Ejemplo Response**                                                       |
| --------------------------------------------------------------- | --------------------------------------------- | --------------- | -------------------------- | ------------------------------------------------------------ | -------------------------------------------------------------------------- |
| `/api/v1/member_contribution`                                   | Listar todas las contribuciones de miembros   | GET             | —                          | `GET /api/v1/member_contribution`                            | `[ { "id": "mc-1", "memberId": 10, "contributionId": "contrib-1" }, ... ]` |
| `/api/v1/member_contribution/bycontributionid/{contributionId}` | Obtener MemberContribution por ContributionId | GET             | path: `{ contributionId }` | `GET /api/v1/member_contribution/bycontributionid/contrib-1` | `[ { "id": "mc-1", "contributionId": "contrib-1", "memberId": 10 }, ... ]` |
| `/api/v1/member_contribution/bymemberid/{memberId}`             | Obtener MemberContribution por MemberId       | GET             | path: `{ memberId }`       | `GET /api/v1/member_contribution/bymemberid/10`              | `[ { "id": "mc-1", "memberId": 10, "contributionId": "contrib-1" }, ... ]` |
| `/api/v1/member_contribution/{id}`                              | Eliminar MemberContribution                   | DELETE          | path: `{ id }`             | `DELETE /api/v1/member_contribution/mc-1`                    | `{ "deleted": true }`                                                      |


### 5.2.8 Team Collaboration Insights

Durante el desarrollo de los sprints, el equipo trabajó de manera colaborativa utilizando GitHub y una estrategia basada en ramas individuales e integración continua.

Aportes principales:

- **Carlos Guimaraes**: Desarrollo de secciones clave del frontend y apoyo en integración.
- **Camila Huamani**: Implementación de funcionalidades relacionadas a usuarios y configuración.
- **Angelo Solano**: Desarrollo de lógica de contribuciones y flujo del sistema.
- **Martin Gonzales**: Implementación de vistas y componentes del frontend.
- **Renzo Uribe**: Desarrollo del backend y endpoints principales.

Evidencias de colaboración:

<p align="center"> <img src="https://i.imgur.com/gnKYLlk.png"/> </p> <p align="center"> <img src="https://i.imgur.com/jcH0AH7.png"/> </p>

El trabajo en equipo permitió una integración eficiente entre frontend y backend, logrando una versión funcional del sistema.

## 5.3 Video About-the-Product

En el siguiente video se muestra una vista detallada del uso y proposito de la aplicación deasarrollada: https://shorturl.at/1Hyl0
