# Guía de Uso del Frontend y del Motor de Evaluación (ImpactoSocial)

Esta guía unificada documenta el ecosistema de **ImpactoSocial**. Está dividida en dos secciones independientes para facilitar su lectura y comprensión:
*   **Parte 1:** Guía detallada sobre el uso de la interfaz de usuario (Frontend).
*   **Parte 2:** Guía de uso, configuración y consumo del Motor de Evaluación (Backend).

---

## Parte 1: Guía de Uso del Frontend (front-v2)

Esta sección documenta el uso de la interfaz web de la plataforma construida en React con Chakra UI. Detalla paso a paso los flujos de trabajo del usuario dentro de la aplicación.

### 1.1 Portal de Autenticación (Sign In)
*   **Ruta de acceso:** `/auth/signin`
*   **Flujo de Acceso:**
    1. Ingrese a la URL de inicio de sesión de la plataforma.
    2. Haga clic en el botón destacado **"Iniciar Sesión con Original Auth"**.
    3. El navegador lo redirigirá a la pasarela externa de Original Auth. Ingrese su correo institucional y su contraseña.
    4. Tras la validación de sus credenciales, la plataforma lo redirigirá de regreso al panel interno (**Dashboard**).

### 1.2 Tablero Principal e Indicadores Clave (Dashboard)
*   **Ruta de acceso:** `/admin/dashboard`
*   **Información Disponible:**
    *   **Tarjetas Métricas Superiores (KPIs):** Muestran el conteo en tiempo real del progreso corporativo:
        *   **Iniciativas Totales:** Número total de proyectos registrados por su organización.
        *   **Iniciativas Activas / Completadas:** Estado operativo de sus campañas.
        *   **Progreso de Recaudación:** Porcentaje de cumplimiento general de las metas de insumos.
        *   **Impacto Recibido:** Suma de aportaciones captadas.
    *   **Gráfico de Tendencias:** Un gráfico de barras interactivo que detalla la cantidad de aportaciones registradas mes a mes. Pase el cursor sobre cada barra para ver los detalles numéricos exactos.

### 1.3 Módulo de Reportes ESG (Reports)
*   **Ruta de acceso:** `/admin/reports`
*   **Subir y Analizar un Reporte:**
    1. Diríjase a la sección superior marcada con un recuadro de borde discontinuo.
    2. Arrastre sus archivos (PDF, Word o texto plano) o haga clic sobre el área para seleccionar los archivos desde su computadora (se permite la carga simultánea de hasta 20 documentos).
    3. **Identificación de Organización:** Si carga un reporte que no cuenta con el nombre de su empresa explícito en el texto del documento, ingréselo en el campo **"Nombre de la Organización (Opcional)"**.
    4. Presione **"Analizar Reporte"** y espere a que la barra de progreso complete el análisis.
*   **Consulta de Historial y Visualización:**
    *   **Tabla del Historial:** Muestra las evaluaciones previas (nombre, año fiscal, fecha de creación, color corporativo y el ImpactIndex obtenido). Puede usar el buscador superior para filtrar por organización o año.
    *   **Acciones:** Haga clic en el icono de "ojo" para visualizar un reporte o en el icono de "papelera" para borrarlo de forma permanente del sistema.
    *   **Dashboard de Resultados:** Al abrir un reporte, se despliegan:
        *   Los puntajes **ImpactIndex** e **AlternativeIndex**.
        *   Un gráfico de radar interactivo con el desempeño en las 7 dimensiones ESG.
        *   Un **Resumen Ejecutivo** conceptual en español.
        *   **Fichas de Detalle:** Haga clic sobre cualquier dimensión (ej. *Social* o *Gobernanza*) para leer la justificación del análisis, la cita textual que sirve de evidencia en su documento, y las etiquetas de estándares internacionales vinculados (ODS, GRI, SASB).
        *   **Exportar:** Use el botón **"Descargar PDF"** en la esquina superior derecha para generar una copia imprimible de la evaluación.

