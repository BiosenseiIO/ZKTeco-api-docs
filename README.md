# ZKTeco-api-docs
# BioSensei API Integration Guide 🚀

Bienvenido api para integrar ZKTECO con **BioSensei**. BioSensei es una plataforma avanzada para el control de asistencia y acceso mediante dispositivos biométricos. Este documento detalla cómo interactuar con nuestra API para gestionar dispositivos y ejecutar acciones remotas, como la apertura de puertas.

## 📌 Configuración Inicial

Para interactuar con la API, todas las peticiones deben incluir los siguientes encabezados de autenticación:

| Header | Descripción |
| :--- | :--- |
| `Authorization` | `Bearer <TU_TOKEN_JWT>` |
| `Content-Type` | `application/json` |

**Base URL:** `https://api.biosensei.io`

---

## 🛠️ Operaciones Principales

### 1. Obtener Lista de Dispositivos
Permite listar todos los dispositivos vinculados al workspace actual, incluyendo su estado de conexión (online/offline) y números de serie.

**Endpoint:**
`GET /api/devices`

**Ejemplo de Petición (cURL):**
```bash
curl -X GET "https://api.biosensei.io/api/devices" \
     -H "Authorization: Bearer <TOKEN>"
```

**Respuesta Exitosa:**
```json
{
  "devices": [
    {
      "id": 123,
      "name": "Main Door",
      "serial_number": "ABC123456789",
      "status": "online",
      "ip": "192.168.1.10"
    }
  ]
}
```

---

### 2. Abrir Puerta de Forma Remota
Envía un comando instantáneo a uno o varios dispositivos para liberar el cierre electrónico de la puerta.

**Endpoint:**
`POST /api/devices/actions`

**Cuerpo de la Petición (JSON):**
| Parámetro | Tipo | Descripción |
| :--- | :--- | :--- |
| `door_ids` | `Array<Integer>` | Lista de IDs de las puertas/dispositivos a accionar. |
| `command_action` | `String` | Acción a ejecutar. Usar `OpenDoor`. |

**Ejemplo de Petición:**
```bash
curl -X POST "https://api.biosensei.io/api/doors/actions" \
     -H "Authorization: Bearer <TOKEN>" \
     -H "Content-Type: application/json" \
     -d '{
           "door_id": 123,
           "command_action": "OpenDoor"
         }'
```

**Respuesta Exitosa:**
```json
{
  "message": "Actions processed",
  "result": {
    "processed": 1,
    "errors": []
  }
}
```

---

## 💡 Notas Adicionales

*   **Tiempo Real:** BioSensei utiliza WebSockets (ActionCable) para notificar cambios de estado en los dispositivos y nuevos registros de acceso.
*   **Seguridad:** Asegúrate de no exponer tu `X-Workspace-Code` ni tu token JWT en entornos públicos o en el frontend directamente sin las medidas de seguridad adecuadas.
*   **Soporte:** Para dudas adicionales, consulta la documentación oficial en [biosensei.io/docs](https://www.biosensei.io/docs/api/intro).

---

> Propiedad de BioSensei. Todos los derechos reservados. 2024.
