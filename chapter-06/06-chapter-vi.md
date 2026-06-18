# Capítulo VI: Product Verification & Validation

## 6.1. Testing Suites & Validation
    
En esta sección se definiran las estrategias, herramientas y niveles de prueba que garantizarán la calidad, estabilidad y correcto funcionamiento del sistema tanto a nivel interno (backend) como en su interacción con las interfaces de usuario.

### 6.1.1. Core Entities Unit Tests

Las pruebas unitarias del núcleo del sistema se centran en validar la lógica de negocio contenida en las entidades de dominio (Aggregates) y sus respectivos comandos. El objetivo es asegurar que las reglas de negocio críticas, la autogeneración de identificadores y las asignaciones de propiedades funcionen correctamente antes de interactuar con servicios externos o bases de datos.

#### Tecnologías y Herramientas

- **xUnit**: Framework de pruebas de última generación para .NET, utilizado para la ejecución y estructuración de los casos de prueba.  
- **Fluent Assertions**: Librería utilizada para escribir aserciones de forma más legible y descriptiva.  
- **.NET 9 SDK**: Entorno de ejecución y compilación de las suites de prueba.

#### Metodología de Diseño: Patrón AAA

Todas las pruebas se han estructurado siguiendo el patrón **AAA (Arrange, Act, Assert)** para garantizar la claridad y mantenibilidad:

- **Arrange**: Se configuran los datos de entrada, como comandos y parámetros esperados.  
- **Act**: Se invoca el constructor de la entidad o el método de negocio a validar.  
- **Assert**: Se verifica que el estado resultante de la entidad coincida con las expectativas.

#### Casos de Prueba Implementados

Se han desarrollado un total de **17 pruebas unitarias** cubriendo los siguientes dominios:

| Dominio        | Entidad Validada     | Descripción de la Lógica Probada |
|---------------|---------------------|----------------------------------|
| Bills        | Bill                | Validación de autogeneración de ID con prefijo `"BG"`, parseo de fechas y mapeo de montos. |
| Households   | HouseHold           | Reglas defensivas: forzado de miembros mínimos (1) y moneda por defecto (USD) ante datos inválidos. |
| IAM          | User                | Registro de usuarios, manejo de roles (Admin, Member) y asignación de identificadores de hogar. |
| Contributions| Contribution        | Validación de estrategias de división (Even, IncomeBased) y corrección de bugs en asignación de parámetros. |
| Members      | MemberContribution  | Gestión de estados de pago (Pending) y vinculación correcta entre miembro y gasto. |
| Invitations  | Invitation          | Generación automática de tokens únicos (GUID) y establecimiento de fechas de expiración a 7 días. |
| Allocations  | IncomeAllocation    | Lógica de actualización de porcentajes y prefijos de identificación `"IA-"`. |

#### Evidencia de Ejecución

A continuación, se presenta los archivos y el resultado de la ejecución de la suite de pruebas unitarias desde la terminal del sistema:

<img src="../assets/chapter-06/Unit-Tests-Files.png" alt="Archivos de Pruebas Unitarias" width="600"/>
<img src="../assets/chapter-06/Unit-Tests-Summary.png" alt="Ejecución de Pruebas Unitarias" width="600"/>


### 6.1.2. Core Integration Tests

Las pruebas de integración validan que las diferentes capas del sistema (Aplicación, Dominio e Infraestructura) colaboren correctamente. A diferencia de las pruebas unitarias, estas verifican la persistencia real en la base de datos y la correcta ejecución de los servicios de comando cuando interactúan con múltiples repositorios.

#### Tecnologías y Herramientas

- **Entity Framework Core In-Memory**: Utilizado para simular una base de datos relacional en memoria, permitiendo pruebas rápidas y aisladas sin necesidad de un servidor SQL real.  
- **Moq**: Empleado para simular servicios externos y dependencias de infraestructura que no forman parte del flujo principal de persistencia (como servicios de hashing o envío de tokens).  
- **UnitOfWork**: Se utiliza la implementación real para asegurar que las transacciones y el método `CompleteAsync()` impacten realmente en la base de datos de prueba.

#### Escenarios de Interacción Validados

Se han implementado **5 pruebas de integración críticas** que cubren flujos de extremo a extremo en el backend:

