Rules allow you to react to events in your app and to synchronize contents and assets with other systems.

A rule has two elements:

1. **Trigger**: Define when a rule is executed, for example when a content is created.

2. **Action**: Defines what will be executed when the rule is triggered.

Almost all text settings for actions support placeholder. At the moment the following placehold are supported:

* `$APP_ID`: The id of your app (guid).
* `$APP_NAME`: The name of your app.
* `$TIMESTAMP_DATE`: The date when the event has happened (usually different from the time when the rule is executed) in the following format: `yyyy-MM-dd`.
* `$TIMESTAMP_DATETIME`; The date when the event has happened (usually different from the time when the rule is executed) in the following format: `yyyy-MM-dd-hh-mm-ss`.

For *ContentChangedTrigger*:

* `$SCHEMA_ID`: The id of the schema.
* `$SCHEMA_NAME`: The name of the schema.
* `$CONTENT_ACTION`: The content event (created, updated, deleted).