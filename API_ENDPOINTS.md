# Documentación de Endpoints de la API (IaaS)

Esta es la guía de integración técnica para el backend de Impact as a Service (IaaS). Este documento detalla el contrato exacto para cada uno de los endpoints disponibles en el sistema, organizados por dominio funcional, e incluye las especificaciones introducidas durante las fases recientes de migración.

## Aspectos Globales de Autenticación
Salvo excepciones específicas (como webhooks externos), todos los endpoints requieren autenticación mediante un Autorizador de API Gateway (`IaaSAuthorizer`).
- **Cabecera Requerida:** `x-api-key: <TU_API_KEY>`
- **Base URL:** Todas las rutas expuestas en este documento se asumen bajo un endpoint base, representado como `<BASE_URL>`.

---

## Índice General
1. Autenticación (Original Auth)
2. Gestión de Clientes
3. Manejo de Iniciativas
4. Impactos y Metas
5. Métricas y Analítica (Fases 1 y 2)
6. Generación de Reportes (Fase 2)
7. Visualización Pública y Búsqueda (Fase 3)
8. Archivos, Notificaciones y Misceláneos

---

## 1. Autenticación (Original Auth)
Nueva arquitectura de inicio de sesión que reemplaza a AWS Cognito, delegando la identidad al proveedor Original Auth.

### 1.1 Iniciar Sesión de Autenticación
- **Ruta:** `GET /auth/generate_session`
- **Descripción:** Inicia el flujo de autenticación "Single Sign-On".
- **Comportamiento Esperado:** Genera un estado local temporal (`session_id` con estado `PENDING`) que el frontend debe usar para redirigir al proveedor de Original Auth.
- **Ejemplo cURL:**
  ```bash
  curl --location --request GET '<BASE_URL>/auth/generate_session'
  ```

### 1.2 Webhook de Validación (Callback)
- **Ruta:** `POST /auth/callback`
- **Descripción:** Endpoint interno consumido exclusivamente por los servidores de Original Auth de forma asíncrona cuando un usuario completa el login.
- **Comportamiento Esperado:** Valida la firma del proveedor usando una clave secreta interna (`secret_key`), marca la sesión como `VALIDATED` en la base de datos, y provee temporalmente (en el contexto de ejecución) los datos del usuario. De no existir cuenta bajo este correo en la tabla local de Clientes, procede a provisionar una cuenta nueva automáticamente.
- **Cuerpo de la Petición (Ejemplo):**
  ```json
  {
      "session_id": "uuid-sesion",
      "user_data": {
          "email": "usuario@ejemplo.com",
          "name": "Nombre Usuario"
      }
  }
  ```

### 1.3 Validación Final Frontend
- **Ruta:** `POST /auth/validate_session`
- **Descripción:** Confirma del lado del cliente que la sesión ha sido validada exitosamente por el proveedor.
- **Comportamiento Esperado:** Otorga la respuesta definitiva requerida por el frontend y emite una cookie HTTP-only segura conteniendo el token JWT para uso subsecuente.
- **Cuerpo de la Petición:**
  ```json
  {
      "session_id": "uuid-sesion"
  }
  ```
- **Ejemplo cURL:**
  ```bash
  curl --location --request POST '<BASE_URL>/auth/validate_session' \
  --header 'Content-Type: application/json' \
  --data-raw '{ "session_id": "uuid-sesion" }'
  ```

---

## 2. Gestión de Clientes
Endpoints destinados a la consulta y actualización de perfiles de organizaciones empresariales o donadores.

### 2.1 Obtener todos los Clientes
- **Ruta:** `GET /client`
- **Descripción:** Listado completo de clientes registrados en el sistema.
- **Ejemplo cURL:**
  ```bash
  curl --location --request GET '<BASE_URL>/client' \
  --header 'x-api-key: <TU_API_KEY>'
  ```

### 2.2 Consultar Perfil Privado
- **Ruta:** `GET /client/{client_id}`
- **Descripción:** Obtiene los datos demográficos y configuraciones internas del cliente proporcionado.
- **Ejemplo cURL:**
  ```bash
  curl --location '<BASE_URL>/client/{client_id}' \
  --header 'x-api-key: <TU_API_KEY>'
  ```

