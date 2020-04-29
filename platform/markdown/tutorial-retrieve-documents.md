# Overview

In this tutorial we will retrieve some of the current data from a data contract. Data is stored in the form of documents as described in the Dash Platform Protocol [Document explanation](explanation-platform-protocol-document).

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
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet',\n  apps: {\n    tutorialContract: {\n      contractId: '5wpZAEWndYcTeuwZpkmSa8s49cHXU5q2DhdibesxFSu8'\n    }\n  }\n};\nconst client = new Dash.Client(clientOpts);\n\nconst getDocuments = async function () {\n  try {\n    await client.isReady();\n    \n    const queryOpts = {\n      limit: 1 // Only retrieve 1 document\n    };\n    const documents = await client.platform.documents.get(\n      'tutorialContract.note',\n      queryOpts\n    );\n    console.log(documents);\n  } catch (e) {\n    console.error('Something went wrong:', e);\n  } finally {\n    client.disconnect();\n  }\n};\n\ngetDocuments();",
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

The values returned by `.getData()` (and also shown in the console.dir() `data` property) represent _only_ the properties defined in the `note` document described by the [tutorial data contract](tutorial-register-a-data-contract#section-code).
[block:code]
{
  "codes": [
    {
      "code": "{\n  'id': \n  '$type': 'note',\n  '$dataContractId': '5wpZAEWndYcTeuwZpkmSa8s49cHXU5q2DhdibesxFSu8',\n  '$ownerId': 'HcgaeTzwiwGMTpYFDBJuKERv8kjbDS2oDGDkQ4SN4Mi1',\n  '$revision': 1,\n  message: 'Tutorial Test @ Wed, 25 Mar 2020 16:42:50 GMT'\n}",
      "language": "json",
      "name": ".toJSON()"
    },
    {
      "code": "{\n  message: 'Tutorial Test @ Wed, 25 Mar 2020 16:42:50 GMT'\n}",
      "language": "json",
      "name": ".getData()"
    },
    {
      "code": "Tutorial Test @ Wed, 25 Mar 2020 16:42:50 GMT",
      "language": "text",
      "name": ".data.label"
    },
    {
      "code": "Document {\n  entropy: undefined,\n  id: '5Zqim5LkL76dBMqa1kE2AFRng2yqpgyVTKK6kTqWbYmu',\n  type: 'note',\n  dataContractId: '5wpZAEWndYcTeuwZpkmSa8s49cHXU5q2DhdibesxFSu8',\n  ownerId: 'HcgaeTzwiwGMTpYFDBJuKERv8kjbDS2oDGDkQ4SN4Mi1',\n  revision: 1,\n  data: {\n    message: 'Tutorial Test @ Wed, 25 Mar 2020 16:42:50 GMT'\n  }\n}",
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
  "body": "Note: Access to the DPNS contract is built into DashJS. DPNS documents may be accessed via the `dpns` app name (e.g. `dpns.domain`)."
}
[/block]
If you need more than the first 100 documents, you'll have to make additional requests with `startAt` incremented by 100 each time. In the future, DashJS may return documents with paging information to make this easier and reveal how many documents are returned in total.