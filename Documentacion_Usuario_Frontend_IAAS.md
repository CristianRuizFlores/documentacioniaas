# Manual de Usuario del Frontend: Interfaz de ImpactoSocial (front-v2)

Esta guía documenta la interfaz de usuario (Frontend) de la plataforma **ImpactoSocial** construida en React con Chakra UI. Está diseñada para ser completamente intuitiva y autoexplicativa, detallando los flujos de navegación y la interacción con los controles visuales.

---

## 🧭 Índice de Vistas del Ecosistema
1. [🔑 Portal de Autenticación (Sign In)](#1--portal-de-autenticación-sign-in)
2. [📊 Tablero Principal (Dashboard)](#2--tablero-principal-dashboard)
3. [📁 Módulo de Auditoría ESG y Reportes (Reports)](#3--módulo-de-auditoría-esg-y-reportes-reports)
4. [🌿 Gestión de Iniciativas (Initiatives)](#4--gestión-de-iniciativas-initiatives)
5. [💬 Detalle de Iniciativa y Muro Social (My Initiative)](#5--detalle-de-iniciativa-y-muro-social-my-initiative)
6. [🎯 Gestión de Metas y Necesidades (Needs)](#6--gestión-de-metas-y-necesidades-needs)
7. [⚙️ Perfil y Configuración Corporativa (Profile)](#7--perfil-y-configuración-corporativa-profile)
8. [🌐 Vistas Públicas (Landing & Ranking)](#8--vistas-públicas-landing--ranking)

---

## 1. 🔑 Portal de Autenticación (Sign In)

*   **Ruta:** `/auth/signin`
*   **Propósito:** Acceso seguro para las organizaciones autorizadas.
*   **Layout Visual:** Una pantalla limpia con fondo oscuro translúcido donde resalta el logotipo central de **ImpactoSocial** y una tarjeta interactiva de inicio de sesión.

### Pasos para iniciar sesión:
1. Al ingresar a la ruta, se presenta el botón destacado **"Iniciar Sesión con Original Auth"**.
2. Al hacer clic, el sistema te redirigirá automáticamente a la plataforma externa segura de **Original Auth** para ingresar tu correo institucional y contraseña.
3. Una vez validada tu cuenta, serás redirigido de vuelta al frontend (`/auth/return`), el cual guardará tu sesión en una cookie segura y te dará la bienvenida directamente en el **Dashboard**.

---

## 2. 📊 Tablero Principal (Dashboard)

*   **Ruta:** `/admin/dashboard`
*   **Propósito:** Mostrar una vista consolidada e inmediata del impacto de la organización mediante indicadores clave de rendimiento (KPIs).
*   **Layout Visual:** Un diseño de rejilla que se adapta a cualquier tamaño de pantalla, compuesto por tarjetas con bordes difuminados (glassmorphism) y gráficos dinámicos.

### Elementos interactivos del Dashboard:
*   **Tarjetas de Indicadores (KPI Cards):** Cuatro tarjetas grandes en la parte superior que muestran en tiempo real:
    *   **Iniciativas Totales:** Número acumulado de proyectos.
    *   **Iniciativas Activas / Completadas:** Relación del estado operativo actual.
    *   **Progreso de Recaudación:** Porcentaje de cumplimiento de todas las metas.
    *   **Impacto Recibido:** Suma de aportaciones de donantes.
*   **Gráfico de Series Temporales:** En la zona central, un gráfico interactivo (ApexCharts) detalla la cantidad de aportaciones registradas por mes, permitiendo pasar el cursor por cada barra para ver los detalles exactos.

---

## 3. 📁 Módulo de Auditoría ESG y Reportes (Reports)

Este módulo gestiona la subida de los informes corporativos de sostenibilidad y visualiza los puntajes generados por el motor RAG.

*   **Ruta:** `/admin/reports`

### 📥 A. Zona de Carga de Reportes (Ingesta)
1. Ubica el panel superior con borde discontinuo que dice **"Arrastra tus reportes aquí o haz clic para seleccionar"** (implementado con `react-dropzone`).
2. Suelta tus archivos PDF, Word o Texto (puedes subir hasta 20 archivos a la vez).
3. **Control Inteligente:** Si tu archivo es un reporte externo que no tiene el nombre de tu empresa explícito en el texto, escribe el nombre correcto en el campo **"Nombre de la Organización (Opcional)"** antes de presionar el botón **"Analizar Reporte"**.
4. Una barra de progreso te indicará visualmente el avance del procesamiento en el backend.

### 📋 B. Historial de Reportes Evaluados
En la parte inferior de la pantalla de carga se encuentra la tabla de reportes previos:
*   **Filtros Rápidos:** Puedes filtrar el historial escribiendo el nombre de la organización o seleccionando el año específico en los campos de búsqueda de la cabecera.
*   **Acciones de la Tabla:** Cada fila de reporte cuenta con un botón de **"Ver Detalle"** (icono de ojo) para cargar los resultados y un botón **"Eliminar"** (icono de papelera) que borra de forma permanente los datos en Postgres y en el almacenamiento S3.

### 📈 C. Visualización de Resultados ESG (El Dashboard de Calificación)
Al hacer clic en ver detalle de un reporte, se despliega el panel interactivo de resultados:
*   **Puntuaciones de Impacto:** En tarjetas destacadas verás el **ImpactIndex** (promedio de las 7 dimensiones) y el **AlternativeIndex** (puntuación balanceada ponderada).
*   **Gráfico de Radar ESG:** Un gráfico circular interactivo que muestra visualmente los picos de mejor desempeño y las áreas de oportunidad de la organización a través de las 7 dimensiones de impacto.
*   **Resumen Ejecutivo:** Un recuadro de texto en español que describe de manera clara y profesional los hallazgos principales del motor de IA.

### 🔍 D. Fichas de Auditoría por Dimensión
Debajo de los gráficos, se listan las 7 dimensiones evaluadas de forma interactiva:
1. Haz clic en cualquiera de las dimensiones (ej. *Social* o *Gobernanza*) para expandir su contenido.
2. Cada ficha expandida muestra:
    *   **Calificación del Indicador:** Calificaciones IPS, IMS, SCS, EQS y NIS de esa dimensión.
    *   **Razonamiento del Analista (Reasoning):** Justificación técnica explicada en bloques de *Análisis, Evidencia y Conclusión*.
    *   **Cita Directa:** Fragmento del texto copiado literalmente del reporte que respalda la evaluación.
    *   **Marcos de Cumplimiento (Tags):** Etiquetas coloreadas que representan los estándares mundiales asociados (ej. `ODS-8`, `GRI-305`).
3. Puedes hacer clic en el botón superior **"Descargar PDF"** para generar una hoja limpia e imprimible con todo el desglose.

---

## 4. 🌿 Gestión de Iniciativas (Initiatives)

Permite la administración y monitoreo de los proyectos específicos de sostenibilidad y comunidad.

*   **Rutas:** `/admin/initiatives` (Listado) y `/admin/newinitiative` (Formulario)

### Cómo usar la interfaz de Iniciativas:
*   **Panel de Lista:** Muestra tarjetas dinámicas para cada campaña. Cada tarjeta incluye la portada del proyecto, el nombre, la categoría principal destacada con color, y una barra de progreso que indica los días restantes de la campaña.
*   **Crear Iniciativa:** Al pulsar el botón flotante **"Crear Iniciativa"**, se abre el formulario de registro:
    1. Escribe el título, descripción y selecciona la categoría en el menú desplegable.
    2. Define la fecha de inicio y de término mediante el selector de calendario (`react-datepicker`).
    3. Carga una imagen representativa para la portada.
    4. Presiona **"Guardar Iniciativa"** para publicarla inmediatamente en el ecosistema.

---

## 5. 💬 Detalle de Iniciativa y Muro Social (My Initiative)

*   **Ruta:** `/admin/my-initiative/:id`
*   **Propósito:** Ficha interna e interactiva de un proyecto.

### Controles Visuales Clave:
*   **Termómetro de Metas:** Una barra de progreso horizontal animada que muestra el porcentaje total de avance financiero o material (Needs satisfechas por Impacts).
*   **Lista de Donadores (Aportes):** Una lista ordenada de las últimas aportaciones que incluye el avatar del donante, cantidad otorgada y la fecha del aporte.
*   **Muro de Comentarios:** Un feed estilo red social al final de la página. Permite a los miembros de la comunidad interactuar:
    *   Escribe tu opinión o retroalimentación en la caja de texto.
    *   Presiona **"Enviar Comentario"** para publicarlo con tu nombre y marca de tiempo actual de forma instantánea.

---

## 6. 🎯 Gestión de Metas y Necesidades (Needs)

Administración cuantitativa de lo requerido para cada proyecto.

*   **Ruta:** `/admin/needs` y `/admin/newneed`

### Interacción con Metas:
*   **Tabla de Metas:** Muestra de forma tabular las necesidades declaradas. Incluye columnas para el tipo de necesidad (ej. Económica, Social), la cantidad total requerida, la unidad, y un indicador de progreso porcentual del cumplimiento.
*   **Añadir Meta:** Al hacer clic en **"Nueva Meta"**, completa los campos del formulario modal (Monto, Unidad de medida y Descripción) y selecciona la iniciativa a la que pertenece antes de guardar.

---

## 7. ⚙️ Perfil y Configuración Corporativa (Profile)

*   **Ruta:** `/admin/profile` y `/admin/edit-profile`

### Controles de Identidad:
*   **Imagen Corporativa:** Haz clic sobre el contenedor de imagen de perfil para abrir el selector de archivos local y cambiar el avatar de tu organización.
*   **Información General:** Campos de texto interactivos para modificar tu nombre comercial, descripción de misión/visión, correo de soporte y dirección física.
*   **Enlaces Sociales:** Cuatro campos dedicados para ingresar las URLs de las redes sociales de tu empresa (Facebook, X, Instagram y TikTok) que se renderizarán de forma interactiva en tu perfil público.

---

## 8. 🌐 Vistas Públicas (Landing & Ranking)

Secciones diseñadas para el público general, donantes externos y navegadores web. No requieren inicio de sesión y están altamente optimizadas.

*   **Rutas:** `/landing` (Inicio), `/landing/ranking` (Ranking Global) y `/clients/:slug` (Perfil de Empresa)

### Contenido Público:
*   **El Ranking Global:** Tabla interactiva donde los visitantes pueden observar la clasificación de todas las empresas de mayor a menor impacto de acuerdo con sus índices evaluados. Permite realizar búsquedas rápidas mediante una barra superior.
*   **Ficha Pública del Cliente:** Al entrar al perfil con URL amigable (slug), la web adapta toda su interfaz (colores de botones, enlaces, bordes) a la paleta corporativa que el motor de IA detectó del reporte de la empresa de forma automatizada. Muestra al visitante los datos de contacto y el catálogo de sus iniciativas sociales vigentes.