### 2.3 Perfil Público mediante Subruta (Slug)
- **Ruta:** `GET /clients/{slug}`
- **Descripción:** Realiza una búsqueda mediante la URL amigable (slug) del cliente para el renderizado público web (por ejemplo, misitio.com/empresas/mi-empresa).
- **Ejemplo cURL:**
  ```bash
  curl --location '<BASE_URL>/clients/mi-empresa' \
  --header 'x-api-key: <TU_API_KEY>'
  ```

### 2.4 Modificación del Perfil de Cliente
- **Rutas:** `PATCH /client` (Perfil General) | `PATCH /updateClientSM` (Redes Sociales Exclusivas)
- **Descripción:** Actualiza de modo parcial o total la información o enlaces sociales del cliente autenticado.
- **Cuerpo de la Petición (`PATCH /client`):**
  ```json
  {
      "client_id": "id-del-cliente",
      "name": "Títulos y Nombres",
      "description": "Explicación biográfica o corporativa"
  }
  ```
- **Ejemplo cURL:**
  ```bash
  curl --location --request PATCH '<BASE_URL>/client' \
  --header 'x-api-key: <TU_API_KEY>' \
  --header 'Content-Type: application/json' \
  --data-raw '{
      "client_id": "123",
      "name": "Corporación XYZ"
  }'
  ```

---

## 3. Manejo de Iniciativas
Endpoints encargados del registro y modificación de las campañas y proyectos sociales expuestos a los agentes inversores.

### 3.1 Obtener y Filtrar Iniciativas Activas
- **Ruta:** `GET /initiatives`
- **Descripción:** Devuelve un arreglo paginado de las iniciativas globales con filtros ejecutados en backend.
- **Parámetros GET (Query String):**
  - `search` (string): Búsqueda textual por coincidencia (case-insensitive) en el nombre de la iniciativa.
  - `category` (string): Busca exactitud literal por categoría de rubro.
  - `dateFrom`, `dateTo` (ISO Date): Rango temporal para filtrar `creation_date`.
  - `status` (string, separador por comas): Opciones soportadas son `completed`, `active`, `inProgress`, `recentAdded`, `topInitiatives`.
  - `page`, `limit` (número): Control de paginación de los registros.
- **Ejemplo cURL:**
  ```bash
  curl --location '<BASE_URL>/initiatives?status=active,completed&limit=20&page=1' \
  --header 'x-api-key: <TU_API_KEY>'
  ```

### 3.2 Listar Iniciativas Propias de un Cliente
- **Ruta:** `GET /initiativesofclient/{client_id}`
- **Descripción:** Implementa la misma lógica de los filtros de listado descritos anteriormente (3.1), pero limitando su resultado mediante una clave de búsqueda restrictiva (`client_id`).
- **Ejemplo cURL:**
  ```bash
  curl --location '<BASE_URL>/initiativesofclient/{client_id}?status=active' \
  --header 'x-api-key: <TU_API_KEY>'
  ```

### 3.3 Consultas Transaccionales Directas
Estas operaciones realizan búsquedas, mutaciones o borrados atómicos de entidades Iniciativa.
- **Leer Iniciativa Individual:** `GET /initiatives/{initiative_id}`
- **Modificar Metadatos:** `PATCH /initiatives` (El documento JSON de la solicitud requiere de `initiative_id` mandatoriamente en su primer nivel).
- **Borrado Lógico y Físico:** `DELETE /initiatives` (El documento de la solicitud requiere `initiative_id`).
- **Listar el Árbol de Categorías:** `GET /initiatives/getInitiativeCategoriesOfClient/{client_id}` (Devuelve solo las categorías exactas registradas exitosamente para este cliente, ordenadas de modo alfabético).

---

## 4. Impactos y Metas
Funcionalidad dedicada a medir métricas de consecución de aportes (Impacts) comparado contra los objetivos pre-solicitados (Needs).

### 4.1 Crear un Objetivo (Need/Goal)
- **Ruta:** `POST /initiatives/{initiative_id}/needs`
- **Descripción:** Añade un nuevo nivel de meta (sea monetaria, material, o técnica) vinculándolo a la campaña deseada.
- **Cuerpo de la Petición:**
  ```json
  {
      "type": "Economic",
      "amount": 50000,
      "description": "Meta mínima operativa"
  }
  ```
