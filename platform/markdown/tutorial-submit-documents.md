# Overview

In this tutorial we will submit some data to an application on Dash Platform. Data is stored in the form of [documents](explanation-platform-protocol-document) which are encapsulated in a [state transition](explanation-platform-state-transition) before being submitted to DAPI. 

## Prerequisites
- [node.js](https://nodejs.org/en/)
- Basic familiarity with JavaScript asychronous functions using [async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
- The Dash JavaScript SDK is initialized (covered in [Connecting to EvoNet](tutorial-connecting-to-evonet))

# Code
[block:callout]
{
  "type": "info",
  "title": "Initializing the Client with a contract identity",
  "body": "The example below shows how access to contract documents via `<contract name>.<contract document>` syntax (e.g. `tutorialContract.note`) can be enabled by passing a contract identity to the constructor. Please refer to the [DashJS documentation](https://dashevo.github.io/DashJS/#/getting-started/multiple-apps) for details."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  mnemonic: 'a Dash wallet mnemonic with evonet funds goes here',\n  apps: {\n    tutorialContract: {\n      contractId: '6kk9sqg3SLwDk7kn92VzxcWRWV8qe2g2JixxK6V2cQsm'\n    }\n  }  \n};\nconst client = new Dash.Client(clientOpts);\n\nconst submitNoteDocument = async function () {\n  const platform = client.platform;\n  await client.isReady();\n\n  try {\n    const identity = await platform.identities.get('an identity ID goes here');\n\n    docProperties = {\n      message: 'Tutorial Test @ ' + new Date().toUTCString()\n    }\n\n    // Create the note document\n    const noteDocument = await platform.documents.create(\n      'tutorialContract.note',\n      identity,\n      docProperties,\n    );\n\n    const documentBatch = {\n      create: [noteDocument],\n    \treplace: [],\n    \tdelete: [],\n    }\n    // Sign and submit the document(s)\n    await platform.documents.broadcast(documentBatch, identity);\n  } catch (e) {\n    console.error('Something went wrong:', e);\n  } finally {\n    client.disconnect();\n  }\n};\n\nsubmitNoteDocument();\n",
      "language": "javascript"
    }
  ]
}
[/block]
# What's happening

After we initialize the Client, we create a document that matches the structure defined by the data contract of the application being referenced (e.g. a `note` document for the contract registered in the [data contract tutorial](tutorial-register-a-data-contract#section-code)). The `platform.documents.create` method takes three arguments: a document locator, an identity, and the document data. The document locator consists of an application name (e.g. `tutorialContract`) and the document type being created (e.g. `note`). The document data should contain values for each of the properties defined for it in the data contract (e.g. `message` for the tutorial contract's note).

Once the document has been created, we still need to submit it to [DAPI](explanation-dapi). Documents are submitted in batches that may contain multiple documents to be created, replaced, or deleted. In this example, a single document is being created. The `documentBatch` object defines the action to be completed for the document (the empty action arrays - `replace` and `delete` in this example - may be excluded and are shown for reference only here).

The `platform.documents.broadcast` method then takes the document batch and an identity parameter. Internally, it creates a [State Transition](explanation-platform-protocol-state-transition) containing the previously created document, signs the state transition, and submits the signed state transition to DAPI.