# Overview

Data contracts define the schema (structure) of data an application will store on Dash Platform. Contracts are described using [JSON Schema](https://json-schema.org/understanding-json-schema/) which allows the platform to validate the contract-related data submitted to it.

The following sections provide details that developers need to construct valid contracts: [documents](#section-documents) and [definitions](#section-definitions). All data contracts must define one or more documents, whereas definitions are optional and may not be used for simple contracts.

# General Constraints
[block:callout]
{
  "type": "warning",
  "body": "There are a variety of constraints currently defined for performance and security reasons. The following constraints are applicable to all aspects of data contracts.",
  "title": "Constraints"
}
[/block]
## Keyword

| Keyword | Constraint |
| - | - |
| `default` | Restricted - cannot be used |
| `propertyNames` | Restricted - cannot be used |
| `uniqueItems: true` | `maxItems` must be defined (maximum: 100000) |
| `pattern: <something>` | `maxLength` must be defined (maximum: 50000) |
| `format: <something>` | `maxLength` must be defined (maximum: 100000) |
| `$ref: <something>` | `$ref` can only reference `definitions` - <br> remote references not supported |

## Data Size
Additionally, there are several constraints limiting the overall size of data contracts and related data as defined here:

| Description | Constraint |
| - | - |
| Maximum size of serialized data contract | 15 KB |
| Maximum size of CBOR-encoded data | 16 KB |

# Documents
The `documents` object defines each type of document required by the data contract. At a minimum, a document must consist of 1 or more properties. Documents may also define [indices](#section-document-indices) and a list of [required properties](#section-required-properties).

The following example shows a minimal `documents` object defining a single document (`note`) that has one property (`message`).
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"note\": {\n    \"properties\": {\n      \"message\": {\n        \"type\": \"string\"\n      }\n    },\n    \"additionalProperties\": \"false\"\n  }\n}",
      "language": "json",
      "name": "Minimal document definition"
    }
  ]
}
[/block]
## Document Properties

The `properties` object defines each field that will be used by a document. Each field consists of an object that, at a minimum, must define its data `type` (`string`, `number`, `integer`, `boolean`, `array`, `object`). Fields may also apply a variety of optional JSON Schema constraints related to the format, range, length, etc. of the data.
[block:callout]
{
  "type": "info",
  "title": "JSON Schema",
  "body": "A full explanation of the capabilities of JSON Schema is beyond the scope of this document. For more information regarding its data types and the constraints that can be applied, please refer to the [JSON Schema reference](https://json-schema.org/understanding-json-schema/reference/index.html) documentation."
}
[/block]
### Property Constraints
There are a variety of constraints currently defined for performance and security reasons. 

| Description | Value |
| - | - |
| Minimum number of properties | 1 |
| Maximum number of properties | 100 |
| Minimum property name length | 1 |
| Maximum property name length | 63 |
| Property name first/last characters | ** Alphanumeric only (`A-Z`, `a-z`, `0-9`)**|
| Property name characters | Alphanumeric (`A-Z`, `a-z`, `0-9`)<br>Hypen (`-`) <br>Underscore (`_`) |

### Required Properties

Each document may have some fields that are required for the document to be valid and other fields that are optional. Required fields are defined via the `required` array which consists of a list of the field names from the document that must be present.
[block:code]
{
  "codes": [
    {
      "code": "\"required\": [\n  \"<field name a>\",\n  \"<field name b>\"\n]",
      "language": "json",
      "name": "Syntax"
    }
  ]
}
[/block]

