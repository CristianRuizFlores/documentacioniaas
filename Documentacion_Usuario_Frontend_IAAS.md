# Manual de Usuario del Frontend: Interfaz de ImpactoSocial (front-v2)

Esta guía documenta la interfaz de usuario (Frontend) de la plataforma **ImpactoSocial** construida en React con Chakra UI. Detalla cada una de las páginas, los componentes interactivos del cliente y señala dónde deben colocarse las capturas de pantalla de la interfaz para ilustrar los flujos.

---

## Índice de Páginas del Frontend
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
*   **Componente Principal:** `SignIn.tsx`
*   **Descripción:** Portal de acceso seguro para las organizaciones. Implementa autenticación única (SSO) mediante el proveedor de identidad **Original Auth**.
*   **Acciones del Usuario:**
    1. Introducir las credenciales o hacer clic en "Iniciar Sesión".
    2. Redirección automática al callback seguro `/auth/return` tras la validación en Original Auth.
    3. Emisión de cookies seguras HTTP-only con el JWT y redirección al Dashboard.

---
> [!NOTE]
> **CAPTURA DE PANTALLA #1: Portal de Acceso**
> *   **Nombre:** `front_01_signin.png`
> *   **Ubicación:** Justo debajo del título de la sección 1.
> *   **Contenido visual:** Formulario de inicio de sesión con el logotipo de ImpactoSocial y el botón de inicio de sesión de Original Auth.
---

---

## 2. Tablero Principal (Dashboard)

*   **Ruta:** `/admin/dashboard`
*   **Componente Principal:** `Dashboard.tsx`
*   **Descripción:** Panel de control central del cliente. Proporciona resúmenes ejecutivos rápidos de las operaciones de impacto mediante tarjetas métricas (KPIs).
*   **Elementos Visuales:**
    *   **Tarjetas de Resumen (KPI Cards):**
        *   Total de Iniciativas registradas.
        *   Iniciativas activas y completadas.
        *   Porcentaje general de metas de recaudación cubiertas.
        *   Total acumulado de impactos o donaciones registradas en el periodo actual.
    *   **Gráficos Rápidos:** Gráficos de barra que muestran la distribución mensual de aportaciones de impacto.

---
> [!NOTE]
> **CAPTURA DE PANTALLA #2: Panel del Dashboard Central**
> *   **Nombre:** `front_02_dashboard.png`
> *   **Ubicación:** Debajo de la lista de Elementos Visuales.
> *   **Contenido visual:** Vista del dashboard principal mostrando las tarjetas métricas con bordes degradados (estilo glassmorphism) y gráficos de barras de resumen mensual.
---

---

## 3. Módulo de Auditoría ESG y Reportes (Reports)

Este es el módulo central de integración con el motor de evaluación RAG. Permite la carga, filtrado y lectura interactiva de las calificaciones del ImpactIndex.

*   **Ruta:** `/admin/reports`
*   **Componente Principal:** `index.tsx` (en carpeta `VReports`)
*   **Sub-componentes Clave:**

### A. Área de Ingesta y Carga
*   **Componente:** `ReportsFilters.tsx` / Dropzone
*   **Descripción:** Zona interactiva para subir archivos utilizando la librería `react-dropzone`.
*   **Acciones:** Arrastrar archivos o hacer clic para seleccionar PDFs o documentos Word. Permite ingresar manualmente el nombre de la organización si es un archivo externo desconocido.

---
> [!NOTE]
> **CAPTURA DE PANTALLA #3: Zona de Carga de Reportes**
> *   **Nombre:** `front_03_reports_upload.png`
> *   **Ubicación:** Debajo de la descripción del Área de Ingesta.
> *   **Contenido visual:** Zona punteada de drag & drop activa para archivos PDF, con un cuadro de texto para ingresar opcionalmente el nombre de la organización.
---

### B. Historial de Reportes Evaluados
*   **Componente:** `ReportsTable.tsx`
*   **Descripción:** Tabla que lista las evaluaciones anteriores del cliente. Muestra el nombre de la organización, año del reporte, fecha de análisis, color representativo detectado, y el puntaje consolidado de **ImpactIndex**. Incluye botones para visualizar y borrar.

