# Overview

In this tutorial we will retrieve some of the current data from a data contract. Data is stored in the form of documents as described in the Dash Platform Protocol [Document explanation](explanation-platform-protocol-document).

## Prerequisites
- [node.js](https://nodejs.org/en/) (v12+)
- Basic familiarity with JavaScript asychronous functions using [async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
- The Dash JavaScript SDK is initialized (covered in [Connecting to EvoNet](tutorial-connecting-to-evonet))

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
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  apps: {\n    tutorialContract: {\n      contractId: 'Q894cs83D8REQNo7mAetj1wPJK2W3svrwqaN61aP25W',\n    },\n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst getDocuments = async () => {\n  return client.platform.documents.get(\n    'tutorialContract.note',\n    {\n      limit: 1, // Only retrieve 1 document\n    },\n  );\n};\n\ngetDocuments()\n  .then((d) => console.log(d))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
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
      "code": "{\n  '$protocolVersion': 0,\n  '$id': '4K9T3mJPeDVJrSzWWgf82ngcjqpkaYBTx6Qgg9Mr6VA5',\n  '$type': 'note',\n  '$dataContractId': 'Q894cs83D8REQNo7mAetj1wPJK2W3svrwqaN61aP25W',\n  '$ownerId': 'BhC9M3fQHyUCyuxH4WHdhn1VGgJ4JTLmer8qmTTHkYTe',\n  '$revision': 1,\n  message: 'Tutorial Test @ Thu, 29 Oct 2020 20:39:40 GMT'\n}",
      "language": "json",
      "name": ".toJSON()"
    },
    {
      "code": "{\n  message: 'Tutorial Test @ Thu, 29 Oct 2020 20:39:40 GMT'\n}",
      "language": "json",
      "name": ".getData()"
    },
    {
      "code": "Tutorial Test @ Thu, 29 Oct 2020 20:39:40 GMT",
      "language": "text",
      "name": ".data.message"
    },
    {
      "code": "Document {\n  dataContract: DataContract {\n    protocolVersion: 0,\n    id: Identifier(32) [Uint8Array] [\n        5, 236, 110,  66, 246, 123, 205, 227,\n       74, 231,  87, 141, 168, 144, 212,  48,\n      224, 134,  31,  51, 194, 251,   7, 199,\n      174, 122, 163, 170, 200,  63,   9, 105\n    ],\n    ownerId: Identifier(32) [Uint8Array] [\n      158, 224, 179, 186, 104,  63, 179, 146,\n      194, 213,  42, 101, 129, 188, 237,  15,\n       21, 125,   1, 246,  65, 175,  92,  13,\n      146,  83,  56, 204, 175,  49, 103, 221\n    ],\n    schema: 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',\n    documents: { note: [Object] },\n    definitions: undefined,\n    binaryProperties: { note: {} }\n  },\n  entropy: undefined,\n  protocolVersion: 0,\n  id: Identifier(32) [Uint8Array] [\n     49,  57,  17, 154, 188, 135, 185, 215,\n    168, 240, 118, 208, 187, 133, 159,  11,\n    124, 204, 153, 237,  33, 101,  28,  76,\n    121,  20, 157, 193,  53,   7,  57, 214\n  ],\n  type: 'note',\n  dataContractId: Identifier(32) [Uint8Array] [\n      5, 236, 110,  66, 246, 123, 205, 227,\n     74, 231,  87, 141, 168, 144, 212,  48,\n    224, 134,  31,  51, 194, 251,   7, 199,\n    174, 122, 163, 170, 200,  63,   9, 105\n  ],\n  ownerId: Identifier(32) [Uint8Array] [\n    158, 224, 179, 186, 104,  63, 179, 146,\n    194, 213,  42, 101, 129, 188, 237,  15,\n     21, 125,   1, 246,  65, 175,  92,  13,\n    146,  83,  56, 204, 175,  49, 103, 221\n  ],\n  revision: 1,\n  data: { message: 'Tutorial Test @ Thu, 29 Oct 2020 20:39:40 GMT' }\n}",
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