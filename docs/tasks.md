# Task Retrieval

`POST https://api.acedata.cloud/minimax/tasks` is free and never charges the generation again.

## Retrieve one

```json
{
  "action": "retrieve",
  "id": "TASK_ID"
}
```

## Retrieve several

```json
{
  "action": "retrieve_batch",
  "ids": ["TASK_ID_1", "TASK_ID_2"]
}
```

The response is `{ "task": {...} }`. Poll approximately every 10 seconds until `task.status` is `succeeded`, `failed`, or `cancelled`. On success, read the generated video from `task.content.url`.

## Delete a task record

```json
{
  "action": "delete",
  "id": "TASK_ID"
}
```

Batch retrieval accepts optional `ids`, `limit`, `offset`, `created_at_min`, and `created_at_max`. Its response contains `items` and `total`.

Deletion is state-dependent:

| Current status | Result |
| --- | --- |
| `queued` | Cancels the task |
| `succeeded` or `failed` | Deletes the task record |
| `running` or `cancelled` | Returns an error |

A successful deletion response is `{ "id": "TASK_ID", "deleted": true }`.
