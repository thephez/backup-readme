# Overview

As described briefly in the [Dash Platform Protocol explanation](explanation-platform-protocol#section-data-contract), Dash Platform uses data contracts to define the schema (structure) of data it stores. Therefore, an application must first register a data contract before using the platform to store its data. Then, whenever the application requests to store or change data on Dash Platform, the request will only succeed if the new data matches the data contract's schema.

The first two data contracts are DashPay wallet and [Dash Platform Name Service (DPNS)](explanation-dpns). The concept of the social, username based DashPay wallet served as the catalyst for development of the platform, with DPNS providing the mechanism to support usernames.

# Details

## Ownership

Data contracts are owned by the [application identity](explanation-identity#section-application-registration) that registers them. Since an application identity is used as the contract's ID, a unique identity must be used for each data contract. 

## Constraints

On certain networks, such as a local devnet, anyone can create a new data contract. After launch, this will also be true on Testnet and Mainnet.

## Structure
Each data contract must define several fields. When using the [JavaScript implementation](https://github.com/dashevo/js-dpp) of the Dash Platform Protocol, some of these fields are automatically set to a default value and do not have to be explicitly provided:
 - The platform protocol schema version it uses (default: defined by js-dpp)
 - A contract version number (default: 1)
 - A contract ID (the application identity registering the contract)
 - One or more documents

In the [example contract](#section-example-contract) shown below, a `contact` document and a `profile` document are defined. Each of these documents then defines the properties and indices it requires.

## Registration

[block:callout]
{
  "type": "warning",
  "title": "Evonet Limitation",
  "body": "Registration of third-party data contracts is not enabled on the initial phase of Evonet; however, the DPNS contracts is registered. To register data contracts without restriction, please create a devnet."
}
[/block]

Once a [Dash Platform Protocol](explanation-platform-protocol) compliant data contract has been defined, it may be registered on the platform. Registration is completed by submitting a state transition containing the data contract to [DAPI](explanation-dapi).

The drawing below illustrates the steps an application developer follows to complete registration.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/f60c959-Creating_and_Using_an_Application.png",
        "Creating and Using an Application.png",
        960,
        720,
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
      "code": "{\n  \"contact\": {\n    \"indices\": [\n      {\n        \"unique\": true,\n        \"properties\": [\n          {\n            \"$userId\": \"asc\"\n          },\n          {\n            \"toUserId\": \"asc\"\n          }\n        ]\n      }\n    ],\n    \"required\": [\n      \"toUserId\",\n      \"publicKey\"\n    ],\n    \"properties\": {\n      \"toUserId\": {\n        \"type\": \"string\"\n      },\n      \"publicKey\": {\n        \"type\": \"string\"\n      }\n    },\n    \"additionalProperties\": false\n  },\n  \"profile\": {\n    \"indices\": [\n      {\n        \"unique\": true,\n        \"properties\": [\n          {\n            \"$userId\": \"asc\"\n          }\n        ]\n      }\n    ],\n    \"required\": [\n      \"avatarUrl\",\n      \"about\"\n    ],\n    \"properties\": {\n      \"about\": {\n        \"type\": \"string\"\n      },\n      \"avatarUrl\": {\n        \"type\": \"string\",\n        \"format\": \"url\"\n      }\n    },\n    \"additionalProperties\": false\n  }\n}",
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
      ]
    }
  ]
}
[/block]