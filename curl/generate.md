# MiniMax H3 API (Hailuo 03): cURL quickstart

Submit an async video task with model `hailuo-03`:

```bash
export POYO_API_KEY="your-api-key"
export POYO_BASE_URL="https://api.poyo.ai"

curl -X POST "$POYO_BASE_URL/api/generate/submit" \
  -H "Authorization: Bearer $POYO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
  "model": "hailuo-03",
  "input": {
    "prompt": "A paper dragon awakens on an illustrator's desk, warm morning light, precise paper texture, gentle camera orbit",
    "duration": 5,
    "resolution": "2K",
    "aspect_ratio": "16:9"
  }
}'
```

Save `data.task_id`, then check status:

```bash
curl "$POYO_BASE_URL/api/generate/status/<task_id>" \
  -H "Authorization: Bearer $POYO_API_KEY"
```

For production, add a public HTTPS `callback_url` to the top-level request and make the receiver idempotent.

See the [full README](../README.md) and [production notes](../docs/production-notes.md).

