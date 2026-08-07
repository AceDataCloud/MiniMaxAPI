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

The response contains `items` and `count`. Poll approximately every five seconds until the stored `response` contains a successful result or an error.

## Delete a task record

```json
{
  "action": "delete",
  "id": "TASK_ID"
}
```

Deletion removes the stored task record only. It does not cancel a generation already in progress.