- **Ejemplo cURL:**
  ```bash
  curl --location --request POST '<BASE_URL>/initiatives/{initiative_id}/needs' \
  --header 'x-api-key: <TU_API_KEY>' \
  --header 'Content-Type: application/json' \
  --data-raw '{ "type": "Economic", "amount": 50000, "description": "Detalles" }'
  ```

### 4.2 Registrar una Contribución (Impact)
- **Ruta:** `POST /initiatives/{initiative_id}/impacts`
- **Descripción:** Formaliza el registro del impacto del lado de un usuario.
- **Cuerpo de la Petición:**
  ```json
  {
      "amount": "500",
      "type": "Social",
      "contributor_id": "id-del-contribuyente"
  }
  ```

### 4.3 Consultas Transaccionales Tradicionales
- **GET /impacts/{client_id}:** Lista un extracto crudo de todas las transacciones de un donador.
- **POST /impacts/totalgiven:** Retorna la métrica absoluta total de cuánto ha entregado en general un contribuyente al día de hoy.
- **POST /impacts/totalreceived:** Métrica global absoluta correspondiente a la captación neta de la ONG al día de hoy.
- **GET /needs/{need_id}:** Recuperación independiente del estado individual de cumplimiento de una meta.

---

## 5. Métricas y Analítica (Fases 1 y 2)
Endpoints de procesamiento avanzado desarrollados para anular y evitar el efecto de redundancia múltiple generada por las llamadas repetidas desde el navegador y reducir consultas secundarias sobre DynamoDB (N+1 queries).

### 5.1 Estado General e Inventario (Dashboard Empresarial)
- **Ruta:** `GET /clients/{client_id}/metrics/general`
- **Respuesta Esperada:**
  ```json
  {
      "totalInitiatives": 25,
      "activeInitiatives": 10,
      "completedInitiatives": 8,
      "pendingInitiatives": 7
  }
  ```
- **Ejemplo cURL:**
  ```bash
  curl --location '<BASE_URL>/clients/{client_id}/metrics/general' \
  --header 'x-api-key: <TU_API_KEY>'
  ```

### 5.2 Agregación de Series de Tiempo (Gráficos)
- **Ruta:** `GET /initiatives/{initiative_id}/impacts/aggregated`
- **Parámetros Requeridos:** `period` (Valores permitidos `weekly`, `monthly`, `yearly`). Variables de año (`year`) son opcionales y usarán el presente en caso de omitirse.
- **Respuesta Esperada:** Listas paralelas con la abscisa (labels) y ordenada (series). Ideal para inyección directa a componentes React Recharts o ApexCharts.

### 5.3 Proporciones de Distribución
- **Distribución de Categorías:** `GET /initiatives/{initiative_id}/impacts/by-type` (Entregando un objeto estandarizado y el tipo exacto agrupado como `Social` o `Economic`).
- **Estado de Objetivos Consolidado:** `GET /initiatives/{initiative_id}/goals/progress` (Computa la operación interna: `impacto_acumulado / monto_objetivo * 100`).

### 5.4 Consolidado de Métrica de Iniciativa Completa
- **Ruta:** `GET /initiatives/{initiative_id}/metrics/complete`
- **Descripción:** Resuelve los datos calculados e inyecta algoritmos para determinar la 'salud' del proyecto: progreso de fechas restantes (`daysRemaining`), diferencial de velocidad contra el inicio (`progressStatus`), así como su puntuación técnica (`efficiencyRating`). Múltiples requerimientos paralelos resueltos por una instancia única de Lambda.

### 5.5 Analíticas Auxiliares
- **Actividad de Impacto:** `GET /initiatives/{initiative_id}/activity?period=week` (Transacciones recientes de los últimos siete o treinta días).
- **Métricas Retrospectivas del Cliente:** `GET /clients/{client_id}/impacts/stats?days=60`
- **Tabla de Contribuyentes Clasificados:** `GET /initiatives/{initiative_id}/contributors/detailed?limit=5` (Entrega las identidades asignadas a niveles jerárquicos monetario o por recurrencia).

---

