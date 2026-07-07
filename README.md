# 🍃 FoodLoop

Plataforma web con Inteligencia Artificial para la generación de recetas personalizadas a partir de los ingredientes disponibles, orientada a reducir el desperdicio de alimentos en los hogares.

---

## 📌 Inicio del Proyecto

FoodLoop nace de una problemática cotidiana: las personas suelen tener ingredientes disponibles en casa pero no saben cómo combinarlos antes de que se echen a perder, lo que contribuye al desperdicio de alimentos.

El proyecto se desarrolla desde el cuatrimestre anterior, periodo en el cual se construyó un **prototipo funcional** que fue presentado en la feria de ciencias. En el cuatrimestre actual, el trabajo se ha enfocado en la documentación formal, el refinamiento del diseño y el fortalecimiento de la arquitectura técnica del sistema.

- **Periodo de este informe:** 01/06/2026 - 30/06/2026
- **Avance general del proyecto:** ~75%
- **Riesgo general:** Bajo
- **Cumplimiento de cronograma:** En tiempo

---

## 🎯 Gestión de Alcance

El alcance del proyecto se definió mediante metodología ágil (Scrum), incluyendo:

- Levantamiento de requerimientos funcionales y no funcionales.
- Definición del alcance y visión del producto.
- Elaboración del **Product Backlog**, con historias de usuario, criterios de aceptación y priorización según objetivos de sprint.
- Definición de roles e hitos del equipo.

El alcance contempla el desarrollo completo de la plataforma web (frontend, backend, base de datos e integración con IA), así como toda la documentación académica y técnica correspondiente.

---

## 🛠️ Tecnologías Utilizadas

| Categoría | Tecnología |
|---|---|
| Frontend | React + TypeScript |
| Backend | Node.js + Express / FastAPI |
| Base de datos | PostgreSQL |
| Inteligencia Artificial | Google Gemini (principal), OpenAI (respaldo) |
| APIs externas de recetas | Spoonacular, Edamam |
| Diseño UI/UX | Figma |
| Control de versiones | GitHub (estrategia Git Flow) |
| Gestión de tareas | Trello / ClickUp |
| Comunicación | WhatsApp, Google Meet |
| Documentación | Google Docs / Drive, Swagger (`/api-docs`) |
| Redes remotas | Tailscale (entornos locales de base de datos) |
| Autenticación y seguridad | JWT, bcrypt, express-validator |
| Carga de archivos | Multer (detección de ingredientes por imagen) |
| Testing (roadmap) | Jest / Supertest |

---

## 📖 Descripción del Proyecto

**FoodLoop** es una plataforma web que utiliza inteligencia artificial para generar recetas personalizadas a partir de los ingredientes que el usuario tiene disponibles, con el objetivo de reducir el desperdicio de alimentos en los hogares.

El usuario ingresa los ingredientes con los que cuenta y el sistema, apoyado en modelos de IA (Google Gemini como proveedor principal y OpenAI como respaldo), genera recetas prácticas y adaptadas a esos insumos. La plataforma integra además fuentes de datos externas (Spoonacular, Edamam) para enriquecer las recomendaciones.

**Funcionalidades actuales del backend:**

- Registro y login de usuarios con validación de datos.
- Autenticación con JWT y middleware de rutas protegidas.
- Generación de recetas mediante IA, con guardado en PostgreSQL.
- Detección de ingredientes a partir de imágenes (carga de archivos con Multer).
- Guardado y consulta de recetas e ingredientes, incluyendo marcado de favoritos.
- Rate limiting y registro de intentos de login para mitigar ataques de fuerza bruta.
- Documentación de la API con Swagger disponible en `/api-docs`.

---

## 📚 [Documentación](https://github.com/Jon-ram/FoodLoop_Public/tree/main/Documentation)

Entregables generados durante el proyecto:

- Informe de Avances para Monitoreo y Control de Proyectos TI.
- Business Model Canvas (socios clave, propuesta de valor, segmentos de usuario).
- Documento de Design Thinking — fases de Empatizar y Definir, basado en encuesta real a usuarios (14 respuestas).
- Product Backlog priorizado y definición de roles e hitos (Scrum).
- Prototipo de alta fidelidad en Figma (wireframes + prototipo interactivo).
- Bitácora de actividades del equipo.
- Plan de Acuerdos de Comunicación del Proyecto.
- Propuesta de algoritmos de Machine Learning aplicados a FoodLoop.
- Documentación técnica de base de datos (esquema, modelo entidad-relación, estrategia de respaldo y calidad de datos).
- Documentación de la API REST con Swagger (`/api-docs`), generada con JSDoc en `src/routes/`.

---

## 🏗️ Estructura del Proyecto

Vista general del monorepo:

