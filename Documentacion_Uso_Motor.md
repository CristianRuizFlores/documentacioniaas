# Guía de Uso del Motor de Evaluación: API y Backend (motor_IaaS)

Esta guía documenta el uso, configuración, comandos de consola y consumo de APIs del **Motor de Evaluación de Impacto ESG (motor_IaaS)** de la plataforma **ImpactoSocial**.

---

## Índice de Secciones
1. [Requisitos y Configuración de Entorno](#1-requisitos-y-configuración-de-entorno)
2. [Comandos de Consola y Scripts de Mantenimiento](#2-comandos-de-consola-y-scripts-de-mantenimiento)
3. [El Pipeline del Motor RAG y Configuración de IA](#3-el-pipeline-del-motor-rag-y-configuración-de-ia)
4. [Referencia para el Consumo de la API REST](#4-referencia-para-el-consumo-de-la-api-rest)

---

## 1. Requisitos y Configuración de Entorno

El motor requiere Node.js v20+, una base de datos PostgreSQL con soporte para la extensión `pgvector`, y una clave API de OpenAI.

### Variables de Entorno (Archivo `.env`):
Antes de iniciar el servidor, configure el archivo `.env` en la raíz del proyecto con las siguientes claves:

*   `DATABASE_URL`: Cadena de conexión de PostgreSQL (ej. `postgresql://usuario:pass@host:5432/dbname`).
*   `OPENAI_API_KEY`: Clave de acceso a la API de OpenAI (utilizada para embeddings y evaluaciones GPT-4o).
*   `PORT`: Puerto en el que el servidor de Express escuchará las peticiones (por defecto `3000`).
*   `SKIP_MIGRATIONS`: Configurar en `true` si desea omitir la ejecución automática de migraciones de base de datos durante el inicio del servidor.
*   `MEDIAAS_API_KEY` y `MEDIAAS_SECRET_KEY`: Credenciales necesarias para firmar las peticiones y subir reportes al repositorio AWS S3 mediante el servicio externo Media-as-a-Service.

---

## 2. Comandos de Consola y Scripts de Mantenimiento

El motor incluye utilidades de consola que facilitan el desarrollo, depuración y mantenimiento de los datos.

### A. Ejecución del Servidor
*   **Modo Desarrollo (con recarga automática):**
    ```bash
    npm run dev
    ```
*   **Compilación de TypeScript a JavaScript:**
    ```bash
    npm run build
    ```
*   **Modo Producción (ejecuta el código compilado):**
    ```bash
    npm start
    ```

### B. Purga de la Caché Inmutable
El motor guarda copias locales en formato JSON de los análisis de reportes completados en el directorio `pdf/cache/`. Si desea forzar un análisis completo de los documentos la próxima vez que sean subidos:
```bash
npm run clean:cache
```
*   **Efecto:** Borra todos los archivos JSON del directorio de caché local, obligando a realizar búsquedas semánticas y consultas RAG desde cero.

### C. Recálculo de Índices (Consola de Recálculo)
Si realiza modificaciones en los pesos porcentuales del algoritmo matemático de calificación de la clase `ImpactCalculator` y desea actualizar todas las evaluaciones previamente guardadas en la base de datos sin gastar saldo de OpenAI:
```bash
npx ts-node src/scripts/recalculate_alternative_index.ts
```
*   **Efecto:** Lee todos los registros de la tabla `reports` en PostgreSQL, vuelve a calcular el sub-índice `AlternativeIndex` basándose en los datos analíticos previamente guardados y actualiza cada fila.

### D. Restablecimiento de la Base de Datos
Para limpiar por completo los datos de auditoría de su entorno de desarrollo:
```bash
npx ts-node clear_db.ts
```
*   **Efecto:** Vacía por completo las tablas `reports`, `chunks` y `documents`, dejando la base de datos lista para pruebas desde cero.

---

## 3. El Pipeline del Motor RAG y Configuración de IA

El motor combina el procesamiento de lenguaje natural y el álgebra vectorial para calificar los reportes.

### A. Segmentación y Embeddings (pgvector):
*   **Chunking:** El texto extraído de los archivos subidos es fragmentado mediante la clase `RecursiveCharacterTextSplitter` de LangChain. Se utiliza un tamaño de fragmento (`chunkSize`) de **2500 caracteres** y un solapamiento (`chunkOverlap`) de **300 caracteres**.
*   **Vectores:** Cada fragmento se convierte en un embedding utilizando el modelo `text-embedding-3-small` de OpenAI (vectores de 1536 dimensiones).
*   **Búsqueda Coseno:** El backend realiza búsquedas semánticas para cada dimensión ESG de la plataforma utilizando el operador de similitud coseno de pgvector (`<=>` en SQL). Recupera un límite de **8 fragmentos** ordenados por coincidencia semántica.
*   **SoBERANÍA DE DATOS (Privacidad):** Inmediatamente después de que se completa la evaluación de un reporte y se guardan sus métricas finales en la tabla `reports`, el backend ejecuta la función `deleteDocumentVectors`. Esta función **borra físicamente** los vectores y fragmentos de texto del reporte de las tablas `chunks` y `documents` en Postgres, garantizando que el texto privado de la organización no permanezca almacenado.

### B. Reglas del Evaluador LLM (GPT-4o):
Las evaluaciones son realizadas por `gpt-4o` utilizando el formato estructurado de OpenAI (*Structured Outputs*). El motor inyecta las siguientes directrices al modelo de lenguaje:
*   **Regla Antifraude:** La IA no debe realizar los cálculos aritméticos finales ("La IA evalúa, el Backend calcula"). Su tarea es emitir calificaciones cualitativas conforme a catálogos fijos de la plataforma.
*   **Formato Rígido de Razonamiento:** El campo `reasoning` devuelto por el modelo debe tener obligatoriamente la siguiente estructura textual:
    `**Análisis:** [análisis de lo encontrado] **Evidencia:** [cifras o hechos del texto] **Conclusión:** [veredicto de cumplimiento]`
*   **Cero Alucinación:** Si el fragmento RAG no contiene suficiente evidencia para respaldar una dimensión, la IA debe calificarla con materialidad "bajo" en lugar de adivinar o asumir datos.

---

## 4. Referencia para el Consumo de la API REST

Los endpoints del motor están expuestos bajo el prefijo `/api` y requieren autenticación mediante llave API en la cabecera en entornos protegidos.

### A. Subir y Procesar Reportes
*   **Método:** `POST`
*   **Ruta:** `/api/process-report`
*   **Content-Type:** `multipart/form-data`
*   **Parámetros:**
    *   `documents` (File, múltiples): Array de archivos a procesar (PDF, DOCX, TXT).
    *   `organizationName` (String, opcional): Nombre manual de la organización.
*   **Respuesta Exitosa (200 OK):**
    ```json
    {
      "success": true,
      "data": {
        "companyName": "Nombre Empresa",
        "reportYear": "2023",
        "slug": "nombre-empresa",
        "primaryColor": "#234a36",
        "finalScores": {
          "impactIndex": 82.5,
          "alternativeIndex": 79.4,
          "analytics": {
            "impactPerformanceScore_IPS": 85.0,
            "impactManagementScore_IMS": 80.0,
            "standardsCoverageScore_SCS": 75.0,
            "evidenceQualityScore_EQS": 100.0,
            "netImpactScore_NIS": 78.5
          }
        },
        "executiveSummary": "Resumen narrativo..."
      },
      "slug": "nombre-empresa"
    }
    ```

### B. Listar Todos los Reportes Evaluados
*   **Método:** `GET`
*   **Ruta:** `/api/reports`
*   **Respuesta Exitosa (200 OK):**
    ```json
    {
      "success": true,
      "data": [
        {
          "id": 1,
          "slug": "empresa-ejemplo",
          "organizationName": "Empresa Ejemplo",
          "createdAt": "2026-06-24T14:14:38.000Z",
          "primaryColor": "#234a36",
          "companyDomain": "empresa.com",
          "impactIndex": "82.5",
          "reportYear": "2023"
        }
      ]
    }
    ```

### C. Obtener Detalle de una Evaluación
*   **Método:** `GET`
*   **Ruta:** `/api/report/:slug`
*   **Parámetros Query:** `?year=2023` (Opcional, filtra por año del reporte)
*   **Respuesta Exitosa (200 OK):** Devuelve el objeto JSON completo con las evaluaciones desglosadas por indicador, razonamientos de la IA, citas textuales y referencias de archivos en MediaAAS.

### D. Obtener Tabla de Posiciones y Ranking
*   **Método:** `GET`
*   **Ruta:** `/api/ranking`
*   **Respuesta Exitosa (200 OK):** Devuelve una lista optimizada de organizaciones con sus puntuaciones generales e información de marca (slug, dominio, color representativo, ImpactIndex e index alternativo) diseñada para renderizarse rápidamente en el ranking público.

### E. Eliminar una Evaluación
*   **Método:** `DELETE`
*   **Ruta:** `/api/report/:slug`
*   **Parámetros Query:** `?year=2023` (Opcional)
*   **Comportamiento:** Borra el registro de la tabla `reports` en Postgres y realiza una llamada en segundo plano a la API de MediaAAS para eliminar físicamente los archivos cargados del contenedor de S3.
