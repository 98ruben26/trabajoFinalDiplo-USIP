#GraphQL API - Esquema y Documentación

##Diseño del Esquema GraphQL (SDL)

El esquema se centra en la relación entre contratos, firmantes y plantillas, integrando tipos escalares para manejar fechas y estados.
GraphQL

"""
Representa el estado actual de un contrato en su ciclo de vida.
"""
  enum ContractStatus {
    DRAFT
    IN_REVIEW
    PENDING_SIGNATURE
    SIGNED
    EXPIRED
    VOID
  }

"""
Entidad principal que contiene la información legal y metadatos.
"""
  type Contract {
    id: ID!
    title: String!
    status: ContractStatus!
    content: String
    template: Template
    parties: [Party!]!
    auditLogs: [AuditLog!]!
    createdAt: String!
    updatedAt: String!
    signedAt: String
  }

  type Party {
    id: ID!
    name: String!
    email: String!
    role: String! # Ej: 'SENDER', 'SIGNER', 'VIEWER'
    hasSigned: Boolean!
  }

  type Template {
    id: ID!
    name: String!
    version: String!
    baseContent: String!
  }

  type AuditLog {
    id: ID!
    action: String!
    timestamp: String!
    ipAddress: String
    userId: ID
  }

# Queries: Lectura de datos
  type Query {
    getContract(id: ID!): Contract
    listContracts(status: ContractStatus, limit: Int): [Contract!]!
    me: Party!
  }

# Mutations: Modificación de datos
  type Mutation {
    createContract(input: CreateContractInput!): Contract!
    updateContractStatus(id: ID!, status: ContractStatus!): Contract!
    addPartyToContract(contractId: ID!, partyInput: PartyInput!): Contract!
    sendForSignature(id: ID!): Boolean!
  }

  input CreateContractInput {
    title: String!
    templateId: ID!
    parties: [PartyInput!]!
  }

  input PartyInput {
    name: String!
    email: String!
    role: String!
  }

2. Documentación de Operaciones
   
Query de Ejemplo: Dashboard de Contratos

Un cliente puede solicitar una lista de contratos pendientes de firma, obteniendo solo los nombres de los firmantes y la fecha de creación.
GraphQL

  query GetPendingContracts {
    listContracts(status: PENDING_SIGNATURE, limit: 10) {
      id
      title
      createdAt
      parties {
        name
        email
      }
    }
  }

Mutation de Ejemplo: Creación de Contrato

Permite instanciar un contrato a partir de una plantilla predefinida.
GraphQL

  mutation NewContract {
    createContract(input: {
      title: "NDA - Proyecto Alpha",
      templateId: "tmpl_998",
      parties: [
        { name: "Tech Corp", email: "legal@tech.com", role: "SENDER" },
        { name: "John Doe", email: "j.doe@partner.com", role: "SIGNER" }
      ]
    }) {
      id
      status
    }
  }

4. Estrategia de API Management para GraphQL

Implementar GraphQL en Contract Manager Pro requiere controles específicos que no existen en REST:

  Análisis de Complejidad (Query Cost): Dado que un contrato puede tener cientos de cláusulas y logs de auditoría, se asigna un "costo" a 
  cada campo. Si una consulta supera el costo permitido (ej. 500 puntos), el API Gateway la rechaza.
  Dataloader Pattern: Para evitar el problema de N+1 al consultar contratos y sus respectivos AuditLogs, 
  se implementa una capa de procesamiento por lotes (Batching) en el servidor.
  Persisted Queries: Para mejorar el rendimiento y la seguridad, los clientes envían un hash de la 
  consulta en lugar de la cadena completa de GraphQL.
  Authorization at Field Level: Se implementan directivas (ej. @auth) para asegurar que solo los usuarios 
  con rol ADMIN puedan ver el campo ipAddress dentro de AuditLog.

5. Comparativa: REST vs GraphQL en este Sistema

      Característica	                        REST API (v1)	                            GraphQL API (v1)
      Flexibilidad	                      Estricta (Endpoints fijos)	            Alta (El cliente define la forma)
      Eficiencia de Red	Múltiples        llamadas para datos relacionados	      Una sola llamada (Batching)
      Versionado	Por URL                   (/v1/, /v2/)	                      Evolución continua (campos @deprecated)
      Ideal para...	                    Integraciones Server-to-Server	          Dashboards y Clientes Web/Móvil
