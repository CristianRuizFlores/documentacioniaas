# Guía de Usuario y Documentación Pública (ImpactoSocial)

Esta guía detalla el funcionamiento de la plataforma **ImpactoSocial** orientado al usuario final, clientes y visitantes públicos. Integra los flujos visuales del Frontend con el procesamiento automático del Motor RAG/ESG.

---

## Índice General de la Guía
1. [Introducción a la Plataforma](#1-introducción-a-la-plataforma)
2. [Carga de Reportes Corporativos (Ingesta)](#2-carga-de-reportes-corporativos-ingesta)
3. [Visualización del Reporte de Evaluación ESG (Dashboard)](#3-visualización-del-reporte-de-evaluación-esg-dashboard)
4. [Iniciativas de Impacto y Metas Cuantitativas](#4-iniciativas-de-impacto-y-metas-cuantitativas)
5. [Ranking y Perfil Público de Organizaciones](#5-ranking-y-perfil-público-de-organizaciones)

---

## 1. Introducción a la Plataforma

**ImpactoSocial** es un ecosistema digital que permite a las organizaciones auditar sus reportes corporativos bajo dimensiones ESG (Social, Ambiental, Gobernanza, etc.), registrar iniciativas de desarrollo social y transparentar las metas e impactos alcanzados ante inversionistas y la comunidad.

*   **El Frontend:** Una SPA moderna construida con principios de diseño premium (glassmorphism) que permite navegar por tableros interactivos, rankings públicos y perfiles empresariales.
*   **El Motor:** Un motor cognitivo (RAG) que analiza reportes no estructurados (PDF/DOCX), genera métricas de impacto y calcula el **ImpactIndex** de manera automatizada.

---

## 2. Carga de Reportes Corporativos (Ingesta)

Las organizaciones pueden auditar su documentación anual cargando archivos directamente a la plataforma.

### Flujo del Usuario:
1. Navega a la sección **Mis Reportes** en el panel lateral.
2. Haz clic en el área de carga (drag & drop) o selecciona hasta 20 archivos simultáneamente.
3. *(Opcional)* Modifica el nombre de la organización si el documento no lo contiene de forma explícita.
4. Presiona **Subir y Analizar**.

---
> [!NOTE]
> **UBICACIÓN DE CAPTURA DE PANTALLA #1:**
> *   **Nombre sugerido:** `captura_carga_reporte.png`
> *   **Qué debe mostrar:** La pantalla de carga en `/admin/reports` del Frontend. Debe verse el área de drag & drop, la barra de progreso de carga y el cuadro de entrada manual para el nombre de la organización.
---

### Qué ocurre en el Motor (Procesamiento Técnico Público):
*   El motor extrae el texto del archivo, identifica el año del reporte y su color corporativo principal de manera automática.
*   Calcula un hash MD5 único del archivo para evitar reprocesamiento y mantener coherencia de los datos.

---

## 3. Visualización del Reporte de Evaluación ESG (Dashboard)

Una vez completado el procesamiento RAG, el motor entrega los resultados al frontend, el cual los visualiza en un panel interactivo premium.

### Flujo del Usuario:
1. El usuario visualiza la puntuación consolidada **ImpactIndex** y **AlternativeIndex**.
2. Explora el gráfico de radar que desglosa el puntaje de las 7 dimensiones clave (Social, Ambiental, Cultural, Performance, Colaboración, Económico, Gobernanza).
3. Lee el **Resumen Ejecutivo en Español** redactado automáticamente por el motor de IA.
4. Descarga la evaluación o visualiza las citas textuales que justifican cada calificación.

---
> [!NOTE]
> **UBICACIÓN DE CAPTURA DE PANTALLA #2:**
> *   **Nombre sugerido:** `captura_dashboard_evaluacion.png`
> *   **Qué debe mostrar:** El dashboard de resultados en `/admin/reports` (o la ficha de reporte del cliente). Debe destacar el puntaje del **ImpactIndex**, el gráfico de radar o de barras con las 7 dimensiones y el párrafo del Resumen Ejecutivo.
---

---
> [!NOTE]
> **UBICACIÓN DE CAPTURA DE PANTALLA #3:**
> *   **Nombre sugerido:** `captura_detalle_indicadores.png`
> *   **Qué debe mostrar:** El listado de métricas evaluadas dentro del reporte, mostrando la justificación textual (**Reasoning**), los estándares asociados (ej. ODS-8, GRI-305) y los bloques de evidencia citados del documento original.
---

---

## 4. Iniciativas de Impacto y Metas Cuantitativas

Los clientes pueden crear iniciativas para recaudar fondos o gestionar proyectos de desarrollo y sostenibilidad.

### Flujo del Usuario:
1. Ve al panel de **Iniciativas** y selecciona **Nueva Iniciativa**.
2. Rellena los datos (Categoría, fechas de inicio y fin, imagen de la campaña, objetivos).
3. Añade una **Meta de Recaudación (Need)**, por ejemplo: *"Adquisición de 100 páneles solares (Ambiental) por un valor de $15,000 USD"*.
4. Observa el progreso a medida que los donantes registran aportaciones (**Impacts**).

---
> [!NOTE]
> **UBICACIÓN DE CAPTURA DE PANTALLA #4:**
> *   **Nombre sugerido:** `captura_iniciativa_detalle.png`
> *   **Qué debe mostrar:** La vista de una iniciativa individual (`/admin/my-initiative/:id`), mostrando la barra de progreso de cumplimiento de metas, la lista de donadores recientes, y el muro de comentarios interactivo.
---

---

## 5. Ranking y Perfil Público de Organizaciones

Para promover la transparencia corporativa, la plataforma provee páginas públicas donde se listan las empresas de acuerdo con su desempeño ESG.

### Flujo del Usuario:
1. Los visitantes acceden a la URL pública del **Ranking Global** (`/landing/ranking`).
2. Ordenan las organizaciones por su puntuación **ImpactIndex** o por categorías específicas.
3. Al hacer clic en una empresa, acceden a su **Perfil Público** (`/clients/{slug}`).
4. El perfil público muestra el color corporativo extraído por el motor, sus iniciativas de impacto activas y el progreso consolidado de sus metas de sostenibilidad.

---
> [!NOTE]
> **UBICACIÓN DE CAPTURA DE PANTALLA #5:**
> *   **Nombre sugerido:** `captura_ranking_global.png`
> *   **Qué debe mostrar:** La pantalla del Ranking de Organizaciones en el Frontend, mostrando la tabla de posiciones con los logos, nombres de empresas y sus respectivos puntajes de impacto.
---

---
> [!NOTE]
> **UBICACIÓN DE CAPTURA DE PANTALLA #6:**
> *   **Nombre sugerido:** `captura_perfil_publico.png`
> *   **Qué debe mostrar:** El perfil público de una empresa (`/clients/{slug}`) con la paleta de colores personalizada, la información demográfica corporativa y las tarjetas de sus iniciativas activas.
---
