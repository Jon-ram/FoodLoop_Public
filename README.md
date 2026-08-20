# FoodLoop

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![TailwindCSS](https://img.shields.io/badge/TailwindCSS-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)
![Google Gemini](https://img.shields.io/badge/Google_Gemini-8E75B2?style=for-the-badge&logo=googlegemini&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)

**FoodLoop** es una plataforma web que utiliza inteligencia artificial (Gemini/OpenAI) para generar recetas
personalizadas basadas en los ingredientes que el usuario tiene disponibles. El objetivo principal
es reducir el desperdicio de alimentos ayudando a las personas a aprovechar al máximo los
ingredientes en su despensa, además de apoyarlas a controlar su presupuesto de compra semanal.

Proyecto desarrollado por estudiantes de la **Universidad Tecnológica de Xicotepec de Juárez (UTXJ)**,
Ingeniería en Desarrollo y Gestión de Software, grupo 9B.

---

## Descripción del proyecto

FoodLoop nace como respuesta a un problema cotidiano: gran parte de los alimentos que se compran
terminan por vencerse sin usarse, generando desperdicio de comida y gasto innecesario. La plataforma
conecta tres piezas clave:

1. **Registro de ingredientes** disponibles en la despensa del usuario (manual o por imagen).
2. **Generación de recetas con IA** (Gemini como motor principal, OpenAI como respaldo) a partir de esos
   ingredientes.
3. **Planeación y presupuesto**: recetas planificadas, lista de compra semanal inteligente y control de
   gasto mensual, pensado también para integrarse con un smartwatch u otro cliente externo.

Como parte del Proyecto Integrador de **Extracción de Conocimiento en Bases de Datos**, FoodLoop suma un
módulo de Machine Learning que complementa la IA generativa con modelos propios entrenados sobre datos
del sistema (despensa, presupuesto, recetas planificadas).

---

## Justificación del problema

El desperdicio de alimentos en México es un problema de gran escala. De acuerdo con estimaciones de la
FAO, en el país se pierden o desperdician cada año cerca de 20 millones de toneladas de alimentos,
alrededor de una tercera parte de la producción nacional. A nivel individual, distintos estudios
calculan que cada mexicano desperdicia entre 90 y 105 kilogramos de comida al año, principalmente por
mala planificación en el hogar.

El impacto no es solo económico: también es ambiental, ya que el desperdicio de alimentos en México
genera decenas de millones de toneladas de CO2 al año, una huella comparable a la de millones de
automóviles en circulación. A nivel mundial, los hogares concentran la mayor parte de este desperdicio
frente al sector comercial y de servicios.

FoodLoop busca atacar directamente una de las causas más señaladas por especialistas: la falta de
planificación de compras y de aprovechamiento de lo que ya se tiene en casa, ofreciendo una herramienta
que convierte los ingredientes disponibles en recetas concretas y ayuda a planear la compra semanal
antes de que los alimentos se echen a perder.

**Fuentes:** [La Jornada de Oriente — FAO/UNAM](https://www.lajornadadeoriente.com.mx/noticias/economia_y_ecologia/el-sistema-alimentario-en-mexico-inseguridad-y-desperdicio/) ·
[Infobae — UNAM/Banco Mundial](https://www.infobae.com/mexico/2026/02/08/mexico-desperdicia-millones-de-toneladas-de-alimentos-cada-ano-y-contribuye-al-calentamiento-global-advierte-la-unam/)

---

## Misión

Ayudar a las personas a reducir el desperdicio de alimentos y optimizar su gasto en despensa, mediante
tecnología accesible que convierte lo que ya tienen en casa en soluciones prácticas para comer mejor y
gastar menos.

##  Visión

Convertirnos en la herramienta de referencia para la planeación inteligente de comidas y compras en el
hogar, integrando inteligencia artificial y análisis de datos para que cada vez más personas y familias
reduzcan su desperdicio de alimentos de forma simple y cotidiana.

---

##  Objetivos

### Objetivo general

Desarrollar una plataforma web inteligente que, a partir de los ingredientes disponibles del usuario,
genere recomendaciones de recetas, planificación de comidas y control de presupuesto, reduciendo el
desperdicio de alimentos y facilitando la toma de decisiones de compra.

### Objetivos específicos

- Generar recetas personalizadas usando IA generativa a partir de los ingredientes registrados por el usuario.
- Detectar ingredientes a partir de imágenes para agilizar el registro en la despensa.
- Permitir la planificación de recetas y la generación automática de listas de compra semanales.
- Ofrecer control de presupuesto mensual con alertas por umbral de gasto.
- Incorporar modelos de Machine Learning propios (riesgo de desperdicio, proyección de gasto,
  segmentación de usuarios y detección de anomalías de gasto) que complementen la IA generativa.
- Facilitar la integración de la experiencia de usuario con un smartwatch u otro dispositivo externo.

---

##  Público objetivo

FoodLoop está dirigido principalmente a:

- **Personas y familias que cocinan en casa** y buscan aprovechar mejor lo que compran, sin necesidad
  de planear con anticipación cada receta.
- **Estudiantes y personas que viven solas**, con presupuesto ajustado y despensas pequeñas, donde el
  desperdicio de un solo ingrediente tiene más impacto proporcional.
- **Personas interesadas en llevar un control de gasto** de sus compras de comida, más allá de solo
  cocinar con lo que tienen.
- Usuarios con disposición a usar tecnología (app web, eventualmente smartwatch) como apoyo en tareas
  cotidianas del hogar.

---

##  Análisis de competencia

| Aplicación | Enfoque principal | Diferencia con FoodLoop |
|---|---|---|
| Yummly | Recomendación de recetas y planeación de menús | No se centra en reducir desperdicio ni en el control de presupuesto de despensa |
| SuperCook | Genera recetas a partir de ingredientes que el usuario ya tiene | No integra presupuesto, lista de compra inteligente ni modelos de ML sobre hábitos del usuario |
| Too Good To Go | Conecta comida próxima a caducar de comercios con consumidores | Ataca el desperdicio del lado comercial, no ayuda a planear ni cocinar en casa |
| Apps de listas de compra genéricas | Organizan lo que se necesita comprar | No cruzan despensa, vencimientos, recetas planificadas y presupuesto en un solo flujo |

**Ventaja diferenciadora de FoodLoop:** combina generación de recetas con IA, control de presupuesto,
lista de compra inteligente cruzada con vencimientos y recetas planificadas, y un módulo de Machine
Learning propio que aprende de los hábitos del usuario — algo que ninguna de las alternativas anteriores
ofrece de forma integrada.

---

##  Alcance del proyecto

**Incluye:**
- Registro, autenticación y gestión de cuenta de usuario.
- Registro de ingredientes (manual y por detección de imagen).
- Generación de recetas mediante IA (Gemini/OpenAI).
- Presupuesto mensual, recetas planificadas y lista de compra semanal inteligente.
- Guía de receta paso a paso, disponible desde web y desde un cliente externo tipo smartwatch.
- Módulo de Machine Learning con mecanismos supervisados y no supervisados, entrenados sobre un
  dataset sintético documentado.

**No incluye (por ahora):**
- Una aplicación nativa completa para smartwatch (se usa un cliente mínimo/emulador).
- Procesamiento de pagos reales (las suscripciones se manejan como registro, no como pasarela de pago activa).
- Entrenamiento de modelos de ML con datos 100% reales (se documenta como limitación mientras crece el uso).

---

##  Gestión del alcance

Para mantener el alcance controlado durante el desarrollo se sigue este criterio:

- Toda funcionalidad nueva se evalúa contra los objetivos específicos antes de aceptarse.
- Los cambios importantes (nuevas tecnologías, nuevos mecanismos de ML, nuevos módulos) se documentan
  primero antes de implementarse.
- El roadmap (ver sección **Estado de funcionalidades**) se actualiza en cada entrega, marcando qué está
  completo, en progreso o pendiente.
- Cambios que excedan el alcance definido arriba (ej. pagos reales, app nativa completa) quedan fuera de
  esta entrega y se registran como trabajo futuro, no como alcance actual.

---

##  Metodología de trabajo

El equipo trabaja bajo metodología ágil (**Scrum**) adaptada al contexto académico:

- Levantamiento de requerimientos funcionales y no funcionales, y definición de alcance/visión del producto.
- **Product Backlog** con historias de usuario, criterios de aceptación y priorización por sprint.
- **Tablero de tareas** (Trello / ClickUp) para dar seguimiento al avance de cada actividad.
- **Git Flow** como estrategia de ramas: una rama por funcionalidad, revisión antes de integrar a
  `develop`, y `main` reservada para versiones estables. Todo Pull Request requiere revisión de un
  integrante distinto al autor antes del merge.
- **Entregas incrementales**, alineadas a las fases del cronograma, en vez de un único entregable al final.

---

##  Gestión de interesados

Principales interesados (stakeholders) identificados en el proyecto:

- **Equipo de desarrollo:** integrantes responsables de construir y documentar la plataforma.
- **Usuarios finales:** personas encuestadas durante la fase de Empatizar del Design Thinking, cuyas
  necesidades guían el diseño de la solución.
- **Docente / evaluador académico:** receptor formal de los entregables y responsable de validar el
  cumplimiento de objetivos.
- **Proveedores de servicios externos:** APIs de Google Gemini, OpenAI, Spoonacular y Edamam, como
  socios clave identificados en el Business Model Canvas.


---

##  Gestión de costos

Al ser un proyecto académico, no se manejan costos monetarios formales. Los "costos" reales del proyecto son:

- Horas de trabajo del equipo por sprint/entrega.
- Consumo de planes gratuitos o académicos de las APIs externas (Google Gemini, OpenAI).
- Herramientas de colaboración sin costo (GitHub, Figma, Trello/ClickUp, Google Drive).

##  Gestión de adquisiciones

Las únicas "adquisiciones" del proyecto son servicios externos necesarios para su funcionamiento:

- **Proveedores de IA:** Google Gemini (principal) y OpenAI (respaldo).
- **Herramientas de infraestructura y colaboración:** GitHub, Figma y Tailscale (para entornos remotos
  de desarrollo).


##  Matriz de riesgos

| Riesgo | Probabilidad | Impacto | Mitigación |
|---|---|---|---|
| Retraso en diseño de interfaz por iteraciones de usabilidad | Media | Medio | Reorganización de actividades y revisión semanal del backlog |
| Integración entre frontend (React) y backend (Node.js/Express) | Baja | Alto | Definición temprana de contratos de API y pruebas de integración |
| Bajo volumen de datos reales para entrenar los modelos de ML | Alta | Alto | Uso de dataset sintético documentado y reproducible mientras crece el uso real |
| Dependencia de APIs externas de IA (Gemini/OpenAI) | Media | Alto | Gemini como motor principal y OpenAI como respaldo automático ante fallos |
| Retrasos por carga académica del equipo | Media | Medio | Entregas incrementales y reuniones semanales para detectar bloqueos a tiempo |

---

##  Tecnologías utilizadas

| Capa | Tecnología |
|---|---|
| Frontend | React 19, TypeScript, Vite, Tailwind CSS, React Router |
| Backend | Node.js, Express |
| Autenticación | JWT, bcrypt |
| Email | Nodemailer |
| IA generativa | Gemini (principal) / OpenAI (respaldo) |
| Machine Learning | Python, scikit-learn, pandas |
| APIs de recetas (roadmap) | Spoonacular / Edamam |
| Control de versiones | Git Flow sobre GitHub |

---

##  Módulo de Inteligencia Artificial 
FoodLoop incorpora un módulo de Machine Learning que complementa la generación de recetas por IA
generativa con modelos entrenados sobre datos propios del sistema, siguiendo la metodología **CRISP-DM**.

Se seleccionaron 2 mecanismos supervisados y 2 no supervisados para esta entrega:

| Tipo | Mecanismo | Para qué sirve |
|---|---|---|
| Supervisado — Clasificación | Predicción de riesgo de desperdicio | Estima qué tan probable es que un ingrediente se venza sin usarse |
| Supervisado — Regresión | Proyección de gasto mensual | Proyecta el gasto total al cierre del mes según el avance actual |
| No supervisado — Agrupamiento | Segmentación de usuarios | Agrupa usuarios por hábitos de consumo para personalizar recomendaciones |
| No supervisado — Anomalías | Detección de anomalías en gasto semanal | Detecta semanas donde el gasto se dispara fuera de lo normal |

---

##  Funcionalidades principales

- Registro y login de usuarios, con control de intentos y seguridad básica.
- Generación de recetas por IA y guardado de historial.
- Detección de ingredientes desde imagen, con precio estimado.
- Presupuesto mensual con alertas por umbral.
- Recetas planificadas y lista de compra semanal inteligente.
- Guía de receta paso a paso, pensada para seguirse desde web o desde un dispositivo externo (smartwatch).

---

## Modelos de alta fidelidad
 
Prototipos y versiones del producto:
 
| Modelo | Enlace |
|---|---|
| Versión web | [Ver](https://www.figma.com/design/3ULsuEsxu20uYFEtoq3jGw/FoodLoop?node-id=0-1&t=3SVNSh5daCMtQzkF-1) |
| Versión móvil | [Ver](https://www.figma.com/design/3ULsuEsxu20uYFEtoq3jGw/FoodLoop?node-id=0-1&t=3SVNSh5daCMtQzkF-1) |
| Versión smartwatch | [Ver](https://www.figma.com/design/3ULsuEsxu20uYFEtoq3jGw/FoodLoop?node-id=0-1&t=3SVNSh5daCMtQzkF-1) |


##  Seguridad

- Autenticación de usuarios basada en tokens, con rutas protegidas.
- Contraseñas siempre almacenadas con hashing (nunca en texto plano).
- Validación de datos en registro/login.
- Control de acceso por roles (usuario / administrador).
- Protección contra abuso: límite de intentos y registro de accesos fallidos.
- Recuperación de contraseña mediante enlaces de un solo uso con expiración.
- Buenas prácticas: secretos y claves siempre fuera del repositorio.

---

##  Documentación del proyecto

Toda la documentación formal del proyecto está disponible en el repositorio, dentro de la carpeta [`Documentation`](https://github.com/Jon-ram/FoodLoop_Public/tree/main/Documentation):

| Documento | Contenido |
|---|---|
| [Propuesta de valor](https://github.com/Jon-ram/FoodLoop_Public/blob/main/Documentation/FoodLoop_Propuestas_de_Valor%20(1).pdf) | Modelo de negocio, propuesta de valor y segmentos de usuario |
| [Requerimientos](https://github.com/Jon-ram/FoodLoop_Public/blob/main/Documentation/FoodLoop_Requerimientos.pdf) | Requerimientos funcionales y no funcionales del sistema |
| [Documentación técnica](https://github.com/Jon-ram/FoodLoop_Public/blob/main/Documentation/Documentaci%C3%B3n%20T%C3%A9cnica%20del%20Proyecto%20Foodloopdocx.pdf) | Arquitectura, base de datos y documentación técnica del sistema |
| [Documentación de servicios web](https://github.com/Jon-ram/FoodLoop_Public/blob/main/Documentation/foodloop_webServices.pdf) | Documentación de la API y los servicios web del sistema |
| [Estimación de esfuerzo](https://github.com/Jon-ram/FoodLoop_Public/blob/main/Documentation/FoodLoop_Estimacion_Esfuerzo.xlsx) | Estimación de esfuerzo de desarrollo del proyecto |
| [Diagrama de actividades](https://github.com/Jon-ram/FoodLoop_Public/blob/main/Documentation/Diagrama_actividades.xlsx) | Diagrama de actividades del proyecto |
| [Business Model Canvas](https://github.com/Jon-ram/FoodLoop_Public/blob/main/Documentation/Reportes/Modelo_Canvas_FoodLoop.pdf) | Socios clave, propuesta de valor y segmentos de usuario |
| [Informe de avances](https://github.com/Jon-ram/FoodLoop_Public/blob/main/Documentation/Reportes/Informe_de_Avances_FoodLoop.pdf) | Informe de avances para monitoreo y control del proyecto |
| [Bitácora de actividades](https://github.com/Jon-ram/FoodLoop_Public/blob/main/Documentation/Reportes/Bit%C3%A1cora%20de%20Actividades%20FoodLoop.pdf) | Bitácora de hitos y actividades del equipo |
| [Diagramas del proyecto](https://github.com/Jon-ram/FoodLoop_Public/blob/main/Documentation/Reportes/Diagramas%20FoodLoop.pdf) | Diagramas de arquitectura y modelo entidad-relación |
| [Plan de comunicación](https://github.com/Jon-ram/FoodLoop_Public/blob/main/Documentation/Reportes/Plan_de_Acuerdos_Completo_FoodLoop_v3.pdf) | Plan de acuerdos de comunicación del equipo |
| [Buenas Practicas y Roles](https://github.com/Jon-ram/FoodLoop_Public/blob/main/Documentation/FoodLoop_Buenas_Practicas_Roles.pdf) | Buenas prácticas y matriz de roles y responsabilidades del equipo |



---


##  Estado de funcionalidades

| Funcionalidad | Estado |
|---|---|
| Registro y login de usuarios | ✅ Completo |
| Generación de recetas con IA | ✅ Completo |
| Detección de ingredientes por imagen | ✅ Completo |
| Marcado de favoritos | ✅ Completo |
| Presupuesto mensual con alertas | ✅ Completo |
| Recetas planificadas | ✅ Completo |
| Lista de compra semanal inteligente | ✅ Completo |
| Versión móvil y smartwatch | 🔜 Casi completo |
| Integración con smartwatch (compra + presupuesto) | ✅ Completo |
| Guía de receta paso a paso (web/smartwatch) | ✅ Completo |
| Predicción de riesgo de desperdicio (ML) | ⏳ Pendiente |
| Proyección de gasto mensual (ML) | ⏳ Pendiente |
| Segmentación de usuarios (ML) | ⏳ Pendiente |
| Detección de anomalías de gasto (ML) | ⏳ Pendiente |

---

##  Próximas mejoras

1. Finalizar la versión móvil y de smartwatch, actualmente casi completa.
2. Módulo de Machine Learning (`ml_service/`) con los 4 mecanismos seleccionados.
3. Tests unitarios y e2e para backend y `ml_service/`.
4. Refresh tokens y mejora de la estrategia de sesiones.
5. Endurecer el flujo de envío de emails en producción.

---

## Contribuidores

|Integrante|Contacto|Rol|
|----------|-------|---|
|Jonathan Baldemar Ramirez Reyes|[@Jon-ram](https://github.com/Jon-ram)|Desarrollador de DataBases|
|Christian Paul Rodriguez Perez|[@ChrisRodriguez-0430](https://github.com/ChrisRodriguez-0430)|Desarrollador BackEnd y LLM|
|Josue Martinez Otero|[@Josue-Martinez-Otero](https://github.com/Josue-Martinez-Otero)|Desarrollador FontEnd|
|Jose Agustin Jimenez Castillo|[@agustin963](https://github.com/agustin963)|Ingeniero de software en el desarrollo de Wear OS |

**Universidad Tecnológica de Xicotepec de Juárez — Tecnologías de la Información**
Ingeniería en Desarrollo y Gestión de Software — Grupo 9B
