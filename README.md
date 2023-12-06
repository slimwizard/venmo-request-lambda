# venmo-request-lambda
AWS Lambda function for sending Venmo requests; recurring requests can be scheduled using EventBridge Scheduler or a similar cron-like trigger mechanism. Leverages the **venmo-api** [pip package](https://pypi.org/project/venmo-api/).

## Calling the Function
Function expects the following arguments within the request body:

```
{
  "request_amount": 1000,
  "request_message": "example note",
  "request_recipient": "12345678910"
}
```

## Getting a Venmo User's ID
You must pass the recipient's Venmo user ID and *not* the recipient's username as the request_recipient value. The user ID cannot be found within the app and must be pulled via the Venmo API. The following code can be used to grab a user ID for a particular username (assumes venmo-api is installed; install via 'pip install venmo-api).

```
from venmo_api import Client

access_token = Client.get_access_token(username='<your Venmo username>', password='your Venmo password')
client = Client(access_token=access_token)
user_id = client.user.search_for_users("Madisonmoore35")[0].id
print(user_id)
```
## Authenticating the Function against the Venmo API
Function expects your Venmo access token to be set as an environment variable named "ACCESS_TOKEN". Your access token can be pulled via the following code:

```
from venmo_api import Client

access_token = Client.get_access_token(username='<your Venmo username>', password='your Venmo password')
print(access_token)
```

Note that access tokens NEVER expire and must be explicitly revoked. Instructions on how to delete an access token can be found in the venmo-api [documentation](https://venmo.readthedocs.io/en/latest/).