| Módulo        | Flujo de Integración              | Validación de Interacción |
|--------------|----------------------------------|--------------------------|
| IAM          | Registro de Usuario (Sign Up)     | Interacción entre `UserCommandService`, `HashingService` y persistencia en la tabla `Users`. |
| Households   | Creación de Hogar                | Validación del flujo `HouseHoldCommandService` → `HouseHoldRepository` y creación automática del miembro representante. |
| Bills        | Registro de Gasto                | Verificación de vinculación de gastos con el `HouseholdId` y persistencia en la tabla `Bills`. |
| Invitations  | Gestión de Invitaciones          | Validación de existencia del hogar y persistencia de tokens de invitación únicos. |
| Allocations  | Distribución de Ingresos         | Registro de porcentajes de contribución (`IncomeAllocation`) asociados a usuarios y hogares reales. |

#### Evidencia de Ejecución

A continuación, se detalla el reporte de ejecución de la suite de pruebas de integración. Estas pruebas fueron aisladas mediante filtros de ejecución para validar exclusivamente la interoperabilidad entre los servicios de aplicación y la capa de persistencia (Base de Datos en Memoria).

<img src="../assets/chapter-06/Integration-Tests-Files.png" alt="Ejecución de Pruebas de Integración" width="600"/>

<img src="../assets/chapter-06/Integration-Tests-Summary.png" alt="Resumen de Pruebas de Integración" width="600"/>

### 6.1.3. Core Behavior-Driven Development (BDD)

En esta sección, el equipo aplicó técnicas de **Behavior-Driven Development (BDD)** para validar que el sistema se comporte según las expectativas del usuario final. Se utilizó el lenguaje **Gherkin** para definir escenarios de negocio legibles y **SpecFlow** para automatizar su ejecución.

#### Escenarios de Comportamiento Definidos

Se implementaron escenarios clave que describen flujos críticos de la aplicación desde una perspectiva funcional:

- **Gestión de Hogares (Household Management)**: Validación del flujo de creación de un grupo de gastos compartidos, asegurando la asignación correcta del rol de representante.  
- **Registro de Usuarios (User Registration)**: Verificación del proceso de alta en la plataforma, asegurando la integridad de los datos de identidad (Email/Password).

#### A. Especificación Gherkin para Household Management

```gherkin
Feature: Household Management
  As a software engineering student
  I want to create a new household in Budgetly
  So that I can start managing shared expenses with my team

  Scenario: Create a household successfully
    Given I am a registered user with ID 1
    When I request to create a household named "Apartamento 402" with a limit of 5 members
    Then the system should generate the household aggregate successfully
    And the household name should be "Apartamento 402"
    And I should be assigned as the representative
```

#### B. Especificación Gherkin para User Registration

```gherkin
Feature: User Registration
  As a new visitor of Budgetly
  I want to register a new account
  So that I can start using the platform to split my expenses

  @user_registration
  Scenario: Successful user registration
    Given I provide the name "Carlos Perez" and email "carlos@example.com"
    And I choose a password "Password123!" and the role "Admin"
    When I submit the registration
    Then the user should be created with the email "carlos@example.com"
    And the account should be active by default
```
#### Evidencia de Ejecución

Al ejecutar la suite de pruebas, SpecFlow interpreta los archivos .feature y ejecuta los Step Definitions vinculados. La siguiente captura muestra la ejecución exitosa de los 24 tests totales, incluyendo los escenarios de comportamiento (BDD) que aparecen detallados por pasos en la salida de consola:

<img src="../assets/chapter-06/Behavior-Driven-Development-Files.png" alt="Archivos de Pruebas BDD" width="600"/>
<img src="../assets/chapter-06/BDD-Summary.png" alt="Ejecución de Pruebas BDD" width="600"/>

### 6.1.4. Core System Tests

Las pruebas de sistema representan el nivel más alto de validación, donde se verifica que el ecosistema completo (Frontend, Backend y Base de Datos) interactúe correctamente bajo condiciones de uso real. Se enfocan en flujos críticos de usuario  tanto en la plataforma Web (Angular) como en la aplicación móvil (Flutter).

#### Estrategia de Pruebas (E2E)

Se han seleccionado los flujos con mayor impacto en la experiencia del usuario para garantizar la estabilidad del sistema:

| Escenario de Sistema            | Entorno        | Descripción de la Prueba |
|--------------------------------|---------------|--------------------------|
| Flujo de Gasto Completo        | Web / Móvil   | Registro de un gasto → Cálculo de deuda → Actualización de balance en tiempo real. |
| Persistencia Multi-plataforma  | Web → Móvil   | Crear un hogar en la Web y verificar su disponibilidad inmediata en la App móvil vía API. |
| Sincronización de Invitaciones | Móvil         | Recepción de notificación → Aceptación de invitación → Acceso al dashboard del hogar. |

#### Herramientas Utilizadas

