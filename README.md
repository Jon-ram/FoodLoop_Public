# 📖 FoodLoop

**FoodLoop** es una plataforma web que utiliza inteligencia artificial (Gemini/OpenAI) para generar recetas
personalizadas basadas en los ingredientes que el usuario tiene disponibles. El objetivo principal
es reducir el desperdicio de alimentos ayudando a las personas a aprovechar al máximo los
ingredientes en su despensa.



## 🤖 Módulo de Inteligencia Artificial — Extracción de Conocimiento (Proyecto Integrador)

Como parte del Proyecto Integrador de **Extracción de Conocimiento en Bases de Datos**, FoodLoop
incorpora un módulo de Machine Learning que complementa la generación de recetas por IA generativa
(Gemini) con modelos entrenados sobre datos propios del sistema (despensa, presupuesto, recetas
planificadas). El flujo completo sigue **CRISP-DM**:

```text
Problema → Datos → Simulación → ETL → EDA → Entrenamiento → Evaluación
   → Selección de modelos → API inteligente → Dashboard → Integración → Pruebas
```

### Metodología

Se utiliza **CRISP-DM** (Cross Industry Standard Process for Data Mining) por ser la metodología
más estándar para proyectos de minería de datos y ML aplicado, y porque se ajusta bien a un ciclo
iterativo: comprensión del negocio (reducir desperdicio y sobregasto) → comprensión y preparación de
los datos (despensa, presupuesto, recetas) → modelado → evaluación → despliegue como endpoints
consumidos desde el frontend web.

### Mecanismos seleccionados

De las propuestas evaluadas (10 supervisadas / 10 no supervisadas, documentadas en
`docs/proposals.md`), se seleccionaron los siguientes **4 para implementación** en esta primera
entrega: 2 supervisados y 2 no supervisados.

#### 🟢 Supervisado 1 — Predicción de riesgo de desperdicio (Clasificación)

| Elemento | Descripción |
|---|---|
| Tipo | Supervisado — Clasificación |
| Problema | Estimar la probabilidad de que un ingrediente de la despensa se venza sin usarse |
| Usuario beneficiado | Usuario final (despensa) |
| Datos necesarios | `user_ingredients`, `recipe_ingredients`, `meal_plans`, historial de eliminación/consumo |
| Entradas | Categoría, días en despensa, cantidad, si aparece en alguna receta planificada, tasa histórica de desperdicio del usuario en esa categoría |
| Salida | `desperdiciado` (0/1) + probabilidad |
| Algoritmo sugerido | Regresión Logística (interpretable) / Random Forest como comparación |
| Evaluación | Accuracy, Precision, Recall, F1-score, matriz de confusión |
| Integración | Badge de riesgo en `MisIngredientes.tsx`, endpoint `POST /api/ml/supervised/waste-risk/predict` |
| Riesgos | Dataset inicial simulado por bajo volumen real; posible sesgo hacia categorías con más historial |

#### 🟢 Supervisado 2 — Proyección de gasto mensual (Regresión)

| Elemento | Descripción |
|---|---|
| Tipo | Supervisado — Regresión |
| Problema | Proyectar el gasto total que tendrá el usuario al cierre del mes, dado su avance actual |
| Usuario beneficiado | Usuario final (presupuesto) |
| Datos necesarios | `budgets`, `shopping_lists`, `shopping_list_items`, `meal_plans` |
| Entradas | Gasto acumulado a la fecha, día del mes, recetas planificadas restantes, tamaño de despensa, categoría de gasto dominante |
| Salida | Gasto proyectado a fin de mes (MXN) |
| Algoritmo sugerido | Regresión Lineal (interpretable) / Random Forest Regressor como comparación |
| Evaluación | MAE, MSE, RMSE, R² |
| Integración | Card "Proyección a fin de mes" en `Presupuesto.tsx`, endpoint `POST /api/ml/supervised/spend-forecast/predict` |
| Riesgos | Requiere al menos 2-3 meses de historial real por usuario para ser confiable; se simula historial para la demo |

#### 🔵 No supervisado 1 — Segmentación de usuarios (Agrupamiento)

| Elemento | Descripción |
|---|---|
| Tipo | No supervisado — Agrupamiento |
| Problema | Agrupar usuarios por hábitos de consumo para personalizar recomendaciones y alertas |
| Usuario beneficiado | Usuario final (personalización) y equipo (analítica) |
| Datos necesarios | `budgets`, `shopping_list_items`, `gemini_usage`, `recipes`, `user_ingredients`, `meal_plans` |
| Entradas | % de presupuesto usado promedio, frecuencia de generación de recetas, % de ingredientes usados antes de vencer, diversidad de categorías en despensa, anticipación de planificación |
| Salida | `cluster_id` + etiqueta de segmento (ej. "Planificador", "De último minuto") |
| Algoritmo sugerido | K-Means (k determinado con método del codo / silhouette score) |
| Evaluación | Silhouette Score, Davies-Bouldin, inercia, interpretación de perfiles |
| Integración | Badge de perfil en dashboard, endpoint `POST /api/ml/unsupervised/user-segment/analyze` |
| Riesgos | Con pocos usuarios reales, los clusters pueden no ser estables; se valida con dataset simulado |

#### 🔵 No supervisado 2 — Detección de anomalías en el gasto semanal

| Elemento | Descripción |
|---|---|
| Tipo | No supervisado — Detección de anomalías |
| Problema | Detectar semanas donde el gasto se dispara fuera de lo normal para ese usuario, más allá del % fijo de presupuesto |
| Usuario beneficiado | Usuario final (alertas de presupuesto) |
| Datos necesarios | `shopping_lists.estimated_total` histórico por usuario, `shopping_list_items` |
| Entradas | Total semanal estimado, variación vs. semanas previas, proporción de ítems de reposición vs. receta planificada, día del mes |
| Salida | `anomaly_score` + bandera `is_anomaly` |
| Algoritmo sugerido | Isolation Forest (alternativa simple: z-score sobre historial propio del usuario) |
| Evaluación | Proporción de anomalías detectadas, revisión manual de casos frontera, estabilidad |
| Integración | Cuarto estado de alerta (`anomaly`) en `Presupuesto.tsx`/`ListaCompra.tsx`, endpoint `POST /api/ml/unsupervised/spend-anomaly/analyze` |
| Riesgos | Falsos positivos en usuarios con historial muy corto o muy irregular |

