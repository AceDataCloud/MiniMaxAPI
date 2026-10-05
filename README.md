# MiniMax H3 API

Generation waits for completion by default. Send `async=true` or `callback_url` to receive a `task_id`, then poll `/minimax/tasks`. The published model is `MiniMax-H3`; H3 Max, prompt enhancement and regeneration are not published.

Generate 4–15 second videos from text, first/last frames, and multimodal references through AceDataCloud.

MCP integration: [MiniMax H3 MCP](https://github.com/AceDataCloud/MinimaxMCP).

The MCP supports `MiniMax-H3` at `768P` or `2K`, using text, reference images, or audio. After generation returns a task ID, keep querying the task until its status is `succeeded`, `failed`, or `cancelled`. Only use `task.content.url` after `succeeded`; for failure or cancellation, inspect the task's error information. Receiving a task ID does not mean the video is ready. See the [task retrieval procedure](docs/tasks.md).

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
    "duration": 4,
    "async": true
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
