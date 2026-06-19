# Capítulo VIII: Experiment-Driven Development

## 8.1. Experiment Planning

### 8.1.1. As-Is Summary

Budgetly es una plataforma SaaS de gestión financiera colaborativa orientada a hogares con ingresos desiguales. Actualmente, el sistema permite a los usuarios registrarse bajo dos roles diferenciados —**Representante del Hogar** y **Miembro del Hogar**— y ofrece funcionalidades de registro de gastos, cálculo proporcional de aportes basado en ingresos, seguimiento de contribuciones y generación de reportes mensuales.

El producto cuenta con una **Landing Page** desplegada en GitHub Pages, una **Web Application** en Vue.js alojada en Firebase Hosting, una **API RESTful** desarrollada en ASP.NET Core y desplegada en Azure, y una **aplicación móvil nativa** construida con Flutter. La arquitectura sigue principios de Domain-Driven Design (DDD) con bounded contexts bien definidos: IAM, Households, Incomes, Bills, Contributions y Settings.

A pesar de estos avances técnicos, el equipo reconoce que **no se han validado empíricamente** las hipótesis centrales del negocio:

- Que los usuarios adoptarán el cálculo proporcional de aportes como mecanismo de distribución justo.
- Que la automatización reduce efectivamente los conflictos financieros dentro del hogar.
- Que la interfaz actual es suficientemente intuitiva para usuarios con baja alfabetización digital.
- Que la funcionalidad de metas de ahorro compartidas incentiva la cooperación económica.

El modelo de monetización freemium no ha sido probado en usuarios reales. Se desconoce si los usuarios estarían dispuestos a pagar por funcionalidades premium como exportación de datos, análisis histórico avanzado o personalización de reglas de distribución.

El pipeline de CI/CD está operativo y permite despliegues continuos, lo que facilita la iteración rápida sobre el producto para ejecutar experimentos controlados en producción.

---

### 8.1.2. Raw Material: Assumptions, Knowledge Gaps, Ideas, Claims

#### Assumptions (Suposiciones del equipo)

| # | Supuesto | Área | Riesgo |
|---|----------|------|--------|
| A1 | Los usuarios prefieren un cálculo automático de aportes basado en ingresos sobre una división igualitaria. | Producto | Alto |
| A2 | La transparencia financiera dentro del hogar reduce los conflictos por dinero. | Negocio | Alto |
| A3 | Los usuarios completarán el onboarding sin necesidad de asistencia técnica. | UX | Alto |
| A4 | Al menos el 50% de los hogares creará una meta de ahorro compartida en los primeros dos meses. | Producto | Medio |
| A5 | El Representante del Hogar liderará activamente la adopción de la app para los demás miembros. | Comportamiento | Alto |
| A6 | Los usuarios están dispuestos a ingresar sus ingresos personales en una aplicación digital. | Privacidad | Alto |
| A7 | Las notificaciones push y correos de recordatorio reducen los pagos atrasados. | Retención | Medio |
| A8 | La versión freemium es suficiente para generar una base de usuarios activos que luego conviertan a premium. | Monetización | Alto |
| A9 | La aplicación móvil (Flutter) tendrá mayor adopción que la web application entre el segmento objetivo. | Canal | Medio |
| A10 | Los usuarios con experiencia en Excel adoptarán Budgetly como reemplazo sin fricciones significativas. | Adopción | Medio |

#### Knowledge Gaps (Brechas de conocimiento)

| # | Brecha | Impacto |
|---|--------|---------|
| KG1 | No sabemos cuántos hogares peruanos urbanos enfrentan desigualdad de ingresos significativa entre sus miembros. | Alto |
| KG2 | Se desconoce cuántos usuarios abandonan el flujo de registro antes de completarlo. | Alto |
| KG3 | No hay datos sobre la frecuencia con la que los usuarios regresan a la app después de su primera visita (Day 7, Day 30 retention). | Alto |
| KG4 | No se ha medido la satisfacción del usuario (NPS, CSAT) tras las primeras semanas de uso. | Alto |
| KG5 | Se desconoce si el modelo de distribución por ingresos genera percepción de equidad o de inequidad invertida en algunos perfiles. | Medio |
| KG6 | No se conoce el ticket promedio que un usuario estaría dispuesto a pagar por el plan premium. | Medio |
| KG7 | No se ha validado qué canal de adquisición (redes sociales, boca a boca, SEO) genera usuarios de mayor calidad. | Medio |
| KG8 | Se desconoce si los representantes del hogar son el perfil correcto para liderar la adopción o si debería orientarse a los miembros. | Alto |