### Simulación de datos

Dado el bajo volumen de datos reales disponibles en esta etapa del proyecto, el entrenamiento se
apoya en un **dataset sintético generado con reglas del contexto** (no aleatorio puro), siguiendo el
mismo enfoque ya usado en el proyecto de datos de salud de Huauchinango: distribuciones realistas por
categoría de ingrediente, estacionalidad de gasto, y relación entre anticipación de planificación y
probabilidad de desperdicio. Semilla fija (`np.random.seed(2026)`) para reproducibilidad.

Los datos originales (reales) del sistema **no se modifican ni se editan a mano**; el dataset
sintético se genera y documenta por separado en `simulation/` con su propio script generador.

### Arquitectura del módulo de ML

```text
FoodLoop DB (Postgres)
        │
   Script ETL (Python + pandas)
        │
 Data Mart analítico (tablas de features)
        │
 Entrenamiento (scikit-learn) ──► Modelos serializados (joblib)
        │
 Servicio de inferencia (FastAPI, ml_service/)
        │
 Node/Express (proxy + persistencia de inferencias)
        │
 Socket.IO (dashboard en tiempo real)
        │
 Frontend web (React) — dashboard + integración en Presupuesto/MisIngredientes
```

Node/Express sigue siendo el backend principal; el servicio de ML vive como microservicio Python
independiente (`ml_service/`) para no acoplar Express a scikit-learn, y Node consume sus resultados
igual que ya consume cualquier otra tabla de Postgres.

### Endpoints inteligentes planeados

```http
POST /api/ml/supervised/waste-risk/predict
POST /api/ml/supervised/spend-forecast/predict
POST /api/ml/unsupervised/user-segment/analyze
POST /api/ml/unsupervised/spend-anomaly/analyze
GET  /api/ml/models
GET  /api/ml/models/{id}/metrics
GET  /api/ml/inferences
GET  /api/ml/health
```

Cada endpoint expone: modelo utilizado, versión, salida, confianza/score y fecha de inferencia;
todas las inferencias quedan registradas para trazabilidad y para alimentar el dashboard en tiempo
real vía Socket.IO.

### Nuevas tablas previstas (Data Mart de ML)

| Tabla | Propósito |
|---|---|
| `ingredient_outcomes` | Historial de si un ingrediente se usó o se venció sin usarse (etiqueta para el modelo de riesgo de desperdicio) |
| `monthly_spend_history` | Gasto real total por usuario-mes (etiqueta para el modelo de proyección de gasto) |
| `user_segments` | Resultado vigente de la segmentación (cluster asignado por usuario) |
| `budget_anomalies` | Semanas marcadas como anómalas junto con su `anomaly_score` |
| `ml_inferences` | Bitácora de toda inferencia servida por la API (modelo, entrada, salida, confianza, fecha) |

### Roadmap de implementación (este módulo)

1. Definir reglas de simulación y generar dataset sintético (`simulation/`).
2. Diseñar Data Mart (tabla de hechos + dimensiones) sobre el esquema ya existente.
3. Construir ETL reproducible (extracción desde Postgres → features → carga).
4. EDA de las 4 variables objetivo antes de modelar.
5. Entrenar y comparar al menos 2 configuraciones por mecanismo.
6. Serializar los 4 modelos y documentar métricas.
7. Levantar `ml_service/` (FastAPI) con los 4 endpoints de inferencia.
8. Conectar Node/Express como proxy + persistencia en `ml_inferences`.
9. Dashboard con Socket.IO (nuevas predicciones, clusters, alertas de anomalía).
10. Integrar resultados en `MisIngredientes.tsx`, `Presupuesto.tsx` y `ListaCompra.tsx`.
11. Pruebas de dataset, ETL, modelos, API e integración.
12. Documentar ética/sesgos y uso de IA generativa en el proceso (`docs/ethics.md`, `docs/ai-usage.md`).

> ⚠️ Nota de honestidad técnica: con pocos usuarios reales, ningún modelo de esta primera entrega es
> genuinamente predictivo todavía. El dataset sintético permite demostrar el pipeline completo
> (datos → features → modelo → API → producto) funcionando de extremo a extremo, que es lo que pide
> la guía del proyecto integrador; según crezca el uso real de FoodLoop, los modelos pueden
> reentrenarse con datos reales.

---

## 🚀 Instalación y Setup

### 1. **Configurar la Base de Datos**

Si tienes una base de datos anterior, ejecuta estos comandos para actualizar las columnas de tokens:

```bash
# Desde PowerShell
psql -U postgres -h localhost -d foodloop -c "
ALTER TABLE users ALTER COLUMN verification_token TYPE TEXT;
ALTER TABLE users ALTER COLUMN reset_password_token TYPE TEXT;
DELETE FROM users WHERE verification_token IS NULL OR email_verified = false;
CREATE INDEX IF NOT EXISTS idx_verification_token ON users(verification_token);
CREATE INDEX IF NOT EXISTS idx_reset_token ON users(reset_password_token);
"
```

**O si quieres empezar limpio (borra todos los datos):**

```bash
psql -U postgres -h localhost -d foodloop -c "DROP TABLE IF EXISTS users CASCADE; DROP TABLE IF EXISTS login_attempts CASCADE; DROP TABLE IF EXISTS site_visits CASCADE; DROP TABLE IF EXISTS subscriptions CASCADE; DROP TABLE IF EXISTS gemini_usage CASCADE;"

psql -U postgres -h localhost -d foodloop -f "foodloop(backend)/database/schema.sql"
```

### 2. **Backend**

```bash
cd foodloop(backend)
npm install
npm run dev
```

**El servidor estará en:** `http://localhost:3001`

### 3. **Frontend**

```bash
cd Frontend
npm install
npm run dev
```

**La aplicación estará en:** `http://localhost:5173`

### 4. **Nuevas pantallas funcionales**

