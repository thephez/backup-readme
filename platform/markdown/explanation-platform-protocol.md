# Overview

To ensure the consistency and integrity of data stored on Layer 2, all data is governed by the Dash Platform Protocol (DPP). Dash Platform Protocol describes serialization and validation rules for the platform's 3 core data structures: data contracts, documents, and state transitions. Each of these structures are briefly described below.

# Structure Descriptions

## Data Contract
A data contract is a database schema that a developer needs to register with the platform in order to start using any decentralized storage functionality. Data contracts are described using the JSON Schema language and must follow some basic rules as described in the platform protocol repository. Contracts are serialized to binary form using [CBOR](https://cbor.io/).

For additional detail, see the [Data Contract](explanation-platform-protocol-data-contract) explanation.

## Document
A document is an atomic entity used by the platform to store user-submitted data. It resembles the documents stored in a document-oriented DB (e.g. MongoDB). All documents must follow some specific rules that are defined by a generic document schema. Additionally, documents are always related to a particular application, so they must comply with the rules defined by the application’s data contract. Documents are submitted to the platform API ([DAPI](explanation-dapi)) by users during their use of the application.

[block:callout]
{
  "type": "info",
  "body": "Information about document-oriented databases can be found on the [MongoDB site](https://www.mongodb.com/document-databases) and in this [Wikipedia article](https://en.wikipedia.org/wiki/Document-oriented_database).",
  "title": "Document-Oriented Databases"
}
[/block]
For additional detail, see the [Document](explanation-platform-protocol-document) explanation.

## State Transition
A state transition represents a change made by a user to the application and platform states. It consists of either an array of documents or one data contract, the ID of the application to which the change is made, and a user signature. The user signature is made for the binary representation of the state transition using the private key associated with a user identity. A state transition is constructed by the client-side library when the user creates documents and submits them to the platform API.

For additional detail, see the [State Transition](explanation-platform-protocol-state-transition) explanation.