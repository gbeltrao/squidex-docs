# Schemas

A schema defines the structure of the content.

You have to publish your schema before you can create content.

## Field States

Each field has multiple states:

1. **Locked**: The field cannot be updated or deleted.
2. **Hidden**: The field will not be returned by the api and only visible in the management portal.
3. **Disabled**: The field cannot be manipulated in the management portal. Do not use it together with the required operator.

## Field Types

Field types define how a field is structured in the API and in the processing pipeline. You can define the editor for each field, so a string field can either be a html text, markdown or a list of allowed values with a dropdown editor. We use a product catalog as an example to describe the different field types.

### String

![String](../images/fields/string.png)

A string is the most used field type and can be used for any kind of texts, like product names, descriptions and additional information.

#### API representation

```json
{
    "name": "Squidex T-Shirt",
    "description": "For our fans. <p>Available in <strong>multiple colors ..."
}
```

### Number

![Number](../images/fields/number.png)

A number can either be a point number or integer. Typical examples when to use numbers are quantities, IDs and prices.

#### API representation

```json
{
    "quantity": 100,
    "price": 9.99
}
```

### Boolean

![Boolean](../images/fields/boolean.png)

Booleans have only 2 states: True or false, yes or no, 1 or 0.

#### API representation

```json
{
    "isSoldOut": true,
    "isOffer": false
}
```

### DateTime

![DateTime](../images/fields/datetime.png)

Date and time in the ISO8601 standard. The format is: `YYYY-MM-DDTHH:mm:ss.sssZ`.

#### API representation

```json
{
    "sellUntil": "2020-02-02T12:00:00Z"
}
```

### Assets

![Assets](../images/fields/assets.png)

Asset fields have references to assets.

#### API representation

```json
{
    "images": [
        "7722daf6-1ba7-4b2a-a5bb-fc57e22f5645",
        "b666b172-9918-4764-ac26-300ba4857d5f"
    ]
}
```

### References & Array

![References](../images/fields/references.png)

References fields are used to model relationship to other content items. For example you could have a schema for products and a schema for product categories. A product has a field with references to the categories it belongs. Both, products and categories can be created, updated and managed independently.

#### API representation

```json
{
    "categories": [
        "7722daf6-1ba7-4b2a-a5bb-fc57e22f5645",
        "b666b172-9918-4764-ac26-300ba4857d5f"
    ]
}
```

### Array

![Arrays](../images/fields/array.png)

Some content items only exist as child content for another content item. For example a product could have variations like different sizes and prices. These content items can be represented with array fields, where each item in the field has a specified structured, that is called `nested schema`. 

#### API representation

```json
{
    "sizes": [{
        "size": "XL",
        "quantity": 100,
        "price": 30
    }, {
        "size": "L",
        "quantity": 100,
        "price": 28.5
    }]
}
```

### Geolocation

![Geolocation](../images/fields/geolocation.png)

```json
{
    "location": {
        "latitude": 14.9212444,
        "longitude": 57.21212
    }
}
```

### Tags

![Tags](../images/fields/tags.png)

#### API representation

```json
{
    "tags": [
        "t-shirts",
        "fan-products"
    ]
}
```

### Json

![Json](../images/fields/json.png)

#### API representation

```json
{
    "sold": {
        "Europe": {
            "Germany": {
                "quantity": 100,
                "averagePrice": 15.44
            }
        }
    }
}
```