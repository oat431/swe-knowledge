# tiny mchwa — API Specification

> Base URL: `/api/v1`
> Auth: Bearer token (Keycloak JWT)
> Gateway: Flowero Gate

---

## Response Format

All responses follow this structure:

```json
{
    "data": {},
    "error": null,
    "meta": {}
}
```

- `data` — Response payload (object or array)
- `error` — Error message (null on success)
- `meta` — Pagination info (null for single items)

---

## Pagination

For list endpoints, use query parameters:

```
?page=1&perPage=20
```

Response `meta` structure:

```json
{
    "meta": {
        "page": 1,
        "perPage": 20,
        "total": 150,
        "totalPages": 8
    }
}
```

---

## Filtering

All filter parameters are optional and combinable.

**Todolists:**
```
?status=inprogress&sourceService=blog&title=grocery
```

**Tasks:**
```
?status=pending&title=milk
```

---

## Endpoints — Todolists

### Create Todolist

```
POST /api/v1/todolists
```

**Request:**
```json
{
    "title": "Grocery List",
    "description": "Weekly groceries",
    "sourceService": "todolist"
}
```

**Response (201):**
```json
{
    "data": {
        "id": "uuid",
        "title": "Grocery List",
        "description": "Weekly groceries",
        "status": "pending",
        "createdAt": "2026-07-01T10:00:00Z",
        "updatedAt": "2026-07-01T10:00:00Z",
        "ownedBy": "keycloak-uuid",
        "sourceService": "todolist"
    },
    "error": null,
    "meta": null
}
```

---

### List Todolists

```
GET /api/v1/todolists?page=1&perPage=20&status=inprogress&sourceService=blog&title=grocery
```

**Response (200):**
```json
{
    "data": [
        {
            "id": "uuid",
            "title": "Grocery List",
            "description": "Weekly groceries",
            "status": "inprogress",
            "createdAt": "2026-07-01T10:00:00Z",
            "updatedAt": "2026-07-01T10:00:00Z",
            "ownedBy": "keycloak-uuid",
            "sourceService": "todolist"
        }
    ],
    "error": null,
    "meta": {
        "page": 1,
        "perPage": 20,
        "total": 45,
        "totalPages": 3
    }
}
```

---

### Get Todolist

```
GET /api/v1/todolists/:id
```

**Response (200):**
```json
{
    "data": {
        "id": "uuid",
        "title": "Grocery List",
        "description": "Weekly groceries",
        "status": "inprogress",
        "createdAt": "2026-07-01T10:00:00Z",
        "updatedAt": "2026-07-01T10:00:00Z",
        "ownedBy": "keycloak-uuid",
        "sourceService": "todolist"
    },
    "error": null,
    "meta": null
}
```

---

### Update Todolist

```
PUT /api/v1/todolists/:id
```

**Request:**
```json
{
    "title": "Updated Title",
    "description": "Updated description"
}
```

**Response (200):** Same as Get Todolist.

**Note:** `status` cannot be set directly. It is computed from tasks.

---

### Delete Todolist

```
DELETE /api/v1/todolists/:id
```

**Response (204):** No content.

**Note:** Cascades to delete all tasks in this todolist.

---

## Endpoints — Tasks

### Create Task

```
POST /api/v1/todolists/:todolistId/tasks
```

**Request:**
```json
{
    "title": "Buy milk",
    "description": "2% fat, 1 gallon"
}
```

**Response (201):**
```json
{
    "data": {
        "id": "uuid",
        "todolistId": "todolist-uuid",
        "title": "Buy milk",
        "description": "2% fat, 1 gallon",
        "status": "pending",
        "createdAt": "2026-07-01T10:00:00Z",
        "updatedAt": "2026-07-01T10:00:00Z"
    },
    "error": null,
    "meta": null
}
```

---

### List Tasks

```
GET /api/v1/todolists/:id/tasks?page=1&perPage=20&status=pending&title=milk
```

**Response (200):**
```json
{
    "data": [
        {
            "id": "uuid",
            "todolistId": "todolist-uuid",
            "title": "Buy milk",
            "description": "2% fat, 1 gallon",
            "status": "pending",
            "createdAt": "2026-07-01T10:00:00Z",
            "updatedAt": "2026-07-01T10:00:00Z"
        }
    ],
    "error": null,
    "meta": {
        "page": 1,
        "perPage": 20,
        "total": 12,
        "totalPages": 1
    }
}
```

---

### Get Task

```
GET /api/v1/tasks/:id
```

**Response (200):**
```json
{
    "data": {
        "id": "uuid",
        "todolistId": "todolist-uuid",
        "title": "Buy milk",
        "description": "2% fat, 1 gallon",
        "status": "pending",
        "createdAt": "2026-07-01T10:00:00Z",
        "updatedAt": "2026-07-01T10:00:00Z"
    },
    "error": null,
    "meta": null
}
```

---

### Update Task

```
PUT /api/v1/tasks/:id
```

**Request:**
```json
{
    "title": "Buy milk",
    "description": "2% fat, 1 gallon",
    "status": "done"
}
```

**Response (200):** Same as Get Task.

**Note:** Updating task status triggers todolist status recomputation.

---

### Delete Task

```
DELETE /api/v1/tasks/:id
```

**Response (204):** No content.

**Note:** Deleting a task triggers todolist status recomputation.

---

## Error Responses

**400 Bad Request:**
```json
{
    "data": null,
    "error": {
        "code": 400,
        "message": "Validation failed",
        "details": ["title is required"]
    },
    "meta": null
}
```

**401 Unauthorized:**
```json
{
    "data": null,
    "error": {
        "code": 401,
        "message": "Invalid or missing token"
    },
    "meta": null
}
```

**403 Forbidden:**
```json
{
    "data": null,
    "error": {
        "code": 403,
        "message": "Not owned by this user"
    },
    "meta": null
}
```

**404 Not Found:**
```json
{
    "data": null,
    "error": {
        "code": 404,
        "message": "Todolist not found"
    },
    "meta": null
}
```

---

## Related Documents

- [[00 Project Overview]]
- [[01 Data Model]]
- [[03 Status Logic]]
