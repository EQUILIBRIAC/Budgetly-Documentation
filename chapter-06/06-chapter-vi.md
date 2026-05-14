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
