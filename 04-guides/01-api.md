# API

We demonstrate the API concepts based on the following example:

## Example

Lets assume you have an app `geodata` with two languages (en, de) and a schema `cities` with two fields:

1. `name`: String (Localizable)
2. `population`: Number (Not Localizable)

Then your content has the following structure in the API:

    { 
        "id": "01",
        "created": "2017-02-25T19:56:35Z",
        "createdBy": "...",
        "lastModified": "2017-02-25T19:56:35Z",
        "lastModifiedBy": "...",
        "data": {
            "name": {
                "de": "München",
                "en": "Munich"
            },
            "population": {
                "sv": 1400000
            }
        }
    }


## General structure
The data of the content to be created or updated.
            
Please note that each field is an object with one entry per language. 
If the field is not localizable we use `iv` (Invariant Language) as a key.
When you change the field to be localizable the value will become the value for the master language, depending what the master language is at this point of time.

## OData Conventions
The squidex API supports the OData url convention to query data. 

We support the following query options.

### $top

The `$top` query option requests the number of items in the queried collection to be included in the result. The default value is 20 and the maximum allowed value is 200.

    https://cloud.squidex.io/api/content/geodata/cities?$top=30

### $skip

The `$skip` query option requests the number of items in the queried collection that are to be skipped and not included in the result. Use it together with $top to read the all your data page by page.

    https://cloud.squidex.io/api/content/geodata/cities?$skip=20

or combined with top

    https://cloud.squidex.io/api/content/geodata/cities?$skip=20&$top=30

### $search

The $search query option allows clients to request entities matching a free-text search expression. We add the data of all fields for all languages to a single field in the database and use this combined field to implement the full text search.

    https://cloud.squidex.io/api/content/geodata/cities?$search=Munich

> Note: You can either use `$search` or `$filter` but not both.

### $filter

The $filter system query option allows clients to filter a collection of resources that are addressed by a request URL.

Find the city with the english name Munich

    https://cloud.squidex.io/api/content/geodata/cities?$filter=data/name/de eq Munich

Find all cities with a population or more than 100000 people

    https://cloud.squidex.io/api/content/geodata/cities?$filter=data/population/iv gt 100000

> Note: You can either use `$search` or `$filter` but not both.

### $orderby

The $orderby system query option allows clients to request resources in a particular order.

e.g. find the top 20 biggest cities by population:

    https://cloud.squidex.io/api/content/geodata/cities?$orderby=data/population/iv desc$top=20

Read more about OData at: http://docs.oasis-open.org/odata/odata/v4.0/odata-v4.0-part2-url-conventions.html

## Consistency

The API uses the eventual consistency. Events are handled in the background as described under [architecture](../02-architecture/01-overview.md). This means that it can take to up a second until the data is available to the query side. Under very high load it can even take more time. If you receive a success status code when you create or update an content you have the guarantee, that it has been written to the database successfully. You can also make another write operation directly, e.g. to publish the content.

### Versioning

The API tracks the version of each content element and provides this information in the `ETag` content header if you make an update (POST, PUT, PATCH) or if you request a single resource. If you request multiple resources, the version is provided as a field to each entry.

You can use this header for two use cases:

1. When you make an update you get the new version. This information can be used to find out if your change has already been written to the read store when you receive the same resource after your update.

2. When you make an update you can use the `If-Match` header to pass the expected version to the API. If the version does not match to the version in the database another user or client has changed the same resource. Then a `412 (Precondition Failed)` status code is returned. You should provide this information to the user and ask if the user wants to reload the data or if the resource should be overwritten (just do not use the `If-Match` header for the second request).

Read more about the If-Match header at: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Match
