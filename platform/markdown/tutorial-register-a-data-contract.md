# Overview

In this tutorial we will register a data contract.

## Prerequisites
- [General prerequisites](tutorials-introduction#prerequisites) (Node.js / Dash SDK installed)
- A wallet mnemonic with some funds in it: [Tutorial: Create and Fund a Wallet](tutorial-create-and-fund-a-wallet)
- A Dash Platform Identity: [Tutorial: Register an Identity](tutorial-register-an-identity) 

# Code

## Defining contract documents

As described in the [data contract explanation](explanation-platform-protocol-data-contract#structure), data contracts must include one or more developer-defined [documents](explanation-platform-protocol-document). 

The most basic example below (tab 1) demonstrates a data contract containing a single document type (`note`) which has a single string property (`message`). 

The second tab shows the same data contract with an index defined on the `$ownerId` field. This would allow querying for documents owned by a specific identity using a [where clause](reference-query-syntax#where-clause).

> 🚧
>
> Since Platform v0.23, an index can [only use the ascending order](https://github.com/dashevo/platform/pull/435) (`asc`). Future updates will remove this restriction.

The third tab shows a data contract using the [JSON-Schema $ref feature](https://json-schema.org/understanding-json-schema/structuring.html#reuse) that enables reuse of defined objects.

> 🚧
>
> The `$ref` keyword has been [temporarily disabled](https://github.com/dashevo/platform/pull/300) since Platform v0.22.

The fourth tab shows a data contract requiring the optional `$createdAt` and `$updatedAt` [base fields](explanation-platform-protocol-document#base-fields). Using these fields enables retrieving timestamps that indicate when a document was created or modified.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"note\": {\n    \"type\": \"object\",\n    \"properties\": {\n      \"message\": {\n        \"type\": \"string\"\n      }\n    },\n    \"additionalProperties\": false\n  }\n}",
      "language": "json",
      "name": "1. Minimal contract"
    },
    {
      "code": "{\n  \"note\": {\n    \"type\": \"object\",\n    \"indices\": [\n      {\n        \"name\": \"ownerId\",\n        \"properties\": [{ \"$ownerId\": \"asc\" }], \"unique\": false }\n    ],\n    \"properties\": {\n      \"message\": {\n        \"type\": \"string\"\n      }\n    },\n    \"additionalProperties\": false\n  }\n}\n\n/*\nAn identity's documents are accessible via a query including a where clause like:\n{\n  where: [['$ownerId', '==', 'an identity id']],\n}\n*/\n",
      "language": "json",
      "name": "2. Indexed"
    },
    {
      "code": "// NOTE: The `$ref` keyword is temporarily disabled for Platform v0.22.\n{\n  customer: {\n    type: \"object\",\n    properties: {\n      name: { type: \"string\" },\n      billing_address: { $ref: \"#/$defs/address\" },\n      shipping_address: { $ref: \"#/$defs/address\" }\n    },\n    additionalProperties: false\n  },\n}\n\n/*\nThe contract document defined above is dependent on the following object \nbeing added to the contract via the contracts `.setDefinitions` method:\n\n{\n  address: {\n    type: \"object\",\n    properties: {\n      street_address: { type: \"string\" },\n      city:           { type: \"string\" },\n      state:          { type: \"string\" }\n    },\n    required: [\"street_address\", \"city\", \"state\"],\n    additionalProperties: false\n  }\n}\n*/",
      "language": "json",
      "name": "3. References ($ref)"
    },
    {
      "code": "{\n  \"note\": {\n    \"type\": \"object\",\n    \"properties\": {\n      \"message\": {\n        \"type\": \"string\"\n      }\n    },\n    \"required\": [\"$createdAt\", \"$updatedAt\"],\n    \"additionalProperties\": false\n  }\n}\n\n/*\nIf $createdAt and/or $updatedAt are added to the list of required properties \nfor a document, all documents of that type will store a timestamp indicating\nwhen the document was created or modified. \n\nThis information will be returned when the document is retrieved.\n*/",
      "language": "json",
      "name": "4. Timestamps"
    },
    {
      "code": "{\n \"block\": {\n   \"type\": \"object\",\n    \"properties\": {\n      \"hash\": {\n        \"type\": 'array',\n        \"byteArray\": true,\n        \"maxItems\": 64,\n        \"description\": 'Store block hashes',\n      },\n    },\n    \"additionalProperties\": false,\n  },\n}\n \n/*\nSetting `\"byteArray\": true` indicates that the provided data will be an \narray of bytes (e.g. a NodeJS Buffer).\n*/",
      "language": "json",
      "name": "5. Binary data"
    }
  ]
}
[/block]
> 📘
>
> Please refer to the [data contract reference page](reference-data-contracts) for more comprehensive details related to contracts and documents.

## Registering the data contract

The following examples demonstrate the details of creating contracts using the features [described above](#defining-contract-documents):
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  wallet: {\n    mnemonic: 'a Dash wallet mnemonic with funds goes here',\n    unsafeOptions: {\n      skipSynchronizationBeforeHeight: 650000, // only sync from early-2022\n    },    \n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst registerContract = async () => {\n  const { platform } = client;\n  const identity = await platform.identities.get('an identity ID goes here');\n\n  const contractDocuments = {\n    note: {\n      type: 'object',\n      properties: {\n        message: {\n          type: 'string',\n        },\n      },\n      additionalProperties: false,\n    },\n  };\n\n  const contract = await platform.contracts.create(contractDocuments, identity);\n  console.dir({ contract });\n\n  // Make sure contract passes validation checks\n  await platform.dpp.initialize();\n  const validationResult = await platform.dpp.dataContract.validate(contract);\n\n  if (validationResult.isValid()) {\n    console.log('Validation passed, broadcasting contract..');\n    // Sign and submit the data contract\n    return platform.contracts.publish(contract, identity);\n  }\n  console.error(validationResult); // An array of detailed validation errors\n  throw validationResult.errors[0];\n};\n\nregisterContract()\n  .then((d) => console.log('Contract registered:\\n', d.toJSON()))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript",
      "name": "Minimal"
    },
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  wallet: {\n    mnemonic: 'a Dash wallet mnemonic with funds goes here',\n    unsafeOptions: {\n      skipSynchronizationBeforeHeight: 650000, // only sync from early-2022\n    },\n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst registerContract = async () => {\n  const { platform } = client;\n  const identity = await platform.identities.get('an identity ID goes here');\n\n  const contractDocuments = {\n    note: {\n      type: 'object',\n      indices: [\n        name: \"ownerId\",\n        { properties: [{ $ownerId: 'asc' }], unique: false },\n      ],\n      properties: {\n        message: {\n          type: 'string',\n        },\n      },\n      additionalProperties: false,\n    },\n  };\n\n  const contract = await platform.contracts.create(contractDocuments, identity);\n  console.dir({ contract });\n\n  // Make sure contract passes validation checks\n  await platform.dpp.initialize();\n  const validationResult = await platform.dpp.dataContract.validate(contract);\n\n  if (validationResult.isValid()) {\n    console.log('Validation passed, broadcasting contract..');\n    // Sign and submit the data contract\n    return platform.contracts.publish(contract, identity);\n  }\n  console.error(validationResult); // An array of detailed validation errors\n  throw validationResult.errors[0];\n};\n\nregisterContract()\n  .then((d) => console.log('Contract registered:\\n', d.toJSON()))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript",
      "name": "Indexed"
    },
    {
      "code": "// NOTE: The `$ref` keyword is temporarily disabled for Platform v0.22.\nconst Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  wallet: {\n    mnemonic: 'a Dash wallet mnemonic with funds goes here',\n    unsafeOptions: {\n      skipSynchronizationBeforeHeight: 650000, // only sync from early-2022\n    },\n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst registerContract = async () => {\n  const { platform } = client;\n  const identity = await platform.identities.get('an identity ID goes here');\n\n  // Define a reusable object\n  const definitions = {\n    address: {\n      type: 'object',\n      properties: {\n        street_address: { type: 'string' },\n        city: { type: 'string' },\n        state: { type: 'string' },\n      },\n      required: ['street_address', 'city', 'state'],\n      additionalProperties: false,\n    },\n  };\n\n  // Create a document with properties using a definition via $ref\n  const contractDocuments = {\n    customer: {\n      type: 'object',\n      properties: {\n        name: { type: 'string' },\n        billing_address: { $ref: '#/$defs/address' },\n        shipping_address: { $ref: '#/$defs/address' },\n      },\n      additionalProperties: false,\n    },\n  };\n  \n  const contract = await platform.contracts.create(contractDocuments, identity);\n\n  // Add reusable definitions referred to by \"$ref\" to contract\n  contract.setDefinitions(definitions);\n  console.dir({ contract });\n\n  // Make sure contract passes validation checks\n  await platform.dpp.initialize();\n  const validationResult = await platform.dpp.dataContract.validate(contract);\n\n  if (validationResult.isValid()) {\n    console.log('Validation passed, broadcasting contract..');\n    // Sign and submit the data contract\n    return platform.contracts.publish(contract, identity);\n  }\n  console.error(validationResult); // An array of detailed validation errors\n  throw validationResult.errors[0];\n};\n\nregisterContract()\n  .then((d) => console.log('Contract registered:\\n', d.toJSON()))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript",
      "name": "References ($ref)"
    },
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  wallet: {\n    mnemonic: 'a Dash wallet mnemonic with funds goes here',\n    unsafeOptions: {\n      skipSynchronizationBeforeHeight: 650000, // only sync from early-2022\n    },\n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst registerContract = async () => {\n  const { platform } = client;\n  const identity = await platform.identities.get('an identity ID goes here');\n\n  const contractDocuments = {\n    note: {\n      type: 'object',\n      properties: {\n        message: {\n          type: 'string',\n        },\n      },\n      required: ['$createdAt', '$updatedAt'],\n      additionalProperties: false,\n    },\n  };\n\n  const contract = await platform.contracts.create(contractDocuments, identity);\n  console.dir({ contract });\n\n  // Make sure contract passes validation checks\n  await platform.dpp.initialize();\n  const validationResult = await platform.dpp.dataContract.validate(contract);\n\n  if (validationResult.isValid()) {\n    console.log('Validation passed, broadcasting contract..');\n    // Sign and submit the data contract\n    return platform.contracts.publish(contract, identity);\n  }\n  console.error(validationResult); // An array of detailed validation errors\n  throw validationResult.errors[0];\n};\n\nregisterContract()\n  .then((d) => console.log('Contract registered:\\n', d.toJSON()))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript",
      "name": "Timestamps"
    },
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  wallet: {\n    mnemonic: 'a Dash wallet mnemonic with funds goes here',\n    unsafeOptions: {\n      skipSynchronizationBeforeHeight: 650000, // only sync from early-2022\n    },\n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst registerContract = async () => {\n  const { platform } = client;\n  const identity = await platform.identities.get('an identity ID goes here');\n\n  const contractDocuments = {\n    block: {\n      type: 'object',\n      properties: {\n        hash: {\n          type: 'array',\n          byteArray: true,\n          maxItems: 64,\n          description: 'Store block hashes',\n        },\n      },\n      additionalProperties: false,\n    },\n  };\n\n  const contract = await platform.contracts.create(contractDocuments, identity);\n  console.dir({ contract }, { depth: 5 });\n\n  // Make sure contract passes validation checks\n  await platform.dpp.initialize();\n  const validationResult = await platform.dpp.dataContract.validate(contract);\n\n  if (validationResult.isValid()) {\n    console.log('Validation passed, broadcasting contract..');\n    // Sign and submit the data contract\n    return platform.contracts.publish(contract, identity);\n  }\n  console.error(validationResult); // An array of detailed validation errors\n  throw validationResult.errors[0];\n};\n\nregisterContract()\n  .then((d) => console.log('Contract registered:\\n', d.toJSON()))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript",
      "name": "Binary data"
    }
  ]
}
[/block]
> 👍
>
> **Make a note of the returned data contract `$id` as it will be used used in subsequent tutorials throughout the documentation.**

# What's Happening

After we initialize the Client, we create an object defining the documents this data contract requires (e.g. a `note` document in the example). The `platform.contracts.create` method takes two arguments: a contract definitions JSON-schema object and an identity. The contract definitions object consists of the document types being created (e.g. `note`). It defines the properties and any indices. 

Once the data contract has been created, we still need to submit it to DAPI. The `platform.contracts.publish` method takes a data contract and an identity parameter. Internally, it creates a State Transition containing the previously created contract, signs the state transition, and submits the signed state transition to DAPI. A response will only be returned if an error is encountered.

> 📘Wallet Operations
>
> The JavaScript SDK does not cache wallet information. It re-syncs the entire Core chain for some wallet operations (e.g. `client.getWalletAccount()`) which can result in wait times of  5+ minutes. 
>
> A future release will add caching so that access is much faster after the initial sync. For now, the `skipSynchronizationBeforeHeight` option can be used to sync the wallet starting at a certain block height.