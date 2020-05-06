# Overview

Dash Platform is based on [document-oriented database](https://en.wikipedia.org/wiki/Document-oriented_database) concepts and uses related terminology. In short, JSON documents are stored into document collections which can then be fetched back using a [query language](reference-query-syntax) similar to common document-oriented databases like MongoDB, CouchDB, or Firebase. 

Documents are defined in an application's [Data Contract](explanation-platform-protocol-data-contract) and represent the structure of application-specific data. Each document consists of one or more fields and the indices necessary to support querying.
[block:callout]
{
  "type": "danger",
  "title": "Work in Progress",
  "body": "This document is an work in progress. Details may be missing or incomplete and are subject to change."
}
[/block]
# Details

## Base Fields

Dash Platform Protocol (DPP) defines a set of base fields that must be present in all documents. For the [`js-dpp` reference implementation](https://github.com/dashevo/js-dpp/), the base fields shown below are defined in the [document base schema](https://github.com/dashevo/js-dpp/blob/v0.12.1/schema/document/documentBase.json).

| Field Name | Description |
| - | - |
| $id | Document ID |
| $type | Type of document (must match one of the document types defined in the data contract) |
| $dataContractId | Identity that registered the data contract defining this document  |
| $ownerId | Identity submitting the document  |
| $revision | Document revision |

## Data Contract Fields

Each application defines its own fields via document definitions in its data contract. Details of the DPNS data contract documents are described below as an example. This contract defines two document types (`preorder` and `domain`) and provides the functionality described in the [Name Service explanation](explanation-dpns).

| Document Name | Field Name | Data Type |
| - | - | - |
| preorder | saltedDomainHash | string |
| --- | --- | --- |
| domain | nameHash | string |
| domain | label | string |
| domain | normalizedLabel | string |
| domain | normalizedParentvDomainName | string |
| domain | preorderSalt | string |
| domain | records | object |

## Document Submission

Once a document has been created, it must be encapsulated in a State Transition to be sent to the platform. The structure of a document state transition is shown below. For additional details, see the [State Transition](explanation-platform-protocol-state-transition) explanation.

| Field Name | Description |
| - | - | 
| protocolVersion | Dash Platform Protocol version (currently `0`) |
| type | State transition type (`1` for documents) |
| ownerId | Identity submitting the document(s) |
| transitions |  Document `create`, `replace`, or `delete` transitions (up to 10 objects) |
| signaturePublicKeyId | The `id` of the identity public key that signed the state transition |
| signature | Signature of state transition data |

# Example Document

The following example shows the structure of a DPNS `domain` document as output from the document `.toJSON()` method. Note the `$` prefix indicating the base fields.
[block:code]
{
  "codes": [
    {
      "code": "{\n  '$id': 'GbMwuXdxzN3BB9efMgLPRhbbqCA5jnyfka2jnbyvGyih',\n  '$type': 'domain',\n  '$dataContractId': '295xRRRMGYyAruG39XdAibaU9jMAzxhknkkAxFE7uVkW',\n  '$ownerId': 'HjRuBghdteSmiN8w1VvGe9VHShdAxgt329xBHNQhyUGL',\n  '$revision': 1,\n  label: 'testName1234',\n  records: {\n    dashIdentity: 'HjRuBghdteSmiN8w1VvGe9VHShdAxgt329xBHNQhyUGL'\n  },\n  nameHash: '5620938ed72967fa06cfd9ebcd3bf9e5a5abf366563f5ea1f1874a531e7d78e75500',\n  preorderSalt: 'ya9GK3VwCeLxyAS9jnQX7WTkGcXMJV96mX',\n  normalizedLabel: 'testname1234',\n  normalizedParentDomainName: 'dash'\n}",
      "language": "json",
      "name": ".toJSON()"
    }
  ]
}
[/block]