```
FoodLoop/
├── frontend/          # Aplicación React + TypeScript
├── backend/           # Servicios Node.js/Express (detalle abajo)
├── database/          # Esquema y modelo entidad-relación PostgreSQL
├── docs/              # Documentación técnica y académica
└── design/            # Wireframes y prototipos de Figma
```

### Arquitectura de carpetas del Backend

```
├── 📁 config
│   └── 📄 database.js
├── 📁 database
│   ├── 📄 schema.sql
│   └── 📄 seed_demo.sql
├── 📁 models
│   ├── 📄 GeminiUsage.js
│   ├── 📄 Ingredient.js
│   ├── 📄 Recipe.js
│   ├── 📄 SitiVisit.js
│   ├── 📄 Subscription.js
│   └── 📄 User.js
├── 📁 src
│   ├── 📁 controllers
│   │   ├── 📄 adminController.js
│   │   ├── 📄 authController.js
│   │   ├── 📄 ingredientController.js
│   │   └── 📄 recipeController.js
│   ├── 📁 middlewares
│   │   ├── 📄 auth.js
│   │   ├── 📄 isAdmin.js
│   │   ├── 📄 trackVisit.js
│   │   ├── 📄 upload.js
│   │   └── 📄 validate.js
│   ├── 📁 routes
│   │   ├── 📄 admin.js
│   │   ├── 📄 auth.js
│   │   ├── 📄 ingredients.js
│   │   └── 📄 recipes.js
│   ├── 📁 services
│   │   ├── 📄 analyticsService.js
│   │   ├── 📄 gemineiaservice.js
│   │   └── 📄 openiaservice.js
│   └── 📄 swagger.js
├── ⚙️ .env.example.txt
├── ⚙️ .gitignore
├── 📄 app.js
├── ⚙️ package-lock.json
└── ⚙️ package.json
```

Esta vista ayuda a ubicarse rápido en el código y en qué carpeta añadir nuevas funcionalidades.

---

## 🗄️ Base de Datos

### ¿Por qué PostgreSQL?

| Característica | Beneficio para FoodLoop |
|---|---|
| Open Source | Sin costos de licencia, ideal para desarrollo |
| ACID Compliant | Garantiza integridad en transacciones críticas |
| Relacional | Perfecto para datos estructurados con relaciones claras |
| JSON / JSONB Support | Permite almacenar respuestas de IA en formato semi-estructurado |
| Escalabilidad | Soporta desde proyectos pequeños hasta aplicaciones empresariales |
| Comunidad activa | Amplia documentación y soporte |
| Seguridad | Sistema robusto de usuarios, roles y permisos |

### Tablas principales

| Tabla | Propósito |
|---|---|
| `users` | Usuarios del sistema: autenticación, roles y verificación de email |
| `login_attempts` | Control de intentos de login (detección de fuerza bruta) |
| `ingredients` | Catálogo normalizado de ingredientes |
| `user_ingredients` | Despensa personal del usuario (cantidad, unidad, caducidad) |
| `recipes` | Recetas generadas por la IA para cada usuario |
| `recipe_ingredients` | Ingredientes requeridos por cada receta |
| `recipe_steps` | Pasos de preparación ordenados |
| `subscriptions` | Suscripciones premium (estado, vigencia, pago) |
| `site_visits` | Registro de visitas (autenticadas y anónimas) para analítica |
| `gemini_usage` | Monitoreo del consumo de la API de IA (tokens y costo estimado) |

### Modelo Entidad-Relación (resumen de relaciones)

- `users` → `recipes`, `user_ingredients`, `login_attempts`, `subscriptions`, `site_visits`, `gemini_usage` — todas 1:N
- `recipes` → `recipe_ingredients`, `recipe_steps` — 1:N
- `ingredients` → `user_ingredients` — 1:N

### Estrategia de Respaldo

| Tipo | Frecuencia | Retención | Propósito |
|---|---|---|---|
| Diario | Cada 24h (7:00 AM) | 7 días | Recuperación ante fallos recientes |
| Semanal | Cada lunes | 30 días | Puntos de restauración intermedios |
| Mensual | Primer día del mes | 365 días | Archivo histórico y auditoría |

Scripts automatizados: `backup_postgres.bat`, `verify_backup.bat`, `maintenance_postgres.bat` (VACUUM/ANALYZE/REINDEX semanal) y `restore_postgres.bat` para recuperación manual ante desastres. Tiempo de recuperación estimado (RTO): **< 15 minutos**.

### Calidad de los Datos

Criterios aplicados: exactitud (restricciones CHECK y validaciones en backend), integridad (PK/FK con CASCADE/SET NULL), disponibilidad (respaldos en 3 niveles), confiabilidad (transacciones ACID y triggers de auditoría), utilidad (purga automática de datos obsoletos), accesibilidad (roles con privilegios mínimos) y normalización (diseño hasta 3NF).

