# Data Contract Overview

Data contracts define the schema (structure) of data an application will store on Dash Platform. Contracts are described using [JSON Schema](https://json-schema.org/understanding-json-schema/) which allows the platform to validate the contract-related data submitted to it.

The following sections provide details that developers need to construct valid contracts: [documents](#data-contract-documents) and [definitions](#data-contract-definitions). All data contracts must define one or more documents, whereas definitions are optional and may not be used for simple contracts.

# General Constraints

**Note:** There are a variety of constraints currently defined for performance and security reasons. The following constraints are applicable to all aspects of data contracts. Unless otherwise noted, these constraints are defined in the platform's JSON Schema rules (e.g. [js-dpp data contract meta schema](https://github.com/dashevo/js-dpp/blob/v0.21.0/schema/dataContract/dataContractMeta.json)).

## Keyword
[block:callout]
{
  "type": "warning",
  "title": "Dash Platform Protocol 0.20",
  "body": "Updating to the JSON Schema 2012-12 specification replaced the `definitions` keyword with the `defs` keyword."
}
[/block]
| Keyword | Constraint |
| - | - |
| `default` | Restricted - cannot be used (defined in DPP logic) |
| `propertyNames` | Restricted - cannot be used (defined in DPP logic) |
| `uniqueItems: true` | `maxItems` must be defined (maximum: 100000) |
| `pattern: <something>` | `maxLength` must be defined (maximum: 50000) |
| `format: <something>` | `maxLength` must be defined (maximum: 50000) |
| `$ref: <something>` | `$ref` can only reference `$defs` - <br> remote references not supported |
| `if`, `then`, `else`, `allOf`, `anyOf`, `oneOf`, `not` | Disabled for data contracts |

## Data Size

**Note:** These constraints are defined in the Dash Platform Protocol logic (not in JSON Schema).

All serialized data (including state transitions) is limited to a maximum size of [16 KB](https://github.com/dashevo/js-dpp/blob/v0.21.0/lib/util/serializer.js#L5).

## Additional Properties

Although JSON Schema allows additional, undefined properties [by default](https://json-schema.org/understanding-json-schema/reference/object.html?#properties), they are not allowed in Dash Platform data contracts. Data contract validation will fail if they are not explicitly forbidden using the `additionalProperties` keyword anywhere `properties` are defined (including within document properties of type `object`).

Include the following at the same level as the `properties` keyword to ensure proper validation:

```json
"additionalProperties": false
```

# Data Contract Object

The data contract object consists of the following fields as defined in the JavaScript reference client ([js-dpp](https://github.com/dashevo/js-dpp/blob/v0.21.0/schema/dataContract/dataContractMeta.json)):

| Property | Type | Required | Description |
| - | - | - | - |
| protocolVersion | integer | Yes | The platform protocol version ([currently `1`](https://github.com/dashevo/js-dpp/blob/v0.21.0/lib/version/protocolVersion.js#L2)) |
| $schema | string | Yes  | A valid URL (default: https://schema.dash.org/dpp-0-4-0/meta/data-contract)
| $id | array of bytes| Yes | Contract ID generated from `ownerId` and entropy ([32 bytes; content media type: `application/x.dash.dpp.identifier`](https://github.com/dashevo/js-dpp/blob/v0.21.0/schema/dataContract/dataContractMeta.json#L346-L352)) |
| ownerId | array of bytes | Yes | [Identity](platform-protocol-reference-identity) that registered the data contract defining the document ([32 bytes; content media type: `application/x.dash.dpp.identifier`](https://github.com/dashevo/js-dpp/blob/v0.21.0/schema/dataContract/dataContractMeta.json#L353-L359) |
| documents | object | Yes | Document definitions (see [Documents](#data-contract-documents) for details) |
| $defs | object | No | Definitions for `$ref` references used in the `documents` object (if present, must be a non-empty object with <= 100 valid properties) |

## Data Contract Schema

Details regarding the data contract object may be found in the [js-dpp data contract meta schema](https://github.com/dashevo/js-dpp/blob/v0.21.0/schema/dataContract/dataContractMeta.json). A truncated version is shown below for reference:

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://schema.dash.org/dpp-0-4-0/meta/data-contract",
  "type": "object",
  "$defs": {
    // Truncated ...
  },
  "properties": {
    "protocolVersion": {
      "type": "integer",
      "$comment": "Maximum is the latest protocol version"
    },
    "$schema": {
      "type": "string",
      "const": "https://schema.dash.org/dpp-0-4-0/meta/data-contract"
    },
    "$id":{
      "type": "array",
      "byteArray": true,
      "minItems": 32,
      "maxItems": 32,
      "contentMediaType": "application/x.dash.dpp.identifier"
    },
    "ownerId":{
      "type": "array",
      "byteArray": true,
      "minItems": 32,
      "maxItems": 32,
      "contentMediaType": "application/x.dash.dpp.identifier"
    },
    "documents": {
      "type": "object",
      "propertyNames": {
        "pattern": "^[a-zA-Z][a-zA-Z0-9-_]{1,62}[a-zA-Z0-9]$"
      },
      // Truncated ...
      "minProperties": 1,
      "maxProperties": 100
    },
    "$defs": {
      "$ref": "#/$defs/documentProperties"
    }
  },
  "required": [
    "protocolVersion",
    "$schema",
    "$id",
    "ownerId",
    "documents"
  ],
  "additionalProperties": false
}
```

**Example**

```json
{
  "id": "AoDzJxWSb1gUi2dSmvFeUFpSsjZQRJaqCpn7vCLkwwJj",
  "ownerId": "7NUbPf231ixt1kVBQsBvSMMBxd7AgPad8KtdtfFGhXDP",
  "schema": "https://schema.dash.org/dpp-0-4-0/meta/data-contract",
  "documents": {
    "note": {
      "properties": {
        "message": {
          "type": "string"
        }
      },
      "additionalProperties": false
    }
  }
}
```

## Data Contract id

The data contract `$id` is a hash of the `ownerId` and entropy as shown [here](https://github.com/dashevo/js-dpp/blob/v0.21.0/lib/dataContract/generateDataContractId.js).

```javascript
// From the JavaScript reference implementation (js-dpp)
// generateDataContractId.js
function generateDataContractId(ownerId, entropy) {
  return hash(
    Buffer.concat([
      ownerId,
      entropy,
    ]),
  );
}
```

## Data Contract Documents

The `documents` object defines each type of document required by the data contract. At a minimum, a document must consist of 1 or more properties. Documents may also define [indices](#document-indices) and a list of [required properties](#required-properties-optional). The `additionalProperties` properties keyword must be included as described in the [constraints](#additional-properties) section.

The following example shows a minimal `documents` object defining a single document (`note`) that has one property (`message`).

```json
{
  "note": {
    "type": "object",
    "properties": {
      "message": {
        "type": "string"
      }
    },
    "additionalProperties": false
  }
}
```

### Document Properties

The `properties` object defines each field that will be used by a document. Each field consists of an object that, at a minimum, must define its data `type` (`string`, `number`, `integer`, `boolean`, `array`, `object`). Fields may also apply a variety of optional JSON Schema constraints related to the format, range, length, etc. of the data.

**Note:** The `object` type is required to have properties defined either directly or via the data contract's [$defs](#data-contract-definitions).  For example, the body property shown below is an object containing a single string property (objectProperty):

```javascript
const contractDocuments = {
  message: {
    "type": "object",
    properties: {
      body: {
        type: "object",
        properties: {
          objectProperty: {
            type: "string"
          },
        },
        additionalProperties: false,
      },
      header: {
        type: "string"
      }
    },
    additionalProperties: false
  }
};
```

**Note:** A full explanation of the capabilities of JSON Schema is beyond the scope of this document. For more information regarding its data types and the constraints that can be applied, please refer to the [JSON Schema reference](https://json-schema.org/understanding-json-schema/reference/index.html) documentation.

#### Property Constraints

There are a variety of constraints currently defined for performance and security reasons.

| Description | Value |
| - | - |
| Minimum number of properties | [1](https://github.com/dashevo/js-dpp/blob/v0.21.0/schema/dataContract/dataContractMeta.json#L22) |
| Maximum number of properties | [100](https://github.com/dashevo/js-dpp/blob/v0.21.0/schema/dataContract/dataContractMeta.json#L23) |
| Minimum property name length | [3](https://github.com/dashevo/js-dpp/blob/v0.21.0/schema/dataContract/dataContractMeta.json#L9) |
| Maximum property name length | [64](https://github.com/dashevo/js-dpp/blob/v0.21.0/schema/dataContract/dataContractMeta.json#L9) |
| Property name first/last characters | \** First: (`A-Z`, `a-z`); Last: Alphanumeric (`A-Z`, `a-z`, `0-9`)**|
| Property name characters | Alphanumeric (`A-Z`, `a-z`, `0-9`)<br>Hypen (`-`) <br>Underscore (`_`) |

#### Required Properties (Optional)

Each document may have some fields that are required for the document to be valid and other fields that are optional. Required fields are defined via the `required` array which consists of a list of the field names from the document that must be present. The `required` object should be excluded for documents without any required properties.

```json
"required": [
  "<field name a>",
  "<field name b>"
]
```

**Example**
The following example (excerpt from the DPNS contract's `domain` document) demonstrates a document that has 6 required fields:

```json
"required": [
  "label",
  "normalizedLabel",
  "normalizedParentDomainName",
  "preorderSalt",
  "records",
  "subdomainRules"
]
```

### Document Indices

**Note:** The `indices` object should be excluded for documents that do not require indices.

Document indices may be defined if indexing on document fields is required.

The `indices` array consists of:

 - One or more objects that each contain:
   - A `properties` array composed of a `<field name: sort order>` object for each document field that is part of the index (sort order: `asc` or `desc`)
   - An (optional) `unique` element that determines if duplicate values are allowed for the document

**Note:** When defining an index with multiple properties (i.e a compound index), the order in which the properties are listed is important. Refer to the [mongoDB documentation](https://docs.mongodb.com/manual/core/index-compound/#prefixes) for details regarding the significance of the order as it relates to querying capabilities.

```json
"indices": [
  {
    "properties": [
      { "<field name a>": "<asc"|"desc>" },
      { "<field name b>": "<asc"|"desc>" }
    ],
    "unique": true|false
  },
  {
    "properties": [
      { "<field name c>": "<asc"|"desc>" },
    ],
  }
]
```

#### Index Constraints

**Note:** For performance and security reasons, indices have the following constraints. These constraints are subject to change over time.

| Description | Value |
| - | - |
| Maximum number of indices | [10](https://github.com/dashevo/js-dpp/blob/v0.21.0/schema/dataContract/dataContractMeta.json#L400) |
| Maximum number of unique indices | [3](https://github.com/dashevo/js-dpp/blob/v0.21.0/lib/errors/consensus/basic/dataContract/UniqueIndicesLimitReachedError.js#L22) |
| Maximum number of properties in a single index | [10](https://github.com/dashevo/js-dpp/blob/v0.21.0/schema/dataContract/dataContractMeta.json#L390) |
| Maximum length of indexed string property | [1024](https://github.com/dashevo/js-dpp/blob/v0.21.0/lib/dataContract/validation/validateDataContractFactory.js#L23) |

**Example**
The following example (excerpt from the DPNS contract's `preorder` document) creates an index on `saltedDomainHash` that also enforces uniqueness across all documents of that type:

```json
"indices": [
  {
    "properties": [
      {
        "saltedDomainHash": "asc"
      }
    ],
    "unique": true
  }
]
```

### Full Document Syntax

This example syntax shows the structure of a documents object that defines two documents, an index, and a required field.

```json
{
  "<document name a>": {
    "type": "object",
    "properties": {
      "<field name b>": {
        "type": "<field data type>"
      },
      "<field name c>": {
        "type": "<field data type>"
      },
    },
    "indices": [
      {
        "properties": [
          {
            "<field name c>": "<asc|desc>"
          }
        ],
        "unique": true|false
      },
    ],
    "required": [
      "<field name c>"
    ]
    "additionalProperties": false
  },
  "<document name x>": {
    "type": "object",
    "properties": {
      "<property name y>": {
        "type": "<property data type>"
      },
      "<property name z>": {
        "type": "<property data type>"
      },
    },
    "additionalProperties": false
  },    
}
```

### Document Schema

Full document schema details may be found in this section of the [js-dpp data contract meta schema](https://github.com/dashevo/js-dpp/blob/v0.21.0/schema/dataContract/dataContractMeta.json#L360-L415).

## Data Contract Definitions

The optional `$defs` object enables definition of aspects of a schema that are used in multiple places. This is done using the JSON Schema support for [reuse](https://json-schema.org/understanding-json-schema/structuring.html#defs). Items defined in `$defs` may then be referenced when defining `documents` through use of the `$ref` keyword.

**Note:** Properties defined in the `$defs` object must meet the same criteria as those defined in the `documents` object (e.g. the `additionalProperties` properties keyword must be included as described in the [constraints](#additional-properties) section).

**Note:** Data contracts can only use the `$ref` keyword to reference their own `$defs`. Referencing external definitions is not supported by the platform protocol.

**Example**
The following example shows a definition for a `message` object consisting of two properties:

```json
{
  // Preceding content truncated ...
  "$defs": {
    "message": {
      "type": "object",
      "properties": {
        "timestamp": {
          "type": "number"
        },
        "description": {
          "type": "string"
        }
      },
      "additionalProperties": false
    }
  }
}
```

**Note:** In the `js-dpp` reference implementation, definitions are added to a data contract via the `.setDefinitions()` method (e.g. `myContract.setDefinitions({\"message\": { ... }})`). This must be done prior to broadcasting the contract for registration.

# Data Contract Creation

Data contracts are created on the platform by submitting the [data contract object](#data-contract-object) in a data contract create state transition consisting of:

| Field | Type | Description|
| - | - | - |
| protocolVersion | integer | The platform protocol version ([currently `1`](https://github.com/dashevo/js-dpp/blob/v0.21.0/lib/version/protocolVersion.js#L2)) |
| type | integer | State transition type (`0` for data contract) |
| dataContract | [data contract object](#data-contract-object) | Object containing the data contract details
| entropy | array of bytes | Entropy used to generate the data contract ID. Generated as [shown here](platform-protocol-reference-state-transition#entropy-generation). (32 bytes) |
| signaturePublicKeyId | number | The `id` of the [identity public key](platform-protocol-reference-identity#identity-publickeys) that signed the state transition |
| signature | array of bytes | Signature of state transition data (65 bytes) |

Each data contract state transition must comply with this JSON-Schema definition established in [js-dpp](https://github.com/dashevo/js-dpp/blob/v0.21.0/schema/dataContract/stateTransition/dataContractCreate.json):

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "protocolVersion": {
      "type": "integer",
      "$comment": "Maximum is the latest protocol version"
    },
    "type": {
      "type": "integer",
      "const": 0
    },
    "dataContract": {
      "type": "object"
    },
    "entropy": {
      "type": "array",
      "byteArray": true,
      "minItems": 32,
      "maxItems": 32
    },
    "signaturePublicKeyId": {
      "type": "integer",
      "minimum": 0
    },
    "signature": {
      "type": "array",
      "byteArray": true,
      "minItems": 65,
      "maxItems": 65
    }
  },
  "additionalProperties": false,
  "required": [
    "protocolVersion",
    "type",
    "dataContract",
    "entropy",
    "signaturePublicKeyId",
    "signature"
  ]
}
```

**Example State Transition**

```json
{
  "protocolVersion": 1,
  "type": 0,
  "signature": "IG2Tr16rS2+FNoiH71eAva94H5BLV5QNl7Fg25s8ZzWvPlR4wihupdqYupvzTXGiAqPqSK3KQE1EouATMhgHPDc=",
  "signaturePublicKeyId": 0,
  "dataContract": {
    "protocolVersion": 0,
    "$id": "AoDzJxWSb1gUi2dSmvFeUFpSsjZQRJaqCpn7vCLkwwJj",
    "$schema": "https://schema.dash.org/dpp-0-4-0/meta/data-contract",
    "ownerId": "7NUbPf231ixt1kVBQsBvSMMBxd7AgPad8KtdtfFGhXDP",
    "documents": {
      "note": {
        "properties": {
          "message": {
            "type": "string"
          }
        },
        "additionalProperties": false
      }
    }
  },
  "entropy": "ahw7IvTAYkZcPaGcvh6BCVYP9rh/KyBkoeCGk28yoAw="
}
```

## Data Contract State Transition Signing

Data contract state transitions must be signed by a private key associated with the contract owner's identity.

The process to sign a data contract state transition consists of the following steps:

1. Canonical CBOR encode the state transition data - this include all ST fields except the `signature` and `signaturePublicKeyId`
2. Sign the encoded data with a private key associated with the `ownerId`
3. Set the state transition `signature` to the value of the signature created in the previous step
4. Set the state transition`signaturePublicKeyId` to the [public key `id`](platform-protocol-reference-identity#public-key-id) corresponding to the key used to sign