La aplicación ya incluye dos módulos nuevos dentro del flujo principal, ambos pensados para funcionar como experiencia web y como fuente de datos para integración externa:

- **Presupuesto**: permite definir el límite mensual, el umbral de alerta y consultar el estado real del uso del presupuesto.
- **Lista de Compra**: genera una lista semanal cruzando receta planificada, ingredientes en inventario, vencimientos y presupuesto disponible; además expone un payload ligero útil para clientes de móvil y smartwatch.

Estas pantallas están pensadas para operar desde el frontend con las rutas:

- `/presupuesto`
- `/lista-compra`

Y su estado puede ser consumido también por endpoints ligeros de sincronización externa.

### 5. **Configurar Variables de Entorno**

Crear archivo `.env` en `foodloop(backend)/`:

```env
# API Keys
GEMINI_API_KEY=tu_clave_de_gemini
OPENAI_API_KEY=tu_clave_de_openai

# Base de Datos
DB_USER=postgres
DB_HOST=localhost
DB_NAME=foodloop
DB_PASSWORD=1234
DB_PORT=5432

# JWT
JWT_SECRET=tu_secreto_super_seguro
JWT_EXPIRES_IN=7d

# Bcrypt
BCRYPT_ROUNDS=10

# URLs
FRONTEND_URL=http://localhost:5173

# Email (Gmail con App Password)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=tu_email@gmail.com
SMTP_PASS=tu_app_password_de_16_caracteres
```