#### Ideas

- Implementar un **modo simulación** en la landing page que permita a los visitantes probar el cálculo proporcional sin registrarse.
- Añadir un **asistente de onboarding** paso a paso que guíe al nuevo usuario desde el registro hasta el primer gasto registrado.
- Incorporar **gamificación** (badges, rachas de pagos puntuales) para incentivar el uso continuo.
- Crear un **tablero de transparencia** donde todos los miembros vean en tiempo real el estado de los aportes.
- Ofrecer una **integración bancaria básica** (lectura de saldo) para reducir la fricción del ingreso manual de datos.
- Desarrollar un **widget de resumen mensual** que pueda compartirse por WhatsApp entre los miembros del hogar.

#### Claims (Afirmaciones a validar)

| # | Afirmación | Fuente |
|---|-----------|--------|
| C1 | "El 70% de los usuarios reportará menos discusiones por dinero en los primeros 3 meses." | Hipótesis 1 del Lean UX |
| C2 | "Los pagos atrasados se reducirán en un 50% con alertas automáticas." | Hipótesis 2 del Lean UX |
| C3 | "El 80% de los usuarios completará el registro sin asistencia técnica." | Hipótesis 3 del Lean UX |
| C4 | "Al menos el 50% de hogares creará una meta de ahorro en los primeros 2 meses." | Hipótesis 4 del Lean UX |
| C5 | "Los usuarios con ingresos dispares perciben la distribución proporcional como más justa." | Entrevistas de needfinding |

---

### 8.1.3. Experiment-Ready Questions

Las siguientes preguntas han sido priorizadas porque pueden ser respondidas mediante experimentos controlados en un plazo razonable y con los recursos disponibles del equipo.

| ID | Pregunta | Tipo |
|----|----------|------|
| ERQ-01 | ¿Qué porcentaje de usuarios que visitan la landing page inician el proceso de registro? | Cuantitativa |
| ERQ-02 | ¿Cuántos usuarios completan el flujo de onboarding (registro → crear/unirse a hogar → primer gasto) sin abandonar? | Cuantitativa |
| ERQ-03 | ¿Los usuarios que reciben recordatorios automáticos registran sus pagos con mayor puntualidad que los que no los reciben? | Comparativa |
| ERQ-04 | ¿La visualización gráfica de aportes proporcionales aumenta la percepción de equidad respecto a mostrar solo montos en texto? | Comparativa (A/B) |
| ERQ-05 | ¿Qué funcionalidades premium generan mayor intención de pago en los usuarios del plan gratuito? | Cuantitativa |
| ERQ-06 | ¿El modo simulación en la landing page aumenta la tasa de registro comparado con no tenerlo? | Comparativa (A/B) |
| ERQ-07 | ¿Los hogares con representante activo tienen mayor tasa de retención a 30 días que los hogares sin representante activo? | Correlacional |
| ERQ-08 | ¿Cuántos días tarda un usuario en registrar su segundo gasto después del primero? | Cuantitativa |

---

### 8.1.4. Question Backlog

El backlog de preguntas está ordenado por impacto en el negocio y viabilidad de ejecución en el corto plazo.

| Prioridad | ID | Pregunta | Impacto | Esfuerzo | Estado |
|-----------|-----|----------|---------|----------|--------|
| 1 | ERQ-02 | ¿Cuántos usuarios completan el onboarding sin abandonar? | Alto | Bajo | Listo para experimentar |
| 2 | ERQ-01 | ¿Qué % de visitantes inicia el registro? | Alto | Bajo | Listo para experimentar |
| 3 | ERQ-03 | ¿Los recordatorios mejoran la puntualidad de pagos? | Alto | Medio | Listo para experimentar |
| 4 | ERQ-04 | ¿Los gráficos mejoran la percepción de equidad? | Alto | Medio | En diseño |
| 5 | ERQ-06 | ¿El simulador en landing page aumenta el registro? | Alto | Medio | En diseño |
| 6 | ERQ-07 | ¿El representante activo impacta la retención? | Alto | Alto | Pendiente |
| 7 | ERQ-08 | ¿Cuántos días hasta el segundo gasto? | Medio | Bajo | Listo para experimentar |
| 8 | ERQ-05 | ¿Qué features premium generan más intención de pago? | Medio | Medio | Pendiente |

