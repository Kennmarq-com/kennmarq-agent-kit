# Any language: the HTTP API

`POST https://api.kennmarq.com/v1/mark`, multipart form data, with `Authorization: Bearer $KENNMARQ_API_KEY`. The
response body is the marked file; `X-Mark-Id` and `X-Watermark-Id` identify it.

```bash
curl https://api.kennmarq.com/v1/mark \
  -H "Authorization: Bearer $KENNMARQ_API_KEY" \
  -F file=@image.png \
  -F source=created \
  -F provider="Acme Inc." \
  -F app_name="Acme Studio" -F app_version="1.0" \
  -F markets=eu,us-ca \
  -o image.marked.png
```

- Fields: `file`, `source` (`created`, `edited`, `composite`), `provider`, `app_name`, `app_version`, `model`,
  `markets` (comma-separated), `deepfake=true`, `label=true` and the `label_*` options.
- Files over 4 MB: `POST /v1/uploads` with `{"bytes": <size>}`, `PUT` the file to the `url` it answers with its
  `headers` (no Authorization header) within 15 minutes, then send `upload=<id>` instead of `file`.
- Videos answer `202` with a job: `GET /v1/marks/{id}` until `status` is `completed`, then download `download.url`.
- Errors are JSON: `{"error": {"code", "message", "request_id"}}`. `429` carries `Retry-After`.

Reference: https://kennmarq.com/docs/api/mark, and the OpenAPI document: https://api.kennmarq.com/v1/openapi.json
