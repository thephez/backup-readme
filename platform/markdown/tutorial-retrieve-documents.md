# Overview

In this tutorial we will retrieve some of the current data from a data contract. Data is stored in the form of documents as described in the Dash Platform Protocol [Document explanation](explanation-platform-protocol-document).

## Prerequisites
- [node.js](https://nodejs.org/en/) (v12+)
- Basic familiarity with JavaScript asychronous functions using [async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
- The Dash JavaScript SDK is initialized (covered in [Connecting to a Network](tutorial-connecting-to-testnet))

# Code
[block:callout]
{
  "type": "info",
  "title": "Initializing the Client with a contract identity",
  "body": "The example below shows how access to contract documents via `<contract name>.<contract document>` syntax (e.g. `tutorialContract.note`) can be enabled by passing a contract identity to the constructor. Please refer to the [Dash JavaScript SDK documentation](https://dashevo.github.io/js-dash-sdk/#/getting-started/multiple-apps) for details."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  apps: {\n    tutorialContract: {\n      contractId: 'C96rCVpck4RdBQXG3zzP5KH4RKzfKVTsmTauu8FQenJi',\n    },\n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst getDocuments = async () => {\n  return client.platform.documents.get(\n    'tutorialContract.note',\n    {\n      limit: 2, // Only retrieve 1 document\n    },\n  );\n};\n\ngetDocuments()\n  .then((d) => {\n    for (const n of d) {\n      console.log('Document(s):\\n', n.toJSON());\n    }\n  })\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
      "language": "javascript"
    }
  ]
}
[/block]
## Queries

The example code uses a very basic query to return only one result. More extensive querying capabilities are covered in the [query syntax reference](reference-query-syntax).

# Example Document

The following examples show the structure of a `note` document (from the data contract registered in the tutorial) returned from the SDK when retrieved with various methods. 

The values returned by `.toJSON()` include the base document properties (prefixed with `$`) present in all documents along with the data contract defined properties. 
[block:callout]
{
  "type": "info",
  "body": "Note: When using `.toJSON()`, binary data is displayed as a base64 string (since JSON is a text-based format)."
}
[/block]
The values returned by `.getData()` (and also shown in the console.dir() `data` property) represent _only_ the properties defined in the `note` document described by the [tutorial data contract](tutorial-register-a-data-contract#code).
[block:code]
{
  "codes": [
    {
      "code": "{\n  '$protocolVersion': 0,\n  '$id': 'H1WzTmPnHnM8oitLVXa7oZG7ZrrunkxChGqHXdh1U8QC',\n  '$type': 'note',\n  '$dataContractId': 'C96rCVpck4RdBQXG3zzP5KH4RKzfKVTsmTauu8FQenJi',\n  '$ownerId': 'FgPx8YHbWv4rMeiP52DfmnAXGtjiCiSaNKCbgT7eCRxh',\n  '$revision': 1,\n  message: 'Tutorial CI Test @ Mon, 08 Mar 2021 14:34:45 GMT'\n}",
      "language": "json",
      "name": ".toJSON()"
    },
    {
      "code": "{\n  message: 'Tutorial CI Test @ Mon, 08 Mar 2021 14:34:45 GMT'\n}",
      "language": "json",
      "name": ".getData()"
    },
    {
      "code": "Tutorial CI Test @ Mon, 08 Mar 2021 14:34:45 GMT",
      "language": "text",
      "name": ".data.message"
    },
    {
      "code": "Document {\n  dataContract: DataContract {\n    protocolVersion: 0,\n    id: Identifier(32) [Uint8Array] [\n      165, 131, 208, 183,  34, 175, 156, 255,\n      138,  15, 177, 121, 155, 214,  14, 208,\n      114, 110, 251, 113, 225,  37, 161, 163,\n      232,   3,  56,  20,  98,  91, 250,  15\n    ],\n    ownerId: Identifier(32) [Uint8Array] [\n      218,  27, 56,   0,  87, 229, 149,  21,\n      146, 180, 72, 240, 101,  46, 163, 209,\n        9, 132, 87,   7,  55,  33, 235,  71,\n      209, 147, 84, 126, 169,  89,   8, 198\n    ],\n    schema: 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',\n    documents: { note: [Object] },\n    definitions: undefined,\n    binaryProperties: { note: {} }\n  },\n  entropy: undefined,\n  protocolVersion: 0,\n  id: Identifier(32) [Uint8Array] [\n    237, 220, 230, 196,  50, 167, 132,\n      8, 188, 140,  16,  42, 131,  66,\n     10,  36, 145, 124, 245,  37, 243,\n    139, 252, 222, 119, 221,  98, 224,\n    110, 246, 156, 117\n  ],\n  type: 'note',\n  dataContractId: Identifier(32) [Uint8Array] [\n    165, 131, 208, 183,  34, 175, 156, 255,\n    138,  15, 177, 121, 155, 214,  14, 208,\n    114, 110, 251, 113, 225,  37, 161, 163,\n    232,   3,  56,  20,  98,  91, 250,  15\n  ],\n  ownerId: Identifier(32) [Uint8Array] [\n    218,  27, 56,   0,  87, 229, 149,  21,\n    146, 180, 72, 240, 101,  46, 163, 209,\n      9, 132, 87,   7,  55,  33, 235,  71,\n    209, 147, 84, 126, 169,  89,   8, 198\n  ],\n  revision: 1,\n  data: { message: 'Tutorial CI Test @ Mon, 08 Mar 2021 14:34:45 GMT' }\n}",
      "language": "json",
      "name": "console.dir(document)"
    }
  ]
}
[/block]
# What's happening

After we initialize the Client, we request some documents. The `client.platform.documents.get` method takes two arguments: a record locator and a query object. The records locator consists of an app name (e.g. `tutorialContract`) and the top-level document type requested, (e.g. `note`).
[block:callout]
{
  "type": "info",
  "title": "DPNS Contract",
  "body": "Note: Access to the DPNS contract is built into the Dash SDK. DPNS documents may be accessed via the `dpns` app name (e.g. `dpns.domain`)."
}
[/block]
If you need more than the first 100 documents, you'll have to make additional requests with `startAt` incremented by 100 each time. In the future, the Dash SDK may return documents with paging information to make this easier and reveal how many documents are returned in total.