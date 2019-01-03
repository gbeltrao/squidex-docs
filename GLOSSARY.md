## App
An app is the place for all your content. You can organize your project in one or multiple apps, depending on your requirements and organization.

## Client
A client is another application that consumes or creates the content. The Management UI is just another client, that uses the same API.

## Content
Everything you want to store in Squidex. Blog posts, articles, products, users, payment options, settings, feature toggles...

## Event
An event is something that happened in the past and contains only the bare minimum of information to describe it. 

## Event Sourcing
Event sourcing is an approach to thinking about persistent data where the primary record is a log of all events that make updates. A traditional representation of database state can be entirely recreated by reprocessing this event log. Event sourcing's benefits include strong auditing, creation of historic state, and replaying of events for debugging and analysis. Event sourcing has been around for a while, but we think it is used much less than it should be.

## Partitioning
Defines the structure of each field.

## Schema
A schema defines the structure of your content elements. It holds a list of fields with unique names, the data type for each field and valdiation rules.

## Swagger
Swagger is a powerful open source framework backed by a large ecosystem of tools that helps you design, build, document, and consume your RESTful APIs. Squidex provides a swagger specifications for the management api and the api that serves the content for you app.

From: http://swagger.io/