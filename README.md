# MiniMax H3 API (Hailuo 03): runnable cURL and Node.js examples

[![PoYo model page](https://img.shields.io/badge/Try%20on%20PoYo-minimax--h3--api-84cc16)](https://poyo.ai/models/minimax-h3) [![API docs](https://img.shields.io/badge/API-docs-22d3ee)](https://docs.poyo.ai/api-manual/video-series/hailuo-03) [![Check](https://github.com/PoyoAPI/MiniMax-H3-API/actions/workflows/check.yml/badge.svg)](https://github.com/PoyoAPI/MiniMax-H3-API/actions/workflows/check.yml)

Call MiniMax H3, exposed as model ID hailuo-03, for 2K text-to-video and reference-driven video generation.

**Answer:** To use the MiniMax H3 API (Hailuo 03), send a server-side request to `POST https://api.poyo.ai/api/generate/submit` with model `hailuo-03` and a PoYo API key. The examples in this repository are runnable with Node.js 22+ or cURL.

Last verified against the live PoYo schema and pricing on **November 16, 2026**.

## Quickstart

1. Create a key in the [PoYo dashboard](https://poyo.ai/dashboard/api-key).
2. Copy `.env.example` to `.env` and replace the placeholder.
3. Run `node node/index.mjs`.

```dotenv
POYO_API_KEY=YOUR_POYO_API_KEY_HERE
POYO_BASE_URL=https://api.poyo.ai
```

The complete cURL request is in [curl/generate.md](curl/generate.md). The Node.js example submits the task and polls until `finished` or `failed`.

## Model IDs

- `hailuo-03`

Use the exact IDs above. Product names and API model IDs are not always identical.

## Request fields

| Field | Requirement | Meaning |
| --- | --- | --- |
| prompt | required | Video description, up to 2,000 characters. |
| duration | optional | Integer from 5 to 15 seconds; default 5. |
| resolution | optional | 2K (default). |
| aspect_ratio | optional | adaptive, 21:9, 16:9, 4:3, 1:1, 3:4, or 9:16. |
| image_urls | optional | One or two first/last-frame images. |
| reference_image_urls | optional | Up to 9 reference images. |
| reference_video_urls | optional | Up to 3 clips; each 2-15 seconds and at most 15 seconds total. |
| reference_audio_urls | optional | Up to 3 clips; each 2-15 seconds and at most 15 seconds total. |

## Pricing snapshot

| Usage | Price |
| --- | --- |
| Output without reference video | $0.105 per output second |
| Output with reference video | $0.105 per billed second |
| Reference images 1-5 | Free |
| Each reference image after 5 | $0.032 |

Pricing can change. Confirm the current amount on the [model page](https://poyo.ai/models/minimax-h3) before a production rollout.

## Best for

- 2K cinematic clips
- first-and-last-frame motion
- multi-reference character shots

## Important behavior

- Use the API model ID hailuo-03 even though the product name is MiniMax H3.
- First/last-frame image mode cannot be mixed with reference inputs.
- When reference video is used, billed duration includes output plus reference-video duration.
- Reference audio requires at least one reference image or video.

## Async production flow

```text
submit -> persist data.task_id -> poll during development -> receive callback_url webhook in production
```

Treat `finished` and `failed` as terminal states. Accept a webhook only for task IDs your system created, process it idempotently, and reconcile the final state through the status endpoint.

## Repository map

- [`curl/generate.md`](curl/generate.md) — copy-paste request
- [`node/index.mjs`](node/index.mjs) — dependency-free Node.js example
- [`docs/production-notes.md`](docs/production-notes.md) — limits and rollout checklist

## Links

- [Try MiniMax H3 API (Hailuo 03) on PoYo](https://poyo.ai/models/minimax-h3)
- [API reference](https://docs.poyo.ai/api-manual/video-series/hailuo-03)
- [All PoYo examples](https://github.com/PoyoAPI/poyo-examples)
- [PoYo model catalog](https://poyo.ai/models)

## License

MIT

