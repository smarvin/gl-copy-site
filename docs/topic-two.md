---
layout: default
title: Tasks API Reference
permalink: /docs/topic-two/
---

# Tasks API Reference

## Base URL
```
https://api.example.com/v1
```

## Authentication - OAuth 2.0 (Client Credentials)

### Token Request
```
POST https://api.example.com/oauth/token
```

#### Body (x-www-form-urlencoded)
```
grant_type=client_credentials
client_id=YOUR_CLIENT_ID
client_secret=YOUR_CLIENT_SECRET
pat=YOUR_PAT
```

### Example Response
```json
{
  "access_token": "eyJhbGciOi...",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Use the token:
```
Authorization: Bearer YOUR_ACCESS_TOKEN
```

## OAuth Token Refresh
```
POST https://api.example.com/oauth/token
```

#### Body
```
grant_type=refresh_token
refresh_token=YOUR_REFRESH_TOKEN
client_id=YOUR_CLIENT_ID
client_secret=YOUR_CLIENT_SECRET
```

### Example Response
```json
{
  "access_token": "eyJhbGciOi...",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

## Rate Limiting
- **Limit:** 1000 requests per hour

#### Example Rate Limit Headers
```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 750
X-RateLimit-Reset: 1711046700
```

- Returns `429 Too Many Requests` if exceeded.

## Pagination

#### Supported Parameters:
| Parameter | Type   | Description                               |
|---------- |------- |-------------------------------------------|
| `limit`   | int    | Max items per page (default: 50)           |
| `cursor`  | string | Cursor token from the previous response    |

### Example Response
```json
{
  "tasks": [ /* Array of task objects */ ],
  "next_cursor": "cursor_abc123"
}
```

## Filtering and Sorting

### Filtering Example
```
GET /tasks?status=open&priority=high&due_before=2025-05-01
```

#### Supported Filters:
- `status`: `open`, `completed`
- `priority`: `low`, `medium`, `high`
- `due_before`: ISO 8601 DateTime

### Sorting Example
```
GET /tasks?sort_by=due_date&sort_order=asc
```

#### Sort Options:
- `sort_by`: `due_date`, `priority`, `created_at`
- `sort_order`: `asc` or `desc`

## 📋 Task Endpoints

### Get Tasks
```
GET /tasks
```

#### Example:
```
GET /tasks?status=open&limit=10
```

#### Example Response:
```json
{
  "tasks": [
    {
      "id": "task_123",
      "title": "Write API documentation",
      "description": "Create detailed API docs",
      "status": "open",
      "due_date": "2025-03-31T23:59:59Z",
      "priority": "high"
    }
  ],
  "next_cursor": "cursor_abc123"
}
```

### Create a Task
```
POST /tasks
```

#### Example Request:
```json
{
  "title": "Finalize project plan",
  "description": "Include milestones and deadlines",
  "due_date": "2025-04-01T17:00:00Z",
  "priority": "medium"
}
```

#### Example Response:
```json
{
  "id": "task_125",
  "title": "Finalize project plan",
  "status": "open",
  "due_date": "2025-04-01T17:00:00Z"
}
```

### Update a Task
```
PATCH /tasks/{id}
```

#### Example Request:
```json
{
  "status": "completed",
  "priority": "low"
}
```

#### Success Response:
```
200 OK
```

### Delete a Task
```
DELETE /tasks/{id}
```

#### Success Response:
```
204 No Content
```

#### Error Response:
```
404 Not Found
```

## 🔔 Webhooks

### Create Webhook
```
POST /webhooks
```

#### Example Request:
```json
{
  "url": "https://yourapp.com/webhook-handler",
  "events": ["task.created", "task.completed"]
}
```

#### Example Response:
```json
{
  "id": "wh_001",
  "status": "active"
}
```

### Supported Events:
| Event            | Description                                 |
|------------------|---------------------------------------------|
| `task.created`   | New task created                            |
| `task.updated`   | Task updated                                |
| `task.completed` | Task marked as completed                    |
| `task.deleted`   | Task deleted                                |

### Example Webhook Payload:
```json
{
  "event": "task.completed",
  "timestamp": "2025-03-21T15:30:00Z",
  "data": {
    "id": "task_456",
    "title": "Complete API spec",
    "status": "completed"
  }
}
```

## 🌐 Multi-Language Examples

### cURL Example
```bash
curl -H "Authorization: Bearer YOUR_ACCESS_TOKEN" "https://api.example.com/v1/tasks?status=open&limit=10"
```

### Python (requests)
```python
import requests
r = requests.get("https://api.example.com/v1/tasks",
                 headers={"Authorization": "Bearer YOUR_ACCESS_TOKEN"},
                 params={"status": "open", "limit": 10})
print(r.json())
```

### Node.js (Axios)
```javascript
const axios = require('axios');
axios.get('https://api.example.com/v1/tasks', {
  headers: { Authorization: 'Bearer YOUR_ACCESS_TOKEN' },
  params: { status: 'open', limit: 10 }
}).then(console.log);
```

## 🚨 Error Response Schema
```json
{
  "error": {
    "code": 400,
    "message": "Invalid request parameter",
    "details": ["Field 'title' is required."]
  }
}
```

