# Localization

Each field in Squidex can be localized. 

## Basic concept

We have implemented an abstract concepts which we call partitioning. It means that the values for an field can be associated to a list of other values, for example languages.

It is easy to understand when you have a look to an content object from the API:

```json
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
            "iv": 1400000
        }
    }
}
```

Each partition has a list of allowed keys where one key is called the master key. You have to define a value for the master key, if the field is set to required. Other can be marked as optional, which means that you do not have to define a value, even if the field is required.

* The `population` field uses the Invariant-Partitioning. Which only allows a single key `iv`. This key is also the master key.
* The `name` field uses the Language-Partitioning. The allowed keys are defined by the language configuration for your app.

## Why so complicated?

When we implemented the localization feature we realized that it might be very helpful to extend this feature to other type of keys, for example you could...

* ... define your prices for different currencies.
* ... write your texts for different countries.
* ... define customer groups.

So we implemented the localization feature with the idea in mind that we might extend it in coming versions. 