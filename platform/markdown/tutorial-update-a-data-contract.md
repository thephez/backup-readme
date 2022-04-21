# Overview

Since Dash Platform v0.22, it is possible to update existing data contracts in certain backwards-compatible ways. This includes:
 - Adding new documents
 - Adding new optional properties to existing documents
 - Adding _non-unique_ indices for properties added in the update.

In this tutorial we will update an existing data contract. 

## Prerequisites
- [General prerequisites](tutorials-introduction#prerequisites) (Node.js / Dash SDK installed)
- A wallet mnemonic with some funds in it: [Tutorial: Create and Fund a Wallet](tutorial-create-and-fund-a-wallet)
- A Dash Platform Identity: [Tutorial: Register an Identity](tutorial-register-an-identity)
- A Dash Platform Contract ID: [Tutorial: Register a Data Contract](tutorial-register-a-data-contract) 

# Code

The following example demonstrates updating an existing contract to add a new property to an existing document:
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  wallet: {\n    mnemonic: 'a Dash wallet mnemonic with funds goes here',\n    unsafeOptions: {\n      skipSynchronizationBeforeHeight: 650000, // only sync from early-2022\n    },    \n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst updateContract = async () => {\n  const { platform } = client;\n  const identity = await platform.identities.get('an identity ID goes here');\n\n  const existingDataContract = await platform.contracts.get('a contract ID goes here');\n  const documents = existingDataContract.getDocuments();\n\n  documents.note.properties.author = {\n    type: 'string',\n  };\n\n  existingDataContract.setDocuments(documents);\n    \n  // Make sure contract passes validation checks\n  await platform.dpp.initialize();\n  const validationResult = await platform.dpp.dataContract.validate(existingDataContract);\n\n  if (validationResult.isValid()) {\n    console.log('Validation passed, broadcasting contract..');\n    // Sign and submit the data contract\n    return platform.contracts.update(existingDataContract, identity);\n  }\n  console.error(validationResult); // An array of detailed validation errors\n  throw validationResult.errors[0];\n};\n\nupdateContract()\n  .then((d) => console.log('Contract updated:\\n', d.toJSON()))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript",
      "name": "JavaScript"
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
# What's Happening

After we initialize the Client, we retrieve an existing contract owned by our identity. We then get the contract's documents and modify a document (adding an `author` property to the `note` document in the example).The `setDocuments` method takes one argument: the object containing the updated document types.

Once the data contract has been updated, we still need to submit it to DAPI. The `platform.contracts.update` method takes a data contract and an identity parameter. Internally, it creates a State Transition containing the updated contract, signs the state transition, and submits the signed state transition to DAPI. A response will only be returned if an error is encountered,
[block:callout]
{
  "type": "info",
  "title": "Wallet Operations",
  "body": "Currently, the JavaScript SDK does not cache wallet information, and therefore, it re-syncs the entire Core chain for some wallet operations. This can result in wait times of  5+ minutes. \n\nAn upcoming release will add a persistence feature to cache wallet information during initial sync so that subsequent access is much faster. For now, the `skipSynchronizationBeforeHeight` option can be used to only sync the wallet starting at a certain block height."
}
[/block]