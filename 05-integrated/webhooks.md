Webhooks are endpoints where Squidex will send events for a given schema to:

* **Created**: The content item has been created.
* **Published**: The content item has been updated.
* **Unpublished**: The content item has been unpublished.
* **Updated**: The content item has been saved.
* **Deleted**: The content item has been deleted.

The secret is used to sign the requests for security reasons. Each request will contain a `X-Signature` header which is calculated the following way: 

    Sha256(RequestBody + Secret)

You can calculate the signature when you receive an event to check if the request has been sent by Squidex. Do **not share** the secret.

Squidex will make several attempts to send an event:

1. After a few seconds.
2. After 5 minutes
3. After 1 hour.
4. After 6 hours.
6. After 12 hours.

An request will be treated as failed if the response does not return a 2XX status code or when it times out after 2 seconds. Consider internal queues for slow operations.

Events expire after 2 days and will be deleted automatically.
