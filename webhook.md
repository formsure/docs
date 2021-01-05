# Webhook

Where do you want your form data to be sent? Get every submission sent straight to a compatible web app or URL as soon as it’s submitted with Webhooks.

## What is a Webhook? <!-- {docsify-ignore} -->

In general terms, a Webhook is simply a notification sent over the web, which is triggered automatically whenever a specific event occurs.

In this case, the event is a new form submission. Whenever a new form submission comes in, a notification containing the response data is immediately sent to your chosen destination — the Webhook URL which you set in the configuration page.

Webhook notifications are sent via HTTP `POST` request, and the request body (containing the response data) is in `JSON` format.

## Setup guide

If you need to make a test URL – to collect test submissions – you can do this at https://requestbin.net.

1. Open Workflows page of your form
2. Click Add a webhook:
3. Enter a Destination URL. (This is where we will make HTTP POST requests to):
4. Now click Save webhook, and you’ll be taken back to the webhooks tab. By default your new webhook will be set to OFF until you turn it on by clicking the toggle.

## Example POST Data

```json
POST https://your-webhook.com/form-submission HTTP/1.1
Content-Type: application/json
User-Agent: Formsure-Webhook/1.0
X-Formsure-Payload-Signature: fffe04bc29239452feb4aad9255dbde6cb20810a

{
  "formName": "Workflow test",
  "submission": [
    {
      "question": "What is your name",
      "answer": "Workflow"
    },
    {
      "question": "Your message",
      "answer": "Workflow Workflow Workflow"
    }
  ],
  "meta": {
    "formId": "5ff28998c4b65982ead3xxxx",
    "formSlug": "FXsPbpxxxx",
    "createdAt": "2021-01-05T06:03:39.504Z",
    "eventId": "KqCbeRgKCexxxx"
  }
}
```

## Webhooks signature

You will get a header named `X-Formsure-Payload-Signature` with [`HMAC`](https://en.wikipedia.org/wiki/HMAC) signed version of your payload with the `Secret` you have given in the webhook add form in the Formsure dashboard.

### Verifying signature

Nodejs example code for verifying the request payload is as follows

```javascript
'use strict'
const crypto = require('crypto')

let payloadSig = crypto
    .createHmac('sha1', WEBHOOK_SECRET)
    .update(REQUEST.DATA)
    .digest('hex')
let isValidSig =
    payloadSig === REQUEST.HEADERS['x-formsure-payload-signature']
        ? 'Valid'
        : 'Not valid'

console.log(isValidSig)
```

## Failure behaviour

We will retry a Webhook maximum of 3 times, if the request end up a non `2xx` response from your server.