---

## 🔒 Seguridad

- **Autenticación:** tokens JWT firmados con `JWT_SECRET`; rutas protegidas mediante middleware `auth`.
- **Hashing de contraseñas:** `bcrypt` con rondas configurables (`BCRYPT_ROUNDS`).
- **Validación de entradas:** `express-validator` en registro y login.
- **Control de acceso:** campo `role` en `users` + middleware `isAdmin` para rutas administrativas.
- **Protección contra abuso:** rate limiting y registro de intentos en `login_attempts`.
- **Recuperación de contraseña:** tokens de un solo uso (`reset_password_token`, `reset_password_expires`); requiere configuración SMTP.
- **Buenas prácticas:** claves y secretos gestionados vía `.env`, nunca subidos al repositorio.

---

## 🚀 Cómo Usar (API)

```bash
git clone <url-del-repositorio>
cd FoodLoop
npm install
```

Crear la base de datos y cargar esquema:

```bash
createdb -U postgres foodloop
psql -U postgres -d foodloop -f database/schema.sql
psql -U postgres -d foodloop -f database/seed_demo.sql
```

Iniciar servidor (`npm run dev`) y consultar la documentación interactiva en `http://localhost:3001/api-docs`.

**Flujo principal:** registro (`POST /api/auth/register`) → login (`POST /api/auth/login`, obtiene token JWT) → generar recetas (`POST /api/recipes/generate`, requiere token) → consultar recetas guardadas (`GET /api/recipes/user`) → detectar ingredientes por imagen (`POST /api/ingredients/detect`).

---

## ✅ Estado de Funcionalidades (Backend)

| Funcionalidad | Estado |
|---|---|
| Registro de usuarios | ✅ Completo |
| Login con JWT | ✅ Completo |
| Rate limiting | ✅ Completo |
| Generar recetas con IA | ✅ Completo |
| Guardar recetas en BD | ✅ Completo |
| Ver recetas guardadas | ✅ Completo |
| Marcar favoritos | ✅ Completo |
| Detectar ingredientes en imagen | ✅ Completo |
| Guardar ingredientes en catálogo | ⏳ Pendiente |
| Guardar ingredientes en despensa | ⏳ Pendiente |

---

## 👥 Gestión de Recursos Humanos

| Integrante | Rol Principal | Responsabilidades |
|---|---|---|
| Josué Atlai Martínez Otero | Líder de Frontend y UX/UI | Wireframes/prototipos en Figma, maquetación responsiva en React, navegación, consumo de API REST |
| José Agustín Jiménez Castillo | Desarrollador Frontend y Apoyo | Codificación de interfaces en React, estilos de componentes, pruebas de usabilidad |
| Christian Paúl Rodríguez Pérez | Líder de Backend e IA | Microservicios con FastAPI, integración con patrón Adapter para Google Gemini, lógica de negocio |
| Jonathan Baldemar Ramírez Reyes | Líder de Base de Datos | Modelo Entidad-Relación en PostgreSQL, optimización de consultas, triggers, seguridad de datos |
| Todo el equipo | Gestión y Documentación | Redacción y mantenimiento colectivo de documentación técnica, reportes y bitácoras |

### Contribuidores (GitHub)

