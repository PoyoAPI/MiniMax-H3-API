# MiniMax H3 API (Hailuo 03) production notes

## Confirmed contract

- Endpoint: `POST /api/generate/submit`
- Model ID: `hailuo-03`
- Authentication: `Authorization: Bearer <POYO_API_KEY>`

## Input limits

| Field | Requirement | Constraint |
| --- | --- | --- |
| prompt | required | Video description, up to 2,000 characters. |
| duration | required | Integer from 5 to 15 seconds. |
| resolution | required | 2K. |
| aspect_ratio | optional | adaptive, 21:9, 16:9, 4:3, 1:1, 3:4, or 9:16. |
| image_urls | optional | One or two first/last-frame images. |
| reference_image_urls | optional | Up to 9 reference images. |
| reference_video_urls | optional | Up to 3 clips; each 2-5 seconds and at most 15 seconds total. |
| reference_audio_urls | optional | Up to 3 clips; each 2-5 seconds. |

## Operational checklist

- Keep API keys in a server-side secret manager.
- Validate user input before calling the API.
- Persist `data.task_id` immediately after submission.
- Use five-second polling only for development; prefer `callback_url` in production.
- Make webhook processing idempotent and reconcile status before fulfilling an order.
- Copy output files to durable storage before provider URLs expire.
- Recheck the live model page before hard-coding prices.

## Failure handling

Treat non-2xx HTTP responses and non-success API `code` values as errors. Preserve the HTTP status, API code, request ID when present, and a redacted response body for debugging.

Schema: [PoYo API documentation](https://docs.poyo.ai/api-manual/video-series/hailuo-03)

