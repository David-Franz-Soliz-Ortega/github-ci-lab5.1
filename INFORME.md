# Informe de Laboratorio 5.1: Integración Continua (CI) con GitHub Actions

## 1. Descripción del Proyecto y del Pipeline Configurado

### Proyecto
Se desarrolló una API REST en **Node.js** utilizando el framework **Express** para la gestión de películas (`/movies`). La entidad principal cuenta con operaciones CRUD completas en memoria:
- `GET /movies`: Obtiene el listado completo de películas.
- `GET /movies/:id`: Obtiene el detalle de una película por su ID.
- `POST /movies`: Crea una nueva película (`title`, `director`, `year`).
- `PUT /movies/:id`: Actualiza los datos de una película existente.
- `DELETE /movies/:id`: Elimina una película por su ID.

Además, se implementaron pruebas automatizadas con **Jest** y **Supertest** (`app.test.js`) que validan todos los casos de éxito y error (códigos `200`, `201`, `204`, `400` y `404`), alcanzando un **100% de cobertura de código**.

### Pipeline de Integración Continua (CI)
El workflow de GitHub Actions se encuentra definido en `.github/workflows/ci.yml` y se dispara automáticamente en cada `push` y `pull_request` hacia la rama `main`. Sus etapas son:
1. **Checkout code** (`actions/checkout@v4`): Descarga el código fuente del repositorio en el runner.
2. **Setup Node.js** (`actions/setup-node@v4`): Configura el entorno con Node.js v20 y habilita la caché de `npm`.
3. **Install dependencies** (`npm ci`): Instala de manera limpia y reproducible las dependencias definidas en `package-lock.json`.
4. **Build check** (`npm run build`): Verifica la sintaxis de los archivos principales (`index.js` y `app.js`).
5. **Lint** (`npm run lint`): Ejecuta análisis estático de código con **ESLint** para asegurar buenas prácticas y ausencia de errores sintácticos.
6. **Test and Coverage** (`npm run test:cov`): Ejecuta las pruebas con **Jest** y genera la tabla de cobertura de código (`--coverage`).

---

## 2. Evidencias y Capturas de Pantalla

### 2.1. Historial de ejecuciones en la pestaña Actions
![Historial de ejecuciones en Actions](./capturas/1-historial-actions.png)

### 2.2. Detalle de un workflow exitoso con reporte de cobertura
![Workflow exitoso con cobertura](./capturas/2-workflow-exitoso-cobertura.png)

### 2.3. Detalle de un workflow fallido (forzado intencionalmente)
![Workflow fallido](./capturas/3-workflow-fallido.png)

### 2.4. Configuración de la regla de protección de rama (`main`)
![Regla de protección de rama](./capturas/4-proteccion-rama.png)

---

## 3. Conclusiones y Reflexión sobre la Utilidad de CI

- **Detección temprana de errores:** Configurar un pipeline de Integración Continua permite detectar fallos de sintaxis, problemas de estilo (linting) o regresiones en la lógica del negocio inmediatamente después de subir un cambio, antes de que llegue a la rama principal (`main`).
- **Calidad garantizada mediante protección de ramas:** Al combinar GitHub Actions con reglas de protección de rama (*Branch Protection Rules*) y requerir que el check `ci` pase obligatoriamente antes de fusionar un Pull Request, se evita que código roto contamine el entorno principal por error humano.
- **Confianza mediante cobertura de pruebas:** La ejecución automática de pruebas junto con el reporte de cobertura (`jest --coverage`) ofrece visibilidad clara sobre qué porcentaje del código está siendo validado, dando seguridad al equipo al refactorizar o agregar nuevas funcionalidades.
- **Estandarización del entorno:** Al ejecutarse en un contenedor limpio (`ubuntu-latest`) usando `npm ci`, se elimina el clásico problema de *"en mi máquina sí funciona"*, garantizando que el proyecto se construya y pruebe bajo las mismas condiciones siempre.