- **Selenium / Cypress (Web)**: Para automatizar la navegación en el navegador y validar la respuesta de la interfaz Angular.  
- **Postman Collection**: Para validar que las APIs de Budgetly responden con tiempos de latencia menores a 200 ms en condiciones de carga normal.

#### Evidencia de Ejecución

Las pruebas confirmaron que la comunicación entre el frontend y el backend es fluida a través de los servicios REST. Se validó que los estados de carga y las validaciones de formulario funcionan correctamente en ambos entornos, garantizando una experiencia de usuario consistente y sin interrupciones.

Script en Selenium para validar flujo de inicio de sesión:
```csharp
using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;
using OpenQA.Selenium.Support.UI;
using FluentAssertions;
using Xunit;

namespace com.split.backend.Tests.SystemTests
{
    public class LoginSystemTests : IDisposable
    {
        private readonly IWebDriver _driver;
        private readonly string _baseUrl = "https://budgetly-exp-app.web.app";

        public LoginSystemTests()
        {
            var options = new ChromeOptions();
            options.AddArgument("--disable-blink-features=AutomationControlled");
            _driver = new ChromeDriver(options);
            _driver.Manage().Window.Maximize();
        }

        [Fact]
        public void WebApp_Production_UserCanLoginSuccessfully()
        {
            _driver.Navigate().GoToUrl($"{_baseUrl}/login");

            var wait = new WebDriverWait(_driver, TimeSpan.FromSeconds(20));

            var emailInput = wait.Until(d => d.FindElement(By.CssSelector("input[type='email']")));
            emailInput.Clear();
            emailInput.SendKeys("usuarionuevo@yopmail.com");

            var passwordInput = wait.Until(d => d.FindElement(By.CssSelector("input[type='password']")));
            passwordInput.Clear();
            passwordInput.SendKeys("12345678");


            var loginButton = wait.Until(d => d.FindElement(By.XPath("//button[contains(., 'Sign In')]")));

            ((IJavaScriptExecutor)_driver).ExecuteScript("arguments[0].scrollIntoView(true);", loginButton);

            loginButton.Click();

            wait.Until(d => d.Url.Contains("/dashboard") || d.Url.Contains("/home"));

            _driver.Url.Should().ContainAny("/dashboard", "/home");

            Thread.Sleep(10000);
        }

        public void Dispose()
        {
            _driver.Quit();
            _driver.Dispose();
        }
    }
}
```

## 6.2. Static testing & Verification

## 6.2.1. Static Code Analysis

### 6.2.1.1. Coding standard & Code conventions

### 6.2.1.2. Code Quality & Code Security.

## 6.2.2 Static Code Analysis

## 6.3. Validation Interviews

### 6.3.1. Diseño de Entrevistas

### 6.3.2. Registro de Entrevistas.

### 6.3.3. Evaluaciones según heurísticas.

## 6.4. Auditoría de Experiencias de Usuario

## 6.4.1 Auditoría realizada.

### 6.4.1.1. Información del grupo auditado.

El grupo al que se le realizó la auditoría recibe el nombre de “UI-Topic” y el producto que desarrollaron se llama “Restock”. A continuación, se adjunta la lista de integrantes del grupo auditado.

- Guerra Perez, Jose Jahaziel (u202319831)
- Shapiama Rivera, Gabriela Nicole (u202319448) 
- Castro Alejos, Julio (u202021885)
- Sanchez Guevara, Ivan Fernando (u202218181)
- Chavez Uribe, Ario Joel (u202213468)

Inmediatamente, se adjunta el enlace de acceso a la organizacion del grupo que desarrollo “Restock”:
https://github.com/ui-topic-diseno-de-experimentos-10253

### 6.4.1.2. Cronograma de auditoría realizada.

En esta sección, se cronometran las principales actividades realizadas para el desarrollo de la auditoría hacia el trabajo del grupo “Restock”.

| Actividad de auditoría realizada | Fecha/Hora        | Realizado por            |
|----------------------------------|-------------------|--------------------------|
| Solicitud de información al grupo auditado        | 12/06/2026 12:20pm | Camila Huamani |
| Envío de información solicitada por parte del grupo auditado a nuestro grupo para el desarrollo de la auditoría  | 12/06/2026 8:20pm   | Camila Huamani |
| Ejecución de la auditoría | 14/06/2026 10:00pm         | Camila Huamani<br>Martin Gonzales<br>Angelo Solano<br>Carlos Guimare<br>Renzo Uribe |
| Elaboración del informe de la auditoría | 14/06/2026 11:30pm         | Camila Huamani<br>Martin Gonzales<br>Angelo Solano<br>Carlos Guimare<br>Renzo Uribe |
| Finalización y envío del informe de auditoría | 15/06/2026 1:00pm         | Camila Huamani |