---
> [!NOTE]
> **CAPTURA DE PANTALLA #4: Tabla de Historial de Reportes**
> *   **Nombre:** `front_04_reports_table.png`
> *   **Ubicación:** Debajo de la descripción del Historial.
> *   **Contenido visual:** Tabla con registros de reportes evaluados, mostrando botones de acción rápidos (Ver Detalle / Eliminar) y el año correspondiente.
---

### C. Visualización Gráfica y Resultados
*   **Componente:** `ReportCharts.tsx` y `ReportsChartsSummary.tsx`
*   **Descripción:** Renderiza los puntajes finales devueltos por el motor. Utiliza **Recharts** para dibujar gráficos de radar y barras que representan las 7 dimensiones ESG.
*   **Contenidos:**
    *   Visualizador del **ImpactIndex** (puntuación del 0 al 100).
    *   Visualizador del **AlternativeIndex** (puntuación balanceada ponderada).
    *   Texto interactivo del **Resumen Ejecutivo** en español generado por IA.

---
> [!NOTE]
> **CAPTURA DE PANTALLA #5: Gráficos de Resultados ESG**
> *   **Nombre:** `front_05_reports_charts.png`
> *   **Ubicación:** Debajo de la sección de Visualización Gráfica.
> *   **Contenido visual:** Gráfico de radar multicolor que desglosa las dimensiones ESG y los medidores radiales del ImpactIndex global.
---

### D. Detalle de Indicadores y Evidencias
*   **Componente:** `ReportPreviewAndDownload.tsx`
*   **Descripción:** Fichas interactivas desplegables para auditar cada dimensión.
*   **Elementos Visuales:**
    *   Calificación detallada del indicador (IPS, IMS, SCS, EQS, NIS).
    *   Justificación metodológica (**Reasoning**): Muestra la estructura de *Análisis, Evidencia y Conclusión*.
    *   Cita original: El texto exacto extraído del reporte por el LLM.
    *   Estándares asociados: Etiquetas de marcos internacionales (ODS-8, GRI-305, SASB).
    *   Botón **Exportar a PDF**: Genera una hoja limpia lista para impresión o envío.

---
> [!NOTE]
> **CAPTURA DE PANTALLA #6: Ficha de Auditoría e Indicadores**
> *   **Nombre:** `front_06_reports_detail.png`
> *   **Ubicación:** Debajo de la descripción del Detalle de Indicadores.
> *   **Contenido visual:** Ficha de una dimensión (ej. Gobernanza) expandida, mostrando la justificación textual del modelo y los tags de ODS/GRI asociados.
---

---

## 4. Gestión de Iniciativas (Initiatives)

Módulo encargado de administrar los proyectos socioambientales individuales de la empresa.

*   **Rutas:** `/admin/initiatives` (Listado) y `/admin/newinitiative` (Creación)
*   **Componente Principal:** `Iniciativas.tsx` y `CrearIniciativa.tsx`
*   **Flujo del Usuario:**
    1. El usuario accede al grid de iniciativas en donde se listan tarjetas con el título, categoría (ej. Sostenibilidad, Educación), y estado (Activo/Terminado).
    2. Hace clic en "Crear Nueva Iniciativa" para desplegar un formulario donde captura:
        *   Nombre de la iniciativa.
        *   Descripción.
        *   Categoría principal.
        *   Fecha de inicio y fin.
        *   Subida de imagen de portada de la campaña.
        *   Metas cualitativas y cuantitativas preliminares.

---
> [!NOTE]
> **CAPTURA DE PANTALLA #7: Grid de Iniciativas y Formulario de Registro**
> *   **Nombre:** `front_07_initiatives.png`
> *   **Ubicación:** Debajo del flujo del usuario en la sección 4.
> *   **Contenido visual:** Tarjetas de iniciativas con barras de progreso de tiempo y el botón flotante para crear nuevos proyectos.
---

---

## 5. Detalle de Iniciativa y Muro Social (My Initiative)

