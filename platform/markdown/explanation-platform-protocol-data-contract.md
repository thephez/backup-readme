# Overview

As described briefly in the [Dash Platform Protocol explanation](explanation-platform-protocol#data-contract), Dash Platform uses data contracts to define the schema (structure) of data it stores. Therefore, an application must first register a data contract before using the platform to store its data. Then, whenever the application requests to store or change data on Dash Platform, the request will only succeed if the new data matches the data contract's schema.

The first two data contracts are DashPay wallet and [Dash Platform Name Service (DPNS)](explanation-dpns). The concept of the social, username-based DashPay wallet served as the catalyst for development of the platform, with DPNS providing the mechanism to support usernames.

# Details

## Ownership

Data contracts are owned by the [identity](explanation-identity) that registers them. Since Dash Platform Protocol version 0.12.0, a single identity may be used to create multiple data contracts.

## Structure
Each data contract must define several fields. When using the [JavaScript implementation](https://github.com/dashevo/js-dpp) of the Dash Platform Protocol, some of these fields are automatically set to a default value and do not have to be explicitly provided:
 - The platform protocol schema it uses (default: defined by js-dpp)
 - A contract ID (generated from a hash of the data contract's owner identity plus some entropy)
 - One or more documents

In the [example contract](#example-contract) shown below, a `contact` document and a `profile` document are defined. Each of these documents then defines the properties and indices it requires.

## Registration

Once a [Dash Platform Protocol](explanation-platform-protocol) compliant data contract has been defined, it may be registered on the platform. Registration is completed by submitting a state transition containing the data contract to [DAPI](explanation-dapi).

The drawing below illustrates the steps an application developer follows to complete registration.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/e7fc1d6-Creating_and_Using_an_Application_-_All.png",
        "Creating and Using an Application - All.png",
        820,
        719,
        "#eaecee"
      ],
      "caption": "Data Contract Registration Overview"
    }
  ]
}
[/block]
# Example Contract

An example contract for [DashPay](https://github.com/dashevo/dashpay-contract/blob/master/schema/dashpay.schema.json) is included below:
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"profile\": {\n    \"indices\": [\n      {\n        \"properties\": [\n          {\n            \"$ownerId\": \"asc\"\n          }\n        ],\n        \"unique\": true\n      },\n      {\n        \"properties\": [\n          {\n            \"$ownerId\": \"asc\"\n          },\n          {\n            \"$updatedAt\": \"asc\"\n          }\n        ]\n      }\n    ],\n    \"properties\": {\n      \"avatarUrl\": {\n        \"type\": \"string\",\n        \"format\": \"url\",\n        \"maxLength\": 2048\n      },\n      \"publicMessage\": {\n        \"type\": \"string\",\n        \"maxLength\": 140\n      },\n      \"displayName\": {\n        \"type\": \"string\",\n        \"maxLength\": 25\n      }\n    },\n    \"required\": [\n      \"$createdAt\",\n      \"$updatedAt\"\n    ],\n    \"additionalProperties\": false\n  },\n  \"contactInfo\": {\n    \"indices\": [\n      {\n        \"properties\": [\n          {\n            \"$ownerId\": \"asc\"\n          },\n          {\n            \"rootEncryptionKeyIndex\": \"asc\"\n          },\n          {\n            \"derivationEncryptionKeyIndex\": \"asc\"\n          }\n        ],\n        \"unique\": true\n      },\n      {\n        \"properties\": [\n          {\n            \"$ownerId\": \"asc\"\n          },\n          {\n            \"$updatedAt\": \"asc\"\n          }\n        ]\n      }\n    ],\n    \"properties\": {\n      \"encToUserId\": {\n        \"type\": \"array\",\n        \"byteArray\": true,\n        \"minItems\": 32,\n        \"maxItems\": 32\n      },\n      \"rootEncryptionKeyIndex\": {\n        \"type\": \"integer\",\n        \"minimum\": 0\n      },\n      \"derivationEncryptionKeyIndex\": {\n        \"type\": \"integer\",\n        \"minimum\": 0\n      },\n      \"privateData\": {\n        \"type\": \"array\",\n        \"byteArray\": true,\n        \"minItems\": 48,\n        \"maxItems\": 2048,\n        \"description\": \"This is the encrypted values of aliasName + note + displayHidden encoded as an array in cbor\"\n      }\n    },\n    \"required\": [\n      \"$createdAt\",\n      \"$updatedAt\",\n      \"encToUserId\",\n      \"privateData\",\n      \"rootEncryptionKeyIndex\",\n      \"derivationEncryptionKeyIndex\"\n    ],\n    \"additionalProperties\": false\n  },\n  \"contactRequest\": {\n    \"indices\": [\n      {\n        \"properties\": [\n          {\n            \"$ownerId\": \"asc\"\n          },\n          {\n            \"toUserId\": \"asc\"\n          },\n          {\n            \"accountReference\": \"asc\"\n          }\n        ],\n        \"unique\": true\n      },\n      {\n        \"properties\": [\n          {\n            \"$ownerId\": \"asc\"\n          },\n          {\n            \"toUserId\": \"asc\"\n          }\n        ]\n      },\n      {\n        \"properties\": [\n          {\n            \"toUserId\": \"asc\"\n          },\n          {\n            \"$createdAt\": \"asc\"\n          }\n        ]\n      },\n      {\n        \"properties\": [\n          {\n            \"$ownerId\": \"asc\"\n          },\n          {\n            \"$createdAt\": \"asc\"\n          }\n        ]\n      }\n    ],\n    \"properties\": {\n      \"toUserId\": {\n        \"type\": \"array\",\n        \"byteArray\": true,\n        \"minItems\": 32,\n        \"maxItems\": 32,\n        \"contentMediaType\": \"application/x.dash.dpp.identifier\"\n      },\n      \"encryptedPublicKey\": {\n        \"type\": \"array\",\n        \"byteArray\": true,\n        \"minItems\": 96,\n        \"maxItems\": 96\n      },\n      \"senderKeyIndex\": {\n        \"type\": \"integer\",\n        \"minimum\": 0\n      },\n      \"recipientKeyIndex\": {\n        \"type\": \"integer\",\n        \"minimum\": 0\n      },\n      \"accountReference\": {\n        \"type\": \"integer\",\n        \"minimum\": 0\n      },\n      \"encryptedAccountLabel\": {\n        \"type\": \"array\",\n        \"byteArray\": true,\n        \"minItems\": 48,\n        \"maxItems\": 80\n      }\n    },\n    \"required\": [\n      \"$createdAt\",\n      \"toUserId\",\n      \"encryptedPublicKey\",\n      \"senderKeyIndex\",\n      \"recipientKeyIndex\",\n      \"accountReference\"\n    ],\n    \"additionalProperties\": false\n  }\n}",
      "language": "json",
      "name": "Dashpay Data Contract"
    }
  ]
}
[/block]
This is a visualization of the JSON data contract as UML class diagram for better understanding of the structure:

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/964c219-dashpay-plantuml.png",
        "dashpay-plantuml.png",
        962,
        755,
        "#f5f1e8"
      ],
      "caption": "Dashpay Contract Diagram"
    }
  ]
}
[/block]