⚠️ **Para Gmail:** Usa una [contraseña de aplicación (App Password)](https://support.google.com/accounts/answer/185833), no tu contraseña normal.

---

## ✅ Cómo probar Presupuesto y Lista de Compra

### 1. Crear un presupuesto

Primero inicia sesión en el frontend y entra a la página `/presupuesto`.

Desde el formulario define:
- `month`
- `year`
- `limit_amount`
- `alert_threshold`

También puedes probarlo directamente con la API:

```bash
curl -X POST http://localhost:3001/api/budget \
  -H "Authorization: Bearer TU_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "month": 7,
    "year": 2026,
    "limit_amount": 1500,
    "alert_threshold": 80
  }'
```

### 2. Ver el estado del presupuesto actual

```bash
curl -X GET "http://localhost:3001/api/budget/current?month=7&year=2026" \
  -H "Authorization: Bearer TU_TOKEN"
```

Respuesta esperada:

```json
{
  "success": true,
  "hasBudget": true,
  "budget": {
    "limit_amount": 1500,
    "alert_threshold": 80
  },
  "spent": 420,
  "remaining": 1080,
  "percentUsed": 28
}
```

### 3. Generar la lista de compra semanal

Desde la UI entra a `/lista-compra` o usa este endpoint con fechas válidas:

```bash
curl -X POST http://localhost:3001/api/shopping-list/generate \
  -H "Authorization: Bearer TU_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "week_start": "2026-07-27",
    "week_end": "2026-08-02"
  }'
```

Esto debe cruzar:
- recetas planificadas,
- ingredientes actuales en despensa,
- vencimientos próximos,
- y el presupuesto configurado para el mes.

### 4. Consultar la lista activa

```bash
curl -X GET http://localhost:3001/api/shopping-list/current \
  -H "Authorization: Bearer TU_TOKEN"
```

### 5. Marcar un ítem como comprado

```bash
curl -X PATCH http://localhost:3001/api/shopping-list/items/123 \
  -H "Authorization: Bearer TU_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "is_purchased": true
  }'
```

---

## ⌚ Smartwatch / endpoint ligero para conectar un dispositivo externo

El backend ya expone un endpoint pensado para polling o sincronización con un smartwatch o dispositivo portátil:

```http
GET /api/shopping-list/smartwatch
```

### Uso

```bash
curl -X GET http://localhost:3001/api/shopping-list/smartwatch \
  -H "Authorization: Bearer TU_TOKEN"
```

### Qué devuelve

Es un payload liviano, orientado a notificaciones proactivas, por ejemplo:

```json
{
  "success": true,
  "expiring_count": 2,
  "missing_items": [
    "pollo",
    "tomate"
  ],
  "budget_warning": false,
  "week_status": "active"
}
```

### Para qué sirve

- avisar qué ingredientes están próximos a vencer,
- mostrar qué falta comprar para las recetas planificadas,
- notificar si el presupuesto del mes está cerca del límite o ya fue excedido.

> Este endpoint es el punto de conexión recomendado si quieres integrar un reloj inteligente o una pantalla externa que consulte estado de compra y presupuesto de forma ligera.

---

## ⌚ Guía guiada de receta para web y smartwatch

Además de la lista de compra, el backend ya expone un endpoint pensado para seguir una receta paso a paso en una pantalla guiada, tanto desde la web como desde un reloj o un dispositivo externo.

### Endpoint de guía de receta

```http
GET /api/recipes/smartwatch/:recipeId
```

### Uso

```bash
curl -X GET "http://localhost:3001/api/recipes/smartwatch/123" \
  -H "Authorization: Bearer TU_TOKEN" \
  -H "Content-Type: application/json"
```

### Qué devuelve

```json
{
  "success": true,
  "guide": {
    "id": 123,
    "name": "Sopa de verduras",
    "description": "Receta ligera y rápida",
    "time": 25,
    "difficulty": "Fácil",
    "steps": [
      {
        "description": "Pica cebolla y ajo",
        "duration_minutes": 2,
        "confirmation_text": "Confirmar paso",
        "smartwatch_action": "Vibra y avanza al siguiente paso"
      }
    ]
  },
  "total_steps": 3,
  "current_step": 1
}
```

### Cómo se prueba desde la web

1. Inicia backend y frontend.
2. Entra a `/generador` y genera una receta.
3. Desde la vista de detalle, abre la receta y usa la caja de "Guía confirmada".
4. Cada botón de confirmación avanza al siguiente paso.
5. La sección del reloj muestra el texto de vibración y de siguiente paso para que pueda ser consumido por un dispositivo externo.

### Cómo se prueba desde el emulador de smartwatch / Android Studio

1. Abre Android Studio.
2. Ve a `Device Manager`.
3. Crea o arranca un emulador Wear OS / Android TV o un dispositivo Android con la API adecuada.
4. Desde el emulador, instala una app de prueba o un cliente simple que haga llamadas HTTP a la API.
5. Usa el endpoint:
   - `GET /api/recipes/smartwatch/:recipeId`
   - `GET /api/shopping-list/smartwatch`
6. Pasa el `Authorization: Bearer TU_TOKEN` del usuario autenticado.
7. En la app de prueba o en el emulador, muestra:
   - el `guide.steps` para seguir la receta,
   - el payload de compra para ver qué falta comprar y alertas del presupuesto.
8. En la parte web, confirma que el mismo flujo aparece en `/recetario/:id` y el paso actual cambia con cada confirmación.

### Recomendación para tu entrega

La mejor forma de presentarlo es:

- La web ya tiene la experiencia guiada.
- El smartwatch consume un payload liviano desde la API.
- No es una app completa del ecosistema, sino un cliente externo o un emulador que consulta el backend.

> En resumen: sí, se necesita un cliente mínimo para smartwatch, pero no una interfaz completa ni una reimplementación de toda la app.

### Recomendación de prueba

Para validar el flujo completo en un entorno realista:

- Web: genera la receta y sigue la guía paso a paso.
- Wear / emulador: consulta el payload liviano y muestra la vibración/avance del paso actual.
- Lista de compra: usa `GET /api/shopping-list/smartwatch` para comprobar el estado ligero y hacer sincronización con el reloj.

---

## ✅ Pruebas - Flujo de Registro y Verificación de Email

### Paso 1: Registrar usuario

```bash
curl -X POST http://localhost:3001/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "full_name": "Test User",
    "email": "test@example.com",
    "password": "Password123!",
    "confirm_password": "Password123!",
    "age": 25,
    "terms_accepted": true
  }'
```

**Respuesta esperada (desarrollo):**
```json
{
  "success": true,
  "verification_token": "abc123...",
  "verification_link": "http://localhost:5173/verify-email?token=abc123...",
  "user": {
    "id": 1,
    "full_name": "Test User",
    "email": "test@example.com",
    "created_at": "2026-07-11T20:00:00Z"
  }
}
```

### Paso 2: Obtener token de verificación (desarrollo)

Si no recibiste el email, usa este endpoint de debug:

```bash
curl -X POST http://localhost:3001/api/auth/debug/verification-token \
  -H "Content-Type: application/json" \
  -d '{"email": "test@example.com"}'
```

**Respuesta:**
```json
{
  "success": true,
  "verification_token": "abc123...",
  "verification_link": "http://localhost:5173/verify-email?token=abc123...",
  "email_verified": false
}
```

### Paso 3: Verificar email

**Opción A: Desde el frontend**
- Abre: `http://localhost:5173/verify-email?token=abc123...`
- Deberías ver "Correo verificado" ✅

**Opción B: Desde API**
```bash
curl -X GET "http://localhost:3001/api/auth/verify/abc123..."
```

### Paso 4: Intentar login

```bash
curl -X POST http://localhost:3001/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "Password123!"
  }'
```

**Respuesta exitosa:**
```json
{
  "success": true,
  "token": "eyJhbGc...",
  "user": {
    "id": 1,
    "full_name": "Test User",
    "email": "test@example.com",
    "role": "user"
  }
}
```

---

## 📧 Flujo de Recuperación de Contraseña

### Paso 1: Solicitar recuperación

```bash
curl -X POST http://localhost:3001/api/auth/forgot-password \
  -H "Content-Type: application/json" \
  -d '{"email": "test@example.com"}'
```

### Paso 2: Obtener link (desarrollo)

```bash
curl -X GET "http://localhost:3001/api/auth/reset-password/token123..."
```

### Paso 3: Restablecer contraseña

```bash
curl -X POST http://localhost:3001/api/auth/reset-password \
  -H "Content-Type: application/json" \
  -d '{
    "token": "token123...",
    "password": "NewPassword123!",
    "confirm_password": "NewPassword123!"
  }'
```

---

## 🔍 Troubleshooting

### ❌ "Token inválido" al verificar email

**Causas comunes:**
1. Base de datos antigua con columnas VARCHAR(255) pequeñas
   - **Solución:** Ejecuta los comandos SQL de la sección 1

2. Email no se envía correctamente
   - **Solución:** Verifica credenciales SMTP en `.env`
   - Usa Gmail App Password en lugar de contraseña normal

3. React Strict Mode ejecuta verify dos veces
   - **Solución:** Ya está arreglado en `VerifyEmail.tsx` con useRef

### ❌ SMTP Error con Gmail

**Si ves:** `Invalid login: 535-5.7.8 Username and password not accepted`

**Solución:**
1. Ve a https://myaccount.google.com/apppasswords
2. Selecciona "Mail" y "Windows Computer"
3. Copia la contraseña de 16 caracteres
4. Actualiza `.env`: `SMTP_PASS=LOS_16_CARACTERES`
5. Reinicia el backend

---

## 📚 Tabla de Base de Datos - `users`

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `id` | SERIAL PK | Identificador único |
| `full_name` | VARCHAR(100) | Nombre completo |
| `email` | VARCHAR(100) UNIQUE | Correo electrónico |
| `password_hash` | VARCHAR(255) | Contraseña hasheada |
| `age` | INTEGER | Edad (mín. 18) |
| `terms_accepted` | BOOLEAN | Aceptó términos |
| `email_verified` | BOOLEAN | Email verificado |
| `verification_token` | TEXT | Token para verificar email |
| `reset_password_token` | TEXT | Token para recuperar contraseña |
| `reset_password_expires` | TIMESTAMP | Expiración del token |
| `role` | VARCHAR(20) | Rol (admin/user) |
| `created_at` | TIMESTAMP | Fecha de creación |
| `updated_at` | TIMESTAMP | Última actualización |

---

## 🛠️ Comandos Útiles PostgreSQL

### Ver todos los usuarios
```bash
psql -U postgres -h localhost -d foodloop -c "SELECT id, email, email_verified FROM users;"
```

### Verificar usuario manualmente
```bash
psql -U postgres -h localhost -d foodloop -c "UPDATE users SET email_verified = true WHERE email = 'test@example.com';"
```

### Limpiar tabla de usuarios
```bash
psql -U postgres -h localhost -d foodloop -c "DELETE FROM users;"
```

### Ver estructura de tabla
```bash
psql -U postgres -h localhost -d foodloop -c "\d users"
```

### Ver columnas de tokens
```bash
psql -U postgres -h localhost -d foodloop -c "SELECT column_name, data_type FROM information_schema.columns WHERE table_name='users' AND column_name LIKE '%token';"
```

---

## ⚙️ Stack Tecnológico

- **Frontend:** React 19, TypeScript, Vite, Tailwind CSS, React Router
- **Backend:** Node.js, Express, PostgreSQL
- **Autenticación:** JWT, bcrypt
- **Email:** Nodemailer
- **IA generativa:** Gemini/OpenAI APIs
- **Machine Learning (Proyecto Integrador):** Python, scikit-learn, pandas, FastAPI, Socket.IO

---

## 📋 Resumen Rápido

1. **Clonar e instalar dependencias**
```bash
cd foodloop(backend)
npm install
cd ../Frontend
npm install
```

2. **Actualizar BD desde versión anterior (IMPORTANTE)**
```bash
psql -U postgres -h localhost -d foodloop -c "
ALTER TABLE users ALTER COLUMN verification_token TYPE TEXT;
ALTER TABLE users ALTER COLUMN reset_password_token TYPE TEXT;
"
```

3. **Arrancar servidor y cliente**
```bash
# Terminal 1 - Backend
cd foodloop(backend)
npm run dev

# Terminal 2 - Frontend
cd Frontend
npm run dev
```

4. **Probar registro y verificación de email**
- Abre http://localhost:5173/registro
- Llena el formulario
- Haz clic en el link del email O usa el endpoint de debug
- Verifica que el email se marque como verificado
- Intenta login

---

<details>
<summary>📚 Modelo de datos detallado y estrategias de respaldo/calidad</summary>

### Esquema de Tablas

#### 👤 `users` — Usuarios del sistema

Entidad central del sistema. Almacena toda la información necesaria para autenticación y personalización.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `id` | SERIAL PK | Identificador único |
| `full_name` | VARCHAR(100) | Nombre completo (mín. 2 caracteres) |
| `email` | VARCHAR(100) UNIQUE | Correo electrónico para autenticación |
| `password_hash` | VARCHAR(255) | Contraseña hasheada (nunca texto plano) |
| `age` | INTEGER | Edad (CHECK age >= 18) |
| `terms_accepted` | BOOLEAN | Consentimiento explícito de términos |
| `terms_accepted_at` | TIMESTAMP | Fecha de aceptación de términos |
| `email_verified` | BOOLEAN | Control de verificación de email |
| `verification_token` | VARCHAR(255) | Token para verificar email |
| `reset_password_token` | VARCHAR(255) | Token para restablecer contraseña |
| `reset_password_expires` | TIMESTAMP | Fecha límite del token de recuperación |
| `role` | VARCHAR(20) | Rol del usuario (`admin` o `user`) |
| `created_at` | TIMESTAMP | Fecha de creación |
| `updated_at` | TIMESTAMP | Última actualización (via trigger) |

---

#### 🔐 `login_attempts` — Control de intentos de login

Registra todos los intentos de inicio de sesión para detectar ataques de fuerza bruta.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `id` | SERIAL PK | Identificador único |
| `email` | VARCHAR(100) | Correo utilizado en el intento |
| `ip_address` | INET | Dirección IP del intento |
| `attempt_time` | TIMESTAMP | Momento exacto del intento |
| `success` | BOOLEAN | `true` = exitoso / `false` = fallido |

---

#### 🥕 `ingredients` — Catálogo de ingredientes

Vocabulario controlado y normalizado de todos los ingredientes del sistema.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `id` | SERIAL PK | Identificador único |
| `name` | VARCHAR(100) UNIQUE | Nombre del ingrediente (ej. "Tomate") |
| `category` | VARCHAR(50) | Clasificación: Vegetales, Frutas, Carnes, Lácteos, Granos, Especias, Otros |
| `created_at` | TIMESTAMP | Fecha de incorporación al catálogo |

---

#### 🧺 `user_ingredients` — Despensa del usuario

Relación entre usuarios e ingredientes con información contextual de la despensa personal.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `id` | SERIAL PK | Identificador único |
| `user_id` | INTEGER FK | Referencia al usuario (→ users.id) |
| `ingredient_id` | INTEGER FK | Referencia al catálogo (→ ingredients.id) |
| `quantity` | NUMERIC(10,2) | Cantidad disponible |
| `unit` | VARCHAR(20) | Unidad de medida (gramos, piezas, litros...) |
| `unit_price` | NUMERIC(10,2) | Precio estimado por unidad/kg |
| `expiry_date` | DATE | Fecha de vencimiento |
| `added_at` | TIMESTAMP | Fecha en que se agregó a la despensa |

---

#### 📄 `recipes` — Recetas generadas

Almacena las recetas creadas por la IA para cada usuario.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `id` | SERIAL PK | Identificador único |
| `user_id` | INTEGER FK | Usuario que generó la receta (→ users.id) |
| `name` | VARCHAR(200) | Nombre de la receta |
| `description` | TEXT | Descripción breve de la receta |
| `time_minutes` | INTEGER | Tiempo total de preparación (minutos) |
| `difficulty` | VARCHAR(20) | Nivel: `Fácil`, `Media` o `Difícil` (CHECK) |
| `servings` | INTEGER | Número de porciones |
| `image_url` | TEXT | URL de imagen ilustrativa (Unsplash) |
| `is_favorite` | BOOLEAN | Marcada como favorita por el usuario |
| `generated_at` | TIMESTAMP | Fecha y hora de generación |

---

#### 🧾 `recipe_ingredients` — Ingredientes de cada receta

Vincula recetas con los ingredientes necesarios para prepararlas.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `id` | SERIAL PK | Identificador único |
| `recipe_id` | INTEGER FK | Referencia a la receta (→ recipes.id) |
| `ingredient_name` | VARCHAR(100) | Nombre del ingrediente (texto libre) |
| `quantity` | VARCHAR(50) | Cantidad y unidad (ej. "2 piezas", "500g") |
| `is_optional` | BOOLEAN | Indica si el ingrediente es opcional |

---

#### 📝 `recipe_steps` — Pasos de preparación

Instrucciones de preparación ordenadas secuencialmente.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `id` | SERIAL PK | Identificador único |
| `recipe_id` | INTEGER FK | Referencia a la receta (→ recipes.id) |
| `step_number` | INTEGER | Número de orden del paso (1, 2, 3...) |
| `description` | TEXT | Instrucción detallada del paso |

---

#### 💎 `subscriptions` — Suscripciones premium

Gestiona las suscripciones de acceso a funcionalidades avanzadas.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `id` | SERIAL PK | Identificador único |
| `user_id` | INTEGER FK | Referencia al usuario (→ users.id) |
| `status` | VARCHAR(20) | Estado: `active`, `cancelled`, `expired` (CHECK) |
| `started_at` | TIMESTAMP | Fecha de inicio |
| `expires_at` | TIMESTAMP | Fecha de expiración |
| `amount` | NUMERIC(10,2) | Monto pagado |
| `payment_reference` | VARCHAR(255) | ID de transacción del pago |

---

#### 📊 `site_visits` — Registro de visitas

Registra visitas de usuarios autenticados y anónimos para análisis de comportamiento.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `id` | SERIAL PK | Identificador único |
| `user_id` | INTEGER FK | Usuario (NULL si es anónimo) |
| `ip_address` | INET | Dirección IP del visitante |
| `path` | VARCHAR(255) | Ruta visitada en la aplicación |
| `user_agent` | TEXT | Información del navegador/dispositivo |
| `visited_at` | TIMESTAMP | Fecha y hora de la visita |

---

#### 🤖 `gemini_usage` — Uso de IA

Monitorea el consumo de la API de inteligencia artificial por usuario y operación.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `id` | SERIAL PK | Identificador único |
| `user_id` | INTEGER FK | Usuario que realizó la solicitud |
| `tokens_input` | INTEGER | Tokens enviados a la IA |
| `tokens_output` | INTEGER | Tokens recibidos como respuesta |
| `cost_usd` | NUMERIC(10,6) | Costo estimado en dólares |
| `endpoint` | VARCHAR(100) | Tipo de operación (ej. `generate_recipe`) |
| `used_at` | TIMESTAMP | Fecha y hora del uso |

---

#### 💰 `budgets` — Presupuesto mensual del usuario

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `id` | SERIAL PK | Identificador único |
| `user_id` | INTEGER FK | Referencia al usuario (→ users.id) |
| `month` / `year` | INTEGER | Mes y año del presupuesto |
| `limit_amount` | NUMERIC(10,2) | Límite de gasto mensual |
| `alert_threshold` | NUMERIC(5,2) | % del límite para disparar alerta |

---

#### 📅 `meal_plans` — Recetas planificadas

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `id` | SERIAL PK | Identificador único |
| `user_id` | INTEGER FK | Referencia al usuario |
| `recipe_id` | INTEGER FK | Referencia a la receta planificada |
| `planned_date` | DATE | Fecha planificada |
| `status` | VARCHAR(20) | `planned`, `cooked`, `skipped` |

---

#### 🛒 `shopping_lists` / `shopping_list_items` — Lista de compra semanal

Lista generada por el sistema cruzando inventario, recetas planificadas, vencimientos y presupuesto.
`shopping_list_items` guarda cada ítem con su prioridad (`receta_planificada`, `vencimiento`,
`reposicion`), precio estimado y estado de compra.

---

### Modelo Entidad-Relación

| Relación | Tipo | Descripción |
|----------|------|-------------|
| `users` → `recipes` | 1:N | Un usuario puede tener muchas recetas |
| `users` → `user_ingredients` | 1:N | Un usuario puede tener muchos ingredientes |
| `users` → `login_attempts` | 1:N | Un usuario puede tener muchos intentos de login |
| `users` → `subscriptions` | 1:N | Un usuario puede tener múltiples suscripciones |
| `users` → `site_visits` | 1:N | Un usuario puede generar múltiples visitas |
| `users` → `gemini_usage` | 1:N | Un usuario puede consumir múltiples veces la IA |
| `users` → `budgets` | 1:N | Un usuario puede tener un presupuesto por mes |
| `users` → `meal_plans` | 1:N | Un usuario puede planificar muchas recetas |
| `users` → `shopping_lists` | 1:N | Un usuario puede tener varias listas de compra (una por semana) |
| `recipes` → `recipe_ingredients` | 1:N | Una receta puede tener muchos ingredientes |
| `recipes` → `recipe_steps` | 1:N | Una receta puede tener muchos pasos |
| `ingredients` → `user_ingredients` | 1:N | Un ingrediente puede estar en muchas despensas |
| `shopping_lists` → `shopping_list_items` | 1:N | Una lista puede tener muchos ítems |

---

## 💾 Estrategia de Respaldo

FoodLoop implementa un sistema de respaldo automatizado en **tres niveles**:

| Tipo | Frecuencia | Retención | Propósito |
|------|-----------|-----------|-----------|
| **Diario** | Cada 24h (7:00 AM) | 7 días | Recuperación ante fallos recientes |
| **Semanal** | Cada lunes | 30 días | Puntos de restauración intermedios |
| **Mensual** | Primer día del mes | 365 días | Archivo histórico y auditoría |

### Scripts de automatización

| Script | Frecuencia | Hora | Descripción |
|--------|-----------|------|-------------|
| `backup_postgres.bat` | Diaria | 07:00 AM | Genera respaldo comprimido, gestiona retención |
| `verify_backup.bat` | Diaria | 07:30 AM | Verifica legibilidad, restaurabilidad y antigüedad |
| `maintenance_postgres.bat` | Semanal (domingos) | 08:00 AM | VACUUM, ANALYZE, REINDEX, purga de datos obsoletos |
| `restore_postgres.bat` | Manual ante emergencia | — | Recuperación completa ante desastres |

**Estructura de almacenamiento:**
```
C:\BackupsDB\postgres\
├── daily\
├── weekly\
├── monthly\
└── logs\
```

**Tiempo de recuperación estimado (RTO):** < 15 minutos

---

## ✅ Calidad de los Datos

La estrategia de calidad de datos abarca **7 criterios fundamentales**:

| Criterio | Mecanismo implementado |
|----------|----------------------|
| **Exactitud** | Restricciones CHECK, validaciones semánticas en backend |
| **Integridad** | Claves primarias únicas, claves foráneas con CASCADE/SET NULL |
| **Disponibilidad** | Respaldos en 3 niveles, verificación diaria automatizada |
| **Confiabilidad** | Transacciones ACID, trigger de trazabilidad, registro de auditoría |
| **Utilidad** | Purga automática de datos obsoletos (logins > 90 días, visitas > 180 días) |
| **Accesibilidad** | Modelo de roles con privilegios mínimos necesarios |
| **Normalización** | Diseño hasta 3NF, catálogo centralizado de ingredientes, restricciones UNIQUE |

---

**Universidad Tecnológica de Xicotepec de Juárez — Tecnologías de la Información**  
📅 18 de Marzo de 2026

</details>

## Backend

### Arquitectura de carpetas

Estructura principal del proyecto (raíz del backend):

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
│   ├── 📄 Budget.js
│   ├── 📄 MealPlan.js
│   ├── 📄 ShoppingList.js
│   └── 📄 User.js
├── 📁 src
│   ├── 📁 controllers
│   │   ├── 📄 adminController.js
│   │   ├── 📄 authController.js
│   │   ├── 📄 ingredientController.js
│   │   ├── 📄 recipeController.js
│   │   ├── 📄 budgetController.js
│   │   ├── 📄 mealPlanController.js
│   │   └── 📄 shoppingListController.js
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
│   │   ├── 📄 recipes.js
│   │   ├── 📄 budget.js
│   │   ├── 📄 mealPlans.js
│   │   └── 📄 shoppingList.js
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

### `ml_service/` (previsto — microservicio de Machine Learning)

```
ml_service/
├── data/
│   ├── raw/
│   ├── processed/
│   ├── training/
│   ├── validation/
│   ├── test/
│   └── inference/
├── simulation/
│   └── generate_synthetic_data.py
├── etl/
│   └── extract_transform_load.py
├── notebooks/
│   ├── eda/
│   ├── supervised/
│   └── unsupervised/
├── models/
│   ├── supervised/
│   │   ├── waste_risk_classifier.py
│   │   └── spend_forecast_regressor.py
│   ├── unsupervised/
│   │   ├── user_segmentation.py
│   │   └── spend_anomaly_detector.py
│   └── serialized/
├── api/
│   ├── main.py            # FastAPI
│   ├── routes/
│   └── sockets/
└── requirements.txt
```

Esta vista ayuda a ubicarse rápido en el código y en qué carpeta añadir nuevas funcionalidades.

---
### Características actuales
- Registro y login de usuarios con validación de datos.
- Autenticación con JWT y middleware `auth` para rutas protegidas.
- Generación de recetas mediante IA y guardado en PostgreSQL.
- Detección de ingredientes desde imágenes (upload con `multer`), incluyendo precio estimado por unidad.
- Guardado y consulta de recetas e ingredientes; marcado de favoritos.
- Presupuesto mensual con alertas por umbral.
- Recetas planificadas y lista de compra semanal inteligente (prioriza despensa existente).
- Valor total de despensa visible en tiempo real.
- Rate limiting y registro de intentos de login (tabla `login_attempts`).
- Documentación básica con Swagger en `/api-docs`.

### Qué falta / Roadmap
- Módulo de Machine Learning (`ml_service/`): los 4 mecanismos descritos arriba.
- Prompt de Gemini actualizado para estimar precio por ingrediente detectado.
- Añadir tests unitarios e2e (Jest/Supertest, y pytest para `ml_service/`).
- Implementar refresh tokens y mejorar estrategia de sesiones.
- Subir imágenes a un storage (Cloudinary/S3) en lugar de base64.
- End-to-end de envío de emails en producción (SMTP seguro).

### Seguridad — método y cómo está implementado
- Autenticación: tokens JWT firmados con `JWT_SECRET` (configurado en `.env`). Rutas protegidas usan el middleware `auth` que valida y extrae `userId`.
- Hashing de contraseñas: `bcrypt` con rondas configurables vía `BCRYPT_ROUNDS` (variable de entorno).
- Validación de entradas: `express-validator` para sanitizar y validar datos de registro/login.
- Control de acceso: campo `role` en `users` y middleware `isAdmin` para rutas administrativas.
- Protección contra abuso: rate limiting y registro de intentos (tabla `login_attempts`) para mitigar ataques de fuerza bruta.
- Recuperación de contraseña: tokens de un solo uso almacenados en `users.reset_password_token` y `reset_password_expires` (implementación parcial; requiere SMTP configurado).
- Buenas prácticas: las claves y secretos se cargan desde `.env` y no deben subirse al repositorio.

---

## Resumen rápido (para el docente)

1) Clonar, instalar dependencias:

