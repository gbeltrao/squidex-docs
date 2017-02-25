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
The squidex API partially supports the OData url convention to query data. 

We support the following url options.

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