---

### 8.1.5. Experiment Cards

---

**Experiment Card #1**

| Campo | Detalle |
|-------|---------|
| **ID** | EXP-01 |
| **Nombre** | Tasa de completación del onboarding |
| **Pregunta asociada** | ERQ-02 |
| **Hipótesis** | Creemos que al menos el 80% de los usuarios que inician el registro completarán el flujo de onboarding (registro → hogar → primer gasto) si el proceso se presenta en pasos claros y secuenciales. |
| **Método** | Análisis de embudo (funnel analysis) sobre eventos de navegación registrados en la aplicación. |
| **Segmento** | Nuevos usuarios registrados en los primeros 30 días de la campaña. |
| **Métricas** | Tasa de completación por paso del embudo; tasa de abandono por pantalla. |
| **Criterio de éxito** | ≥ 80% de usuarios completan el paso de creación/unión a hogar dentro de las 24 horas del registro. |
| **Duración** | 4 semanas |
| **Resultado esperado** | Identificar el paso de mayor abandono para priorizar mejoras de UX. |

---

**Experiment Card #2**

| Campo | Detalle |
|-------|---------|
| **ID** | EXP-02 |
| **Nombre** | Impacto de recordatorios automáticos en puntualidad de pagos |
| **Pregunta asociada** | ERQ-03 |
| **Hipótesis** | Creemos que los miembros que reciben recordatorios push 48 horas antes de la fecha límite de aporte registrarán su pago a tiempo con una tasa al menos 30 puntos porcentuales mayor que los que no reciben recordatorio. |
| **Método** | Prueba A/B: Grupo A recibe notificación push + email a 48h de la fecha límite; Grupo B no recibe ningún recordatorio. |
| **Segmento** | Miembros del hogar con al menos un aporte pendiente en el mes en curso. |
| **Métricas** | % de aportes registrados antes de la fecha límite por grupo; tiempo promedio desde el recordatorio hasta el registro del pago. |
| **Criterio de éxito** | El Grupo A presenta una tasa de pagos a tiempo ≥ 50% mayor que el Grupo B. |
| **Duración** | 6 semanas (2 ciclos de pago mensual) |
| **Resultado esperado** | Validar la utilidad de las notificaciones automáticas y determinar el momento óptimo de envío. |

---

**Experiment Card #3**

| Campo | Detalle |
|-------|---------|
| **ID** | EXP-03 |
| **Nombre** | A/B Test: Gráfico vs. texto en panel de aportes |
| **Pregunta asociada** | ERQ-04 |
| **Hipótesis** | Creemos que mostrar la distribución de aportes mediante un gráfico circular interactivo aumentará la percepción de equidad del sistema en al menos un 20% respecto a mostrar solo montos en texto plano. |
| **Método** | Prueba A/B: Variante A muestra los aportes en tabla de texto; Variante B muestra un gráfico circular con porcentajes y montos. Encuesta de percepción de equidad al final de la semana. |
| **Segmento** | Miembros del hogar con al menos 2 semanas de uso activo. |
| **Métricas** | Puntuación de percepción de equidad (escala Likert 1-5); tiempo en pantalla del panel de aportes; número de clics en el panel. |
| **Criterio de éxito** | La Variante B obtiene una puntuación media de percepción de equidad ≥ 4.0/5.0, al menos 20% mayor que la Variante A. |
| **Duración** | 3 semanas |
| **Resultado esperado** | Confirmar que la visualización gráfica es más efectiva para comunicar equidad y justificar su priorización en el roadmap. |

---

**Experiment Card #4**

| Campo | Detalle |
|-------|---------|
| **ID** | EXP-04 |
| **Nombre** | Simulador interactivo en landing page |
| **Pregunta asociada** | ERQ-06 |
| **Hipótesis** | Creemos que añadir un simulador de distribución de gastos en la landing page aumentará la tasa de conversión de visitante a registro en al menos un 15%. |
| **Método** | Prueba A/B: 50% de los visitantes ve la landing page actual (sin simulador); 50% ve la versión con el simulador interactivo. Se mide el CTR del botón "Registrarse". |
| **Segmento** | Visitantes nuevos de la landing page sin cuenta previa. |
| **Métricas** | Tasa de conversión (clics en "Registrarse" / visitantes únicos); tiempo en página; tasa de rebote. |
| **Criterio de éxito** | La variante con simulador presenta una tasa de conversión al menos un 15% superior a la variante de control. |
| **Duración** | 4 semanas |
| **Resultado esperado** | Validar si la experiencia interactiva previa al registro reduce la fricción de adopción y justifica el esfuerzo de desarrollo del simulador. |

