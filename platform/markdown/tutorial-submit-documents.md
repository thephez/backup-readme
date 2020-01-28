# Overview

In this tutorial we will submit some data to an application on Dash Platform. Data is stored in the form of [documents](explanation-platform-protocol-document) which are encapsulated in a [state transition](explanation-platform-state-transition) before being submitted to DAPI. 

## Prerequisites
- [node.js](https://nodejs.org/en/)
- Basic familiarity with JavaScript asychronous functions using [async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
- The DashJS SDK is initialized (covered in [Connecting to EvoNet](tutorial-connecting-to-evonet))

# Code
[block:callout]
{
  "type": "info",
  "body": "The following code demonstrates submitting a DPNS preorder document for informational purposes. This represent the first step in the process of registering a DPNS name. To actually register a name, the SDK provides an easier way that can be found in the [name registration tutorial](tutorial-register-a-name-for-an-identity).",
  "title": "Example Information"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "const DashJS = require('dash');\n\nconst sdkOpts = {\n  network: 'testnet',\n  mnemonic: 'a Dash wallet mnemonic with evonet funds goes here',\n};\nconst sdk = new DashJS.SDK(sdkOpts);\n\nconst submitPreorderDocument = async function () {\n  const platform = sdk.platform;\n  await sdk.isReady();\n\n  try {\n    const identity = await platform.identities.get('an identity ID goes here');\n\n    docProperties = {\n      saltedDomainHash: 'a previously calculated hash'\n    }\n\n    // Create the preorder document\n    const preorderDocument = await platform.documents.create(\n      'dpns.preorder',\n      identity,\n      docProperties,\n    );\n\n    // Sign and submit the document\n    await platform.documents.broadcast(preorderDocument, identity);\n    \n    // Retrieve the submitted document\n    const retrievedPreorder = await platform.documents.get(\n      'dpns.preorder',\n      {\n        where: [['saltedDomainHash', '==', docProperties.saltedDomainHash]]\n      }\n    );\n    console.dir({retrievedPreorder}, {depth:10});\n  } catch (e) {\n    console.error('Something went wrong:', e);\n  } finally {\n    sdk.disconnect();\n  }\n};\n\nsubmitPreorderDocument();\n",
      "language": "javascript"
    }
  ]
}
[/block]
# What's happening

After we initialize the SDK, we create a document that matches the structure defined by the data contract of the application being referenced (e.g. a `preorder` document for the DPNS application). The `platform.documents.create` method takes three arguments: a document locator, an identity, and the document data. The document locator consists of an application name (e.g. `dpns`) and the document type being created (e.g. `preorder`). The document data should contain values for each of the properties defined for it in the data contract (e.g. `saltedDomainHash` for the DPNS preorder).

Once the document has been created, we still need to submit it to [DAPI](explanation-dapi). The `platform.documents.broadcast` method takes a document and an identity parameter. Internally, it creates a [State Transition](explanation-platform-protocol-state-transition) containing the previously created document, signs the state transition, and submits the signed state transition to DAPI.