```bash
git clone <url-del-repositorio>
cd FoodLoop
npm install
```

2) Crear la base de datos y cargar esquema y seed (ajusta rutas si es necesario):

```cmd
createdb -U postgres foodloop
psql -U postgres -d foodloop -f database/schema.sql
psql -U postgres -d foodloop -f database/seed_demo.sql
```

Si en su máquina los archivos están en una ruta absoluta distinta, puede usar los comandos que nos compartiste (ajustando el usuario):

```cmd
psql -U postgres -d foodloop -f "C:\Users\PC-22\FoodLoop\foodloop(backend)\config\schema.sql"
psql -U postgres -d foodloop -f "C:\Users\PC-22\FoodLoop\foodloop(backend)\config\seed_demo.sql"
```

3) Crear `.env` en la raíz con la plantilla (NO poner claves reales en el repo):

```env
PORT=3001
GEMINI_API_KEY=TU_GEMINI_API_KEY_AQUI
OPENAI_API_KEY=TU_OPENAI_API_KEY_AQUI
DB_USER=postgres
DB_HOST=localhost
DB_NAME=foodloop
DB_PASSWORD=TU_PASSWORD
DB_PORT=5432
JWT_SECRET=tu_secreto_super_seguro_cambiame
JWT_EXPIRES_IN=7d
BCRYPT_ROUNDS=10
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=tu_email@gmail.com
SMTP_PASS=tu_contraseña_app
```

