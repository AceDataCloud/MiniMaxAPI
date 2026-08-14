# MiniMax H3 API

Creation is always asynchronous: save the returned `task_id`, then poll `/minimax/tasks` or use `callback_url`.

Generate 4–15 second videos from text, first/last frames, and multimodal references through AceDataCloud.

MCP integration: [MiniMax H3 MCP](https://github.com/AceDataCloud/MinimaxMCP).

## Endpoints

| Method | Path | Purpose |
| --- | --- | --- |
| POST | `https://api.acedata.cloud/minimax/videos` | Create a video task |
| POST | `https://api.acedata.cloud/minimax/tasks` | Retrieve or delete task records |

## Quick start

```bash
curl -X POST https://api.acedata.cloud/minimax/videos \
  -H "Authorization: Bearer $ACEDATACLOUD_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "MiniMax-H3",
    "content": [
      {
        "type": "text",
        "text": "A red fox running through a snowy forest at dawn, low tracking shot"
      }
    ],
    "resolution": "2K",
    "ratio": "16:9",
    "duration": 4
  }'
```

Poll the returned `task_id`:

```bash
curl -X POST https://api.acedata.cloud/minimax/tasks \
  -H "Authorization: Bearer $ACEDATACLOUD_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"action":"retrieve","id":"TASK_ID"}'
```

The API uses a `content` array for the required text prompt and optional image, video, and audio references. Poll the task about every 10 seconds until it reaches a terminal status.

- [Video generation guide](docs/videos.md)
- [Task API guide](docs/tasks.md)
- [AceDataCloud console](https://platform.acedata.cloud/console/applications)
