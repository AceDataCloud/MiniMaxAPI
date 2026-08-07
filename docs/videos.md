# Video Generation

## Request

`POST https://api.acedata.cloud/minimax/videos`

| Field | Type | Default | Constraints |
| --- | --- | --- | --- |
| `model` | string | `minimax-h3` | only `minimax-h3` |
| `prompt` | string | — | required for text-only mode |
| `image_urls` | string[] | — | 1–9 public HTTP(S) URLs |
| `audio_urls` | string[] | — | 1–3 public HTTP(S) URLs |
| `ratio` | string | `16:9` | `16:9` or `9:16` |
| `duration` | integer | 4 | 4–15 |
| `async` | boolean | false | return task ID immediately |
| `callback_url` | string | — | public HTTP(S) webhook |

At least one of `prompt`, `image_urls`, or `audio_urls` is required.

## Image-to-video example

```json
{
  "model": "minimax-h3",
  "prompt": "Preserve the character while the camera slowly pushes in",
  "image_urls": ["https://cdn.acedata.cloud/b1c82e4937.png"],
  "ratio": "9:16",
  "duration": 8,
  "async": true
}
```

## Audio-guided example

```json
{
  "model": "minimax-h3",
  "prompt": "A dancer moves naturally to the rhythm",
  "image_urls": ["https://cdn.acedata.cloud/b1c82e4937.png"],
  "audio_urls": ["https://cdn.acedata.cloud/6f7d62b18b.wav"],
  "ratio": "9:16",
  "duration": 8,
  "async": true
}
```

## Final result fields

A successful final response contains `success`, `task_id`, `trace_id`, and `data`. The first result includes:

- `id`: public AceDataCloud task ID
- `model`: `minimax-h3`
- `mode`: `text_to_video`, `image_to_video`, or `audio_guided`
- `video_url`: AceDataCloud CDN URL
- `state`: `succeeded`
- `duration`: final billed seconds
- `ratio`: output aspect ratio
