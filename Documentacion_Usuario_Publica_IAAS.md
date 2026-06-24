# Guía de Usuario y del Motor de Evaluación (ImpactoSocial)

Esta guía unificada detalla el funcionamiento de la plataforma **ImpactoSocial**, integrando la experiencia visual del Frontend (front-v2) con los procesos analíticos y algorítmicos del Motor de Evaluación (motor_IaaS).

---

## Índice de Contenidos
1. [Introducción a la Plataforma](#1-introducción-a-la-plataforma)
2. [Autenticación y Control de Acceso](#2-autenticación-y-control-de-acceso)
3. [Tablero Principal e Indicadores Clave (Dashboard)](#3-tablero-principal-e-indicadores-clave-dashboard)
4. [Módulo de Auditoría ESG y Procesamiento RAG](#4-módulo-de-auditoría-esg-y-procesamiento-rag)
5. [Gestión de Iniciativas, Metas e Impacto](#5-gestión-de-iniciativas-metas-e-impacto)
6. [Transparencia: Ranking y Perfil Público de Organizaciones](#6-transparencia-ranking-y-perfil-público-de-organizaciones)

---

## 1. Introducción a la Plataforma

El ecosistema **ImpactoSocial** está compuesto por dos componentes principales que trabajan de forma sincronizada:
*   **La Interfaz de Usuario (Frontend):** Una Single Page Application (SPA) construida en React con Chakra UI y Framer Motion. Proporciona una interfaz moderna con efectos visuales avanzados (glassmorphism) y gráficos interactivos para la visualización de datos.
*   **El Motor de Evaluación (Backend):** Servidor continuo en Express.js con soporte de TypeScript, base de datos PostgreSQL enriquecida con la extensión pgvector y modelos de lenguaje de OpenAI. Es el encargado de extraer, indexar, evaluar información no estructurada y calcular los índices ESG de las organizaciones.

---

## 2. Autenticación y Control de Acceso

### Flujo en la Interfaz (Frontend):
*   La pantalla de inicio de sesión (`/auth/signin`) presenta un diseño minimalista con una tarjeta central de acceso.
*   El botón "Iniciar Sesión con Original Auth" redirige al usuario al portal externo seguro del proveedor de identidad.
*   Una vez validadas las credenciales, el flujo retorna a la ruta del cliente (`/auth/return`), donde la interfaz almacena la sesión de manera segura mediante cookies e inicia el Dashboard.

### Operación en el Motor (Backend):
*   El backend expone el endpoint `/api/auth/validate_session` que comprueba la validez del token recibido de Original Auth.
*   Si el correo del usuario no existe en la tabla `clients` de PostgreSQL, el motor provisiona una nueva cuenta y le asigna un identificador único (UUID).
*   Se genera un token JWT firmado para asegurar las siguientes peticiones HTTP del cliente mediante la cabecera `Authorization`.

---

## 3. Tablero Principal e Indicadores Clave (Dashboard)

### Flujo en la Interfaz (Frontend):
*   Al ingresar a `/admin/dashboard`, el usuario visualiza cuatro tarjetas de rendimiento en la parte superior:
    *   **Iniciativas Totales:** Cantidad de proyectos registrados.
    *   **Iniciativas Activas / Completadas:** Estado operativo de las campañas.
    *   **Progreso de Recaudación:** Porcentaje consolidado de las metas del cliente.
    *   **Impacto Recibido:** Suma de aportaciones de donantes.
*   En la parte central, se renderiza un gráfico de series temporales mensual utilizando la librería ApexCharts para facilitar la lectura del flujo histórico de impactos.

### Operación en el Motor (Backend):
*   El motor centraliza las peticiones mediante el endpoint `/api/clients/{client_id}/metrics/general`.
*   Para evitar el problema de consultas secundarias múltiples (N+1 queries) en base de datos, el servidor realiza consultas relacionales optimizadas agregando los datos de las tablas `initiatives`, `needs` e `impacts`.

---

## 4. Módulo de Auditoría ESG y Procesamiento RAG

Este es el módulo central del sistema, donde la interacción visual se acopla directamente con la base de datos vectorial y el modelo de inteligencia artificial.

### A. Subida del Reporte Corporativo (Frontend)
*   En la ruta `/admin/reports`, el usuario dispone de un área de arrastre de archivos (drag & drop) desarrollada con la librería `react-dropzone`.
*   El usuario puede cargar hasta 20 archivos simultáneamente (PDF, Word o Texto plano).
*   Si la documentación pertenece a una organización externa cuyo nombre no está implícito en el texto, el usuario puede ingresarlo manualmente en el campo "Nombre de la Organización".
*   Al pulsar "Analizar Reporte", se muestra una barra de progreso que indica el avance del procesamiento asíncrono.

### B. Indexación y Búsqueda Semántica RAG (Backend)
1.  **Deduplicación:** El motor calcula el hash MD5 del archivo subido. Si ya existe un reporte con ese mismo hash en la base de datos o en la caché JSON del servidor (`pdf/cache/`), devuelve los resultados guardados al instante.
2.  **Extracción de Metadatos:** Si es un reporte nuevo, se envía el inicio del documento a `gpt-4o-mini` para identificar el nombre oficial de la organización, el año fiscal (`reportYear`), su dominio y un color corporativo representativo.
3.  **Vectorización:** El texto completo se segmenta en fragmentos (chunks) de 2500 caracteres con un solapamiento de 300 caracteres para mantener el contexto.
4.  **Almacenamiento Vectorial:** Utilizando el modelo de embeddings `text-embedding-3-small` de OpenAI, se generan vectores de 1536 dimensiones que se guardan temporalmente en la tabla `chunks` con tipo de dato `vector(1536)` (pgvector).
5.  **Búsqueda Coseno:** El motor realiza consultas automáticas sobre los vectores para cada una de las 7 dimensiones ESG de la plataforma (Social, Ambiental, Cultural, Performance, Colaboración, Económico y Gobernanza), seleccionando los 8 fragmentos con mayor similitud semántica.
6.  **Borrado por Privacidad:** Una vez calculadas las calificaciones, el motor ejecuta la función `deleteDocumentVectors` para eliminar permanentemente los vectores e información del documento del espacio vectorial de la base de datos, protegiendo la confidencialidad de la información.

### C. Visualización e Interpretación de Resultados (Frontend)
Una vez finalizado el procesamiento, la interfaz carga las métricas estructuradas en el cliente:
*   **Puntuaciones Globales:** Se muestran las calificaciones consolidadas **ImpactIndex** e **AlternativeIndex** en medidores radiales animados.
*   **Gráfico de Radar ESG:** Desglosa el desempeño de la empresa a través de las 7 dimensiones de impacto, utilizando la librería Recharts.
*   **Resumen Ejecutivo:** Caja de texto con una síntesis conceptual redactada en español por la inteligencia artificial.
*   **Tabla de Auditoría:** Lista expandible de las dimensiones evaluadas. Al abrir una pestaña, el usuario ve el razonamiento de la IA (dividido en Análisis, Evidencia y Conclusión), la cita exacta del texto original y etiquetas que indican su alineación con estándares internacionales (GRI, SASB, ODS) validados por el mapeador normativo del backend.
*   **Exportar a PDF:** Botón que genera y descarga una versión limpia e imprimible de la evaluación usando la librería jspdf.

---

## 5. Gestión de Iniciativas, Metas e Impacto

### Creación de Iniciativas y Carga de Metas (Frontend):
*   El usuario administrador puede crear iniciativas en la pestaña "Iniciativas" pulsando "Crear Iniciativa". El formulario solicita título, descripción, categoría, portada y rango de fechas mediante un selector de calendario (`react-datepicker`).
*   Posteriormente, en la pestaña de "Needs" (Metas), el usuario puede registrar metas de insumos o fondos monetarios en un modal emergente (ej. "Instalar 100 paneles solares").

### Seguimiento de Aportes y Muro Social (Frontend y Backend):
*   En la vista de la iniciativa individual (`/admin/my-initiative/:id`), el frontend muestra un termómetro dinámico de progreso. A medida que se registran aportaciones de impacto (donaciones en la base de datos), la barra se actualiza visualmente.
*   **El Muro Social (Foro):** Permite a los usuarios añadir comentarios sobre los avances. El frontend lee los mensajes de forma ordenada y los envía al endpoint `/api/comments` en el backend, el cual los guarda con autor, texto y marca de tiempo en la tabla `comments` de PostgreSQL.

---

## 6. Transparencia: Ranking y Perfil Público de Organizaciones

### El Ranking Global de Impacto:
*   En la ruta pública `/landing/ranking`, el frontend despliega una tabla que muestra la lista de todas las empresas evaluadas.
*   El usuario puede buscar organizaciones en tiempo real u ordenarlas por su puntaje consolidado de ImpactIndex o AlternativeIndex.
*   El motor sirve esta información desde la tabla `reports` mediante el endpoint `/api/ranking`.

### Perfil Público de la Empresa:
*   Al acceder a `/clients/{slug}`, el frontend carga el perfil de la organización con un slug amigable de URL.
*   **Cromática Dinámica:** La aplicación adapta automáticamente los tonos de la interfaz (botones, acentos de fondo) al color corporativo principal de la empresa que fue extraído previamente del reporte por el motor de IA.
*   Muestra al visitante la información de contacto corporativa de la organización y el listado de sus iniciativas de impacto vigentes con sus respectivos estados de avance.
