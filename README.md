![Squidex Logo](images/logo-wide.png "Squidex")

# What is Squidex?

> A content management platform for all kind of content.

Squidex is a content management platform to manege all your content, this might be:

* Content for your mobile apps
* Content for your website, for example blog posts and articles
* Configuration data for your backend
* Rich content for your product

## How does it work?

The core of squidex is a web service. It provides API's to manage the structure of your content, languages configuration and settings and of course the content itself. You can consume these content from your backend, mobile app, website and other other client application. Of course we also provide a rich user interface for content management.

## Event Sourcing

We use event sourcing to store the all the data and configuration. Instead of just holding the actual state we generate events, whenever you make a change. For example when you update a content we create a ContentUpdatedEvent that holds the changed data. We never delete any data, when you delete a content element we just create a ContentDeletedEvent. This means you cannot loose any data. These events are also handled by the event consumers which are responsible to create an optimized representation of your content which can be queried through the API.

## Custom content representation

We think it is impossible to develop a system that is able to handle every kind of queries in a fast and efficient way. You know best what technology you need for your business case. Think about sql database servers. You need to configure indices by yourself, because creating indices automatically is a very hard problem. If you need your content in another representation or in another storage you can subscribe to the content event and push your content to another database, whenever it has changed. For example: If you build a travel portal you can manage your hotels in squidex. To allow your users to search for hotels you can push the data to elastic search and use the full text search features from elastic.


