# Overview

In this tutorial we will update delete data from Dash Platform. Data is stored in the form of [documents](explanation-platform-protocol-document) which are encapsulated in a [state transition](explanation-platform-protocol-state-transition) before being submitted to DAPI. 

## Prerequisites
- [node.js](https://nodejs.org/en/) (v12+)
- Basic familiarity with JavaScript asychronous functions using [async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
- The Dash JavaScript SDK is initialized (covered in [Connecting to EvoNet](tutorial-connecting-to-evonet))
- Access to a previously created document (e.g., one created using the [Submit Documents tutorial](tutorial-submit-documents))

# Code
[block:callout]
{
  "type": "info",
  "title": "Initializing the Client with a contract identity",
  "body": "The example below shows how access to contract documents via `<contract name>.<contract document>` syntax (e.g. `tutorialContract.note`) can be enabled by passing a contract identity to the constructor. Please refer to the [Dash SDK documentation](https://dashevo.github.io/js-dash-sdk/#/getting-started/multiple-apps) for details."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'evonet',\n  wallet: {\n    mnemonic: 'a Dash wallet mnemonic with evonet funds goes here',\n  },\n  apps: {\n    tutorialContract: {\n      contractId: 'Q894cs83D8REQNo7mAetj1wPJK2W3svrwqaN61aP25W',\n    },\n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst deleteNoteDocument = async () => {\n  const platform = client.platform;\n  const identity = await platform.identities.get('an identity ID goes here');\n  const documentId = 'an existing document ID goes here';\n\n  // Retrieve the existing document\n  const [document] = await client.platform.documents.get(\n    'tutorialContract.note',\n    { where: [['$id', '==', documentId]] },\n  );\n\n  // Sign and submit the document delete transition\n  return platform.documents.broadcast({ delete: [document] }, identity);\n};\n\ndeleteNoteDocument()\n  .then((d) => console.log('Document deleted:\\n', d))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript"
    }
  ]
}
[/block]
# What's happening

After we initialize the Client, we retrieve the document to be deleted via `platform.documents.get` using its `id`. 

Once the document has been retrieved, we must submit it to [DAPI](explanation-dapi). Documents are submitted in batches that may contain multiple documents to be created, replaced, or deleted. In this example, a single document is being deleted.

The `platform.documents.broadcast` method takes the document batch (e.g. `{delete: [documents[0]]}`) and an identity parameter. Internally, it creates a [State Transition](explanation-platform-protocol-state-transition) containing the previously created document, signs the state transition, and submits the signed state transition to DAPI.