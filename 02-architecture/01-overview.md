# Architecture

## Overviews

Squidex is implemented based on the CQRS Pattern:

> CQRS stands for Command Query Responsibility Segregation. It's a pattern that I first heard described by Greg Young. At its heart is the notion that you can use a different model to update information than the model you use to read information.
> 
> From: [Martin Fownler](https://martinfowler.com/bliki/CQRS.html)

For the write side we use event sourcing:

> Event sourcing is an approach to thinking about persistent data where the primary record is a log of all events that make updates. A traditional representation of database state can be entirely recreated by reprocessing this event log. Event sourcing's benefits include strong auditing, creation of historic state, and replaying of events for debugging and analysis. Event sourcing has been around for a while, but we think it is used much less than it should be.

![Architecture Diagram](../images/architecture.png "Architecture")

Although this approach adds a lot of complexity to the system it also has a lot of advantages:

1. You have a history of all your changes.
2. You can consume the events to create your custom storages, e.g. you can use the Elastic Search Stack for full text search or statistics.
3. We don't have to care about data migration when we change the read models with a new version; you just have to run the event consumers from the beginning to populate the read store with the updated data.

Updating the read stores is asynchronous, but usually it is fast enough and we expect that the delay is short enough to even recognize it.

## Frameworks and Tools

Squidex is based on the following frameworks and tools:

* ASP.NET Core for 
* Angular for the Management UI
* MongoDB for the Event Store and the Read Models
* Redis as PubSub system (Only needed if you run Squidex in a Cluster)