### 1.4 Gestión de Iniciativas (Initiatives)
*   **Ruta de acceso:** `/admin/initiatives`
*   **Acciones del Administrador:**
    *   **Listado de Proyectos:** Visualice las tarjetas dinámicas con la portada del proyecto, el nombre, la categoría destacada en color y una barra de progreso que indica los días restantes de la campaña.
    *   **Registrar Iniciativa:** Haga clic en el botón flotante **"Crear Iniciativa"** (ruta `/admin/newinitiative`) para abrir el formulario e ingresar:
        1. Nombre de la campaña y descripción.
        2. Categoría (ej. Sostenibilidad, Educación).
        3. Selector de fechas de inicio y fin (usando calendario interactivo).
        4. Carga de imagen de portada.
        5. Presione **"Guardar Iniciativa"** para publicar la campaña.

### 1.5 Ficha Detallada de Iniciativa y Muro de Comentarios
*   **Ruta de acceso:** `/admin/my-initiative/:id` (se accede haciendo clic sobre cualquier tarjeta de iniciativa)
*   **Componentes de la Vista:**
    *   **Termómetro de Metas:** Barra de progreso que indica el porcentaje total de donaciones o insumos recaudados en esa iniciativa específica.
    *   **Lista de Aportaciones:** Detalle de los donantes con su nombre, cantidad aportada y la fecha del hito de impacto.
    *   **Muro Social (Foro):** Caja de texto al final de la página que permite a los usuarios escribir comentarios de avance o retroalimentación. Escriba su comentario y presione **"Enviar Comentario"** para publicarlo con su firma y fecha actuales.

### 1.6 Administración de Metas y Necesidades (Needs)
*   **Ruta de acceso:** `/admin/needs`
*   **Acciones del Administrador:**
    *   **Tabla de Metas:** Lista las necesidades de recursos asociadas a cada proyecto (tipo de necesidad, cantidad requerida, unidad de medida, descripción y avance acumulado).
    *   **Crear Meta:** Presione **"Nueva Meta"** para desplegar el formulario modal y registrar las cantidades y descripciones deseadas para su proyecto activo.

### 1.7 Perfil y Configuración de la Organización
*   **Ruta de acceso:** `/admin/profile` y `/admin/edit-profile`
*   **Acciones del Administrador:**
    *   **Imagen de Perfil:** Haga clic en el contenedor del avatar para cambiar el logo de la organización.
    *   **Información General:** Edite el nombre de la empresa, descripción corporativa, correo electrónico de contacto y dirección física.
    *   **Redes Sociales:** Registre o actualice los enlaces a las cuentas institucionales de Facebook, X (Twitter), Instagram y TikTok.

### 1.8 Vistas Públicas (Ranking y Ficha Pública)
Páginas que no requieren credenciales de acceso y están expuestas en la web para fines de transparencia.
*   **El Ranking Global:** Tabla interactiva en `/landing/ranking` que lista a las empresas ordenadas por su puntaje **ImpactIndex**.
*   **Ficha Pública del Cliente:** Al entrar al perfil (`/clients/{slug}`), la página adapta dinámicamente toda su combinación de colores y botones al color corporativo que el motor detectó de manera automatizada. Muestra la información de contacto y el catálogo de las iniciativas vigentes de la organización.

---

## Parte 2: Guía de Uso del Motor de Evaluación y Backend (motor_IaaS)

Esta sección documenta el uso, configuración, comandos de consola y consumo de APIs del motor de evaluación de impacto ESG.

### 2.1 Requisitos y Configuración de Entorno
El motor requiere Node.js v20+, una base de datos PostgreSQL con soporte para la extensión `pgvector`, y una clave API de OpenAI.

#### Variables de Entorno (Archivo `.env`):
Configure el archivo `.env` en la raíz del proyecto con las siguientes claves:
*   `DATABASE_URL`: Cadena de conexión de PostgreSQL.
*   `OPENAI_API_KEY`: Clave de acceso a la API de OpenAI.
*   `PORT`: Puerto en el que el servidor de Express escuchará las peticiones (por defecto `3000`).
*   `SKIP_MIGRATIONS`: Configurar en `true` si desea omitir la ejecución de migraciones de base de datos durante el inicio del servidor.
*   `MEDIAAS_API_KEY` y `MEDIAAS_SECRET_KEY`: Credenciales necesarias para firmar las peticiones y subir reportes al repositorio AWS S3 mediante el servicio externo Media-as-a-Service.

