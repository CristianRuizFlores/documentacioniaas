# Manual de Usuario del Frontend: Interfaz de ImpactoSocial (front-v2)

Esta guía documenta la interfaz de usuario (Frontend) de la plataforma **ImpactoSocial** construida en React con Chakra UI. Detalla los flujos de navegación, las interacciones con los controles visuales y las funciones asignadas a cada una de las secciones del sistema.

---

## Índice de Vistas del Ecosistema
1. [Portal de Autenticación (Sign In)](#1-portal-de-autenticación-sign-in)
2. [Tablero Principal (Dashboard)](#2-tablero-principal-dashboard)
3. [Módulo de Auditoría ESG y Reportes (Reports)](#3-módulo-de-auditoría-esg-y-reportes-reports)
4. [Gestión de Iniciativas (Initiatives)](#4-gestión-de-iniciativas-initiatives)
5. [Detalle de Iniciativa y Muro Social (My Initiative)](#5-detalle-de-iniciativa-y-muro-social-my-initiative)
6. [Gestión de Metas y Necesidades (Needs)](#6-gestión-de-metas-y-necesidades-needs)
7. [Perfil y Configuración Corporativa (Profile)](#7-perfil-y-configuración-corporativa-profile)
8. [Vistas Públicas (Landing & Ranking)](#8-vistas-públicas-landing--ranking)

---

## 1. Portal de Autenticación (Sign In)

*   **Ruta:** `/auth/signin`
*   **Propósito:** Control de acceso seguro para las organizaciones autorizadas.
*   **Layout Visual:** Pantalla limpia con fondo translúcido que resalta el logotipo central de **ImpactoSocial** y una tarjeta de inicio de sesión.

### Pasos para iniciar sesión:
1. Al ingresar a la ruta, se presenta el botón "Iniciar Sesión con Original Auth".
2. Al hacer clic, el sistema redirige automáticamente al usuario al proveedor de identidad de **Original Auth** para ingresar el correo electrónico institucional y la contraseña.
3. Tras la validación, el usuario es redirigido de vuelta al frontend (`/auth/return`), el cual registra la sesión mediante una cookie segura y carga el **Dashboard**.

---

## 2. Tablero Principal (Dashboard)

*   **Ruta:** `/admin/dashboard`
*   **Propósito:** Proporcionar un resumen ejecutivo y cuantitativo del impacto de la organización mediante indicadores clave de rendimiento (KPIs).
*   **Layout Visual:** Diseño en rejilla compuesto por tarjetas de información (estilo glassmorphism) y gráficos dinámicos de rendimiento.

### Elementos del Dashboard:
*   **Tarjetas de Indicadores (KPI Cards):** Cuatro paneles superiores que muestran en tiempo real:
    *   **Iniciativas Totales:** Cantidad total de proyectos registrados.
    *   **Iniciativas Activas / Completadas:** Relación del estado operativo de los proyectos.
    *   **Progreso de Recaudación:** Porcentaje de cumplimiento general de las metas.
    *   **Impacto Recibido:** Suma monetaria o material de aportaciones registradas.
*   **Gráfico de Series Temporales:** Gráfico interactivo (ApexCharts) que detalla la cantidad de aportaciones registradas por mes, permitiendo pasar el cursor por cada barra para visualizar los detalles exactos.

---

## 3. Módulo de Auditoría ESG y Reportes (Reports)

Este módulo gestiona la ingesta de los informes corporativos de sostenibilidad y visualiza los puntajes generados por el motor RAG.

*   **Ruta:** `/admin/reports`

### A. Área de Ingesta y Carga
1. Ubique el panel superior con borde discontinuo que indica **"Arrastra tus reportes aquí o haz clic para seleccionar"** (implementado con `react-dropzone`).
2. Coloque sus archivos PDF, Word o Texto (con un límite de hasta 20 archivos simultáneos).
3. **Control Organizacional:** Si el documento pertenece a una organización externa y no tiene el nombre de la empresa explícito en el texto, ingrese el nombre en el campo **"Nombre de la Organización (Opcional)"** antes de presionar el botón **"Analizar Reporte"**.
4. Una barra de progreso indicará el avance del análisis en el servidor.

### B. Historial de Reportes Evaluados
En la parte inferior de la pantalla de carga se encuentra la tabla de reportes previos:
*   **Filtros de Búsqueda:** Permite filtrar el historial escribiendo el nombre de la organización o seleccionando el año en los campos de búsqueda de la cabecera.
*   **Acciones Disponibles:** Cada fila cuenta con un botón de **"Ver Detalle"** (icono de ojo) para cargar los resultados y un botón **"Eliminar"** (icono de papelera) que borra de forma permanente los datos en PostgreSQL y en el almacenamiento S3.

### C. Visualización de Resultados ESG (Dashboard de Calificación)
Al hacer clic en ver detalle de un reporte, se despliega el panel de resultados:
*   **Puntuaciones de Impacto:** En tarjetas destacadas se visualizan el **ImpactIndex** (promedio de las 7 dimensiones) y el **AlternativeIndex** (puntuación balanceada ponderada).
*   **Gráfico de Radar ESG:** Gráfico circular interactivo que muestra los picos de mejor desempeño y las áreas de oportunidad de la organización a través de las 7 dimensiones de impacto.
*   **Resumen Ejecutivo:** Un recuadro de texto en español que sintetiza los hallazgos principales del motor de IA.

### D. Detalle de Indicadores y Evidencias
Debajo de los gráficos se listan las 7 dimensiones evaluadas de forma interactiva:
1. Haga clic en cualquiera de las dimensiones (ej. *Social* o *Gobernanza*) para expandir su contenido.
2. Cada ficha expandida muestra:
    *   **Calificación del Indicador:** Calificaciones IPS, IMS, SCS, EQS y NIS de esa dimensión.
    *   **Razonamiento del Analista (Reasoning):** Justificación técnica explicada en bloques de *Análisis, Evidencia y Conclusión*.
    *   **Cita Directa:** Fragmento del texto copiado literalmente del reporte que respalda la evaluación.
    *   **Marcos de Cumplimiento (Tags):** Etiquetas coloreadas que representan los estándares mundiales asociados (ej. `ODS-8`, `GRI-305`).
3. El botón superior **"Descargar PDF"** permite generar un documento limpio listo para impresión o descarga.

---

## 4. Gestión de Iniciativas (Initiatives)

Permite la administración y monitoreo de los proyectos específicos de sostenibilidad y comunidad.

*   **Rutas:** `/admin/initiatives` (Listado) y `/admin/newinitiative` (Formulario)

### Cómo usar la interfaz de Iniciativas:
*   **Panel de Lista:** Muestra tarjetas dinámicas para cada campaña. Cada tarjeta incluye la portada del proyecto, el nombre, la categoría principal destacada con color, y una barra de progreso que indica los días restantes de la campaña.
*   **Crear Iniciativa:** Al pulsar el botón flotante **"Crear Iniciativa"**, se abre el formulario de registro:
    1. Ingrese el título, descripción y seleccione la categoría en el menú desplegable.
    2. Defina la fecha de inicio y de término mediante el selector de calendario (`react-datepicker`).
    3. Cargue una imagen representativa para la portada.
    4. Presione **"Guardar Iniciativa"** para publicar la campaña.

---

## 5. Detalle de Iniciativa y Muro Social (My Initiative)

*   **Ruta:** `/admin/my-initiative/:id`
*   **Propósito:** Ficha interna e interactiva de un proyecto.

### Controles Visuales Clave:
*   **Termómetro de Metas:** Barra de progreso horizontal animada que muestra el porcentaje total de avance financiero o material (Needs satisfechas por Impacts).
*   **Lista de Donadores (Aportes):** Lista ordenada de las últimas aportaciones que incluye el avatar del donante, cantidad otorgada y la fecha del aporte.
*   **Muro de Comentarios:** Feed de comentarios al final de la página. Permite a los miembros de la comunidad interactuar:
    *   Escriba el mensaje en la caja de texto.
    *   Presione **"Enviar Comentario"** para publicarlo con su nombre y marca de tiempo actual de forma instantánea.

---

## 6. Gestión de Metas y Necesidades (Needs)

Administración cuantitativa de lo requerido para cada proyecto.

*   **Ruta:** `/admin/needs` y `/admin/newneed`

### Interacción con Metas:
*   **Tabla de Metas:** Muestra de forma tabular las necesidades declaradas. Incluye columnas para el tipo de necesidad (ej. Económica, Social), la cantidad total requerida, la unidad, y un indicador de progreso porcentual del cumplimiento.
*   **Añadir Meta:** Al hacer clic en **"Nueva Meta"**, complete los campos del formulario modal (Monto, Unidad de medida y Descripción) y seleccione la iniciativa a la que pertenece antes de guardar.

---

## 7. Perfil y Configuración Corporativa (Profile)

*   **Ruta:** `/admin/profile` y `/admin/edit-profile`

### Controles de Identidad:
*   **Imagen Corporativa:** Haga clic sobre el contenedor de imagen de perfil para abrir el selector de archivos local y cambiar el avatar de su organización.
*   **Información General:** Campos de texto interactivos para modificar su nombre comercial, descripción de misión/visión, correo de soporte y dirección física.
*   **Enlaces Sociales:** Cuatro campos dedicados para ingresar las URLs de las redes sociales de su empresa (Facebook, X, Instagram y TikTok) que se renderizarán de forma interactiva en su perfil público.

---

## 8. Vistas Públicas (Landing & Ranking)

Secciones diseñadas para el público general, donantes externos y navegadores web. No requieren inicio de sesión y están altamente optimizadas.

*   **Rutas:** `/landing` (Inicio), `/landing/ranking` (Ranking Global) y `/clients/:slug` (Perfil de Empresa)

### Contenido Público:
*   **El Ranking Global:** Tabla interactiva donde los visitantes pueden observar la clasificación de todas las empresas de mayor a menor impacto de acuerdo con sus índices evaluados. Permite realizar búsquedas rápidas mediante una barra superior.
*   **Ficha Pública del Cliente:** Al entrar al perfil con URL amigable (slug), la web adapta toda su interfaz (colores de botones, enlaces, bordes) a la paleta de color representativa que el motor de IA detectó del reporte de la empresa de forma automatizada. Muestra al visitante los datos de contacto y el catálogo de sus iniciativas sociales vigentes.
