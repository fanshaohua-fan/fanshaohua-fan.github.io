---
layout: post
title:  "Understanding JSON Schema"
categories: json
tags:  json jsonschema
---

[JSON Schema](https://json-schema.org/understanding-json-schema/) is a powerful tool for validating the structure of JSON data.

## What is JSON schema

To define what JSON Schema is, we should probably first define what JSON is.

JSON stands for “JavaScript Object Notation”, a simple data interchange format. JSON is built on the following data structures:

|JS|Python3|JSON|
|---|---|---|
|string|string|"Hello World"|
|number|int / float|2 / 3.14|
|boolean|bool|true false|
|null|None|null|
|object|dict|{"key": "value"}|
|array|list|["ABC", 3.14, false]|

With these simple data types, all kinds of structured data can be represented. For example, you could imagine representing information about a person in JSON in different ways:

```Json
{
  "name": "George Washington",
  "birthday": "February 22, 1732",
  "address": "Mount Vernon, Virginia, United States"
}

{
  "first_name": "George",
  "last_name": "Washington",
  "birthday": "1732-02-22",
  "address": {
    "street_address": "3200 Mount Vernon Memorial Highway",
    "city": "Mount Vernon",
    "state": "Virginia",
    "country": "United States"
  }
}
```

However, when an application says “give me a JSON record for a person”, it’s important to know exactly how that record should be organized. For example, we need to know what fields are expected, and how the values are represented. That’s where JSON Schema comes in. The following JSON Schema fragment describes how the second example above is structured.

```json
{
  "type": "object",
  "properties": {
    "first_name": { "type": "string" },
    "last_name": { "type": "string" },
    "birthday": { "type": "string", "format": "date" },
    "address": {
      "type": "object",
      "properties": {
        "street_address": { "type": "string" },
        "city": { "type": "string" },
        "state": { "type": "string" },
        "country": { "type" : "string" }
      }
    }
  }
}
```

## [The Basics](https://json-schema.org/understanding-json-schema/basics.html)

### Hello, World

When learning any new language, it’s often helpful to start with the simplest thing possible. In JSON Schema, an empty object is a completely valid schema that will accept any valid JSON.

```json
// This accepts anything, as long as it’s valid JSON
{}
```

### The type keyword

The most common thing to do in a JSON Schema is to restrict to a specific type. The type keyword is used for that.

```json
// For example, in the following, only strings are accepted:
{ "type": "string" }
```

The type keyword is described in more detail in [Type-specific keywords](https://json-schema.org/understanding-json-schema/reference/type.html).

- string
  - Length : minLength, maxLength
  - Regular Expressions : pattern
  - Format : date-time, date, time, email, hostname, ipv4, ipv6, uri, json-pointer, regex
- Numeric types
  - integer
  - number
  - multipleOf
  - Range : minimum, maximum, exclusiveMinimum, exclusiveMaximum
- object
  - properties
  - additionalProperties : is used to control the handling of extra stuff, may be either a boolean or an object.
  - required
  - propertyNames
  - Size : minProperties, maxProperties
  - dependencies
  - patternProperties
- array
  - List validation : items, contains
  - Tuple validation : additionalItems
  - Length : minItems, maxItems
  - uniqueItems
- boolean
- null

The **type** keyword may either be a string or an array:

1. If it’s a string, it is the name of one of the basic types above.
2. If it is an array, it must be an array of strings, where each string is the name of one of the basic types, and each element is unique. In this case, the JSON snippet is valid if it matches any of the given types.

> { "type": ["number", "string"] }

### Declaring a JSON Schema

Since JSON Schema is itself JSON, it’s not always easy to tell when something is JSON Schema or just an arbitrary chunk of JSON. The **$schema** keyword is used to declare that something is JSON Schema. It’s generally good practice to include it, though it is not required.

```json
{ "$schema": "http://json-schema.org/schema#" }
```

### Declaring a unique identifier

It is also best practice to include an **$id** property as a unique identifier for each schema. For now, just set it to a URL at a domain you control, for example:

```json
{ "$id": "http://yourdomain.com/schemas/myschema.json" }
```
