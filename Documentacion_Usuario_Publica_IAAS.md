# Guía de Usuario y Documentación Pública (ImpactoSocial)

Esta guía detalla el funcionamiento de la plataforma **ImpactoSocial** orientado al usuario final, clientes y visitantes públicos. Integra los flujos visuales del Frontend con el procesamiento automático del Motor RAG/ESG.

---

## 🧭 Índice General de la Guía
1. [🌟 Introducción a la Plataforma](#1-introducción-a-la-plataforma)
2. [📥 Carga de Reportes Corporativos (Ingesta)](#2-carga-de-reportes-corporativos-ingesta)
3. [📊 Visualización del Reporte de Evaluación ESG (Dashboard)](#3-visualización-del-reporte-de-evaluación-esg-dashboard)
4. [🌿 Iniciativas de Impacto y Metas Cuantitativas](#4-iniciativas-de-impacto-y-metas-cuantitativas)
5. [🌐 Ranking y Perfil Público de Organizaciones](#5-ranking-y-perfil-público-de-organizaciones)

---

## 1. 🌟 Introducción a la Plataforma

**ImpactoSocial** es un ecosistema digital que permite a las organizaciones auditar sus reportes corporativos bajo dimensiones ESG (Social, Ambiental, Gobernanza, etc.), registrar iniciativas de desarrollo social y transparentar las metas e impactos alcanzados ante inversionistas y la comunidad.

*   **El Frontend (Interfaz de Usuario):** Una aplicación web moderna construida con principios de diseño premium (glassmorphism) que permite navegar por tableros interactivos, rankings públicos y perfiles empresariales.
*   **El Motor (Procesamiento Analítico):** Un motor cognitivo (RAG) que analiza reportes no estructurados (PDF/DOCX), genera métricas de impacto y calcula el **ImpactIndex** de manera automatizada.

---

## 2. 📥 Carga de Reportes Corporativos (Ingesta)

Las organizaciones pueden auditar su documentación anual cargando archivos directamente a la plataforma desde su panel privado.

### Flujo paso a paso del Usuario:
1. Navega a la sección **Mis Reportes** en el menú de navegación lateral.
2. Identifica la zona interactiva superior con borde punteado diseñada para la subida de archivos (drag & drop).
3. Arrastra tus archivos o haz clic en el área para abrir el selector de archivos (se permiten hasta 20 reportes PDF, Word o Texto simultáneamente).
4. **Control de Organización:** Si vas a cargar un documento que no incluye el nombre comercial explícito de tu empresa en el texto, introduce el nombre correcto en el cuadro de texto **"Nombre de la Organización (Opcional)"** antes de continuar.
5. Presiona el botón destacado **"Analizar Reporte"** para iniciar el análisis. Una barra de progreso visual te indicará el avance del proceso.

### Qué ocurre en el Motor (Procesamiento Técnico):
*   El motor extrae el texto del reporte e identifica automáticamente datos como el año de publicación y el color corporativo representativo.
*   Calcula un hash MD5 único del archivo para evitar re-análisis innecesarios y optimizar el tiempo de respuesta.

---

## 3. 📊 Visualización del Reporte de Evaluación ESG (Dashboard)

Una vez completado el procesamiento en el motor, los resultados se visualizan en un panel de control interactivo dentro de la plataforma.

### Secciones interactivas para el Usuario:
*   **Tarjetas de Calificación General:** En la parte superior verás dos puntuaciones clave en medidores circulares animados:
    *   **ImpactIndex:** El promedio general obtenido en la auditoría de sostenibilidad.
    *   **AlternativeIndex:** Un índice secundario ponderado que prioriza la distribución balanceada de las métricas.
*   **Gráfico de Radar ESG:** Un gráfico circular dinámico que muestra visualmente los puntajes desglosados en las 7 dimensiones clave de impacto. Pasar el puntero sobre cada vértice muestra el puntaje exacto de esa dimensión.
*   **Resumen Ejecutivo:** Un recuadro de lectura amigable en español, redactado automáticamente por el motor de IA, que sintetiza los puntos fuertes y los riesgos detectados en el informe.
*   **Fichas Desplegables de Auditoría:** Listado interactivo de las dimensiones evaluadas. Al hacer clic sobre cualquier dimensión (ej. *Social* o *Gobernanza*), se despliega la información metodológica:
    *   La justificación técnica (**Reasoning**) dividida en *Análisis, Evidencia y Conclusión*.
    *   La cita literal del reporte original usada como prueba irrefutable.
    *   Etiquetas con colores correspondientes a marcos internacionales asociados (como `ODS-8` o `GRI-305`).
*   **Exportar Reporte:** Un botón en la esquina superior derecha que permite generar un documento PDF limpio y formateado para su descarga directa.

---

## 4. 🌿 Iniciativas de Impacto y Metas Cuantitativas

Los clientes pueden crear proyectos específicos para canalizar aportaciones y resolver necesidades comunitarias.

### Flujo del Usuario:
1. Dirígete a la pestaña **Iniciativas** en el panel de navegación y pulsa el botón **"Nueva Iniciativa"**.
2. Rellena los datos básicos (nombre, descripción, categoría, fechas de inicio y término) y sube una imagen de portada.
3. **Establecer Metas (Needs):** Dentro de tu iniciativa, crea metas cuantitativas en el modal correspondiente, especificando la cantidad requerida y la unidad de medida (ej. *"Recaudar $15,000 USD"* o *"Plantar 500 árboles"*).
4. **Seguimiento de Aportes (Impacts):** La interfaz muestra un termómetro de progreso interactivo que se llena automáticamente a medida que los donantes registran aportaciones que satisfacen dichas necesidades.
5. **Muro de Comentarios:** En la parte inferior, los usuarios pueden interactuar escribiendo mensajes de apoyo o sugerencias sobre el proyecto en el foro interactivo.

---

## 5. 🌐 Ranking y Perfil Público de Organizaciones

Para fomentar la transparencia, la plataforma expone de cara al público general las clasificaciones de las empresas.

### Flujo del Usuario Externo:
*   **El Ranking Global:** Cualquier visitante web puede acceder al ranking interactivo (`/landing/ranking`), donde se visualiza el listado de las empresas ordenadas de mayor a menor según su puntuación **ImpactIndex**. La tabla cuenta con un buscador rápido en tiempo real.
*   **Perfil Público (`/clients/{slug}`):** Al hacer clic en una empresa del ranking, se carga su perfil público. La interfaz del perfil adapta automáticamente su color temático principal y sus botones a la paleta de color representativa que el motor extrajo del reporte de esa organización.
*   **Exploración de Proyectos:** El visitante puede consultar las iniciativas activas de la organización, ver el progreso de sus metas y participar con aportes o comentarios en el muro de proyectos.