---

## 8.2. Experiment Design

### 8.2.1. Hypotheses

Las hipótesis de experimentación de Budgetly se derivan directamente de las Lean UX Hypothesis Statements definidas en el capítulo I, traducidas ahora a un formato estructurado para su validación empírica.

---

**Hipótesis 1 – Adopción del cálculo proporcional**

> Creemos que **automatizar la asignación de contribuciones según los ingresos individuales** aumentará la equidad percibida y reducirá los conflictos financieros dentro del hogar.  
> Sabremos que hemos tenido éxito cuando el **70% de los usuarios activos** reporte una disminución en las discusiones por dinero, medido mediante encuesta in-app al tercer mes de uso.

---

**Hipótesis 2 – Efectividad de recordatorios automáticos**

> Creemos que **enviar recordatorios automáticos 48 horas antes de la fecha límite de pago** reducirá los aportes atrasados en el hogar.  
> Sabremos que hemos tenido éxito cuando el **número de contribuciones registradas fuera de plazo se reduzca en un 50%** respecto al mes anterior al activar los recordatorios, medido sobre un grupo de al menos 50 hogares activos.

---

**Hipótesis 3 – Onboarding sin fricción**

> Creemos que **una interfaz intuitiva y un flujo de onboarding paso a paso** incrementará la tasa de adopción incluso entre usuarios con poca experiencia digital.  
> Sabremos que hemos tenido éxito cuando el **80% de los nuevos usuarios complete el registro y configure su primer hogar dentro de las 24 horas** de haberse registrado, sin necesidad de contactar al soporte.

---

**Hipótesis 4 – Metas de ahorro compartido**

> Creemos que **introducir metas de ahorro compartidas visibles para todos los miembros** fomentará la cooperación económica y el compromiso financiero dentro del hogar.  
> Sabremos que hemos tenido éxito cuando **al menos el 40% de los hogares activos cree y mantenga activa al menos una meta de ahorro** durante los primeros 60 días de uso.

---

**Hipótesis 5 – Conversión freemium a premium**

> Creemos que **ofrecer un plan premium con reportes avanzados, exportación PDF y personalización de reglas** generará una intención de pago real en usuarios que ya usan activamente el plan gratuito.  
> Sabremos que hemos tenido éxito cuando **al menos el 10% de los usuarios activos del plan gratuito con más de 30 días de uso** indiquen intención de contratar el plan premium en una encuesta de valoración de funcionalidades.

---

### 8.2.2. Domain Business Metrics

Las métricas de negocio de Budgetly se organizan en tres dimensiones clave: **Adquisición**, **Activación y Retención**, y **Monetización**.

#### Adquisición

| Métrica | Descripción | Meta inicial |
|---------|-------------|-------------|
| Visitantes únicos a la landing page | Número de usuarios nuevos que acceden a la landing page por semana. | 500/semana |
| Tasa de conversión landing → registro | % de visitantes que completan el registro. | ≥ 10% |
| Costo de adquisición por canal | Estimación del costo por usuario registrado según canal (orgánico, redes, boca a boca). | Referencial |

#### Activación y Retención

| Métrica | Descripción | Meta inicial |
|---------|-------------|-------------|
| Tasa de onboarding completado | % de usuarios registrados que configuran su hogar y registran su primer gasto. | ≥ 70% |
| Retención Day 7 | % de usuarios que regresan a la app en los 7 días siguientes al registro. | ≥ 40% |
| Retención Day 30 | % de usuarios que regresan a la app en los 30 días siguientes al registro. | ≥ 25% |
| Hogares activos mensuales (MAH) | Número de hogares con al menos un gasto registrado en el mes. | 50 en el primer mes |
| Aportes registrados a tiempo | % de contribuciones registradas antes de la fecha límite. | ≥ 65% |

#### Monetización

| Métrica | Descripción | Meta inicial |
|---------|-------------|-------------|
| Tasa de conversión freemium → premium | % de usuarios gratuitos que contratan el plan premium. | ≥ 5% en 3 meses |
| Ingreso mensual recurrente (MRR) | Suma de suscripciones premium activas por mes. | Referencial (fase beta) |
| Intención de pago por funcionalidad | % de usuarios que seleccionan una funcionalidad premium como "muy importante" en encuesta. | ≥ 30% por feature |