*   **Ruta:** `/admin/my-initiative/:id`
*   **Componente Principal:** `MyInitiative.tsx`
*   **Descripción:** La página de control detallada de una campaña específica. Integra el muro social y el progreso cuantitativo de donaciones.
*   **Secciones del Panel:**
    *   **Indicador de Progreso:** Muestra gráficamente el porcentaje de recaudación cubierto frente a las metas asignadas.
    *   **Lista de Donadores:** Lista de personas, empresas u organizaciones que han registrado aportes de impacto en la iniciativa.
    *   **Muro de Comentarios:** Foro interactivo donde los usuarios dejan mensajes, comentarios y retroalimentación en tiempo real.

---
> [!NOTE]
> **CAPTURA DE PANTALLA #8: Detalle de Iniciativa y Foro**
> *   **Nombre:** `front_08_initiative_detail.png`
> *   **Ubicación:** Debajo de las Secciones del Panel.
> *   **Contenido visual:** Vista de la ficha de campaña mostrando las metas en formato de termómetro de progreso, lista de contribuidores y la caja de comentarios del foro.
---

---

## 6. Gestión de Metas y Necesidades (Needs)

*   **Ruta:** `/admin/needs` y `/admin/newneed`
*   **Componente Principal:** `Needs.tsx`
*   **Descripción:** Panel de administración de las metas requeridas para las iniciativas.
*   **Características:**
    *   Tabla de necesidades mostrando la cantidad requerida (`amount`), la unidad (monedas, árboles, raciones), tipo de necesidad y el total acumulado donado.
    *   Formulario modal para añadir una meta cuantitativa asociada a una iniciativa activa.

---
> [!NOTE]
> **CAPTURA DE PANTALLA #9: Panel de Administración de Metas (Needs)**
> *   **Nombre:** `front_09_needs.png`
> *   **Ubicación:** Debajo de la descripción de la sección 6.
> *   **Contenido visual:** Tabla de metas operativas con indicadores visuales de completado y porcentajes de cumplimiento de aportes recibidos.
---

---

## 7. Perfil y Configuración Corporativa (Profile)

*   **Ruta:** `/admin/profile` y `/admin/edit-profile`
*   **Componente Principal:** `Profile.tsx`
*   **Descripción:** Sección destinada a administrar los datos demográficos y la identidad de la organización cliente.
*   **Acciones:**
    *   Configurar la foto de perfil corporativa (subida directa).
    *   Modificar la descripción, correo de contacto, teléfono y dirección física.
    *   Gestionar y actualizar los enlaces de redes sociales (Facebook, X, Instagram, TikTok).

---
> [!NOTE]
> **CAPTURA DE PANTALLA #10: Perfil y Redes Sociales**
> *   **Nombre:** `front_10_profile.png`
> *   **Ubicación:** Debajo de la descripción de la sección 7.
> *   **Contenido visual:** Interfaz de edición del perfil de empresa con vista previa de imagen y campos para enlazar perfiles de redes sociales.
---

---

## 8. Vistas Públicas (Landing & Ranking)

Páginas expuestas que no requieren inicio de sesión y sirven para transparentar los datos a agentes externos.

*   **Rutas:** `/landing` (Home público), `/landing/ranking` (Ranking global), `/clients/:slug` (Perfil público del cliente)
*   **Componentes Principales:** `Landing.tsx`, `PublicProfile.tsx`
*   **Descripción:**
    *   **Ranking Global:** Tabla pública de clasificación de las organizaciones ordenadas por su puntaje ESG global.
    *   **Ficha Pública:** Muestra al público general la información de contacto, redes sociales, iniciativas activas e historial de la empresa usando el color corporativo que detectó automáticamente el motor durante su evaluación.

---
> [!NOTE]
> **CAPTURA DE PANTALLA #11: Ranking de Organizaciones Público**
> *   **Nombre:** `front_11_ranking.png`
> *   **Ubicación:** Debajo de la descripción de la sección 8.
> *   **Contenido visual:** Vista del ranking de empresas con puntuaciones destacadas de ImpactIndex e insignias de desempeño.
---
