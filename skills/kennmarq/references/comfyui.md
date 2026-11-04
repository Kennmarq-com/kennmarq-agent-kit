# ComfyUI

Kennmarq.com's nodes replace the save nodes: what they save is marked.

```bash
comfy node install comfyui-kennmarq
```

Or search "Kennmarq.com" in ComfyUI-Manager. Start ComfyUI with `KENNMARQ_API_KEY` in its environment: the key is
never a widget, because widgets are saved in the workflow and the workflow travels inside shared PNGs.

| Instead of | Use | Class name (API workflows) |
| --- | --- | --- |
| Save Image | Kennmarq.com Mark & Save Image | `KennmarqMarkAndSaveImage` |
| Save Audio | Kennmarq.com Mark & Save Audio | `KennmarqMarkAndSaveAudio` |
| Save Video | Kennmarq.com Mark & Save Video | `KennmarqMarkAndSaveVideo` |

- Mark & Save Image: inputs `images`, `filename_prefix`, `format` (png, jpeg, webp), `source` (`auto` reads the graph:
  a Load Image node means `composite`), `safety_check`, `keep_workflow`; optional `provider`, `model`, `app_name`,
  `app_version`. Images are marked on the machine; only the manifest (about 13 KB) goes to Kennmarq.com to be signed.
- Audio and video are marked in Kennmarq.com's cloud, so those nodes need the key.
- Without a key, Mark & Save Image still writes the watermark and the IPTC AI label, with no signed Content
  Credentials.
- Two example workflows (text to image, image to image) come with the nodes: Workflow → Browse Templates →
  comfyui-kennmarq.
- Apps that drive ComfyUI through its API (`workflow_api.json`): replace each `SaveImage` node with one that lists
  every required input:

  ```json
  {"class_type": "KennmarqMarkAndSaveImage", "inputs": {"images": ["8", 0], "filename_prefix": "Kennmarq", "format": "png",
   "source": "auto", "safety_check": "off", "keep_workflow": true, "provider": "Acme Inc.", "app_name": "Acme Studio"}}
  ```

Docs: https://kennmarq.com/docs/comfyui
