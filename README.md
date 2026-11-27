# MiniMax API

Generate videos through Ace Data Cloud and retrieve the completed video by task ID.

[API documentation](https://platform.acedata.cloud/documents/minimax-videos-integration?lang=en&utm_source=github&utm_medium=referral&utm_campaign=evergreen) · [Models and pricing](https://platform.acedata.cloud/services/minimax?utm_source=github&utm_medium=referral&utm_campaign=evergreen) · [中文文档](https://platform.acedata.cloud/documents/minimax-videos-integration?lang=zh-cn&utm_source=github&utm_medium=referral&utm_campaign=evergreen)

## Get an API key

1. Open [Applications](https://platform.acedata.cloud/console/applications?lang=en&utm_source=github&utm_medium=referral&utm_campaign=evergreen) and sign in or register.
2. Find **General Application**. If it is missing, select **Initialize General Application**. Use the copy button marked **1** to copy its API key. Confirm service access and enough balance for your first request.

![General Application with the API key copy button marked 1 and Manage Keys marked 2](https://raw.githubusercontent.com/AceDataCloud/SunoDify/8d3d03570e73324220f111f7fe343da0792a8071/_assets/tutorial/get-api-key-en.png)

For a separate key, select **Manage Keys** (marked **2**) → **Create**. Enter a name for your integration, review optional usage limits, expiration and allowed APIs, then select **Create**. The screenshot uses an example name.

![Create New Credential with a name, optional restrictions and the Create button](https://raw.githubusercontent.com/AceDataCloud/SunoDify/8d3d03570e73324220f111f7fe343da0792a8071/_assets/tutorial/create-api-key-en.png)

3. Copy the raw key value and save it in your terminal environment. The HTTP examples add the `Bearer` prefix for you.

```bash
export ACEDATACLOUD_API_KEY='YOUR_API_KEY'
```

[Step-by-step API key guide](https://platform.acedata.cloud/documents/api-key-guide?lang=en&utm_source=github&utm_medium=referral&utm_campaign=evergreen)

## Quickstart

```bash
curl --fail-with-body --silent --show-error https://api.acedata.cloud/minimax/videos \
  --header "Authorization: Bearer $ACEDATACLOUD_API_KEY" \
  --header 'Content-Type: application/json' \
  --data '{
  "model": "MiniMax-H3",
  "content": [
    {
      "type": "text",
      "text": "A sailboat crossing a calm lake at sunrise, slow camera pan"
    }
  ],
  "resolution": "768P",
  "duration": 5,
  "async": true
}'
```

Save the returned `task_id`. An accepted response has this shape.

```json
{"task_id":"YOUR_TASK_ID"}
```

Query that same task approximately every 10 seconds.

```bash
curl --fail-with-body --silent --show-error https://api.acedata.cloud/minimax/tasks \
  --header "Authorization: Bearer $ACEDATACLOUD_API_KEY" \
  --header 'Content-Type: application/json' \
  --data '{"action":"retrieve","id":"YOUR_TASK_ID"}'
```

Continue while `task.status` is `queued` or `running`. Stop on `succeeded`, `failed`, or `cancelled`. On success, open `task.content.url`.

A completed result contains fields like these. The media URL below is illustrative.

```json
{
  "task": {
    "status": "succeeded",
    "content": {
      "url": "https://media.example.com/result.mp4"
    }
  }
}
```

Keep polling the original task after a timeout or page reload. Submit again only when you intend to start a new generation.

## API reference

- [`/minimax/videos`](https://platform.acedata.cloud/documents/minimax-videos?utm_source=github&utm_medium=referral&utm_campaign=evergreen)
- [`/minimax/tasks`](https://platform.acedata.cloud/documents/minimax-tasks?utm_source=github&utm_medium=referral&utm_campaign=evergreen)

## Troubleshooting

- For authentication errors, check the copied API key, service access, expiration and configured usage limits. [API key guide](https://platform.acedata.cloud/documents/api-key-guide?lang=en&utm_source=github&utm_medium=referral&utm_campaign=evergreen)
- For insufficient balance, open the application and review its balance and the current service price.
- For request validation errors, compare the model and required fields with the linked API reference.
- For a long-running or failed task, retain `task_id` and `trace_id` when contacting [support](https://platform.acedata.cloud/support?utm_source=github&utm_medium=referral&utm_campaign=evergreen).

## Trial account concurrency

Accounts with neither a successful paid top-up nor an administrator's manual credit addition may run **one billable request at a time across the entire account**, shared by all API keys and services. Rewards, redemption cards and transferred credits do not count as paid top-ups. A successful paid top-up or manual credit addition removes this trial restriction; balance and key usage-limit checks still apply.

A new billable request returns `429 too_many_requests` while another is running. Wait for the active request's final result before submitting the next one. Free task polling remains available.

For async APIs, acceptance or a task ID is not completion: keep querying the same task until its final result. A disconnect or proxy timeout does not mean execution stopped. If a task has no terminal result for a long time, contact support with `trace_id`.

This restriction changes neither trial API availability nor pricing; fees are settled after execution based on actual usage.
