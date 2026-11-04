# Python: kennmarq

The cloud API has no dependencies; Python 3.10 or later.

```bash
pip install kennmarq
```

```python
from kennmarq import Kennmarq

kennmarq = Kennmarq()  # the key from KENNMARQ_API_KEY

def mark_generated(data: bytes, *, edited: bool = False, deepfake: bool = False) -> bytes:
    """The marked copy of a generated image, song or video. Raises rather than return the original."""
    result = kennmarq.mark(
        data,
        source="edited" if edited else "created",
        provider="Acme Inc.",       # the company's legal name
        app_name="Acme Studio",     # the app, as the AI system
        app_version="1.0",
        markets=["eu", "us-ca"],    # the markets the app serves
        deepfake=deepfake,
    )
    return result.data  # also result.media_type, result.mark_id
```

FastAPI:

```python
from fastapi import FastAPI
from fastapi.responses import Response

app = FastAPI()

@app.post("/images")
def create_image(body: dict):
    png = generate(body["prompt"])  # your model call
    return Response(mark_generated(png), media_type="image/png")
```

- `mark()` takes bytes or a path, and an optional destination path. A video is a job: `mark()` waits for it.
- Errors raise `KennmarqError` (and `ContentBlocked` for the optional safety check, `check="ai-act"`): let them fail
  the request.
- Local mode, where only the manifest leaves the machine: `pip install "kennmarq[local]"`, then `kennmarq.mark_file(...)`.
- Without a key, `kennmarq.watermark_file(...)` (with `kennmarq[local]`) adds only the watermark and the IPTC AI
  label, with no signed Content Credentials.

SDK docs: https://kennmarq.com/docs/sdk/python