---

### 8.2.3. Measures

Las siguientes mediciones se aplicarán para evaluar el progreso de cada experimento:

#### Medidas cuantitativas

| Medida | Descripción | Fuente de datos |
|--------|-------------|-----------------|
| Tasa de completación de onboarding | % de usuarios que completan todos los pasos del flujo de alta. | Eventos de navegación (Firebase Analytics) |
| Tasa de abandono por pantalla | % de usuarios que abandonan el flujo en cada paso específico. | Funnel en Firebase / Mixpanel |
| Tiempo hasta primer gasto registrado | Días transcurridos entre el registro y el primer gasto creado. | Base de datos (tabla `bills`) |
| % de aportes registrados a tiempo | Contribuciones pagadas antes de `fecha_limite` / total de contribuciones. | Base de datos (tabla `member_contributions`) |
| Frecuencia de uso semanal | Promedio de sesiones por usuario activo por semana. | Firebase Analytics |
| CTR en botón de registro (landing page) | Clics en CTA / visitantes únicos de la landing. | Google Analytics / Firebase |

#### Medidas cualitativas

| Medida | Descripción | Instrumento |
|--------|-------------|-------------|
| Percepción de equidad | Grado en que el usuario considera justa la distribución de aportes. | Encuesta Likert in-app (1-5) |
| Satisfacción general (CSAT) | Valoración de la experiencia general con la aplicación. | Encuesta post-sesión |
| Net Promoter Score (NPS) | Probabilidad de recomendar Budgetly a otros. | Encuesta mensual in-app |
| Percepción de facilidad de uso | Grado de facilidad percibida al completar tareas clave. | System Usability Scale (SUS) |
| Intención de pago premium | Disposición declarada a pagar por funcionalidades avanzadas. | Encuesta de valoración de features |

---

### 8.2.4. Conditions

Cada experimento se ejecuta bajo condiciones controladas para garantizar la validez de los resultados:

#### Condiciones generales

- Todos los experimentos se ejecutan sobre usuarios reales del entorno de producción (no staging).
- Los usuarios asignados a grupos de control y tratamiento deben ser mutuamente excluyentes.
- La asignación a grupos A/B se realizará de forma aleatoria mediante un hash del `user_id` para garantizar distribución uniforme.
- Los experimentos no se superpondrán sobre el mismo segmento de usuarios simultáneamente para evitar efectos de interferencia.

#### Condiciones por experimento

| Experimento | Grupo Control | Grupo Tratamiento | Variable controlada |
|-------------|---------------|-------------------|---------------------|
| EXP-01 (Onboarding) | Flujo actual sin cambios | Flujo con barra de progreso y tips contextuales | Solo se modifica la UI del flujo de alta |
| EXP-02 (Recordatorios) | Sin notificación de recordatorio | Notificación push + email a 48h del vencimiento | Mismo perfil de hogar y monto de aporte |
| EXP-03 (Gráfico vs. texto) | Panel en texto plano (tabla) | Panel con gráfico circular interactivo | Mismos datos de aportes para ambos grupos |
| EXP-04 (Simulador landing) | Landing page actual | Landing page con simulador interactivo | Mismo tráfico de entrada por canal |

#### Criterios de exclusión

- Usuarios registrados hace menos de 48 horas (no tienen suficiente historial).
- Hogares con un solo miembro (no aplica la lógica de distribución de aportes).
- Usuarios que ya estén participando en otro experimento activo.

---

### 8.2.5. Scale Calculations and Decisions

Para determinar el tamaño de muestra necesario y la duración de cada experimento, se aplican cálculos básicos de significancia estadística.

#### Parámetros generales

- **Nivel de confianza:** 95% (α = 0.05)
- **Potencia estadística:** 80% (β = 0.20)
- **Efecto mínimo detectable (MDE):** 10-15% de mejora relativa según el experimento

#### Cálculo de muestra para EXP-02 (Recordatorios)

- Tasa de pagos a tiempo en grupo control estimada: **50%**
- Mejora mínima esperada: **+20 puntos porcentuales** (hasta 70%)
- Tamaño de muestra requerido por grupo: **~95 usuarios** (calculado con fórmula de proporción binomial)
- Total requerido: **~190 usuarios activos con aportes pendientes**
- Estimación de tiempo: **6 semanas** asumiendo ~35 nuevos usuarios activos por semana

