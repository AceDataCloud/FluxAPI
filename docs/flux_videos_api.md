# Flux Videos API Integration Guide

The Flux Videos API uses `POST /flux/videos` to complete video generation, keyframe image-to-video, video continuation, and draft enhancement. `action` distinguishes generation, editing, and upscaling, while `mode` selects the generation mode; query results uniformly use the existing `POST /flux/tasks`.

> Currently in Beta. Text-to-video, image-to-video, video continuation, and draft enhancement are available. As tested on 2026-10-02, video editing and upscaling return `service_unavailable`; these two are temporarily unavailable, and failed tasks do not deduct generation fees. HTTP 200 and a task ID only indicate that the task has been accepted; you must continue querying for the final result.

## 1. Obtain an API Token

1. Register or log in at the [Ace Data Cloud Console](https://platform.acedata.cloud/console/applications), create an application, and obtain an API Token. A general API Token can call platform services; please confirm that the application has permission to call the Flux service and has available balance.
2. View plans and prices for each operation on the [Flux service page](https://platform.acedata.cloud/services/flux?tab=pricing). When the balance is insufficient, recharge on the [console balance page](https://platform.acedata.cloud/console/coin).
3. Requests use `Authorization: Bearer <your Token>`. The Token should be stored in server-side environment variables; do not write it into frontend pages, public repositories, screenshots, or callback URLs.

![Apply for an API Token in the console](https://cdn.acedata.cloud/dvc3cg.jpg)

The code in this article uniformly reads environment variables:

```bash
export ACEDATACLOUD_API_TOKEN='替换为你自己的 API Token'
```

| Item      | Value                                           |
| ------- | ----------------------------------------------- |
| API base URL | `https://api.acedata.cloud`                     |
| Submit video task  | `POST /flux/videos`                             |
| Query existing task  | `POST /flux/tasks`                              |
| Authentication      | `Authorization: Bearer $ACEDATACLOUD_API_TOKEN` |
| Request format    | `Content-Type: application/json`                |

For complete fields and online debugging, see [Flux Videos API](https://platform.acedata.cloud/documents/flux-videos); for task queries, see [Flux Tasks API](https://platform.acedata.cloud/documents/flux-tasks).

## 2. Select an Operation and Input

| action                      | mode            | Input                       | Result            |
| --------------------------- | --------------- | ------------------------ | ------------- |
| `generate` (default when action is omitted) | `t2v`           | `prompt`                 | Generate video from text       |
| `generate`                  | `i2v`           | `prompt`, `keyframes`     | Generate video from one or more keyframes |
| `generate`                  | `v2v`           | `prompt`, `start_video`   | Continue based on the input video      |
| `generate`                  | `draft_enhance` | `draft_task_id` of a draft you have completed | Enhance that draft         |
| `edit`                      | Not provided              | `video`, `prompt`         | Video editing (temporarily unavailable)    |
| `upscale`                   | Not provided              | `input_video`            | Video upscaling (temporarily unavailable)    |

The generation model is `flux-3`. Editing and upscaling use their respective fields; do not pass generation parameters such as `model`, `mode`, `duration`, or `draft`. `action=upsale` is a spelling mistake and should be `upscale`.

### Common Generation Parameters

| Parameter                 | Description                                                           |
| ------------------ | ------------------------------------------------------------ |
| `mode`             | Required; `t2v`, `i2v`, `v2v`, or `draft_enhance`                       |
| `prompt`           | Required for normal generation; draft enhancement does not allow overriding the original prompt                                        |
| `duration`         | An integer of 5–20 seconds for t2v/i2v, an integer of 5–15 seconds for v2v, or `auto`; the final output duration may differ slightly from the requested value |
| `resolution`       | `hd`, `fhd`, `qhd`, `uhd`; the default for normal generation is `hd`, and the default for draft enhancement is `fhd`              |
| `aspect_ratio`     | `auto`, `21:9`, `2:1`, `16:9`, `4:3`, `1:1`, `3:4`, `9:16`, `9:21`   |
| `draft`            | Whether to generate a draft first; drafts only support `hd`                                           |
| `generate_audio`   | Whether to generate synchronized audio; `false` is a valid value, so retain the explicit boolean value                               |
| `safety_tolerance` | Optional; an integer from 0–4                                                   |
| `async`            | Recommended to set to `true`, immediately returning a platform task ID and then polling for results                                |
| `callback_url`     | Optional; an HTTP(S) address that receives the final JSON result; it will also be accepted asynchronously when set                        |

Asset URLs must be accessible by the service. If using temporary signed URLs, reserve sufficient validity time for downloading and processing. Do not treat webpage URLs as image or video file URLs.

## 3. Text-to-Video: Complete Tested Request and Result

The following request was successfully executed on the production API before the price adjustment on 2026-10-02. Omitting `action` verified the default generation behavior; `async=true` avoids waiting for the HTTP connection for a long time.

```bash
curl -X POST 'https://api.acedata.cloud/flux/videos' \
  -H "Authorization: Bearer $ACEDATACLOUD_API_TOKEN" \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
    "model": "flux-3",
    "mode": "t2v",
    "prompt": "A small toy sailboat floating on calm blue water in warm morning light, steady camera, no text.",
    "duration": 5,
    "resolution": "hd",
    "draft": true,
    "async": true
  }'
```

Acceptance response (real task ID):

```json
{
  "task_id": "4341eb66-3845-4972-bb86-712a6cfae845",
  "trace_id": "b0f851bd-0925-43ca-acc3-6f634c70d607"
}
```

Save the `task_id` from your own response and continue querying; do not use the task ID in the documentation example to query results from other accounts.

```bash
curl -X POST 'https://api.acedata.cloud/flux/tasks' \
  -H "Authorization: Bearer $ACEDATACLOUD_API_TOKEN" \
  -H 'Content-Type: application/json' \
  -d '{"action":"retrieve","id":"替换为本次返回的 task_id"}'
```

The `response` field returned by the task query contains the final business result. The following is the successfully tested `response` from this run, with outer task metadata omitted. The video URL in the documentation has been replaced with a copy of the same file on a long-term example CDN (SHA-256 consistent); actual calls will return the result URL of your own task:
```json
{
  "success": true,
  "task_id": "4341eb66-3845-4972-bb86-712a6cfae845",
  "trace_id": "b0f851bd-0925-43ca-acc3-6f634c70d607",
  "data": [
    {
      "id": "4341eb66-3845-4972-bb86-712a6cfae845",
      "model": "flux-3",
      "video_url": "https://cdn.acedata.cloud/assets/examples/flux/4341eb66-3845-4972-bb86-712a6cfae845-5f4391e1942c.mp4",
      "seconds": 5.041667,
      "width": 1280,
      "height": 704,
      "fps": 24,
      "draft_task_id": "4341eb66-3845-4972-bb86-712a6cfae845"
    }
  ],
  "usage": {
    "action": "generate",
    "seconds": 5.041667,
    "output_mp_seconds": 4.3326825781250005,
    "mode": "t2v",
    "resolution": "hd",
    "draft": true
  },
  "cost": {
    "amount": 2.6884689277500002,
    "currency": "credit",
    "list_amount": 2.9871876975000005
  }
}
```

[View the video from this test](https://cdn.acedata.cloud/assets/examples/flux/4341eb66-3845-4972-bb86-712a6cfae845-5f4391e1942c.mp4). Media inspection confirms that the output is an MP4 at 1280×704, 24 fps, and 5.041667 seconds, with a file size of 2,607,276 bytes.

| Response field                              | Purpose                                       |
| --------------------------------- | ---------------------------------------- |
| `success`                         | Whether the final task succeeded; returning task_id during the acceptance stage does not equal success=true |
| `task_id`                         | Platform task ID, used for polling and business idempotency handling                      |
| `trace_id`                        | Provide to support personnel when troubleshooting                             |
| `data[].video_url`                | Readable video result URL                               |
| `data[].seconds/width/height/fps` | Actual output duration, dimensions, and frame rate measured by the server                       |
| `data[].draft_task_id`            | Input ID for draft enhancement; appears only when a reusable draft is returned                  |
| `usage`                           | Final billable usage; do not use the requested duration to overwrite actual seconds      |
| `cost.amount`                     | Final Credits deducted for this request; `currency=credit` is not USD   |
| `cost.list_amount`                | Credits before application user consumption discounts; returned when applicable                 |

This is a historical test before the price adjustment: `list_amount=2.9871876975` Credits, and the account had a 10% consumption discount at the time, with an actual `amount=2.68846892775` Credits. The new price on 2026-10-02 has been reduced by approximately 6.33%; the same 5.041667-second draft costs 2.798125185 Credits at the current price (before consumption discounts), or 2.5183126665 Credits if the 10% consumption discount still applies. Historical task bills are not recalculated. Plans and discounts for other accounts may differ; this is not a fixed USD price for all users.

## 4. Image-to-Video: Standard and Timed Keyframes

The following are parameter examples. You need to replace the asset URLs; they are not a declaration that the example has already been successfully executed. After generation is complete, query according to the process above; the result structure is the same.

Use a standard array for one or two images:

```json
{
  "action": "generate",
  "model": "flux-3",
  "mode": "i2v",
  "prompt": "The camera slowly moves around the product in soft studio light.",
  "keyframes": ["https://example.com/first-frame.jpg"],
  "duration": 5,
  "resolution": "hd",
  "generate_audio": false,
  "async": true
}
```

When specifying keyframe times, use `[seconds, image URL]` pairs:

```json
{
  "action": "generate",
  "model": "flux-3",
  "mode": "i2v",
  "prompt": "A smooth transition from morning light to a warm sunset.",
  "keyframes": [[0, "https://example.com/start.jpg"], [5, "https://example.com/end.jpg"]],
  "duration": 5,
  "resolution": "hd",
  "async": true
}
```

1–10 keyframes are allowed. Timed arrays must be in ascending time order, with times from 0–20 seconds; standard URLs and timed items cannot be mixed. Three or more standard keyframes must explicitly specify duration; auto cannot be used.

### Image-to-Video Test Output

The matching input for this test is as follows (only descriptive text is used in place of the complete base64; the remaining fields are the actual request):

```json
{
  "async": true,
  "action": "generate",
  "model": "flux-3",
  "prompt": "The toy sailboat drifts slowly across calm water. A steady camera, gentle daylight, no people or text.",
  "duration": 5,
  "resolution": "hd",
  "generate_audio": false,
  "draft": false,
  "mode": "i2v",
  "keyframes": [
    "<下图 PNG 文件的原始 base64 字符串>"
  ]
}
```

![Reference keyframe for this image-to-video test](https://cdn.acedata.cloud/assets/examples/flux/b1106de8-d586-4e23-b489-381e2f86a10f-input-b0525db595d2.png)

After [downloading this PNG keyframe](https://cdn.acedata.cloud/assets/examples/flux/b1106de8-d586-4e23-b489-381e2f86a10f-input-b0525db595d2.png), use Python's `base64.b64encode(image_bytes).decode("ascii")` to obtain the original string and place it in the keyframes array. Do not use the descriptive text in the document as image input.

The following is the real final response from a production task on 2026-10-01 (not a simulated response); only the video URL has been replaced with a long-term example copy with the same hash. The test input used the original base64 string of a 1280×720 PNG as a single keyframe; the URL input above is an independent parameter example.

```json
{
  "success": true,
  "task_id": "b1106de8-d586-4e23-b489-381e2f86a10f",
  "trace_id": "f62d5c56-cfb2-4f8f-b339-ef019d4c0d10",
  "data": [
    {
      "id": "b1106de8-d586-4e23-b489-381e2f86a10f",
      "model": "flux-3",
      "video_url": "https://cdn.acedata.cloud/assets/examples/flux/b1106de8-d586-4e23-b489-381e2f86a10f-bef14458cc9d.mp4",
      "seconds": 5.041667,
      "width": 1280,
      "height": 704,
      "fps": 24
    }
  ],
  "usage": {
    "action": "generate",
    "seconds": 5.041667,
    "output_mp_seconds": 4.3326825781250005,
    "mode": "i2v",
    "resolution": "hd",
    "draft": false
  },
  "cost": {
    "amount": 7.617328628625,
    "currency": "credit",
    "list_amount": 8.46369847625
  }
}
```
[View live test video](https://cdn.acedata.cloud/assets/examples/flux/b1106de8-d586-4e23-b489-381e2f86a10f-bef14458cc9d.mp4)。

## 5. Video Continuation

Pass an existing video file URL in `start_video`, use `mode=v2v`, with a maximum duration of 15 seconds. It is a different field from the `video` used for editing.

```json
{
  "action": "generate",
  "model": "flux-3",
  "mode": "v2v",
  "prompt": "Continue the sailboat drifting forward with the same steady camera.",
  "start_video": "https://example.com/source.mp4",
  "duration": 5,
  "resolution": "hd",
  "async": true
}
```

### Live Test Output for Video Continuation

The complete live test input for this run is as follows; when reproducing draft enhancement, you must replace it with your own draft ID. The material URL uses a long-term example copy of the same file:

```json
{
  "action": "generate",
  "model": "flux-3",
  "mode": "v2v",
  "start_video": "https://cdn.acedata.cloud/assets/examples/flux/b41293be-94c0-4dc7-9f39-ce04f0a8798d-c055a3079059.mp4",
  "prompt": "Continue the same sailboat drifting gently across calm water in the same continuous steady shot.",
  "duration": 5,
  "resolution": "hd",
  "generate_audio": false,
  "async": true
}
```

The following is the real final response from a production task on 2026-10-01 (not a simulated response); only the video URL has been replaced with a long-term example copy with the same hash.

```json
{
  "success": true,
  "task_id": "8af9aa42-7e4d-47a4-bd61-144d97440c91",
  "trace_id": "4fb49afc-abca-4039-bbaa-adb66e293544",
  "data": [
    {
      "id": "8af9aa42-7e4d-47a4-bd61-144d97440c91",
      "model": "flux-3",
      "video_url": "https://cdn.acedata.cloud/assets/examples/flux/8af9aa42-7e4d-47a4-bd61-144d97440c91-a512d6574811.mp4",
      "seconds": 5,
      "width": 1280,
      "height": 704,
      "fps": 24
    }
  ],
  "usage": {
    "action": "generate",
    "seconds": 5,
    "output_mp_seconds": 4.296875,
    "mode": "v2v",
    "resolution": "hd",
    "draft": false
  },
  "cost": {
    "amount": 18.219375,
    "currency": "credit",
    "list_amount": 20.24375
  }
}
```

[View live test video](https://cdn.acedata.cloud/assets/examples/flux/8af9aa42-7e4d-47a4-bd61-144d97440c91-a512d6574811.mp4)。

The live test input `start_video` is the [completed draft video](https://cdn.acedata.cloud/assets/examples/flux/b41293be-94c0-4dc7-9f39-ce04f0a8798d-c055a3079059.mp4), and the remaining parameters are duration=5, resolution=hd, and generate_audio=false.

## 6. Draft First, Then Enhance

1. Generate a draft with `draft=true` and `resolution=hd`, and wait for success.
2. Obtain the platform draft ID from the final `data[0].draft_task_id`.
3. Submit an enhancement request using application credentials under the same ownership:

```json
{
  "action": "generate",
  "model": "flux-3",
  "mode": "draft_enhance",
  "draft_task_id": "替换为自己的已完成草稿 ID",
  "resolution": "fhd",
  "async": true
}
```

Draft enhancement cannot pass in `prompt`, `duration`, `aspect_ratio`, `version`, `generate_audio`, `draft`, `keyframes`, or `start_video` to override the original content. Draft cache is a temporary resource, so please enhance it promptly; permanent storage or a fixed retention period is not guaranteed. Drafts not belonging to you/not belonging to the current application, incomplete drafts, and expired caches cannot be reused. Drafting and enhancement are two separate tasks and are billed separately after success.

### Live Test Output for Draft Enhancement

The complete live test input for this run is as follows; when reproducing draft enhancement, you must replace it with your own draft ID. The material URL uses a long-term example copy of the same file:

```json
{
  "action": "generate",
  "model": "flux-3",
  "mode": "draft_enhance",
  "draft_task_id": "b41293be-94c0-4dc7-9f39-ce04f0a8798d",
  "resolution": "hd",
  "async": true
}
```

The following is the real final response from a production task on 2026-10-01 (not a simulated response); only the video URL has been replaced with a long-term example copy with the same hash.

```json
{
  "success": true,
  "task_id": "1db6243b-4a05-4484-828c-25bb17de7108",
  "trace_id": "64f93fa1-8d18-4c8b-a6d2-888009eb7cfa",
  "data": [
    {
      "id": "1db6243b-4a05-4484-828c-25bb17de7108",
      "model": "flux-3",
      "video_url": "https://cdn.acedata.cloud/assets/examples/flux/1db6243b-4a05-4484-828c-25bb17de7108-058d92166607.mp4",
      "seconds": 5.041667,
      "width": 1280,
      "height": 704,
      "fps": 24
    }
  ],
  "usage": {
    "action": "generate",
    "seconds": 5.041667,
    "output_mp_seconds": 4.3326825781250005,
    "mode": "t2v",
    "resolution": "hd",
    "draft": false
  },
  "cost": {
    "amount": 7.617328628625,
    "currency": "credit",
    "list_amount": 8.46369847625
  }
}
```

[View live test video](https://cdn.acedata.cloud/assets/examples/flux/1db6243b-4a05-4484-828c-25bb17de7108-058d92166607.mp4)。

The live test input uses your own draft_task_id=b41293be-94c0-4dc7-9f39-ce04f0a8798d and resolution=hd; the final usage.mode=t2v indicates the original draft mode. This task is billed separately from the original draft.

## 7. Python End-to-End Call

Install `requests`, set your own Token, and run the script below to complete “submit once → poll → output video URL”. Both querying and network retries should use the original task_id to avoid repeatedly submitting billable tasks.
```python
import os
import time
import requests

base_url = "https://api.acedata.cloud"
headers = {
    "Authorization": "Bearer " + os.environ["ACEDATACLOUD_API_TOKEN"],
    "Accept": "application/json",
}
payload = {
    "action": "generate",
    "model": "flux-3",
    "mode": "t2v",
    "prompt": "A small toy sailboat floating on calm blue water in warm morning light.",
    "duration": 5,
    "resolution": "hd",
    "draft": True,
    "async": True,
}
submitted = requests.post(base_url + "/flux/videos", json=payload, headers=headers, timeout=120)
submitted.raise_for_status()
accepted = submitted.json()
if accepted.get("error"):
    raise RuntimeError(accepted["error"])
task_id = accepted["task_id"]
print("Task ID:", task_id)  # 持久化保存，用于恢复轮询

# 30 分钟是本示例的客户端等待上限，不是服务的完成时限承诺。
deadline = time.monotonic() + 30 * 60
while time.monotonic() < deadline:
    polled = requests.post(
        base_url + "/flux/tasks",
        json={"action": "retrieve", "id": task_id},
        headers=headers,
        timeout=30,
    )
    polled.raise_for_status()
    task = polled.json()
    if task.get("error"):
        raise RuntimeError(task["error"])
    result = task.get("response") or task.get("result")
    if isinstance(result, dict) and result.get("success") is True:
        print("Video:", result["data"][0]["video_url"])
        print("Usage:", result.get("usage"))
        print("Cost:", result.get("cost"))
        break
    if isinstance(result, dict) and result.get("error"):
        raise RuntimeError(result["error"])
    time.sleep(10)
else:
    raise TimeoutError("Still processing; resume polling with task_id=" + task_id)
```

After a network timeout, do not treat an unknown status as a failure and immediately submit again. If a `task_id` has already been obtained, continue querying that task; record the `task_id` and `trace_id` for troubleshooting. The polling API itself does not charge generation fees.

## 8. Using callbacks

Add `callback_url` when submitting. After the task is completed, the final JSON result will be POSTed to that address. The successful structure is consistent with the preceding response; failures include error.

```json
{
  "action": "generate",
  "model": "flux-3",
  "mode": "t2v",
  "prompt": "A small sailboat on calm water.",
  "duration": 5,
  "resolution": "hd",
  "draft": true,
  "callback_url": "https://your-server.example/flux-callback"
}
```

The callback address should be publicly accessible. After receiving a notification, process it idempotently by `task_id` and return 2xx as soon as possible; business processing can be queued. This document does not declare that callbacks have signature authentication: before performing sensitive actions such as granting business entitlements, use your own Token to query the same task and verify the result. If the callback is not received, you can also continue polling; do not generate again.

## 9. Video editing and upscaling (currently unavailable)

These two operations are still selected through the action of `/flux/videos`. Only the parameter contract is shown below, and successful delivery should not currently be relied upon.

Editing:

```json
{
  "action": "edit",
  "video": "https://example.com/source.mp4",
  "prompt": "Change the scene to warm sunset lighting.",
  "async": true
}
```

Upscaling:

```json
{
  "action": "upscale",
  "input_video": "https://example.com/source.mp4",
  "upscale_factor": 1.5,
  "creativity": 0,
  "async": true
}
```

`upscale_factor` is 1.5–3; `creativity=0` is precise upscaling, and `1` is creative upscaling. It is not a string, and do not omit an explicit 0. The editing prompt length is 1–4096 characters. Do not mix in generation fields.

On 2026-10-02, editing test tasks returned the following final response after being accepted, with billing verification showing a charge of 0; upscaling tests also returned the same unavailable error, with a charge of 0:

```json
{
  "success": false,
  "error": {
    "code": "service_unavailable",
    "message": "The video operation is currently unavailable"
  },
  "trace_id": "7374a332-097a-431f-8207-ecee73dbb740",
  "task_id": "67671cbf-2464-42e1-aa03-afadeb0eabc3"
}
```

## 10. Current billing and pricing table

Updated on 2026-10-02: the unit prices for all tiers of the video API in this update were reduced by approximately 6.33%; the measurement method, plans, and consumption discount rules remain unchanged. The cost in the historical tested responses above is the bill at task completion and does not represent the current quotation.

Generation and editing are billed by **actual output seconds**; upscaling is billed by **actual output MP·seconds**. Below are the current Credits unit prices before applying account consumption discounts, consistent with the rules on the [Flux pricing page](https://platform.acedata.cloud/services/flux?tab=pricing).

| Operation/mode | Resolution | Credits unit price |
| ----------------------- | ---- | ------------: |
| t2v / i2v draft | hd | 0.555 / second |
| v2v draft | hd | 1.11 / second |
| t2v / i2v / corresponding draft enhancement | hd | 1.5725 / second |
| Same as above | fhd | 2.6825 / second |
| Same as above | qhd | 3.7 / second |
| Same as above | uhd | 7.4 / second |
| v2v / corresponding draft enhancement | hd | 3.7925 / second |
| Same as above | fhd | 4.9025 / second |
| Same as above | qhd | 6.0125 / second |
| Same as above | uhd | 8.7875 / second |
| Editing (currently unavailable) | — | 0.2775 / second |
| Precise upscaling creativity=0 (currently unavailable) | Actual output | 0.6475 / MP·second |
| Creative upscaling creativity=1 (currently unavailable) | Actual output | 0.925 / MP·second |

Upscaling usage formula: `seconds × width × height / 1048576 × fps / 24`, where 1 MP = 1024×1024 pixels. It is not billed simply based on input video duration or target multiplier.

USD conversion: `actual cost (USD) = cost.amount (Credits) × plan price / plan amount`. Recharge tiers and consumption discounts affect the actual price; Credits cannot be directly treated as USD. Failed tasks do not incur generation fees; the final amount is subject to the completion result and console call records.

## 11. Frequently asked questions and troubleshooting
| Situation | Recommendation |
| ------------------------ | -------------------------------------------- |
| Parameter error (400) | Check the action spelling, mode, duration, and keyframe format; do not mix fields from different operations |
| Authentication error (401) | Check whether the Bearer Token, application permissions, and credentials are valid |
| Content moderation rejection (403) | Adjust the materials and prompts; do not repeatedly submit them unchanged |
| Rate limit (429) | Reduce concurrency and retry with backoff |
| service_unavailable (503) | The current operation is unavailable; accepted asynchronous tasks may report this error in the final response |
| task_id has been returned but there is no video yet | Continue querying the response; do not treat HTTP 200 as generation completion |
| Draft cannot be reused | Confirm the same user/current application, that the task has been completed, that the original request had draft=true, and check whether the temporary cache is still valid |

When providing feedback, include the task_id, trace_id, request time, and redacted parameters; do not send the API Token. For more methods, see the [Flux MCP Integration Guide](https://platform.acedata.cloud/documents/flux-mcp).