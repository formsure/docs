# Dynamic Links

With dynamic links you can create personalised Form links which you can send to a person or use it in a particular palce to track the reponce seperately. Dynamic links can be created from your Workflow page or via API.

Make sure you have read our [API Quickstart](/dev) and obtained your keys and learned to authenticate with formsure via http requests.

## Creating a dynamic link

Dynamic link can be created against `name`, `email`, `phone` or `uniqId`. Either one of these parameter is required to create a dynamic link. You should make your api calls to `/dynamiclink` prepending `$apiEndpoint` via a `POST` request.

An example `cURL` request will looks like the following.

```bash
curl --request POST \
  --url $apiEndpoint/dynamiclink \
  --header 'Content-Type: application/json' \
  --header 'x-formsure-formtoken: $x-formsure-formtoken' \
  --header 'x-fromsure-accesskey: $x-fromsure-accesskey' \
  --data '{"uniqId": "1001"}'
```

### On Success

If your call is successful you get reponse like the following

```json
{
    "response": {
        "statusCode": 200,
        "message": "Success",
        "error": false,
        "version": "0.1.x"
    },
    "data": {
        "link": "https://app.formsure.co/d/r8nFykbfCIZY"
    }
}
```

Where you can access the generated link by calling the `data.link` from the response object. The generated url will looks like `https://app.formsure.co/d/<12-LETTER-DYNAMICLINK>`

### On Error

On error you will get `422` for parameter error or `400` for bad authentication credentials.

#### Bad Credentials

If you provided any bad credentials in the headers or headers are missing you will see an error like the following.

```json
{
    "response": {
        "statusCode": 400,
        "message": "Unauthorized",
        "error": true,
        "version": "0.1.x"
    },
    "data": []
}
```

Possible reasons are,

-   Authentication headers are not present
-   Authentication header values are wrong

#### Bad paramters

If any of your paramters are wrong you will curresponding error messages like the following.

```json
{
    "response": {
        "statusCode": 422,
        "message": "Email should match format \"email\"",
        "error": true,
        "version": "0.1.x"
    },
    "data": []
}
```

In the above example the callee made a request to create a dynamic link against an `email` by providing a non-email string as input, may be `not-an-email.com` or `not-with[at].com`.