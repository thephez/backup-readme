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
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  apps: {\n    tutorialContract: {\n      contractId: '6Ti3c7nvD1gDf4gFi8a3FfZVhVLiRsGLnQ7nCAF74osi',\n    },\n  },\n};\nconst client = new Dash.Client(clientOpts);\n\nconst getDocuments = async () => {\n  return client.platform.documents.get(\n    'tutorialContract.note',\n    {\n      limit: 1, // Only retrieve 1 document\n    },\n  );\n};\n\ngetDocuments()\n  .then((d) => console.log(d))\n  .catch((e) => console.error('Something went wrong:\\n', e))\n  .finally(() => client.disconnect());",
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
      "code": "{\n  '$protocolVersion': 0,\n  '$id': 'Et9C82SG78WcqSZLmkWhvFZeiqP1BYgZgxqmuau56UC2',\n  '$type': 'note',\n  '$dataContractId': '6Ti3c7nvD1gDf4gFi8a3FfZVhVLiRsGLnQ7nCAF74osi',\n  '$ownerId': 'BSNxbs99zCFvEtK8qrewYTVxNKt4DnFQ8sPbsb8nrDuf',\n  '$revision': 1,\n  message: 'Tutorial Test @ Wed, 30 Dec 2020 21:12:38 GMT'\n}",
      "language": "json",
      "name": ".toJSON()"
    },
    {
      "code": "{ \n  message: 'Tutorial Test @ Wed, 30 Dec 2020 21:12:38 GMT'\n}",
      "language": "json",
      "name": ".getData()"
    },
    {
      "code": "Tutorial Test @ Wed, 30 Dec 2020 21:12:38 GMT",
      "language": "text",
      "name": ".data.message"
    },
    {
      "code": "Document {\n  dataContract: DataContract {\n    protocolVersion: 0,\n    id: Identifier(32) [Uint8Array] [\n       81,  33, 241, 255, 193, 250, 149,\n      155, 217,  24, 166, 198, 150, 211,\n      253, 161, 110,  16, 130, 113, 143,\n      235,  90, 187, 208, 144, 254, 135,\n      202, 219, 252,  77\n    ],\n    ownerId: Identifier(32) [Uint8Array] [\n      155,  21,  55, 220,  49, 179, 219, 164,\n      204,   4, 181, 180, 166,  51,  68, 211,\n      175, 145,  90, 105, 155, 124,  42, 202,\n      108, 229, 199,  92,  17, 107,  61, 246\n    ],\n    schema: 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',\n    documents: { note: [Object] },\n    definitions: undefined,\n    binaryProperties: { note: {} }\n  },\n  entropy: undefined,\n  protocolVersion: 0,\n  id: Identifier(32) [Uint8Array] [\n    206,  65, 208,  81,  54,  13,  34, 238,\n     88, 173,  41,  27, 148, 188, 237,  44,\n     67, 166, 206,  63, 213,  91, 158, 204,\n    173, 102, 101, 151,  39,   9, 113,  19\n  ],\n  type: 'note',\n  dataContractId: Identifier(32) [Uint8Array] [\n     81,  33, 241, 255, 193, 250, 149,\n    155, 217,  24, 166, 198, 150, 211,\n    253, 161, 110,  16, 130, 113, 143,\n    235,  90, 187, 208, 144, 254, 135,\n    202, 219, 252,  77\n  ],\n  ownerId: Identifier(32) [Uint8Array] [\n    155,  21,  55, 220,  49, 179, 219, 164,\n    204,   4, 181, 180, 166,  51,  68, 211,\n    175, 145,  90, 105, 155, 124,  42, 202,\n    108, 229, 199,  92,  17, 107,  61, 246\n  ],\n  revision: 1,\n  data: { message: 'Tutorial Test @ Wed, 30 Dec 2020 21:12:38 GMT' }\n}",
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