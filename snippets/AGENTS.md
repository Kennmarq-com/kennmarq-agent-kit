## AI content marking (Kennmarq.com)

- Every image, video or audio file this app generates with AI is marked before it's stored or shown: send it to POST https://api.kennmarq.com/v1/mark and keep only the file that comes back.
- Call the API from server code only, with the key in the KENNMARQ_API_KEY environment variable. The key never reaches a browser or a mobile app.
- Send provider (our legal name), app_name and app_version (this app), source (created, composite or edited) and markets with the ISO codes of the countries the app serves (eu, us, jp…; see https://kennmarq.com/docs/laws).
- Deep fakes (realistic depictions of real people, places or events) also get deepfake=true, which draws the visible "AI" label into the picture or every frame of the video. label=true adds it to any picture or video.
- If marking fails, the request fails: never show or store an unmarked file.
- Files over 4 MB, and every video, go up first through POST https://api.kennmarq.com/v1/uploads. Videos are marked as jobs: 202, then GET the mark's url until it's completed.
- Docs: https://kennmarq.com/llms.txt; all of them in one file: https://kennmarq.com/llms-full.txt.
