# Flux Images Generation API Integration Guide

This article will introduce a Flux Images Generation API integration guide, which can generate official Flux images by entering custom parameters.

## Application Process

To use the Flux Images Generation API, first go to the [Ace Data Cloud Console](https://platform.acedata.cloud/console/applications) to obtain your API Token and keep it for later use.

![](https://cdn.acedata.cloud/dvc3cg.jpg)

If you have not logged in or registered yet, you will be automatically redirected to the login page and invited to register and log in. After completion, you will automatically return to the current page.

**One API Token can call all services on the platform; there is no need to apply separately for each service.** Your first application will include free credits for a free trial; when credits are insufficient, you can top up your general balance in the [Console](https://platform.acedata.cloud/console/coin).

> 📘 Full documentation: [Flux Images Generation API →](https://platform.acedata.cloud/documents/flux-images)

## Basic Usage

First, let's understand the basic usage method: enter the prompt `prompt`, generation action `action`, and image size `size`, and you can obtain the processed result. First, you need to simply pass an `action` field with the value `generate`, and then we also need to enter a prompt. The specific content is as follows:

<p><img src="https://cdn.acedata.cloud/wz85jt.png" width="500" class="m-auto"></p>

You can see that we set the Request Headers here, including:

- `accept`: The format of the response result you want to receive. Enter `application/json` here, which is JSON format.
- `authorization`: The key for calling the API. After applying, you can directly select it from the dropdown.

We also set the Request Body, including:

- `action`: The action for this image generation task.
- `size`: The size of the image generation result. **The `flux-2-flex` / `flux-2-pro` / `flux-2-max` series must pass an image aspect ratio (such as `1:1`, `16:9`), and do not accept pixel dimensions such as `1024x1024`; omitting it will return 400.**
- `count`: The number of images to generate. The default value is 1. This parameter is only valid for image generation tasks and is invalid for editing tasks.
- `prompt`: Prompt.
- `model`: Generation model, defaulting to `flux-dev`; the latest flagships are `flux-2-pro` and `flux-2-max` (higher image quality, requires image aspect ratio `size`).
- `callback_url`: The URL that needs to receive callback results.
- `async`: Optional. When set to `true`, the API immediately returns `task_id`; there is no need to provide `callback_url`, and results can then be obtained by polling through the corresponding task query API.

The `size` parameter has some special restrictions, mainly divided into two types: `width x height` aspect ratio and `x:y` image aspect ratio. The specifics are as follows:

| Model               | Range                                      |
| ------------------- | ------------------------------------------ |
| flux-dev            | Supports width-height ratios 1024x1024, 1024x1792, 1792x1024, or image aspect ratios |
| flux-pro            | Supports width-height ratios 1024x1024, 1024x1792, 1792x1024, or image aspect ratios |
| flux-2-flex         | Only supports image aspect ratios          |
| flux-2-pro          | Only supports image aspect ratios          |
| flux-2-max          | Only supports image aspect ratios          |
| flux-kontext-pro    | Only supports image aspect ratios          |
| flux-kontext-max    | Only supports image aspect ratios          |

Reference image aspect ratios: "21:9", "16:9", "4:3", "3:2", "1:1", "2:3", "3:4", "9:16", "9:21".

After selecting parameters, the corresponding code will be automatically generated on the right. Before copying, please confirm that the authorization header uses your own API Key, and real credentials should not appear in the documentation or screenshots.

Click the "Try" button to test. Here, we get the following result:

```json
{
  "success": true,
  "task_id": "5456c749-3bbb-4f10-9eb8-cfbcac297500",
  "trace_id": "ae4eecb8-1dd6-45b4-bfb3-a1c48872536e",
  "data": [
    {
      "image_url": "https://cdn.acedata.cloud/assets/examples/flux/5456c749-3bbb-4f10-9eb8-cfbcac297500-d0ef60485f73.jpg"
    }
  ]
}
```

The returned result contains multiple fields, described as follows:

- `success`, the status of the video generation task at this time.
- `task_id`, the ID of the video generation task at this time.
- `trace_id`, the tracking ID of the video generation at this time.
- `data`, the result list of the image generation task at this time.
  - `image_url`, the link for the image generation task at this time.
  - `prompt`, the prompt.

You can see that we obtained satisfactory image information. We only need to obtain the generated Flux image according to the image link address in `data` in the result.

Additionally, if you want to generate corresponding integration code, you can directly copy the generated code. For example, the CURL code is as follows:

```shell
curl -X POST 'https://api.acedata.cloud/flux/images' \
-H 'authorization: Bearer {token}' \
-H 'accept: application/json' \
-H 'content-type: application/json' \
-d '{
  "action": "generate",
  "prompt": "A photorealistic studio product shot of a frosted-glass perfume bottle on wet black slate, single softbox key light, water droplets, dark moody background, 85mm macro.",
  "model": "flux-2-pro",
  "size": "1:1"
}'
```

## Image Editing Task

If you want to edit an image, first, the `image_url` parameter must pass the link to the image that needs to be edited. At this time, `action` only supports `edit`, and you can specify the following content:

- model: The model used for this image editing task. Supports `flux-dev`, `flux-pro`, `flux-kontext-pro`, `flux-kontext-max`, `flux-2-flex`, `flux-2-pro`, and `flux-2-max`.
- image_url: Upload the image that needs to be edited.

The example is as follows:

<p><img src="https://cdn.acedata.cloud/jn9da5.png" width="500" class="m-auto"></p>

After filling it in, the following code is automatically generated:

<p><img src="https://cdn.acedata.cloud/6cwxb8.png" width="500" class="m-auto"></p>

The corresponding code:

```python
import requests

url = "https://api.acedata.cloud/flux/images"

headers = {
    "accept": "application/json",
    "authorization": "Bearer {token}",
    "content-type": "application/json"
}

payload = {
    "action": "edit",
    "prompt": "a white siamese cat",
    "model": "flux-kontext-pro",
    "image_url": "https://cdn.acedata.cloud/ytj2qy.png"
}

response = requests.post(url, json=payload, headers=headers)
print(response.text)
```

Click Run, and you can find that a result is obtained immediately, as follows:

```json
{
  "success": true,
  "task_id": "2a7979ff-1f77-4380-92c6-a2dc37c3b4c8",
  "trace_id": "732b65c0-48d9-49f7-b568-64e5acffe4c0",
  "data": [
    {
      "prompt": "a white siamese cat",
      "image_url": "https://cdn.acedata.cloud/e724d7f13d.png",
      "timings": 1752744073
    }
  ]
}
```
As can be seen, the generated result is the effect of editing the original image, and the result is similar to the above.

## Asynchronous Callback

Since the Flux Images Generation API takes relatively long to generate, approximately 1–2 minutes, if the API does not respond for a long time, the HTTP request will remain connected, resulting in additional system resource consumption. Therefore, this API also provides support for asynchronous callbacks.

The overall process is: when the client initiates a request, it additionally specifies a `callback_url` field. After the client initiates the API request, the API will immediately return a result containing a `task_id` field, representing the current task ID. When the task is completed, the result of the generated image will be sent in POST JSON format to the `callback_url` specified by the client, which also includes the `task_id` field, so that task results can be associated through the ID.

Next, we will use an example to understand how to operate it specifically.

First, a Webhook callback is a service that can receive HTTP requests. Developers should replace it with the URL of their own deployed HTTP server. For convenience of demonstration, a public Webhook sample website https://webhook.site/ is used here. Opening this website will provide a Webhook URL, as shown in the figure:

![](https://cdn.acedata.cloud/cjjfly.png)

Copy this URL, and it can be used as a Webhook. The example here is `https://webhook.site/3d32690d-6780-4187-a65c-870061e8c8ab`.

Next, we can set the field `callback_url` to the above Webhook URL and fill in the corresponding parameters at the same time. The specific content is shown in the figure:

<p><img src="https://cdn.acedata.cloud/wm6caw.png" width="500" class="m-auto"></p>

Click Run, and it can be found that a result is immediately obtained, as follows:

```
{
  "task_id": "6a97bf49-df50-4129-9e46-119aa9fca73c"
}
```

After waiting for a moment, we can observe the result of the generated image at `https://webhook.site/3d32690d-6780-4187-a65c-870061e8c8ab`, as shown in the figure:

![](https://cdn.acedata.cloud/v23lot.png)

The content is as follows:

```json
{
  "success": true,
  "task_id": "6a97bf49-df50-4129-9e46-119aa9fca73c",
  "trace_id": "9b4b1ff3-90f2-470f-b082-1061ec2948cc",
  "data": [
    {
      "prompt": "a white siamese cat",
      "image_url": "https://cdn.acedata.cloud/e724d7f13d.png",
      "seed": 1698551532,
      "timings": {
        "inference": 3.328
      }
    }
  ]
}
```

It can be seen that there is a `task_id` field in the result, and the other fields are similar to those above. Task association can be achieved through this field.

## Error Handling

When calling the API, if an error occurs, the API will return the corresponding error code and message. For example:

- `400 token_mismatched`: Bad request, possibly due to missing or invalid parameters.
- `400 api_not_implemented`: Bad request, possibly due to missing or invalid parameters.
- `401 invalid_token`: Unauthorized, invalid or missing authorization token.
- `429 too_many_requests`: Too many requests, you have exceeded the rate limit.
- `500 api_error`: Internal server error, something went wrong on the server.

### Error Response Example

```json
{
  "success": false,
  "error": {
    "code": "api_error",
    "message": "fetch failed"
  },
  "trace_id": "2cf86e86-22a4-46e1-ac2f-032c0f2a4e89"
}
```

## Conclusion

Through this document, you have learned how to use the Flux Images Generation API to generate images by entering prompts. We hope this document can help you better integrate and use this API. If you have any questions, please feel free to contact our technical support team.