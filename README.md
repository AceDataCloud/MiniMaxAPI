# MiniMax H3 API

Generation waits for completion by default. Send `async=true` or `callback_url` to receive a `task_id`, then poll `/minimax/tasks`. The published model is `MiniMax-H3`; H3 Max, prompt enhancement and regeneration are not published.

Generate 4–15 second videos from text, first/last frames, and multimodal references through AceDataCloud.

MCP integration: [MiniMax H3 MCP](https://github.com/AceDataCloud/MinimaxMCP).

The MCP supports `MiniMax-H3` at `768P` or `2K`, using text, reference images, or audio. After generation returns a task ID, keep querying the task until its status is `succeeded`, `failed`, or `cancelled`. Only use `task.content.url` after `succeeded`; for failure or cancellation, inspect the task's error information. Receiving a task ID does not mean the video is ready. See the [task retrieval procedure](docs/tasks.md).

## Get your Ace Data Cloud API key

1. Open [Ace Data Cloud → Applications](https://platform.acedata.cloud/console/applications?utm_source=github&utm_medium=referral&utm_campaign=evergreen&utm_content=minimax_api_readme_api_key). Sign in, or register an account first.
2. Find **General application**. If it has not been created, choose **Initialize General Application**. A general key can use the services available to your account; a dedicated application's key is limited to its service. Check the required service/model access and [current pricing](https://platform.acedata.cloud/models?utm_source=github&utm_medium=referral&utm_campaign=evergreen&utm_content=minimax_api_readme_pricing).
3. Copy the **API Key** from the application card, or open **Keys / Manage Keys → Create** to create a separate key for this integration. Give it a recognizable name so you can revoke or rotate it independently.
4. When creating a key, review **Expiration Time**, **Set Usage Limitation**, and **Allowed APIs**. Enable only the restrictions you need, allow the operations you will call, then click **Create** and copy the token from the key list. Task lookup does not grant permission to submit a generation. Confirm the application's balance can cover your first request; any trial allowance is whatever your account currently displays.
5. Copy only the token value, without quotation marks, spaces, or `Bearer `. An API Key / API Token field expects this raw value; only a manually configured HTTP `Authorization` header needs the `Bearer ` prefix. A `platform-...` account-management token cannot replace a generation API key.

![Copy an application API key or open Manage Keys](https://raw.githubusercontent.com/AceDataCloud/NanoBananaDify/2522b2f57210597af2db866445319334a1677a23/_assets/tutorial/get-api-key-en.png)

![Create a separately named key with optional restrictions](https://raw.githubusercontent.com/AceDataCloud/NanoBananaDify/2522b2f57210597af2db866445319334a1677a23/_assets/tutorial/create-api-key-en.png)

Save the key in the client's credential store or the environment variable shown below. Never put it in a prompt, shared workflow, screenshot, or Git commit. If authentication fails, check the full copied value, application/service access, expiration, usage limit, and balance before creating another key.

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
- [AceDataCloud console](https://platform.acedata.cloud/console/applications?utm_source=github&utm_medium=referral&utm_campaign=evergreen&utm_content=minimax_api_readme_api_key)
