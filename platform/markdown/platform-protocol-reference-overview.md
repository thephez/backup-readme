# Introduction

The Dash Platform Protocol (DPP) defines a protocol for the data objects (e.g.  [identities](platform-protocol-reference-identity), data contracts, documents, state transitions) that can be stored on [Dash's layer 2 platform](https://dashplatform.readme.io/docs/introduction-what-is-dash-platform). All data stored on Dash Platform is governed by DPP to ensure data consistency and integrity is maintained.

Dash Platform data objects consist of JSON and are validated using the JSON Schema specification via pre-defined JSON Schemas and meta-schemas described in these sections. The meta-schemas allow for creation of DPP-compliant schemas which define fields for third-party Dash Platform applications.

In addition to ensuring data complies with predefined JSON Schemas, DPP also defines rules for hashing and serialization of these objects.

> ❗️ Breaking changes
> 
> **Dash Platform Protocol 0.22**  
> A number of breaking changes were introduced in DPP 0.22. Details can be found in the [GitHub release](https://github.com/dashevo/platform/releases/tag/v0.22.0).
> 
> **Dash Platform Protocol 0.21**  
> A number of breaking changes were introduced in DPP 0.21. Details can be found in the [GitHub release](https://github.com/dashevo/js-dpp/releases/tag/v0.21.0).
> 
> **Dash Platform Protocol 0.20**  
> This release updated to a newer version of JSON Schema (2020-12 spec) and also switched to a new regex module (Re2) for improved security. More details can be found in the [GitHub release](https://github.com/dashevo/js-dpp/releases/tag/v0.20.0).

# Reference Implementation

The current reference implementation is the (JavaScript) [js-dpp](https://github.com/dashevo/platform/tree/master/packages/js-dpp) library. The schemas and meta-schemas referred to in this specification can be found here in the reference implementation: <https://github.com/dashpay/platform/tree/master/packages/js-dpp/schema>.

# Topics

[Identities](platform-protocol-reference-identity)

- [Create](platform-protocol-reference-identity#identity-creation)
- [TopUp](platform-protocol-reference-identity#identity-topup)

[Data Contracts](platform-protocol-reference-data-contract)

- [Documents](platform-protocol-reference-data-contract#data-contract-documents)
  - [Properties](platform-protocol-reference-data-contract#document-properties)
  - [Indices](platform-protocol-reference-data-contract#document-indices)
- [Definitions](platform-protocol-reference-data-contract#data-contract-definitions)

[Document](platform-protocol-reference-document)

[State Transitions](platform-protocol-reference-state-transition)

- [Overview / general structure](platform-protocol-reference-state-transition)
- Types
  - [Identity Create ST](platform-protocol-reference-identity#identity-creation)
  - [Data Contract ST](platform-protocol-reference-data-contract#data-contract-creation)
  - [Document Batch ST](platform-protocol-reference-document)
    - Document Transitions
      - [Document Transition Base](platform-protocol-reference-document#document-base-transition)
      - [Document Create Transition](platform-protocol-reference-document#document-create-transition)
      - [Document Replace Transition](platform-protocol-reference-document#document-replace-transition)
      - [Document Delete Transition](platform-protocol-reference-document#document-delete-transition)
- [Signing](platform-protocol-reference-state-transition#state-transition-signing)

[Data Triggers](platform-protocol-reference-data-trigger)