**Example**
The following example (excerpt from the DPNS contract's `domain` document) demonstrates a document that has 6 required fields:
[block:code]
{
  "codes": [
    {
      "code": "\"required\": [\n  \"nameHash\",\n  \"label\",\n  \"normalizedLabel\",\n  \"normalizedParentDomainName\",\n  \"preorderSalt\",\n  \"records\"\n],",
      "language": "json"
    }
  ]
}
[/block]
## Document Indices
[block:callout]
{
  "type": "info",
  "body": "The `indices` object should be excluded for documents that do not require indices."
}
[/block]
Document indices may be defined if indexing on document fields is required. 

The `indices` array consists of:
 - One or more objects that each contain:
  - A `properties` array composed of a `<field name: sort order>` object for each document field that is part of the index (sort order: `asc` or `desc`)
  - An (optional) `unique` element that determines if duplicate values are allowed for the document
[block:code]
{
  "codes": [
    {
      "code": "\"indices\": [ \n  {\n    \"properties\": [\n      { \"<field name a>\": \"<asc\"|\"desc>\" },\n      { \"<field name b>\": \"<asc\"|\"desc>\" }\n    ], \n    unique: true|false\n  },\n  {\n    \"properties\": [\n      { \"<field name c>\": \"<asc\"|\"desc>\" },\n    ], \n  }    \n]",
      "language": "json",
      "name": "Syntax"
    }
  ]
}
[/block]
### Index Constraints
[block:callout]
{
  "type": "warning",
  "body": "For performance and security reasons, Evonet places the following constraints on indices. These constraints are subject to change over time."
}
[/block]
| Description | Value |
| - | - |
| Maximum number of indices | 10 |
| Maximum number of unique indices | 3 |
| Maximum number of properties in a single index | 10 |

**Example**
The following example (excerpt from the DPNS contract's `preorder` document) creates an index on `saltedDomainHash` that also enforces uniqueness across all documents of that type:
[block:code]
{
  "codes": [
    {
      "code": "\"indices\": [\n  {\n    \"properties\": [\n      { \"saltedDomainHash\": \"asc\" }\n    ],\n    \"unique\": true\n  }\n],",
      "language": "json",
      "name": "Example indices"
    }
  ]
}
[/block]
## Full Document Syntax
This example syntax shows the structure of a documents object that defines two documents, an index, and a required field.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"<document name a>\": {\n    \"properties\": {\n      \"<field name b>\": {\n        \"type\": \"<field data type>\"\n      },\n      \"<field name c>\": {\n        \"type\": \"<field data type>\"\n      },\n    },\n    \"indices\": [\n      {\n        \"properties\": [\n          {\n            \"<field name c>\": \"<asc|desc>\"\n          }\n        ],\n        \"unique\": true|false\n      },\n    ],\n    \"required\": [\n      \"<field name c>\"\n    ]\n    \"additionalProperties\": false\n  },\n  \"<document name x>\": {\n    \"properties\": {\n      \"<property name y>\": {\n        \"type\": \"<property data type>\"\n      },\n      \"<property name z>\": {\n        \"type\": \"<property data type>\"\n      },\n    },\n    \"additionalProperties\": false\n  },    \n}",
      "language": "json"
    }
  ]
}
[/block]
# Definitions
The optional `definitions` object enables definition of aspects of a schema that are used in multiple places. This is done using the JSON Schema support for [reuse](https://json-schema.org/understanding-json-schema/structuring.html#reuse). Items defined in `definitions` may then be referenced when defining `documents` through use of the `$ref` keyword.
[block:callout]
{
  "type": "info",
  "body": "Properties defined in the `definitions` object must meet the same criteria as those defined in the `documents` object."
}
[/block]

[block:callout]
{
  "type": "danger",
  "title": "Remote references blocked",
  "body": "Data contracts can only use the `$ref` keyword to reference their own `definitions`. Referencing external definitions is not supported by the platform protocol."
}
[/block]
**Example**
The following example shows a definition for a `message` object consisting of two properties:
[block:code]
{
  "codes": [
    {
      "code": "{\n  // Preceeding content truncated ...\n  \"definitions\": {\n    \"message\": {\n      \"type\": \"object\",\n      \"properties\": {\n        \"timestamp\": {\n          \"type\": \"number\"\n        },\n        \"description\": {\n          \"type\": \"string\"\n        }\n      },\n      \"additionalProperties\": false\n    }\n  }\n}",
      "language": "json",
      "name": "Example definitions"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "In the `js-dpp` reference implementation, definitions are added to a data contract via the `.setDefinitions()` method (e.g. `myContract.setDefinitions({\"message\": { ... }})`. This must be done prior to broadcasting the contract for registration.",
  "title": "Adding definitions with js-dpp"
}
[/block]
# Additional Properties
Although JSON Schema allows additional, undefined properties [by default](https://json-schema.org/understanding-json-schema/reference/object.html?#properties), they are not allowed in Dash Platform data contracts. Data contract validation will fail if they are not explicitly forbidden using the `additionalProperties` keyword anywhere `properties` are defined.

Include the following at the same level as the `properties` keyword to ensure proper validation:
```json
"additionalProperties": false
```