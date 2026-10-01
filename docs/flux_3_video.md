# FLUX Video Integration Guide

Submit tasks via `POST /flux/videos`, using `action` to select generation, editing, or upscaling. Omitting `action` is equivalent to `generate`. Use the existing `POST /flux/tasks` to query platform tasks. Supports `async=true` and `callback_url`; completed results include the platform video link and server-measured duration, dimensions, and frame rate.

## Generation: action=generate

Supports `mode=t2v/i2v/v2v/draft_enhance`. Regular generation supports integer durations of 5–20 seconds or `auto`; video continuation supports 5–15 seconds. i2v can provide 1–10 keyframes and supports ordered `[seconds, image]` pairs. Output resolutions are hd/fhd/qhd/uhd, with synchronized audio supported. Drafts only output hd and return the platform `draft_task_id`; reusing a draft requires the same ownership and is subject to the generation cache validity period. Draft reproduction cannot override the original prompt and assets.

```json
{"action":"generate","model":"flux-3","mode":"t2v","prompt":"Sunset by the sea","duration":5,"resolution":"hd","async":true}
```

## Editing: action=edit

Pass in `video` and `prompt`. Editing is billed according to the actual output duration in seconds and does not accept generation fields such as `mode` and `duration`.

```json
{"action":"edit","video":"https://example.com/video.mp4","prompt":"Change the scene to blue tones","async":true}
```

## Upscaling: action=upscale

Pass in `input_video`, with optional `upscale_factor` (1.5–3) and `creativity` (0 for precise, 1 for creative). Billed according to actual output MP·seconds: `duration × width × height / 1048576 × frame rate / 24`, where 1 MP = 1024×1024. Generation parameters are not accepted.

```json
{"action":"upscale","input_video":"https://example.com/video.mp4","upscale_factor":2,"creativity":0,"async":true}
```

Each operation uses the current FLUX reference pricing coefficients and is billed according to actual output. Failed tasks are not charged generation fees; the final cost of dynamically billed tasks depends on the completed result. Different operations only accept their respective parameters, and an unknown action returns a parameter error.