### 6.4.1.3. Contenido de auditoría realizada.

#### **TAREAS A EVALUAR:**

El alcance de esta evaluación incluye la revisión de la usabilidad de las siguientes tareas:

1. Registro de un usuario nuevo  
2. Inicio de sesión de un usuario  
3. Acceder a las distintas secciones de la aplicación  
4. Interfaz de la aplicación web  
5. Interfaz de la aplicación móvil  
6. Claridad de la navegación  
7. Realizar pedidos  
8. Creación de insumos  
9. Añadir al inventario  
10. Gestionar pedidos  
11. Gestionar recetas  
    

No están incluidas en esta versión de la evaluación las siguientes tareas:

1. Seleccionar una suscripción  
2. Selección de idiomas  
3. Gestión del perfil de usuario 

### **ESCALA DE SEVERIDAD:**

Los errores serán puntuados tomando en cuenta la siguiente escala de severidad

| *Nivel* | *Descripción* |
| :---- | :---- |
| *1* | *Problema superficial: puede ser fácilmente superado por el usuario o ocurre con muy poca frecuencia. No necesita ser arreglado a no ser que exista disponibilidad de tiempo.* |
| *2* | *Problema menor: puede ocurrir un poco más frecuentemente o es un poco más difícil de superar para el usuario. Se le debería asignar una prioridad baja. Resolverlo de cara al siguiente reléase* |
| *3* | *Problema mayor: ocurre frecuentemente o los usuarios no son capaces de resolverlos. Es importante que sean corregidos y se les debe asignar una prioridad alta.* |
| *4* | *Problema muy grave: un error de gran impacto que impide al usuario continuar con el uso de la herramienta. Es imperativo que sea corregido antes del lanzamiento.* |

### **TABLA RESUMEN:**

| *\#* | *Problema* | *Escala de severidad* | *Heurística/Principio violada(o)* |
| :---: | ----- | :---: | ----- |
| 1 | En la sección de ventas, el texto "REGISTERED SALES NOT DISCOUNTED IN INVENTORY" resulta ambiguo y difícil de interpretar. | 3 | Information Architecture: Is it clear? |
| 2 | En la sección de notificaciones de los dos usuarios, no se muestra información o texto referencial cuando no se cuenta con alguna notificación | 2 | Information Architecture: Is it communicative? |
| 3 | En general la imagen superior ocupa demasiado espacio, esto provoca que la información importante sea empujada hacia abajo. | 3 | Usability: Diseño estético y minimalista |
| 4 | Tanto en las secciones de registro y login de la app web, no se especifican otras opciones disponibles de ingresar a la aplicación (Google, Facebook, etc). | 1 | Usability: Visibilidad del estado del sistema |
| 5 | En la sección de inventario de la app móvil, presenta dos acciones principales: Supply y New batch. Sin embargo, la diferencia entre ambas no resulta evidente para usuarios nuevos.  | 3 | Usability: Relación entre el sistema y mundo real |
| 6 | En la sección de inventario de la app móvil, presenta grandes áreas vacías entre las secciones Supply Catalog e Inventory (Batches).  | 2 | Information Architecture: Is it useful? |
| 7 | En la sección de órdenes de la app móvil, los filtros de estado y precio permanecen visibles incluso cuando no existen pedidos registrados  | 1 | Usability: Diseño estético y minimalista |


### **DESCRIPCIÓN DE PROBLEMAS:**

### **Problema 01:** 

**Heurística violada:** Visibilidad del estado del sistema  
**Severidad:** 1/4 \- Problema superficial  
**Problema Explicado:** En las pantallas de inicio de sesión y registro se muestra el texto "Or sign in with", sugiriendo la existencia de métodos alternativos de autenticación. Sin embargo, no se presentan opciones adicionales como Google o Facebook, lo que puede generar confusión y expectativas incorrectas en los usuarios.  

<img src="https://i.imgur.com/dtDCz1i.png" alt="Imagen 1" width="800"/>   

**Recomendaciones:**

* Eliminar el texto si no existen métodos alternativos de autenticación.  
* Agregar los botones correspondientes si la funcionalidad está prevista.


### **Problema 02:** 

**Heurística violada:** Es Comunicativo  
**Severidad:** 2/4 \- Problema menor  
**Problema Explicado:** Cuando el usuario no tiene notificaciones disponibles, la sección permanece vacía sin mostrar ningún mensaje informativo. Esto puede hacer que el usuario dude si realmente no existen notificaciones o si ocurrió un error al cargar la información.  

