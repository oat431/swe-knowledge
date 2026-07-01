# tiny mchwa — Status Logic

---

## Overview

Todolist status is **computed**, not stored. There is no `status` column in the `todolists` table.

The status is calculated on read by examining all tasks belonging to that todolist.

---

## Computation Rules

| Condition | Todolist Status |
|-----------|-----------------|
| No tasks exist | `pending` |
| All tasks are `pending` | `pending` |
| Any task is `inprogress` | `inprogress` |
| All tasks are `done` | `done` |

---

## When Is Status Recomputed?

Todolist status is recalculated whenever:

1. **Task created** — new task added to todolist
2. **Task updated** — task status changes
3. **Task deleted** — task removed from todolist

---

## Pseudocode

```go
func computeTodolistStatus(tasks []Task) string {
    if len(tasks) == 0 {
        return "pending"
    }
    
    hasInProgress := false
    allDone := true
    
    for _, task := range tasks {
        if task.Status == "inprogress" {
            hasInProgress = true
        }
        if task.Status != "done" {
            allDone = false
        }
    }
    
    if allDone {
        return "done"
    }
    if hasInProgress {
        return "inprogress"
    }
    return "pending"
}
```

---

## SQL Query (Alternative)

If computing in SQL instead of application code:

```sql
SELECT 
    t.id,
    t.title,
    t.description,
    CASE 
        WHEN COUNT(tk.id) = 0 THEN 'pending'
        WHEN BOOL_AND(tk.status = 'done') THEN 'done'
        WHEN BOOL_OR(tk.status = 'inprogress') THEN 'inprogress'
        ELSE 'pending'
    END AS status,
    t.created_at,
    t.updated_at,
    t.owned_by,
    t.source_service
FROM todolists t
LEFT JOIN tasks tk ON tk.todolist_id = t.id
WHERE t.id = $1
GROUP BY t.id;
```

---

## Related Documents

- [[00 Project Overview]]
- [[01 Data Model]]
- [[02 API Spec]]