#### Cálculo de muestra para EXP-04 (Simulador en landing)

- Tasa de conversión actual estimada: **8%**
- Mejora mínima esperada: **+15% relativo** (hasta ~9.2%)
- Tamaño de muestra requerido por grupo: **~2,800 visitantes**
- Total requerido: **~5,600 visitantes únicos**
- Estimación de tiempo: **4-6 semanas** con estrategia de difusión en redes sociales y comunidades financieras

#### Decisiones de escala

| Experimento | Muestra mínima | Duración estimada | Decisión si MDE no se alcanza |
|-------------|----------------|-------------------|-------------------------------|
| EXP-01 | 150 nuevos usuarios | 4 semanas | Revisar el paso de mayor abandono y rediseñar |
| EXP-02 | 190 usuarios activos | 6 semanas | Probar con recordatorio a 24h en lugar de 48h |
| EXP-03 | 100 usuarios activos | 3 semanas | Mantener versión de texto y deprioritizar gráfico |
| EXP-04 | 5,600 visitantes | 4-6 semanas | Evaluar inversión en pauta pagada para acelerar |

---

### 8.2.6. Methods Selection

La selección de métodos combina técnicas cuantitativas y cualitativas para obtener una visión integral del comportamiento del usuario.

#### Métodos cuantitativos

| Método | Aplicación en Budgetly |
|--------|----------------------|
| **Análisis de embudo (Funnel Analysis)** | Identificar dónde abandonan los usuarios en el onboarding. Implementado con Firebase Analytics + eventos personalizados. |
| **Prueba A/B** | Comparar variantes de interfaz (gráfico vs. texto, landing con/sin simulador). Asignación aleatoria por hash de `user_id`. |
| **Análisis de cohortes** | Medir la retención Day 7 y Day 30 por cohorte de registro (semana de alta). |
| **Análisis de correlación** | Evaluar si la actividad del Representante del Hogar correlaciona con la retención de los miembros. |
| **Métricas de eventos (Event Tracking)** | Registrar acciones clave: registro, creación de hogar, primer gasto, primer aporte registrado, uso de gráficos, apertura de notificaciones. |

#### Métodos cualitativos

| Método | Aplicación en Budgetly |
|--------|----------------------|
| **Encuestas in-app** | Medir NPS, CSAT, percepción de equidad y satisfacción con el onboarding. Activadas por evento (primer gasto, día 7, día 30). |
| **Pruebas de usabilidad (moderadas)** | Sesiones de 30-45 minutos con 5-8 usuarios nuevos que completan tareas clave (registrarse, crear hogar, registrar gasto, verificar aportes). |
| **Entrevistas de seguimiento** | Entrevistas breves (15 min) con usuarios que abandonaron el onboarding para entender las barreras. |
| **Análisis de mapas de calor (Heatmaps)** | Identificar zonas de interacción y fricción en la landing page y en el dashboard principal mediante Hotjar o herramienta equivalente. |

#### Criterio de selección

Los métodos cuantitativos se priorizan para validar hipótesis con criterios numéricos claros (EXP-01, EXP-02, EXP-04). Los métodos cualitativos complementan cuando los datos cuantitativos muestran anomalías o cuando se necesita comprender el "por qué" detrás del comportamiento observado (especialmente en casos de alta tasa de abandono).

---

### 8.2.7. Data Analytics: Goals, KPIs and Metrics Selection

#### Goals (Objetivos de análisis)

| Goal | Descripción | Vinculado a |
|------|-------------|-------------|
| G1 | Maximizar la tasa de activación de nuevos usuarios | Hipótesis 3 |
| G2 | Reducir los pagos atrasados entre miembros del hogar | Hipótesis 2 |
| G3 | Aumentar la percepción de equidad en la distribución de gastos | Hipótesis 1 |
| G4 | Incrementar la retención mensual de hogares activos | Hipótesis general de producto |
| G5 | Generar intención de conversión al plan premium | Hipótesis 5 |

#### KPIs y Métricas por objetivo

**G1 – Activación**

| KPI | Métrica asociada | Herramienta |
|-----|-----------------|-------------|
| Tasa de completación de onboarding | % usuarios que completan registro → hogar → primer gasto | Firebase Analytics (funnel) |
| Tiempo hasta activación | Horas desde registro hasta primer gasto | Base de datos (`bills.created_at` vs `users.created_at`) |
| Tasa de abandono por paso | % de usuarios que abandonan en cada pantalla del flujo | Firebase + eventos personalizados |