4) Iniciar servidor:

```bash
npm run dev
```

5) Abrir documentación Swagger:

```
http://localhost:3001/api-docs
```

---
Notas técnicas:
- Swagger se monta en `/api-docs` mediante `swagger-ui-express` y las rutas están documentadas con JSDoc en `src/routes/`.
- No agregué claves reales al README; usa la plantilla `.env` para pruebas.

### **Problema 4: Error de importación**
```
Cannot find module '../models/User'
```
✅ **Solución:** Crear todos los archivos de modelos faltantes y corregir rutas

---

## 🚀 **8. Cómo Usar**

### **Flujo Completo para Usuario**

#### **1. Registro**
```bash
POST http://localhost:3001/api/auth/register
Content-Type: application/json

{
  "full_name": "Juan Pérez",
  "email": "juan@example.com",
  "password": "Test123!",
  "confirm_password": "Test123!",
  "age": 25,
  "terms_accepted": true
}
```

#### **2. Login (obtener token)**
```bash
POST http://localhost:3001/api/auth/login
Content-Type: application/json

{
  "email": "juan@example.com",
  "password": "Test123!"
}
```
*Guardar el token de la respuesta*

#### **3. Generar recetas (con token)**
```bash
POST http://localhost:3001/api/recipes/generate
Headers:
  Authorization: Bearer TU_TOKEN_AQUI
  Content-Type: application/json

{
  "ingredients": ["pollo", "arroz", "tomate"]
}
```

