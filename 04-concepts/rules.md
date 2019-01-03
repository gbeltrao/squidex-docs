# Rules

A rule is a system to react to events.

## Concept

Whenevery you make a change in Squidex, such as creating content or updating settings, an event is created. An event describes what happened in the past and has a unique name, for example `ContentPublished`.

A rule has two parts:

1. A `trigger` defines when to execute the rule.
2. An `action` defines what to do.

For example

```
            Trigger               Action
When [Content is created] then [Send Email]
```

## Workflow

To execute a rule the following steps are excuted:

1. An event has been recorded.
2. All rules for the associated app are retrieved.
3. **Enrichment**: The event is enriched with additional information.
4. **Matching**: The matching rules are determined by comparing the trigger with the enriched event.
5. **Formatting**: An rule job is created and stored. A contains all information to execute the rule for the current event.
6. The rule job is queried from the storage and executed.
7. If the execution is successful it will be marked as succeeded. 
8. **Retry**: Otherwise it will be marked for a retry at a later point of time.

### Enrichment

Events contain only the bare minimum as information. For the `ContentPublished` we only need the id of the app and schema and the id of the content. In addition to that we also store metadata, such as the timestamp and the id of the user who created the content. This is common for all events. But for the matching process we need additional information, for example the content itself, so that we can check based on the data of the content item, whether we want to execute the rule.

The enriched events have the following structure:

#### Content Events

```json
{
    "id": "123...", // Id of the content.
    "actor": { "type": "subject", "id": "123..." }, // Id of the user
    "appId": { "name": "my-app", "id": "123..." }, // App name and id
    "created": "2018-01-01T12:00:00Z",
    "createdBy":  { "type": "subject", "id": "123..." },
    "data": { // Content data
        "city": {
            "en": "Munich",
            "de": "München"
        },
        "population": {
            "iv": 123000
        }
    },
    "lastModified": "2018-01-01T12:00:00Z",
    "lastModifiedBy": { "type": "subject", "id": "123..." },
    "schemaId": { "name": "my-schema", "id": "123..." }, // Schema id
    "status": "Draft", // Status of the content: Draft, Archived, Published
    "timestamp": "2018-01-01T12:00:00Z",
    "type": "Created", // The type of the event.
    "user": { // The user information.
        "id": "123...",
        "name": "John Doe",
        "email": "john@email.com"
    },
    "version": 1 // Version of the content, increased with any operation
}
```

#### Asset Events

```json
{
    "id": "123...", // Id of the asset
    "actor": { "type": "subject", "id": "123..." }, // Id of the user
    "appId": { "name": "my-app", "id": "123..." }, // App name and id
    "created": "2018-01-01T12:00:00Z",
    "createdBy": "subject:123",
    "fileName": "Avatar.png",
    "fileSize:": 512000,
    "fileVersion": 1,
    "isImage": true,
    "lastModified": "2018-01-01T12:00:00Z",
    "lastModifiedBy": { "type": "subject", "id": "123..." },
    "mimeType": "image/png",
    "pixelHeight": 600,
    "pixelWidth": 800,
    "timestamp": "2018-01-01T12:00:00Z",
    "type": "Created", // The type of the event.
    "user": { // User information
        "id": "123...",
        "name": "John Doe",
        "email": "john@email.com"
    },
    "version": 1 // Version of the asset, increased with any operation
}
```

It is important to understand the structure because you can reference fields when you define when a rule should be executed. Some actions juse the some structure to pass over the event to other systems: For example, the webhook action adds the event to the request body in (almost) the same format.

### Matching

In the matching process we check whether the action should be executed. There are several conditions:

1. The event type must be correct:
    * A rule with a `AssetChangedTrigger` can only handle asset events.
    * A rule with a `ContentChangedTrigger` can only handle content events.
2. If a condition is defined it must evaluate to true.

A condition is a javascript expression that must return `true` to execute the rule.

Here are some examples to demonstrate it:

Specific asset events:

    event.type == 'Created' || event.type == 'Updated'

Large assets only:

    event.fileSize > 100000000

Images only:

    event.isImage

Of course it can be more complex if necessary.

### Formatting

COMING SOON

### Retry

Squidex will make several attempts to execute an job:

1. First attempt, a few seconds after the event has happened.
2. After 5 minutes
3. After 1 hour.
4. After 6 hours.
6. After 12 hours.

Jobs expire after 2 days and will be deleted automatically.

