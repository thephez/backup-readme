# Overview

In this tutorial we will register a data contract.

For this tutorial you will need:

- A wallet mnemonic with some funds in it: [Tutorial: Create and Fund a Wallet](tutorial-create-and-fund-a-wallet)
- A dash platform identity: [Tutorial: Register an Identity](tutorial-register-an-identity) 

# Code

## Defining contract documents

As described in the [data contract explanation](explanation-platform-protocol-data-contract#structure), data contracts must include one or more developer-defined [documents](explanation-platform-protocol-document). 

The most basic example below (tab 1) demonstrates a data contract containing a single document type (`note`) which has a single string property (`message`). 

The second tab shows the same data contract with an index defined on the `$ownerId` field. This would allow querying for documents owned by a specific identity using a [where clause](reference-query-syntax#where-clause).

The third tab shows a data contract using the [JSON-Schema $ref feature](https://json-schema.org/understanding-json-schema/structuring.html#reuse) that enables reuse of defined objects.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"note\": {\n    \"properties\": {\n      \"message\": {\n        \"type\": \"string\"\n      }\n    },\n    \"additionalProperties\": false\n  }\n}",
      "language": "json",
      "name": "1. Data contract document (basic)"
    },
    {
      "code": "{\n  \"note\": {\n    \"indices\": [\n      {\n        \"properties\": [{ \"$ownerId\": \"asc\" }], \"unique\": false },\n    ],\n    \"properties\": {\n      \"message\": {\n        \"type\": \"string\"\n      }\n    },\n    \"additionalProperties\": false\n  }\n}\n\n/*\nAn identity's documents are accessible via a query including a where clause like:\n{\n  where: [['$ownerId', '==', 'an identity id']],\n}\n*/\n",
      "language": "json",
      "name": "2. Data contract document (with index)"
    },
    {
      "code": "{\n  customer: {\n    properties: {\n      name: { type: \"string\" },\n      billing_address: { $ref: \"#/definitions/address\" },\n      shipping_address: { $ref: \"#/definitions/address\" }\n    },\n    additionalProperties: false\n  },\n}\n\n/*\nThe contract document defined above is dependent on the following object \nbeing added to the contract via the contracts `.setDefinitions` method:\n\n{\n  address: {\n    type: \"object\",\n    properties: {\n      street_address: { type: \"string\" },\n      city:           { type: \"string\" },\n      state:          { type: \"string\" }\n    },\n    required: [\"street_address\", \"city\", \"state\"],\n    additionalProperties: false\n  }\n}\n*/",
      "language": "json",
      "name": "Data contract document (with $ref)"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Please refer to the [data contract reference page](reference-data-contracts) for more comprehensive details related to contracts and documents."
}
[/block]
## Registering the data contract

The following examples demonstrate the details of creating contracts using the features [described above](#defining-contract-documents):
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  wallet: {\n  \tmnemonic: 'a Dash wallet mnemonic with evonet funds goes here',\n  }\n};\nconst client = new Dash.Client(clientOpts);\n\nconst registerContract = async function () {\n  try {\n    const platform = client.platform;\n    const identity = await platform.identities.get('an identity ID goes here');\n    \n    const contractDocuments = {\n      note: {\n        properties: {\n          message: {\n            type: \"string\"\n          }\n        },\n        additionalProperties: false\n      }};\n    \n    const contract = await platform.contracts.create(contractDocuments, identity);\n    console.dir({contract});\n\n    // Make sure contract passes validation checks\n    const validationResult = await platform.dpp.dataContract.validate(contract);\n\n    if (validationResult.isValid()) {\n      console.log(\"validation passed, broadcasting contract..\");\n      // Sign and submit the data contract\n      await platform.contracts.broadcast(contract, identity);\n    } else {\n      console.error(validationResult) // An array of detailed validation errors\n      throw validationResult.errors[0];\n    }\n    \n  } catch (e) {\n    console.error('Something went wrong:', e);\n  } finally {\n    client.disconnect();\n  }\n}\n\nregisterContract();",
      "language": "javascript",
      "name": "JavaScript (Basic)"
    },
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  wallet: {\n  \tmnemonic: 'a Dash wallet mnemonic with evonet funds goes here',\n  }\n};\nconst client = new Dash.Client(clientOpts);\n\nconst registerContract = async function () {\n  try {\n    const platform = client.platform;\n    const identity = await platform.identities.get('an identity ID goes here');\n    \n    const contractDocuments = {\n  \t  note: {\n       indices: [\n          {\n           properties: [{ \"$ownerId\": \"asc\" }], unique: false },\n        ],\n        properties: {\n          message: {\n            type: \"string\"\n          }\n        },\n        additionalProperties: false\n      }};\n    \n    const contract = await platform.contracts.create(contractDocuments, identity);\n    console.dir({contract});\n\n    // Make sure contract passes validation checks\n    const validationResult = await platform.dpp.dataContract.validate(contract);\n\n    if (validationResult.isValid()) {\n      console.log(\"validation passed, broadcasting contract..\");\n      // Sign and submit the data contract\n      await platform.contracts.broadcast(contract, identity);\n    } else {\n      console.error(validationResult) // An array of detailed validation errors\n      throw validationResult.errors[0];\n    }\n    \n  } catch (e) {\n    console.error('Something went wrong:', e);\n  } finally {\n    client.disconnect();\n  }\n}\n\nregisterContract();",
      "language": "javascript",
      "name": "JavaScript (With index)"
    },
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  wallet: {\n  \tmnemonic: 'a Dash wallet mnemonic with evonet funds goes here',\n  }\n};\nconst client = new Dash.Client(clientOpts);\n\nconst registerContract = async function () {\n  try {\n    const platform = client.platform;\n    const identity = await platform.identities.get('an identity ID goes here');\n    \n    // Define a reusable object\n    const definitions = {\n      address: {\n        type: \"object\",\n        properties: {\n          street_address: { type: \"string\" },\n          city:           { type: \"string\" },\n          state:          { type: \"string\" }\n        },\n        required: [\"street_address\", \"city\", \"state\"],\n        additionalProperties: false\n      }\n    }\n\n    // Create a document with properties using a definition via $ref\n    const contractDocuments = {\n      customer: {\n        properties: {\n          name:             { type: \"string\" },\n          billing_address:  { $ref: \"#/definitions/address\" },\n          shipping_address: { $ref: \"#/definitions/address\" }\n        },\n        additionalProperties: false\n      },\n    };\n    \n    const contract = await platform.contracts.create(contractDocuments, identity);\n    \n    // Add reusable definitions referred to by \"$ref\" to contract\n    contract.setDefinitions(definitions)\n    console.dir({contract});\n\n    // Make sure contract passes validation checks\n    const validationResult = await platform.dpp.dataContract.validate(contract);\n\n    if (validationResult.isValid()) {\n      console.log(\"validation passed, broadcasting contract..\");\n      // Sign and submit the data contract\n      await platform.contracts.broadcast(contract, identity);\n    } else {\n      console.error(validationResult) // An array of detailed validation errors\n      throw validationResult.errors[0];\n    }\n    \n  } catch (e) {\n    console.error('Something went wrong:', e);\n  } finally {\n    client.disconnect();\n  }\n}\n\nregisterContract();",
      "language": "javascript",
      "name": "JavaScript ($ref)"
    }
  ]
}
[/block]
# What's Happening

After we initialize the Client, we create an object defining the documents this data contract requires (e.g. a `note` document in the example). The `platform.contracts.create` method takes two arguments: a contract definitions JSON-schema object and an identity. The contract definitions object consists of the document types being created (e.g. `note`). It defines the properties and any indices. 

Once the data contract has been created, we still need to submit it to DAPI. The `platform.contracts.broadcast` method takes a data contract and an identity parameter. Internally, it creates a State Transition containing the previously created contract, signs the state transition, and submits the signed state transition to DAPI. A response will only be returned if an error is encountered,