#### **4. Ver recetas guardadas**
```bash
GET http://localhost:3001/api/recipes/user
Headers:
  Authorization: Bearer TU_TOKEN_AQUI
```

#### **5. Detectar ingredientes (opcional)**
```bash
POST http://localhost:3001/api/ingredients/detect
Headers:
  Authorization: Bearer TU_TOKEN_AQUI
Body: form-data con campo "image" (archivo de imagen)
```

---

## 📊 **9. Verificación en Base de Datos**

```sql
-- Ver usuarios
SELECT id, full_name, email FROM users;

-- Ver recetas de un usuario
SELECT * FROM recipes WHERE user_id = 1;

-- Ver ingredientes de una receta
SELECT * FROM recipe_ingredients WHERE recipe_id = 1;

-- Ver pasos de una receta
SELECT * FROM recipe_steps WHERE recipe_id = 1 ORDER BY step_number;

-- Ver intentos de login
SELECT * FROM login_attempts ORDER BY attempt_time DESC;
```

---

## ✅ **10. Estado de Funcionalidades**

| Funcionalidad | Estado |
|--------------|--------|
| Registro de usuarios | ✅ Completo |
| Login con JWT | ✅ Completo |
| Rate limiting | ✅ Completo |
| Generar recetas con IA | ✅ Completo |
| Guardar recetas en BD | ✅ Completo |
| Ver recetas guardadas | ✅ Completo |
| Marcar favoritos | ✅ Completo |
| Detectar ingredientes en imagen | ✅ Completo |
| Guardar ingredientes en catálogo | ✅ Completo |
| Guardar ingredientes en despensa | ✅ Completo |
| Precio por ingrediente + valor total de despensa | ✅ Completo |
| Presupuesto mensual con alertas | ✅ Completo |
| Recetas planificadas | ✅ Completo |
| Lista de compra semanal inteligente | ✅ Completo |
| Endpoint smartwatch (compra + presupuesto) | ✅ Completo |
| Guía de receta paso a paso (web/smartwatch) | ✅ Completo |
| Predicción de riesgo de desperdicio (ML) | ⏳ Pendiente |
| Proyección de gasto mensual (ML) | ⏳ Pendiente |
| Segmentación de usuarios (ML) | ⏳ Pendiente |
| Detección de anomalías de gasto (ML) | ⏳ Pendiente |
| Dashboard en tiempo real (Socket.IO) | ⏳ Pendiente |

