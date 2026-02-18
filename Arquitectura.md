# Arquitectura del sistema 

Para este sistema, los recursos principales se dividen en:

    /contracts: Gestión del documento y metadatos.

    /templates: Plantillas preconfiguradas.

    /signatures: Estado y flujos de firma digital.

    /webhooks: Notificaciones en tiempo real para sistemas externos.

2. Especificación de Endpoints (Contratos)
3. 
          A. Gestión de Contratos (/contracts)
   
            Método	               Endpoint	                 Descripción	                      Parámetros Clave
               POST	         /v1/contracts	       Crea un nuevo borrador de contrato.	        template_id, parties,    metadata
               GET	           /v1/contracts/{id}	   Recupera el estado y contenido.	            id (UUID)
               PATCH	         /v1/contracts/{id}	   Actualiza cláusulas o datos.	                content, status
               DELETE	       /v1/contracts/{id}	   Borrado lógico del contrato.	                reason

Ejemplo de Payload (POST):JSON

    {
      "title": "Acuerdo de Confidencialidad - Cliente X",
      "template_id": "tmpl-8823",
      "parties": [
        { "role": "sender", "email": "legal@empresa.com" },
        { "role": "signer", "email": "ceo@cliente.com" }
      ],
      "expires_at": "2026-12-31T23:59:59Z"
    }

B. Flujo de Firma (/signatures) 

Este módulo se integra con proveedores externos (DocuSign, Adobe Sign) o un motor interno.

        Método	                  Endpoint	                         Descripción
        POST	              /v1/contracts/{id}/send	        Inicia el proceso de firma y envía correos.
        GET	              /v1/contracts/{id}/audit-log	  Historial completo de interacciones (IP, timestamp).
        
3. Modelo de Datos y Estados

El contrato debe seguir una máquina de estados estricta para garantizar la integridad legal.
Estados Soportados:

    DRAFT: Edición interna.
    IN_REVIEW: Pendiente de aprobación legal.
    PENDING_SIGNATURE: Enviado a las partes.
    SIGNED: Contrato vinculado legalmente.
    EXPIRED/VOID: Sin validez.

4. Gestión de Errores y Seguridad
   
    Encabezados Requeridos (Headers):
        Authorization: Bearer <JWT_TOKEN>
        X-API-Key: <CLIENT_ID>
        Content-Type: application/json
   
Respuestas de Error Estándar:
    Código	Significado	Motivo Típico
        400	Bad Request	El JSON enviado es inválido o faltan campos obligatorios.
        401	Unauthorized	Token expirado o inválido.
        403	Forbidden	El usuario no tiene permisos sobre este contrato específico.
        422	Unprocessable Entity	Error de lógica de negocio (ej. intentar editar un contrato ya firmado).

5. Webhooks y Eventos Asíncronos

Dado que la firma de un contrato es un proceso humano lento, el sistema debe ser Event-Driven.

Evento: contract.signed
JSON

    {
      "event_type": "contract.signed",
      "timestamp": "2026-02-17T14:00:00Z",
      "data": {
        "contract_id": "uuid-12345",
        "signed_by": "ceo@cliente.com",
        "document_url": "https://storage.provider.com/signed/contract_01.pdf"
      }
    }
