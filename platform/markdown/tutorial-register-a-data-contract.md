[block:callout]
{
  "type": "danger",
  "title": "Dash SDK version 2.x+ required",
  "body": "This tutorial requires version 2.0 of DashJS which is not yet available on npm. It can be accessed on the `develop` branch of the repository (see [here](https://github.com/dashevo/DashJS/pull/33) for status of the v2.0 pull request)."
}
[/block]
# Overview

In this tutorial we will register a data contract.

For this tutorial you will need:

- A wallet mnemonic with some funds in it: [Tutorial: Create and Fund a Wallet](tutorial-create-and-fund-a-wallet)
- A dash platform **application** identity: [Tutorial: Register an Identity](tutorial-register-an-identity) 

# Code

## Defining contract documents

As described in the [data contract explanation](explanation-platform-protocol-data-contract#section-structure), data contracts must include one or more developer-defined [documents](explanation-platform-protocol-document). The most basic example below demonstrates registering a data contract containing a single document type (`note`) which has a single string property (`message`).
[block:callout]
{
  "type": "info",
  "body": "Please refer to the [data contract reference page](reference-data-contracts) for more comprehensive details related to contracts and documents."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"note\": {\n    \"properties\": {\n      \"message\": {\n        \"type\": \"string\"\n      }\n    },\n    \"additionalProperties\": false\n  }\n}",
      "language": "json",
      "name": "Data contract `note` document"
    }
  ]
}
[/block]
## Registering the data contract
[block:code]
{
  "codes": [
    {
      "code": "const DashJS = require('dash');\n\nconst sdkOpts = {\n  network: 'testnet',\n  mnemonic: 'a Dash wallet mnemonic with evonet funds goes here',\n};\nconst sdk = new DashJS.Client(sdkOpts);\n\nconst registerContract = async function () {\n  try {\n    await sdk.isReady();\n    const platform = sdk.platform;\n    const identity = await platform.identities.get('an identity ID goes here');\n    \n    const contractDocuments = {\n      note: {\n        properties: {\n          message: {\n            type: \"string\"\n          }\n        },\n        additionalProperties: false\n      }};\n    const contract = await platform.contracts.create(contractDocuments, identity);\n    console.dir({contract});\n    \n    // Sign and submit the data contract\n    await platform.contracts.broadcast(contract, identity);\n  } catch (e) {\n    console.error('Something went wrong:', e);\n  } finally {\n    sdk.disconnect();\n  }\n}\n\nregisterContract();",
      "language": "javascript"
    }
  ]
}
[/block]
# What's Happening

After we initialize the SDK, we create an object defining the documents this data contract requires (e.g. a `note` document in the example). The `platform.contracts.create` method takes two arguments: a contract definitions JSON-schema object and an application identity. The contract definitions object consists of the document types being created (e.g. `note`). It defines the properties and any indices. 

Once the data contract has been created, we still need to submit it to DAPI. The `platform.contracts.broadcast` method takes a data contract and an identity parameter. Internally, it creates a State Transition containing the previously created contract, signs the state transition, and submits the signed state transition to DAPI. A response will only be returned if an error is encountered,