---

## 🚧 **11. Próximas Mejoras**

1. ✅ Guardar ingredientes detectados en `user_ingredients`
2. ✅ Guardar en catálogo `ingredients`
3. ✅ Implementar frontend web (React)
4. ✅ Presupuesto, recetas planificadas y lista de compra inteligente
5. Módulo de Machine Learning (`ml_service/`) con los 4 mecanismos seleccionados
6. Dashboard en tiempo real con Socket.IO
7. Añadir tests unitarios con Jest y pytest
8. Implementar refresh tokens
9. Subir imágenes a Cloudinary (no solo base64)

---

## 📝 **12. Notas de Desarrollo**

- **AI Services**: Usar Gemini para desarrollo (gratuito con límites). Para producción, considerar OpenAI.
- **Error Handling**: Todas las llamadas a IA incluyen try-catch con fallback a datos simulados.
- **Seguridad**: Nunca subir el archivo `.env` al repositorio.
- **Base de Datos**: Las transacciones aseguran que las recetas se guarden completas o no se guarden.
- **Machine Learning**: dado el bajo volumen de datos reales, el entrenamiento inicial se apoya en dataset sintético documentado y reproducible (semilla fija); no se editan a mano los datos reales del sistema.

## 👥 **13. Contribuidores**

|Integrante|Contacto|Rol|
|----------|-------|---|
|Jonathan Baldemar Ramirez Reyes|[@Jon-ram](https://github.com/Jon-ram)|Desarrollador de DataBases|
|Christian Paul Rodriguez Perez|[@ChrisRodriguez-0430](https://github.com/ChrisRodriguez-0430)|Desarrollador BackEnd|
|Josue Martinez Otero|[@Josue-Martinez-Otero](https://github.com/Josue-Martinez-Otero)|Desarrollador FontEnd|
|Antonio Ocpaco Dolores|[@ANTONIOOCPACODOLORES](https://github.com/ANTONIOOCPACODOLORES)|Documentador|
