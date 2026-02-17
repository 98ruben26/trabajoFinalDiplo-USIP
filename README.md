# trabajoFinalDiplo-USIP
Diplomado USIP-TrabajoFinal

Arquitectura de Software - API Design
📋 Descripción del Proyecto

Este repositorio contiene la documentación técnica del diseño de APIs REST y GraphQL para arquitectura y especificaciones de API para el nuevo sistema de administración de personal y pacientes de un hospital , desarrollado como proyecto final del Módulo 3 de Arquitectura de Software.

🎯 Objetivos
    Gestionar el ciclo de vida del proyecto utilizando Git & GitHub
    Documentar endpoints REST de manera profesional
    Diseñar esquemas GraphQL completos
    Crear diagramas de arquitectura del sistema
    Comparar eficiencia entre REST y GraphQL
 
📂 Estructura del Repositorio

├── README.md                 # Este archivo
├── REST-API.md              # Documentación de endpoints REST
├── GRAPHQL-SCHEMA.md        # Esquema y queries de GraphQL
└── ARCHITECTURE.md          # Diagrama de arquitectura del sistema

🚀 Contenido
1. API REST

Documentación completa de los endpoints REST incluyendo:

    Base URL y autenticación
    Endpoints de Productos (GET, POST, PUT, DELETE)
    Endpoints de Carrito de Compras
    Endpoints de Órdenes
    Códigos de respuesta HTTP

Ver: REST-API.md
2. GraphQL Schema

Definición del esquema GraphQL con:

    Types y relaciones
    Queries disponibles
    Mutations para operaciones de escritura
    Ejemplos de uso
    Ventajas sobre REST

Ver: GRAPHQL-SCHEMA.md
3. Arquitectura del Sistema

Diagrama y descripción de:

    Componentes principales (Frontend, Backend, Base de datos)
    Flujo de datos
    Servicios externos
    Escalabilidad y seguridad

Ver: ARCHITECTURE.md
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

Autor
    Ruben Ariel Acosta Aguilar
    Universidad: USIP
    Módulo: 3 - Arquitectura de Software
    Fecha: Febrero 2026


    Total Agradecimiento a quien corresponda por el ejemplo proporcionado