<img src="https://i.imgur.com/2109J1T.png" alt="Imagen 2" width="800"/> 


**Recomendación:**

* Mostrar un mensaje como: "No tienes notificaciones pendientes".  
* Incorporar un ícono o ilustración que refuerce visualmente el estado vacío. 


### **Problema 03:** 

**Heurística violada:** Diseño estético y minimalista  
**Severidad:** 3/4 \- Problema grave  
**Problema Explicado:** La imagen ubicada en la parte superior de las pantallas consume una cantidad considerable del espacio visible. Como consecuencia, la información y las acciones más relevantes del sistema quedan desplazadas hacia abajo, obligando al usuario a desplazarse para acceder a ellas.  

<img src="https://i.imgur.com/bs7mWnF.png" alt="Imagen 3" width="800"/> 

**Recomendación:** 

* Reducir la altura de la imagen para priorizar el contenido funcional.   
* Reemplazar la imagen por información relevante del negocio o indicadores clave. 


### **Problema 04:** 

**Heurística violada:** Es Claro  
**Severidad:** 3/4 \- Problema grave  
**Problema Explicado:** En la sección de ventas se muestra el texto "REGISTERED SALES NOT DISCOUNTED IN INVENTORY" para identificar un conjunto de registros. Sin embargo, la redacción resulta poco clara y puede generar confusión sobre su significado, ya que el término discounted suele asociarse a descuentos de precio y no a la deducción de insumos del inventario. Esto obliga al usuario a interpretar el mensaje antes de comprender la función de la sección.  

<img src="https://i.imgur.com/f75s5S9.png" alt="Imagen 4" width="800"/> 

**Recomendación:**

* Utilizar una redacción más simple y descriptiva, por ejemplo: "Sales Pending Inventory Update" o "Sales Awaiting Stock Deduction".  
* Incluir una breve descripción o ayuda contextual que explique el estado de estas ventas y su relación con el inventario.


### **Problema 05:** 

**Heurística violada:** Relación entre el sistema y mundo real  
**Severidad:** 3/4 \- Problema mayor  
**Problema Explicado:** La pantalla presenta dos acciones principales: Supply y New batch. Sin embargo, la diferencia entre ambas no resulta evidente para usuarios nuevos, ya que los términos no explican claramente qué elemento se está creando ni cómo se relacionan entre sí dentro del inventario.  

<img src="https://i.imgur.com/QdrAfeJ.png" alt="Imagen 5" width="400"/> 

**Recomendación**

* Incorporar una breve explicación contextual de cada acción.   
* Utilizar nombres más descriptivos, por ejemplo *"Create Supply"* y *"Register Batch"*. 


### **Problema 06:** 

**Heurística violada:** Es Útil  
**Severidad:** 2/4 \- Problema superficial  
**Problema Explicado:** La pantalla presenta grandes áreas vacías entre las secciones Supply Catalog e Inventory (Batches). Cuando no existen registros, el espacio disponible no se aprovecha para informar al usuario sobre el estado actual del inventario ni para orientarlo sobre las acciones que puede realizar. Esto genera una sensación de aplicación incompleta y dificulta comprender qué debe hacerse a continuación. 

<img src="https://i.imgur.com/jogwokj.png" alt="Imagen 6" width="400"/> 

**Recomendación:** 

* Mostrar mensajes de estado vacío como *"No supplies registered"* o *"No inventory batches available"*.   
* Utilizar parte del espacio para proporcionar instrucciones o accesos directos a las acciones principales.

 
### **Problema 07:** 

**Heurística violada:** Diseño estético y minimalista  
**Severidad:** 1/4 \- Problema superficial  
**Problema Explicado:** Los filtros de estado y precio permanecen visibles incluso cuando no existen pedidos registrados. Esto puede generar una experiencia poco eficiente, ya que el usuario puede interpretar que existen datos para filtrar cuando la lista se encuentra vacía.   

<img src="https://i.imgur.com/z1UWQsE.png" alt="Imagen 7" width="400"/> 

**Recomendación:**

* Mostrar los filtros únicamente cuando haya registros disponibles.   
* Ocultar o deshabilitar los filtros cuando no existan pedidos. 

## 6.4.2 Auditoría recibida.

### 6.4.2.1. Información del grupo auditor

### 6.4.2.2. Cronograma de auditoría recibida.

### 6.4.2.3. Contenido de auditoría recibida.

### 6.4.2.4. Resumen de modificaciones para subsanar hallazgo