## 6. Generación de Reportes (Fase 2)
Endpoints asíncronos orientados a auditar documentos, reportes PDF e imprimir memorias sobre campañas de modo sistemático.

### 6.1 Estadísticas Concatenadas (Múltiples Campañas)
- **Ruta:** `POST /reports/stats`
- **Cuerpo de la Petición:**
  ```json
  {
      "initiative_ids": ["init1", "init2", "init3"]
  }
  ```
- **Respuesta Esperada:** Devolverá un bloque de total consolidado de resumen (summary) y métricas segmentadas del tipo (types), para cada iniciativa solicitada.

### 6.2 Desencadenar la Impresión a Archivo S3 (Reporte Formal)
- **Ruta:** `POST /generateInitiativeReport`
- **Descripción:** Lanza la función asíncrona dedicada que compila las métricas del proyecto y las exporta a un documento PDF.
- **Cuerpo de la Petición:**
  ```json
  {
      "initiative_id": "id-iniciativa",
      "client_id": "id-del-cliente"
  }
  ```
- **Ejemplo cURL:**
  ```bash
  curl --location --request POST '<BASE_URL>/generateInitiativeReport' \
  --header 'x-api-key: <TU_API_KEY>' \
  --header 'Content-Type: application/json' \
  --data-raw '{ ... }'
  ```

---

## 7. Visualización Pública y Búsqueda (Fase 3)
Funciones estandarizadas para el ambiente web público sin credenciales explícitas preexistentes pero optimizadas al milisegundo de respuesta.

### 7.1 Panel Transparente Público de Relación de Proyectos
- **Ruta:** `GET /users/{user_id}/initiatives/participated?limit=10`
- **Descripción:** Lista y analiza automáticamente los historiales relacionales del usuario con la causa respectiva; devolviendo el historial consolidado con indicadores de desempeño alfabéticos (trend: `"up"` | `"down"`).

### 7.2 Uso Consolidado de Panel de Donante Público
- **Ruta:** `GET /users/{user_id}/public-profile/stats`
- **Descripción:** Retorna el uso del último mes y el histórico del año por semanas aplicables para un donador expuesto, de modo completamente empaquetado y resuelto.

### 7.3 Búsqueda Global Agrupada (Unified Search)
- **Ruta:** `GET /search`
- **Parámetros Requeridos:** `q=` (Texto)
- **Parámetros Opcionales:** `type=` (`all` por defecto, pudiendo restringirse explícitamente a `clients` o a `initiatives`). El límite general está delimitado a diez elementos por cada respuesta particular.
- **Respuesta Esperada:**
  ```json
  {
      "clients": [ { "id": "...", "name": "..." } ],
      "initiatives": [ { "id": "...", "name": "..." } ],
      "totalResults": 2
  }
  ```

---

## 8. Archivos, Notificaciones y Misceláneos

### 8.1 Autenticación Predictiva de Imágenes en AWS S3
En cumplimiento de buenas prácticas, se implementa URL firma (Presigned URLs) minimizando tiempos de red para blobs y cargas en sistema.
- **Solicitar el Punto Restringido Temporal de Subida:** `POST /presignedGalleryUrls`
  - **Body:** `{"bucket": "galleries", "filenames": ["archivo-1.jpg"]}`
- **Lectura Temporal Asignada (Privado):** `GET /showGalleryImage?key=archivo-1.jpg`

### 8.2 Actualización Paramétrica de Avatares (Imagen)
- **`POST /imageUpload`**: Almacenamiento base perfil.
- **`POST /updateProfileImage`** y su réplica **`POST /client/{client_id}/updateProfileImageUrl`**: Para confirmación asíncrona post carga de bucket.
- **`POST /updateInitiativeImageUrl`**: Referencia atómica de avatar primario de iniciativa.

### 8.3 Correos Transaccionales Amazon SES y Comentarios
- **Foro en Plataforma (Comentarios):**
  - **Lectura de Foro:** `GET /comments?initiative_id=123`
  - **Envío Formal de Comentario al Servidor:** `POST /comments`
- **Servicio Interno Eventos de Correo SES:**
  - **Hito Social Logrado:** `POST /notify-update`
  - **Ticket Administrativo del Cliente hacia el SaaS:** `POST /notify-support`
