# tiny mchwa — Data Model

---

## Todolist

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| id | UUID | auto | Primary key |
| title | string | yes | Max 255 chars |
| description | string | no | Max 1000 chars |
| status | COMPUTED | — | Not stored. Derived from tasks |
| createdAt | ISO8601 | auto | Creation timestamp |
| updatedAt | ISO8601 | auto | Last update timestamp |
| ownedBy | UUID | yes | Keycloak user UUID |
| sourceService | string | yes | Origin service identifier |

### Status (Computed)

Todolist status is NOT stored in the database. It is calculated on read based on child tasks:

| Condition | Status |
|-----------|--------|
| No tasks | `pending` |
| All tasks `pending` | `pending` |
| Any task `inprogress` | `inprogress` |
| All tasks `done` | `done` |

---

## Task

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| id | UUID | auto | Primary key |
| todolistId | UUID | yes | Foreign key → todolist.id |
| title | string | yes | Max 255 chars |
| description | string | no | Max 1000 chars |
| status | enum | yes | `pending` (default), `inprogress`, `done` |
| createdAt | ISO8601 | auto | Creation timestamp |
| updatedAt | ISO8601 | auto | Last update timestamp |

### Status Enum

| Value | Meaning |
|-------|---------|
| `pending` | Not started (default for new tasks) |
| `inprogress` | Currently being worked on |
| `done` | Completed |

---

## Relationships

```
todolist 1 ──── * task
```

- One todolist has many tasks
- Task always belongs to one todolist
- Deleting a todolist cascades to delete all its tasks
- Tasks inherit `ownedBy` and `sourceService` from parent todolist (not stored on task)

---

## PostgreSQL Schema (Draft)

```sql
CREATE TABLE todolists (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    title VARCHAR(255) NOT NULL,
    description VARCHAR(1000),
    owned_by UUID NOT NULL,
    source_service VARCHAR(100) NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE TABLE tasks (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    todolist_id UUID NOT NULL REFERENCES todolists(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    description VARCHAR(1000),
    status VARCHAR(20) NOT NULL DEFAULT 'pending',
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_todolists_owned_by ON todolists(owned_by);
CREATE INDEX idx_tasks_todolist_id ON tasks(todolist_id);
CREATE INDEX idx_tasks_status ON tasks(status);
```

---

## Related Documents

- [[00 Project Overview]]
- [[02 API Spec]]
- [[03 Status Logic]]