**G2 – Reducción de pagos atrasados**

| KPI | Métrica asociada | Herramienta |
|-----|-----------------|-------------|
| % aportes registrados a tiempo | `member_contributions` con `pagado_en` ≤ `contributions.fecha_limite` | Base de datos (query SQL) |
| Tasa de apertura de notificaciones | % de notificaciones push abiertas vs. enviadas | Firebase Cloud Messaging |
| Tiempo desde recordatorio a pago | Horas entre envío de notificación y registro del pago | Logs de notificaciones + BD |

**G3 – Percepción de equidad**

| KPI | Métrica asociada | Herramienta |
|-----|-----------------|-------------|
| Puntuación de equidad percibida | Escala Likert 1-5 en encuesta in-app | Encuesta in-app (formulario nativo) |
| NPS (Net Promoter Score) | Escala 0-10 en encuesta mensual | Encuesta in-app mensual |
| Tiempo en pantalla de aportes | Segundos en el panel de distribución de aportes | Firebase Analytics |

**G4 – Retención**

| KPI | Métrica asociada | Herramienta |
|-----|-----------------|-------------|
| Retención Day 7 | % usuarios con sesión en días 6-8 post-registro | Análisis de cohortes (Firebase) |
| Retención Day 30 | % usuarios con sesión en días 28-32 post-registro | Análisis de cohortes (Firebase) |
| Hogares Activos Mensuales (MAH) | Hogares con ≥1 gasto registrado en el mes | Base de datos |
| Frecuencia de sesiones semanales | Promedio de sesiones por usuario activo por semana | Firebase Analytics |

**G5 – Intención de conversión premium**

| KPI | Métrica asociada | Herramienta |
|-----|-----------------|-------------|
| Intención de pago declarada | % usuarios que seleccionan "muy importante" en encuesta de features premium | Encuesta in-app |
| Clics en "Ver plan premium" | CTR en botón de upgrade desde la UI | Firebase Analytics (evento `premium_view_click`) |
| Funcionalidades más valoradas | Ranking de features premium por puntuación media | Encuesta in-app |

---

### 8.2.8. Web and Mobile Tracking Plan

El plan de tracking define todos los eventos que deben instrumentarse en la **Web Application (Vue.js)** y la **Mobile Application (Flutter)** para alimentar los experimentos y métricas definidos.

#### Convenciones de nomenclatura de eventos

Se utiliza la convención `snake_case` con prefijo de módulo: `{modulo}_{accion}_{objeto}`.

Ejemplo: `auth_completed_registration`, `household_created_home`, `expense_registered_bill`.

---

#### Eventos de Autenticación (IAM)

| Evento | Descripción | Propiedades |
|--------|-------------|-------------|
| `auth_viewed_landing` | Usuario visita la landing page | `source`, `device`, `timestamp` |
| `auth_clicked_register_cta` | Usuario hace clic en el botón de registro | `cta_location` (hero/navbar/footer), `variant` (A/B) |
| `auth_started_registration` | Usuario inicia el formulario de registro | `role_selected` (representante/miembro) |
| `auth_completed_registration` | Usuario completa el registro exitosamente | `role`, `method` (email), `timestamp` |
| `auth_failed_registration` | Registro fallido | `error_type`, `step_failed` |
| `auth_logged_in` | Inicio de sesión exitoso | `role`, `device_type` |
| `auth_logged_out` | Cierre de sesión | `session_duration_minutes` |

---

#### Eventos de Onboarding

| Evento | Descripción | Propiedades |
|--------|-------------|-------------|
| `onboarding_viewed_step` | Usuario ve un paso del onboarding | `step_number`, `step_name` |
| `onboarding_completed_step` | Usuario completa un paso | `step_number`, `step_name`, `time_spent_seconds` |
| `onboarding_abandoned_step` | Usuario abandona en un paso | `step_number`, `step_name`, `time_spent_seconds` |
| `onboarding_completed_full` | Usuario completa todo el flujo de alta | `total_time_minutes`, `device_type` |

---

#### Eventos de Hogares (Households)

