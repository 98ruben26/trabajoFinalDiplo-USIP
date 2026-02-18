# Arquitectura del sistema 


1. Diagrama de Arquitectura de Alto Nivel

El siguiente esquema muestra cómo interactúan los componentes de microservicios, la gestión de APIs y los motores de eventos.

      graph TD
          subgraph Client_Layer [Capa de Cliente]
              A[Web App - React/Next.js]
              B[Mobile App]
          end

          subgraph API_Management [API Management Layer]
              C[API Gateway / Kong or Apigee]
              D[Auth Service - OAuth2/JWT]
              E[Rate Limiting & Logging]
          end

          subgraph Business_Logic [Microservicios Core]
              F[Contract Service]
              G[Workflow/Approval Engine]
              H[Notification Service]
              I[Document Generator - PDF]
          end

          subgraph Data_Storage [Capa de Datos]
              J[(PostgreSQL - Metadata)]
              K[(Redis - Cache)]
              L[S3 - Almacenamiento de Contratos]
          end

       %% Conexiones
       A & B --> C
       C --> D
       C --> E
       E --> F & G
       F --> J
       G --> H
       F --> I
       I --> L
       F -.-> K

Desglose de Componentes

Para que Contract Manager Pro sea robusto, la infraestructura se divide en estas áreas clave:

            Componente	                        Función Principal
      API Gateway	                     Punto de entrada único. Maneja el versionamiento de la API, seguridad y ruteo.
      Contract Service	               El "corazón". Gestiona el ciclo de vida del contrato (borrador, activo, vencido).
      Workflow Engine	               Maneja la lógica de aprobación (quién firma después de quién).
      Document Vault (S3)	         Almacenamiento seguro para los archivos físicos (.pdf, .docx) con cifrado en reposo.
      Event Bus	                     (Opcional) Para que, cuando un contrato se firme, se disparen acciones automáticamente en otros sistemas.

El Flujo del Contrato (Diagrama de Secuencia)

Así interactúan los componentes cuando un usuario sube un contrato:

          Request: El cliente envía el contrato al API Gateway.

          Validation: El API Management verifica la suscripción y permisos del usuario.

          Processing: El Contract Service guarda la metadata en SQL y el archivo en S3.

          Notification: Se dispara un evento al Notification Service para avisar a los firmantes.

2. Desglose de Capas del Sistema
   
A. Capa de API Management (Control Plane)

    Esta es la "aduana" del sistema. Utiliza herramientas como Kong, Apigee o AWS API Gateway.

        Rate Limiting: Controla que un cliente no sature el servicio (ej. 100 req/min).

        Authentication (OAuth2/JWT): Valida la identidad del usuario antes de tocar los servicios.

        Transformation: Convierte protocolos (ej. de XML de sistemas legacy a JSON moderno).

B. Capa de Microservicios (Business Logic)

    Los servicios están desacoplados para escalar de forma independiente:

        Contract Service: El núcleo. Maneja el CRUD y el versionado de documentos.

        Template Engine: Genera PDFs dinámicos usando Handlebars o React-PDF a partir de JSON.

        Signature Provider Adapter: Un "Wrapper" que comunica con proveedores externos (DocuSign/SignEasy) mediante un patrón Strategy.

        Audit Service: Registra cada cambio en un libro de contabilidad (Ledger) inmutable para fines legales.

C. Capa de Persistencia y Cache

        PostgreSQL: Para datos relacionales (Metadatos del contrato, usuarios, permisos).

        Redis: Cache de sesiones y control de idempotencia para evitar duplicados en las firmas.

        S3 / Azure Blob Storage: Almacenamiento de los archivos binarios (PDFs finales) con encriptación AES-256.

3. Flujo de Datos: Creación y Firma

    Para entender cómo viaja la información, podemos observar el diagrama de secuencia del proceso:
    Flujo de Trabajo (Workflow):

        Request: El cliente envía un POST /contracts.

        Validation: El Gateway valida el token y el servicio de contratos verifica la integridad de la plantilla.

        Storage: Se guarda el borrador en la DB y el archivo temporal en el Storage.

        Event: Se dispara un evento a la cola de mensajería (RabbitMQ/Kafka) para notificar al firmante.

        Callback: El proveedor de firma envía un Webhook al sistema cuando el usuario firma, activando la actualización del estado a SIGNED.

4. Stack Tecnológico Recomendado
   
       Componente	Tecnología Sugerida
   
       API Docs	Swagger / OpenAPI 3.0
   
       Backend	Node.js (NestJS) o Go (por su alta concurrencia)
   
       Mensajería	Apache Kafka (para trazas de auditoría masivas)
   
       Infraestructura	Kubernetes (K8s) para orquestación de contenedores
   