### 2.2 Comandos de Consola y Scripts de Mantenimiento
*   **Ejecución en Desarrollo:** `npm run dev`
*   **Compilación del Proyecto:** `npm run build`
*   **Ejecución en Producción:** `npm start`
*   **Purga de la Caché Inmutable:** `npm run clean:cache` (elimina los archivos JSON del directorio local `pdf/cache/`).
*   **Recálculo de Índices (Consola):** `npx ts-node src/scripts/recalculate_alternative_index.ts` (recalcula el `AlternativeIndex` de todos los registros en base de datos sin consumir API de OpenAI).
*   **Limpieza de Base de Datos:** `npx ts-node clear_db.ts` (vacía por completo las tablas `reports`, `chunks` y `documents`).

### 2.3 El Pipeline del Motor RAG y Configuración de IA
*   **Segmentación y Embeddings:** El texto de los archivos se divide en fragmentos (chunks) de 2500 caracteres con un solapamiento de 300 caracteres mediante `RecursiveCharacterTextSplitter`. Los fragmentos se vectorizan con `text-embedding-3-small` (1536 dimensiones) y se almacenan temporalmente en la tabla `chunks`.
*   **Búsqueda Coseno:** El backend realiza búsquedas semánticas para las 7 dimensiones ESG usando el operador de similitud coseno de pgvector (`<=>` en SQL) limitando a los 8 fragmentos con mayor similitud.
*   **Borrado por Privacidad:** Tras guardar los resultados en la tabla `reports`, el backend ejecuta la función `deleteDocumentVectors` que elimina permanentemente los fragmentos y vectores de base de datos para proteger la confidencialidad de la información.
*   **Reglas del Evaluador LLM (GPT-4o):** Las evaluaciones son analizadas por `gpt-4o` mediante *Structured Outputs* de OpenAI. Se inyecta la regla de que la IA evalúa cualitativamente y no debe realizar sumas ni multiplicaciones matemáticas finales ("La IA evalúa, el Backend calcula"). El campo `reasoning` debe seguir la estructura `**Análisis:** [análisis] **Evidencia:** [cifras] **Conclusión:** [veredicto]`.

### 2.4 Referencia para el Consumo de la API REST
Todos los endpoints están prefijados bajo la ruta `/api`.

#### 1. Subir y Procesar Reportes
*   **Método:** `POST` | **Ruta:** `/api/process-report`
*   **Content-Type:** `multipart/form-data`
*   **Parámetros:** `documents` (múltiples archivos a procesar) y `organizationName` (String opcional).
*   **Respuesta Exitosa (200 OK):** Retorna el JSON con los resultados de la evaluación, incluyendo nombre, año, color principal, puntuaciones consolidadas (ImpactIndex, AlternativeIndex, y desglose de sub-índices) y resumen ejecutivo.

#### 2. Listar Todos los Reportes Evaluados
*   **Método:** `GET` | **Ruta:** `/api/reports`
*   **Respuesta:** Listado de reportes analizados en el sistema (slug, nombre de organización, color, dominio, año de reporte e ImpactIndex).

#### 3. Obtener Detalle de una Evaluación
*   **Método:** `GET` | **Ruta:** `/api/report/:slug`
*   **Parámetros Query:** `?year=2023` (opcional).

#### 4. Obtener Tabla de Posiciones y Ranking
*   **Método:** `GET` | **Ruta:** `/api/ranking`
*   **Respuesta:** Listado ordenable optimizado para el ranking público de la plataforma.

#### 5. Eliminar una Evaluación
*   **Método:** `DELETE` | **Ruta:** `/api/report/:slug`
*   **Comportamiento:** Elimina el registro de Postgres y los archivos binarios asociados en AWS S3 mediante la API de MediaAAS.
