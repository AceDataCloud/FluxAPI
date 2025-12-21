# Flux Images Generation API Integration Instructions

This article will introduce the integration instructions for the Flux Images Generation API, which can generate official Flux images by inputting custom parameters.

## Application Process

To use the API, you need to first apply for the corresponding service on the [Flux Images Generation API](https://platform.acedata.cloud/documents/6b9197c5-7a3f-4878-a43f-7f94e7e66394) page. After entering the page, click the "Acquire" button, as shown in the image:

![](https://cdn.acedata.cloud/q6ytrc.png)

If you are not logged in or registered, you will be automatically redirected to the login page inviting you to register and log in. After logging in or registering, you will be automatically returned to the current page.

Upon your first application, there will be a free quota provided, allowing you to use the API for free.

## Basic Usage

First, understand the basic usage method, which involves inputting the prompt `prompt`, the action `action`, and the image size `size` to obtain the processed result. You first need to simply pass a field `action` with the value `generate`, and then we also need to input the prompt, as detailed below:

<p><img src="https://cdn.acedata.cloud/wz85jt.png" width="500" class="m-auto"></p>

Here we can see that we have set the Request Headers, including:

- `accept`: the format of the response result you want to receive, filled in as `application/json`, which means JSON format.
- `authorization`: the key to call the API, which can be directly selected after application.

Additionally, we set the Request Body, including:

- `action`: the action for this image generation task.
- `size`: the size of the generated image result.
- `count`: the number of images to generate, with a default value of 1; this parameter is only valid for image generation tasks and is invalid for editing tasks.
- `prompt`: the prompt.
- `model`: the generation model, defaulting to `flux-dev`.
- `callback_url`: the URL to receive the callback result.

After selection, you can see that the corresponding code is also generated on the right side, as shown in the image:

<p><img src="https://cdn.acedata.cloud/8q7aux.png" width="500" class="m-auto"></p>

Click the "Try" button to test, as shown in the image above, and we get the following result:

```json
{
  "success": true,
  "task_id": "226eb763-9eab-4d06-ad57-d59753a03307",
  "trace_id": "089f8b46-0167-4f25-88ee-3c3f88d80e84",
  "data": [
    {
      "prompt": "a white siamese cat",
      "image_url": "https://fal.media/files/lion/NVhtlwwGYQD6HrGaEfrzu_341484fad6d84b21b73f4f8824a3f98a.png",
      "timings": 1752743801
    },
    {
      "prompt": "a white siamese cat",
      "image_url": "https://fal.media/files/monkey/8UEQpFbQCYVOK1wKP3aV0_9bbc26fad64049b18d0244b99ef66ad1.png",
      "timings": 1752743801
    }
  ]
}
```

The returned result contains multiple fields, described as follows:

- `success`: the status of the image generation task at this time.
- `task_id`: the ID of the image generation task at this time.
- `trace_id`: the tracking ID of the image generation at this time.
- `data`: the result list of the image generation task at this time.
  - `image_url`: the link to the image generation task.
  - `prompt`: the prompt.

We can see that we have obtained satisfactory image information, and we only need to retrieve the generated Flux images based on the image link address in the `data` result.

Additionally, if you want to generate the corresponding integration code, you can directly copy the generated code, for example, the CURL code is as follows:

```shell
curl -X POST 'https://api.acedata.cloud/flux/images' \
-H 'authorization: Bearer {token}' \
-H 'accept: application/json' \
-H 'content-type: application/json' \
-d '{
  "action": "generate",
  "prompt": "a white siamese cat",
  "model": "flux-kontext-pro",
  "count": 2
}'
```

## Editing Image Tasks

If you want to edit a specific image, the parameter `image_url` must first be passed with the link to the image that needs to be edited. At this time, `action` only supports `edit`, and you can specify the following content:

- model: the model used for this image editing task, which currently supports `flux-kontext-max` and `flux-kontext-pro`.
- image_url: the uploaded image that needs to be edited.

An example of the input is as follows:

<p><img src="https://cdn.acedata.cloud/jn9da5.png" width="500" class="m-auto"></p>

After filling in, the code is automatically generated as follows:

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

Clicking run, you can find that you will immediately get a result, as follows:

```json
{
  "success": true,
  "task_id": "2a7979ff-1f77-4380-92c6-a2dc37c3b4c8",
  "trace_id": "732b65c0-48d9-49f7-b568-64e5acffe4c0",
  "data": [
    {
      "prompt": "a white siamese cat",
      "image_url": "https://fal.media/files/monkey/aEUXJZ6Faj9YXUCQVs01Q_af0cea56c558441c9ba8df67b200812d.png",
      "timings": 1752744073
    }
  ]
}
```

As you can see, the generated effect is the result of editing the original image, similar to the previous text.

## Asynchronous Callback

Since the time taken by the Flux Images Generation API is relatively long, approximately 1-2 minutes, if the API does not respond for a long time, the HTTP request will keep the connection open, leading to additional system resource consumption. Therefore, this API also provides support for asynchronous callbacks.

The overall process is: when the client initiates a request, an additional `callback_url` field is specified. After the client initiates the API request, the API will immediately return a result containing a `task_id` field information, representing the current task ID. When the task is completed, the result of the generated image will be sent to the client-specified `callback_url` in the form of a POST JSON, which also includes the `task_id` field, allowing the task result to be associated by ID.

Let’s understand how to operate specifically through an example.

First, the Webhook callback is a service that can receive HTTP requests, and developers should replace it with the URL of their own HTTP server. For demonstration purposes, we use a public Webhook sample site https://webhook.site/, where you can open the site to get a Webhook URL, as shown in the image:

![](https://cdn.acedata.cloud/cjjfly.png)

Copy this URL, and it can be used as a Webhook. The sample here is `https://webhook.site/3d32690d-6780-4187-a65c-870061e8c8ab`.
Next, we can set the field `callback_url` to the above Webhook URL, while filling in the corresponding parameters, as shown in the figure:

<p><img src="https://cdn.acedata.cloud/wm6caw.png" width="500" class="m-auto"></p>

Clicking run, you will find that a result is immediately obtained, as follows:

```
{
  "task_id": "6a97bf49-df50-4129-9e46-119aa9fca73c"
}
```

After a moment, we can observe the generated image result at `https://webhook.site/3d32690d-6780-4187-a65c-870061e8c8ab`, as shown in the figure:

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
      "image_url": "https://sf-maas-uat-prod.oss-cn-shanghai.aliyuncs.com/outputs/f4f8d407-377a-408a-82d0-427a5a836f09_0.png",
      "seed": 1698551532,
      "timings": {
        "inference": 3.328
      }
    }
  ]
}
```

It can be seen that the result contains a `task_id` field, and the other fields are similar to the above text, which allows for task association through this field.

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

Through this document, you have learned how to use the Flux Images Generation API to generate images by inputting prompts. We hope this document helps you better integrate and use the API. If you have any questions, please feel free to contact our technical support team.