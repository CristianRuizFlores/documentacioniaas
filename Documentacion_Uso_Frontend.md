# Guía de Uso del Frontend: Interfaz de Usuario (front-v2)

Esta guía documenta el uso de la interfaz web (Frontend) de **ImpactoSocial** construida en React con Chakra UI. Detalla paso a paso los flujos de trabajo del usuario dentro de la aplicación.

---

## Índice de Secciones
1. [Portal de Autenticación (Sign In)](#1-portal-de-autenticación-sign-in)
2. [Tablero Principal e Indicadores Clave (Dashboard)](#2-tablero-principal-e-indicadores-clave-dashboard)
3. [Módulo de Reportes ESG (Reports)](#3-módulo-de-reportes-esg-reports)
4. [Gestión de Iniciativas (Initiatives)](#4-gestión-de-iniciativas-initiatives)
5. [Ficha Detallada de Iniciativa y Muro de Comentarios](#5-ficha-detallada-de-iniciativa-y-muro-de-comentarios)
6. [Administración de Metas y Necesidades (Needs)](#6-administración-de-metas-y-necesidades-needs)
7. [Perfil y Configuración de la Organización](#7-perfil-y-configuración-de-la-organización)
8. [Vistas Públicas (Ranking y Ficha Pública)](#8-vistas-públicas-ranking-y-ficha-pública)

---

## 1. Portal de Autenticación (Sign In)

Ruta de acceso: `/auth/signin`

### Flujo de Acceso:
1. Ingrese a la URL de inicio de sesión de la plataforma.
2. Haga clic en el botón destacado **"Iniciar Sesión con Original Auth"**.
3. El navegador lo redirigirá a la pasarela externa de Original Auth. Ingrese su correo institucional y su contraseña.
4. Tras la validación de sus credenciales, la plataforma lo redirigirá de regreso al panel interno (**Dashboard**).

---

## 2. Tablero Principal e Indicadores Clave (Dashboard)

Ruta de acceso: `/admin/dashboard`

### Información Disponible:
*   **Tarjetas Métricas Superiores (KPIs):** Muestran el conteo en tiempo real del progreso corporativo:
    *   **Iniciativas Totales:** Número total de proyectos registrados por su organización.
    *   **Iniciativas Activas / Completadas:** Estado operativo de sus campañas.
    *   **Progreso de Recaudación:** Porcentaje de cumplimiento general de las metas de insumos.
    *   **Impacto Recibido:** Suma de aportaciones captadas.
*   **Gráfico de Tendencias:** Un gráfico de barras interactivo que detalla la cantidad de aportaciones registradas mes a mes. Pase el cursor sobre cada barra para ver los detalles numéricos exactos.

---

## 3. Módulo de Reportes ESG (Reports)

Ruta de acceso: `/admin/reports`

### A. Subir y Analizar un Reporte
1. Diríjase a la sección superior marcada con un recuadro de borde discontinuo.
2. Arrastre sus archivos (PDF, Word o texto plano) o haga clic sobre el área para seleccionar los archivos desde su computadora (se permite la carga simultánea de hasta 20 documentos).
3. **Identificación de Organización:** Si carga un reporte que no cuenta con el nombre de su empresa explícito en el texto del documento, ingréselo en el campo **"Nombre de la Organización (Opcional)"**.
4. Presione **"Analizar Reporte"** y espere a que la barra de progreso complete el análisis.

### B. Consulta de Historial y Visualización
*   **Tabla del Historial:** Muestra las evaluaciones previas (nombre, año fiscal, fecha de creación, color corporativo y el ImpactIndex obtenido). Puede usar el buscador superior para filtrar por organización o año.
*   **Acciones:** Haga clic en el icono de "ojo" para visualizar un reporte o en el icono de "papelera" para borrarlo de forma permanente del sistema.
*   **Dashboard de Resultados:** Al abrir un reporte, se despliegan:
    *   Los puntajes **ImpactIndex** e **AlternativeIndex**.
    *   Un gráfico de radar interactivo con el desempeño en las 7 dimensiones ESG.
    *   Un **Resumen Ejecutivo** conceptual en español.
    *   **Fichas de Detalle:** Haga clic sobre cualquier dimensión (ej. *Social* o *Gobernanza*) para leer la justificación del análisis, la cita textual que sirve de evidencia en su documento, y las etiquetas de estándares internacionales vinculados (ODS, GRI, SASB).
    *   **Exportar:** Use el botón **"Descargar PDF"** en la esquina superior derecha para generar una copia imprimible de la evaluación.

---

## 4. Gestión de Iniciativas (Initiatives)

Ruta de acceso: `/admin/initiatives`

### Acciones del Administrador:
*   **Listado de Proyectos:** Visualice las tarjetas dinámicas con la portada del proyecto, el nombre, la categoría destacada en color y una barra de progreso que indica los días restantes de la campaña.
*   **Registrar Iniciativa:** Haga clic en el botón flotante **"Crear Iniciativa"** (ruta `/admin/newinitiative`) para abrir el formulario e ingresar:
    1. Nombre de la campaña y descripción.
    2. Categoría (ej. Sostenibilidad, Educación).
    3. Selector de fechas de inicio y fin (usando calendario interactivo).
    4. Carga de imagen de portada.
    5. Presione **"Guardar Iniciativa"** para publicarla en el portal.

---

## 5. Ficha Detallada de Iniciativa y Muro de Comentarios

Ruta de acceso: `/admin/my-initiative/:id` (se accede haciendo clic sobre cualquier tarjeta de iniciativa)

### Componentes de la Vista:
*   **Termómetro de Metas:** Barra de progreso que indica el porcentaje total de donaciones o insumos recaudados en esa iniciativa específica.
*   **Lista de Aportaciones:** Detalle de los donantes con su nombre, cantidad aportada y la fecha del hito de impacto.
*   **Muro Social (Foro):** Caja de texto al final de la página que permite a los usuarios escribir comentarios de avance o retroalimentación. Escriba su comentario y presione **"Enviar Comentario"** para publicarlo con su firma y fecha actuales.

---

## 6. Administración de Metas y Necesidades (Needs)

Ruta de acceso: `/admin/needs`

### Acciones del Administrador:
*   **Tabla de Metas:** Lista las necesidades de recursos asociadas a cada proyecto (tipo de necesidad, cantidad requerida, unidad de medida, descripción y avance acumulado).
*   **Crear Meta:** Presione **"Nueva Meta"** para desplegar el formulario modal y registrar las cantidades y descripciones deseadas para su proyecto activo.

---

## 7. Perfil y Configuración de la Organización

Ruta de acceso: `/admin/profile` y `/admin/edit-profile`

### Acciones del Administrador:
*   **Imagen de Perfil:** Haga clic en el contenedor del avatar para cambiar el logo de la organización.
*   **Información General:** Edite el nombre de la empresa, descripción corporativa, correo electrónico de contacto y dirección física.
*   **Redes Sociales:** Registre o actualice los enlaces a las cuentas institucionales de Facebook, X (Twitter), Instagram y TikTok.

---

## 8. Vistas Públicas (Ranking y Ficha Pública)

Páginas que no requieren credenciales de acceso y están expuestas en la web para fines de transparencia.

### Secciones Públicas:
*   **El Ranking Global:** Tabla interactiva en `/landing/ranking` que lista a las empresas ordenadas por su puntaje **ImpactIndex**.
*   **Ficha Pública del Cliente:** Al entrar al perfil (`/clients/{slug}`), la página adapta dinámicamente toda su combinación de colores y botones al color corporativo que el motor detectó de manera automatizada. Muestra la información de contacto y el catálogo de las iniciativas vigentes de la organización.
