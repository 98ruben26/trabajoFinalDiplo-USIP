# trabajoFinalDiplo-USIP
    Diplomado USIP-TrabajoFinal
        Autor Ruben Ariel Acosta Aguilar
        Universidad: USIP
        Módulo: 3 - Arquitectura de Software
        Fecha: Febrero 2026

📑 System Design & API Management: Contract Manager Pro

Este repositorio contiene la documentación técnica, diseño de arquitectura y especificaciones de API para el nuevo sistema de administración de contratos. 
El objetivo es centralizar la "fuente de verdad" del proyecto, asegurando trazabilidad y estándares de calidad profesional.

🎯 Objetivos de la Documentación

    Estandarización: Definir contratos de API claros bajo el estándar OpenAPI (Swagger).
    Transparencia: Registrar cada fase del diseño (Diagramas, Entidad-Relación).
    Trazabilidad: Mantener un histórico de decisiones técnicas (ADR).

🗂️ Estructura del Repositorio
La documentación se organiza de la siguiente manera para facilitar la auditoría del avance:
Plaintext

    ├── 📂 docs
    │   ├── 📂 architecture       # Diagramas C4, Flujos de Secuencia y ERD.
    │   ├── 📂 api-contracts      # Archivos YAML/JSON (OpenAPI Spec).
    │   ├── 📂 adr                # Architectural Decision Records (Decisiones clave).
    │   └── 📂 business-rules     # Lógica de negocio y validación de contratos.
    ├── 📂 resources              # Assets, imágenes y prototipos de UI.
    └── README.md                 # Guía principal (este archivo).

🛠️ Especificaciones de la API (Contratos)

El diseño de la comunicación entre servicios se basa en el principio API-First.
Recurso	Método	Endpoint	Descripción	Estado
Auth	POST	/api/v1/auth/login	Autenticación de usuarios.	✅ Finalizado
Contratos	GET	/api/v1/contracts	Listado de contratos activos.	⏳ En desarrollo
Contratos	POST	/api/v1/contracts	Creación de nuevo contrato.	🛠️ Pendiente

Ver: Diseño de Endpoints REST API.md

📐 Diseño del Sistema
Para garantizar la escalabilidad, se han definido los siguientes artefactos:
   
    Definición del esquema GraphQL con:
         Modelo de Datos: Diseño relacional optimizado para la integridad de los contratos legales.
            Types y relaciones
            Queries disponibles
            Mutations para operaciones de escritura
            Ejemplos de uso
            Ventajas sobre REST
Ver: GRAPHQL-SCHEMA.md

    

    Diagrama y descripción de:
        Arquitectura: Basada en capas (Controller, Service, Repository). 
            Flujo de datos
            Servicios externos
            Escalabilidad y seguridad

Ver: ARCHITECTURE.md

    
    Seguridad: Implementación de JWT para la protección de los endpoints.    

🛠️ Tecnologías Propuestas
Backend

    Node.js + Express (REST API)
    Node.js + Apollo Server (GraphQL)
    Socket.io (WebSocket para tiempo real)

Bases de Datos

    PostgreSQL (Base de datos principal)
    Redis (Cache y sesiones)
    MongoDB (Logs y analytics)

Servicios Externos

    Firebase (Analytics y notificaciones push)
    Sentry (Monitoreo de errores)
    Stripe (Procesamiento de pagos)
    AWS S3 (Almacenamiento de imágenes)

📊 Comparativa REST vs GraphQL
REST

✅ Estándar ampliamente adoptado ✅ Simple de entender y usar ✅ Excelente para caching HTTP ❌ Over-fetching de datos ❌ Múltiples peticiones para datos relacionados
GraphQL

✅ Sin over-fetching ni under-fetching ✅ Una sola petición para datos relacionados ✅ Fuertemente tipado ✅ Auto-documentación ❌ Curva de aprendizaje más alta ❌ Más complejo de cachear
📝 Gestión del Proyecto

Este proyecto se gestionó utilizando las mejores prácticas de Git & GitHub:

    Issues: Cada funcionalidad se planificó como un Issue
    Branches: Cada Issue se desarrolló en una rama independiente (feat/*)
    Pull Requests: Todas las integraciones pasaron por PR con descripción detallada
    Commits: Mensajes claros y descriptivos siguiendo convenciones

🤝 Contribución

Este es un proyecto académico desarrollado como parte del curso de Arquitectura de Software.
📄 Licencia
Este proyecto fue creado con fines educativos.



