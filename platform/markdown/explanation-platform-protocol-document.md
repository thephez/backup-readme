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

Dash Platform Protocol (DPP) defines a set of base fields that must be present in all documents. For the [`js-dpp` reference implementation](https://github.com/dashevo/js-dpp/), the base fields shown below are defined in `lib/document/Document.js`.

| Field Name | Description |
| - | - |
| id | Unused in the current version of DPP (always `undefined`) |
| action | `CREATE`, `REPLACE`, or `DELETE` |
| $type | Type of document (must match one of the document types defined in the data contract) |
| $entropy | Randomness to ensure document uniqueness |
| $contractId | Application identity that registered the data contract defining this document  |
| $userId | Identity submitting the document  |
| $rev | Document revision |

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
| protocolVersion | Dash Platform Protocol version |
| type | State transition type (`2` for documents) |
| action(s) | Array containing action type for all documents |
| document(s) | Array of documents |
| signaturePublicKeyId | Index of which of the identity's public keys signed the state transition |
| signature | Signature |

# Example Document

The following example shows the structure of a DPNS `domain` document as output from the document `.toJSON()` method. Note the `$` prefix indicating the base fields.
[block:code]
{
  "codes": [
    {
      "code": "{\n  '$type': 'domain',\n  '$contractId': '2KfMcMxktKimJxAZUeZwYkFUsEcAZhDKEpQs8GMnpUse',\n  '$userId': 'Bb2p582MFR1tQhVQHKrScsAJH6Erqsb6SoroD9dQhJ5e',\n  '$entropy': 'yU6o6YUbhQpA52Cksudxv9vycFHunHvmvp',\n  '$rev': 1,\n  label: 'Alejandro84',\n  records: { \n    dashIdentity: 'Bb2p582MFR1tQhVQHKrScsAJH6Erqsb6SoroD9dQhJ5e'\n  },\n  nameHash: '5610616c656a616e64726f38342e64617368',\n  preorderSalt: 'yTYLYcWnzgcWkwWBwG4M2LZm5SGi2Wf1Rc',\n  normalizedLabel: 'alejandro84',\n  normalizedParentDomainName: 'dash' \n}",
      "language": "json",
      "name": ".toJSON()"
    }
  ]
}
[/block]