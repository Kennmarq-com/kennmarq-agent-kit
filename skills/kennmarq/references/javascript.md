# JavaScript and TypeScript: @kennmarq/sdk

No dependencies; Node 20 or later, Bun, Deno and edge runtimes. Server code only: the key must not reach a browser.

```bash
npm install @kennmarq/sdk
```

```ts
// lib/kennmarq.ts
import { Kennmarq } from "@kennmarq/sdk";

export const kennmarq = new Kennmarq(); // the key from KENNMARQ_API_KEY

/** The marked copy of a generated file (an image, a song or a video). Throws rather than return the original. */
export async function markGenerated(file: Uint8Array | Blob | ArrayBuffer, options: { edited?: boolean; deepfake?: boolean } = {}) {
  return kennmarq.mark(file, {
    source: options.edited ? "edited" : "created",
    provider: "Acme Inc.",    // the company's legal name
    appName: "Acme Studio",   // the app, as the AI system
    appVersion: "1.0",
    markets: ["eu", "us-ca"], // the markets the app serves
    deepfake: options.deepfake,
  }); // .data (Uint8Array), .contentType, .markId
}
```

Use it where the model's output arrives, e.g. in a Next.js route handler or server action:

```ts
const res = await fetch(imageUrlFromTheModel); // or the bytes the model's SDK returns
const marked = await markGenerated(new Uint8Array(await res.arrayBuffer()));
await storage.put(key, marked.data, { contentType: marked.contentType }); // store or return only the marked copy
```

- `mark()` takes a path, a `Blob` or `File`, an `ArrayBuffer` or a `Uint8Array`. Files over 4 MB are uploaded first by
  the SDK; a video is a job, and `mark()` waits for it and downloads the result.
- Errors: `KennmarqError` (with `code`, `status`, `requestId`), `ContentBlocked` (the optional safety check, `check:
  "ai-act"`), `MissingApiKey`. Let them fail the request.
- Next.js: put the key in `.env.local` as `KENNMARQ_API_KEY`, never as `NEXT_PUBLIC_…`. Route handlers that mark
  videos need a long `maxDuration`.

SDK docs: https://kennmarq.com/docs/sdk/javascript. Without the SDK, a fetch helper: https://kennmarq.com/docs/guides/nextjs#helper
