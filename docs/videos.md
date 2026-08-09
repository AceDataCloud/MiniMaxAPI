# Video Generation

## Request

`POST https://api.acedata.cloud/minimax/videos`

| Field | Type | Default | Constraints |
| --- | --- | --- | --- |
| `model` | string | — | required; `MiniMax-H3` |
| `content` | object[] | — | required; contains one non-empty `text` item (max 7000 chars) |
| `resolution` | string | — | required; `768P` or `2K` |
| `duration` | integer | — | required; 4–15 |
| `ratio` | string | `adaptive` | `adaptive`, `21:9`, `16:9`, `4:3`, `1:1`, `3:4`, or `9:16` |
| `callback_url` | string | — | public HTTP(S) webhook |

The API does not accept legacy `prompt`, `image_urls`, `audio_urls`, `messages`, `first_frame_image`, or `async` fields.

## Content items

| `type` | Data field | `role` | Purpose |
| --- | --- | --- | --- |
| `text` | `text` | — | Required prompt |
| `image_url` | `image_url.url` | `first_frame`, `last_frame`, `reference_image` | Frame control or visual reference |
| `video_url` | `video_url.url` | `reference_video` | Motion or camera reference |
| `audio_url` | `audio_url.url` | `reference_audio` | Dialogue, music, or rhythm reference |

Media URLs may be public HTTPS URLs, `mm_file://{file_id}` references, or Base64 data URIs. First/last frames cannot be combined with reference media. Use at most one first frame, one last frame, nine reference images, three reference videos, and three reference audio files; reference media totals at most 12 files.

## Image-to-video example

```json
{
  "model": "MiniMax-H3",
  "content": [
    {
      "type": "text",
      "text": "Preserve the character while the camera slowly pushes in"
    },
    {
      "type": "image_url",
      "image_url": { "url": "https://cdn.acedata.cloud/b1c82e4937.png" },
      "role": "first_frame"
    }
  ],
  "resolution": "2K",
  "ratio": "9:16",
  "duration": 8
}
```

For text-only requests, provide only the `text` item and use a fixed `ratio`. For first/last-frame requests, use `adaptive` or omit `ratio`.

## Response and callbacks

Creating a video returns a task ID:

```json
{ "task_id": "TASK_ID" }
```

When `callback_url` is set, first return the POSTed `challenge` value unchanged within three seconds to verify the callback address. Then handle POSTed task status notifications. Save the `task_id` and poll the task API as a fallback.