| Integrante | GitHub | Rol |
|---|---|---|
| Jonathan Baldemar Ramírez Reyes | [@Jon-ram](https://github.com/Jon-ram) | Desarrollador de Bases de Datos |
| Christian Paúl Rodríguez Pérez | [@ChrisRodriguez-0430](https://github.com/ChrisRodriguez-0430) | Desarrollador Backend |
| Josué Martínez Otero | [@Josue-Martinez-Otero](https://github.com/Josue-Martinez-Otero) | Desarrollador Frontend |
| Antonio Ocpaco Dolores | [@agustin963](https://github.com/agustin963) | Documentador |

> Nota: José Agustín Jiménez Castillo consta como Desarrollador Frontend y Apoyo en el Plan de Acuerdos, pero no aparece en el listado de contribuidores del repositorio; Antonio Ocpaco Dolores aparece como Documentador en GitHub sin figurar en la matriz de roles original. Se recomienda validar con el equipo la lista definitiva de integrantes.

---

## 🤝 Gestión de Interesados

Principales interesados (stakeholders) identificados en el proyecto:

- **Equipo de desarrollo:** los cuatro integrantes, responsables de construir y documentar la plataforma.
- **Usuarios finales:** personas encuestadas (14 respuestas) durante la fase de Empatizar del Design Thinking, cuyas necesidades guían el diseño de la solución.
- **Docente / evaluador académico:** receptor formal de los entregables y responsable de validar el cumplimiento de objetivos.
- **Proveedores de servicios externos:** APIs de Spoonacular, Edamam, Google Gemini y OpenAI, como socios clave identificados en el Business Model Canvas.

---

## ⏱️ Gestión del Tiempo

Principales hitos registrados en la bitácora del proyecto:

| Fecha | Actividad |
|---|---|
| 28/05/2026 | Reunión de coordinación de roles del equipo |
| 03/06/2026 | Planeación de requerimientos funcionales y no funcionales |
| 05/06/2026 | Definición del alcance y visión |
| 10/06/2026 | Elaboración del Product Backlog |
| 13/06/2026 | Finalización de investigación, empatía y validación |
| 16/06/2026 | Diseño de wireframes |
| 21/06/2026 | Cierre de Requerimientos y Gestión Ágil (Scrum) |
| 23/06/2026 | Prototipo interactivo en Figma |
| 30/06/2026 | Elaboración del Informe de Avances |

El cronograma se ha cumplido en tiempo, con la arquitectura técnica como fase actualmente en proceso.

---

## 💰 Gestión de Costos

Al ser un proyecto de carácter académico, no se contemplan costos monetarios formales. Los principales "costos" del proyecto corresponden a:

- Horas de trabajo del equipo.
- Uso de planes gratuitos/académicos de las APIs externas (Spoonacular, Edamam, Google Gemini, OpenAI).
- Herramientas de colaboración sin costo (GitHub, Figma, Trello/ClickUp, Google Drive).

---

## 📦 Gestión de Adquisiciones

Las "adquisiciones" del proyecto se centran en la contratación/uso de servicios externos necesarios para el funcionamiento de la plataforma:

- **APIs de recetas:** Spoonacular y Edamam.
- **Proveedores de IA:** Google Gemini (principal) y OpenAI (respaldo).
- **Herramientas de infraestructura y colaboración:** GitHub, Figma, Tailscale (para entornos remotos de base de datos).

---

## 💬 Gestión de la Comunicación

| Tipo de comunicación | Frecuencia | Canal | Responsable |
|---|---|---|---|
| Actualización de estado (Daily Scrum) | Diaria | WhatsApp / Presencial | Todo el equipo |
| Seguimiento de tareas | Constante | Trello | Scrum Master / Líder de proyecto |
| Revisión de código e integración (PR) | Según entregas | GitHub (Git Flow) | Revisor designado |
| Sincronización técnica y bloqueos | Semanal (Lunes/Jueves) | Google Meet | Líder de proyecto |
| Prácticas de BD y consultas SQL | A demanda | Tailscale + entorno local | Encargado de base de datos |
| Redacción y control de reportes | Constante / por sprint | Google Docs / Drive | Todo el equipo |

**Normas clave de convivencia técnica:**
- Respeto a los tiempos de enfoque profundo (uso de estado "No Molestar" durante codificación).
- Contacto inmediato reservado solo para bloqueos críticos (caída de base de datos, expiración de credenciales de API).
- Centralización de decisiones estructurales en Pull Requests o tableros de tareas, no en chats informales.
- Uso obligatorio de Tailscale para conexiones remotas seguras a entornos de base de datos.
- Todo Pull Request requiere revisión de un integrante distinto al autor antes del merge.

---

## ⚠️ Gestión de Riesgos

| Riesgo | Impacto | Probabilidad | Acción tomada |
|---|---|---|---|
| Retraso en diseño UI/UX por iteraciones de usabilidad | Medio | Media | Reorganización de actividades y revisión semanal del backlog |
| Integración entre frontend (React) y backend (Node.js/Express) | Alto | Baja | Definición temprana de contratos de API y pruebas de integración |

---

## 📈 Estado Actual

- ✅ Investigación, Empatía y Validación — Completado
- ✅ Requerimientos y Gestión Ágil (Scrum) — Completado
- 🔄 Diseño de Interfaz en Figma — En proceso (muy avanzado)
- 🔄 Arquitectura Técnica y Configuración Base — En proceso

**Próximas actividades:**
- Finalizar al 100% el prototipo de alta fidelidad en Figma.
- Continuar el modelado de datos y arquitectura de software (PostgreSQL, Node.js/Express, React).
- Configurar entornos locales y tablero de monitoreo digital.
- Reforzar la integración del proveedor de IA en el flujo de generación de recetas.
- Actualizar la documentación técnica del proyecto.

**Roadmap técnico (Backend):**
- Completar guardado automático de ingredientes detectados en `user_ingredients` y en el catálogo `ingredients`.
- Añadir pruebas unitarias y e2e (Jest/Supertest).
- Implementar refresh tokens y mejorar la estrategia de sesiones.
- Migrar el almacenamiento de imágenes a un servicio en la nube (Cloudinary/S3) en lugar de base64.
- Finalizar el flujo de envío de emails en producción (SMTP seguro) para recuperación de contraseña.
- Desarrollar el frontend web en React.