| Evento | Descripción | Propiedades |
|--------|-------------|-------------|
| `household_created_home` | Representante crea un hogar | `household_id`, `currency` |
| `household_joined_home` | Miembro se une a un hogar | `household_id`, `method` (ID/invitación) |
| `household_invited_member` | Representante envía invitación | `household_id`, `invite_method` |
| `household_accepted_invitation` | Miembro acepta una invitación | `household_id`, `time_to_accept_hours` |
| `household_viewed_members` | Usuario consulta la lista de miembros | `household_id`, `member_count` |

---

#### Eventos de Gastos (Bills & Expenses)

| Evento | Descripción | Propiedades |
|--------|-------------|-------------|
| `expense_viewed_list` | Usuario ve la lista de gastos | `household_id`, `filter_applied` |
| `expense_started_registration` | Usuario abre formulario de nuevo gasto | `household_id` |
| `expense_completed_registration` | Usuario registra un gasto exitosamente | `household_id`, `amount`, `category`, `has_attachment` |
| `expense_abandoned_registration` | Usuario abandona el formulario | `step_abandoned`, `time_spent_seconds` |
| `expense_viewed_charts` | Usuario accede a la vista de gráficos | `chart_type`, `period_selected` |
| `expense_downloaded_report` | Usuario descarga un reporte PDF | `period`, `report_type` |

---

#### Eventos de Contribuciones (Contributions)

| Evento | Descripción | Propiedades |
|--------|-------------|-------------|
| `contribution_viewed_my_amount` | Miembro consulta su monto a pagar | `household_id`, `amount`, `days_to_deadline` |
| `contribution_registered_payment` | Miembro registra un pago | `household_id`, `amount`, `method`, `on_time` (bool) |
| `contribution_viewed_history` | Usuario consulta historial de pagos | `period_selected`, `payment_count` |
| `contribution_viewed_distribution` | Usuario ve la distribución de aportes | `view_type` (texto/gráfico), `variant` |

---

#### Eventos de Notificaciones y Recordatorios

| Evento | Descripción | Propiedades |
|--------|-------------|-------------|
| `notification_sent_reminder` | Sistema envía recordatorio de pago | `channel` (push/email), `hours_to_deadline` |
| `notification_opened_reminder` | Usuario abre el recordatorio | `channel`, `time_to_open_minutes` |
| `notification_payment_registered_after_reminder` | Pago registrado tras recordatorio | `time_after_reminder_hours` |
| `notification_configured_preferences` | Usuario ajusta sus preferencias de notificación | `channels_enabled`, `frequency` |

---

#### Eventos de Encuestas y NPS

| Evento | Descripción | Propiedades |
|--------|-------------|-------------|
| `survey_viewed_nps` | Usuario ve la encuesta NPS | `trigger` (day7/day30/monthly) |
| `survey_completed_nps` | Usuario responde la encuesta NPS | `score`, `comment_provided` |
| `survey_viewed_equity` | Usuario ve la encuesta de equidad percibida | `trigger_context` |
| `survey_completed_equity` | Usuario responde la encuesta de equidad | `equity_score` (1-5) |
| `survey_viewed_premium_features` | Usuario ve la encuesta de features premium | — |
| `survey_completed_premium_features` | Usuario responde la encuesta de features | `top_feature_selected`, `payment_intent` |

---

#### Implementación técnica

| Plataforma | Herramienta de tracking | SDK |
|------------|------------------------|-----|
| Web Application (Vue.js) | Firebase Analytics | `firebase/analytics` JS SDK |
| Mobile Application (Flutter) | Firebase Analytics | `firebase_analytics` Flutter plugin |
| Landing Page | Google Analytics 4 + Firebase | `gtag.js` |
| Backend (ASP.NET Core) | Logs estructurados (Application Insights / Azure Monitor) | `Microsoft.ApplicationInsights` |

#### Consideraciones de privacidad

- Todos los eventos de tracking son **anónimos o seudonimizados**; no se almacena PII (información de identificación personal) en los payloads de eventos.
- El `user_id` utilizado en el tracking es un identificador interno de la plataforma, no vinculado a datos personales en los sistemas de analítica.
- Se presentará al usuario un aviso de cookies y tracking en el primer acceso, con opción de opt-out para analítica no esencial, en cumplimiento con la política de privacidad declarada en el Acuerdo de Servicio SaaS (sección 5.2.4).
- El plan de tracking será revisado y actualizado con cada nuevo experimento para asegurar que las métricas recopiladas son las mínimas necesarias para la toma de decisiones.
