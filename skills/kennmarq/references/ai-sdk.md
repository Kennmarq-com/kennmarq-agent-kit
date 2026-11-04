# Vercel AI SDK (ai 7)

`@kennmarq/ai-sdk` marks what the AI SDK generates, before your code sees it. It reads the key from
`KENNMARQ_API_KEY`.

```bash
npm install @kennmarq/ai-sdk
```

## Images: middleware for wrapImageModel

```ts
// app/api/image/route.ts: server code only
import { generateImage, wrapImageModel } from "ai";
import { openai } from "@ai-sdk/openai";
import { kennmarqImageMiddleware } from "@kennmarq/ai-sdk";

const model = wrapImageModel({
  model: openai.image("gpt-image-1"), // any image model, any provider
  middleware: kennmarqImageMiddleware({
    provider: "Acme Inc.",    // the company's legal name
    appName: "Acme Studio",   // the app, as the AI system
    appVersion: "1.0",
    markets: ["eu", "us-ca"], // the markets the app serves
  }),
});

export async function POST(request: Request) {
  const { prompt } = await request.json();
  const { image } = await generateImage({ model, prompt }); // already marked
  return new Response(image.uint8Array, { headers: { "Content-Type": image.mediaType } });
}
```

- Images made from input images (`prompt: { text, images }`) are marked as `edited`, the rest as `created`.
- `providerMetadata.kennmarq.images[i].markId` identifies each mark (for its receipt).
- To mark every image from a provider: `wrapProvider({ provider, languageModelMiddleware: [], imageModelMiddleware: kennmarqImageMiddleware({...}) })`.
- Add `deepfake: true` for realistic depictions of real people, places or events.

## Speech and video: wrap the model

```ts
import { experimental_generateVideo, generateSpeech } from "ai";
import { fal } from "@ai-sdk/fal";
import { kennmarqSpeechModel, kennmarqVideoModel } from "@kennmarq/ai-sdk";

const origin = { provider: "Acme Inc.", appName: "Acme Studio", appVersion: "1.0", markets: ["eu"] };

const { audio } = await generateSpeech({
  model: kennmarqSpeechModel(openai.speech("gpt-4o-mini-tts"), origin),
  text,
  outputFormat: "mp3", // MP3, WAV or M4A can be marked
});

const { video } = await experimental_generateVideo({
  model: kennmarqVideoModel(fal.video("luma-ray-2"), origin),
  prompt,
}); // MP4; marking a video takes a while, and generateVideo's time limit counts it
```

## If @kennmarq/ai-sdk can't be installed

Mark after generating with `@kennmarq/sdk` (see [javascript.md](javascript.md)):

```ts
const { image } = await generateImage({ model: openai.image("gpt-image-1"), prompt });
const marked = await kennmarq.mark(image.uint8Array, { provider, appName, appVersion, markets: ["eu", "us-ca"] });
return new Response(marked.data, { headers: { "Content-Type": marked.contentType } });
```

Errors throw (`KennmarqError`, `ContentBlocked`, `MissingApiKey`), so `generateImage` fails rather than return an
unmarked image. Guide: https://kennmarq.com/docs/guides/vercel-ai-sdk
