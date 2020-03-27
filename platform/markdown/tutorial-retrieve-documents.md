# Overview

In this tutorial we will retrieve some of the current data from a data contract. Data is stored in the form of documents as described in the Dash Platform Protocol [Document explanation](explanation-platform-protocol-document).

## Prerequisites
- [node.js](https://nodejs.org/en/)
- Basic familiarity with JavaScript asychronous functions using [async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
- The Dash JavaScript SDK is initialized (covered in [Connecting to EvoNet](tutorial-connecting-to-evonet))

# Code
[block:code]
{
  "codes": [
    {
      "code": "const Dash = require('dash');\n\nconst clientOpts = {\n  network: 'testnet'\n};\nconst client = new Dash.Client(clientOpts);\n\nconst getDocuments = async function () {\n  try {\n    await client.isReady();\n    const documents = await client.platform.documents.get('dpns.domain', {\n      where: [\n        ['normalizedParentDomainName', '==', 'dash']\n      ],\n      startAt: 0\n    });\n    console.log(documents);\n  } catch (e) {\n    console.error('Something went wrong:', e);\n  } finally {\n    client.disconnect();\n  }\n};\n\ngetDocuments();",
      "language": "javascript"
    }
  ]
}
[/block]
# Example Document

The following examples show the structure of a DPNS `domain` document returned from the SDK when retrieved with various methods. 

The values returned by `.toJSON()` include the base document properties (prefixed with `$`) present in all documents along with the data contract defined properties. 

The values returned by `.getData()` (and also shown in the console.dir() `data` property) represent _only_ the properties defined in the DPNS `domain` document described by the [DPNS data contract](https://github.com/dashevo/dpns-contract/blob/master/src/schema/dpns-documents.json#L22-L56).
[block:code]
{
  "codes": [
    {
      "code": "{\n  '$type': 'domain',\n  '$contractId': '2KfMcMxktKimJxAZUeZwYkFUsEcAZhDKEpQs8GMnpUse',\n  '$userId': 'Bb2p582MFR1tQhVQHKrScsAJH6Erqsb6SoroD9dQhJ5e',\n  '$entropy': 'yU6o6YUbhQpA52Cksudxv9vycFHunHvmvp',\n  '$rev': 1,\n  label: 'Alejandro84',\n  records: { \n    dashIdentity: 'Bb2p582MFR1tQhVQHKrScsAJH6Erqsb6SoroD9dQhJ5e'\n  },\n  nameHash: '5610616c656a616e64726f38342e64617368',\n  preorderSalt: 'yTYLYcWnzgcWkwWBwG4M2LZm5SGi2Wf1Rc',\n  normalizedLabel: 'alejandro84',\n  normalizedParentDomainName: 'dash' \n}",
      "language": "json",
      "name": ".toJSON()"
    },
    {
      "code": "{\n  label: 'Alejandro84',\n  records: {\n    dashIdentity: 'Bb2p582MFR1tQhVQHKrScsAJH6Erqsb6SoroD9dQhJ5e'\n  },\n  nameHash: '5610616c656a616e64726f38342e64617368',\n  preorderSalt: 'yTYLYcWnzgcWkwWBwG4M2LZm5SGi2Wf1Rc',\n  normalizedLabel: 'alejandro84',\n  normalizedParentDomainName: 'dash'\n}",
      "language": "json",
      "name": ".getData()"
    },
    {
      "code": "Alejandro84",
      "language": "text",
      "name": ".data.label"
    },
    {
      "code": "Document {\n    id: undefined,\n    action: undefined,\n    type: 'domain',\n    entropy: 'yU6o6YUbhQpA52Cksudxv9vycFHunHvmvp',\n    contractId: '2KfMcMxktKimJxAZUeZwYkFUsEcAZhDKEpQs8GMnpUse',\n    userId: 'Bb2p582MFR1tQhVQHKrScsAJH6Erqsb6SoroD9dQhJ5e',\n    revision: 1,\n    data: {\n      label: 'Alejandro84',\n      records: {\n        dashIdentity: 'Bb2p582MFR1tQhVQHKrScsAJH6Erqsb6SoroD9dQhJ5e' \n      },\n      nameHash: '5610616c656a616e64726f38342e64617368',\n      preorderSalt: 'yTYLYcWnzgcWkwWBwG4M2LZm5SGi2Wf1Rc',\n      normalizedLabel: 'alejandro84',\n      normalizedParentDomainName: 'dash'\n    }\n  }",
      "language": "json",
      "name": "console.dir(document)"
    }
  ]
}
[/block]
# What's happening

After we initialize the Client, we request some documents. The `client.platform.documents.get` method takes two arguments: a record locator and a query object. The records locator consists of an app name (e.g. `dpns` - the DPNS contract is automatically included with DashJS) and the top-level document type requested, (e.g. `domain`).

If you need more than the first 100 documents, you'll have to make additional requests with `startAt` incremented by 100 each time. In the future, DashJS may return documents with paging information to make this easier and reveal how many documents are returned in total.