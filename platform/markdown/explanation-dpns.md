# Overview

Dash Platform Name Service (DPNS) is a service used to register names on Dash Platform. It is a general service that is used to provide usernames and application names but can also be extended in the future to resolve other cryptocurrency addresses, websites, and more. DPNS is implemented as an application on top of the platform and leverages platform capabilities.

##  Relationship to identities
DPNS and [Identities](explanation-identity) are closely related to each other. Identities provide a foundation that DPNS builds on to enable name-based interactions -- a user experience similar to what is found in non-cryptocurrency applications. With DPNS, platform users or application developers register a name and associate it with an identity. Once linked, the identity private key allows them to prove ownership of the name to establish trust when they interact with other users and applications.

# Details

## Name Registration Process

To prevent [front-running](https://en.wikipedia.org/wiki/Domain_name_front_running), name registration is split into a two phase process consisting of:
1. Pre-ordering the domain name
2. Registering the domain name

In the pre-order phase, the domain name is salted to obscure the actual domain name being registered (e.g. `hash('alice.dash' + salt)`) and submitted to platform. This is done to prevent masternodes from seeing the names being registered and "stealing" them for later resale. Once the pre-order receives a sufficient number of confirmations, the registration can proceed.

In the registration phase, the domain name (e.g. `alice.dash`) is once again submitted along with the salt used in the pre-order. The salt serves as proof that the registration is from the user that submitted the pre-order. This registration also references the identity being associated with the domain name to complete the identity-domain link.

## Implementation

DPNS names currently have several constraints as defined in the [DPNS data contract](https://github.com/dashevo/dpns-contract/blob/master/src/schema/dpns-documents.json). The constraints exist to maintain compatibility with DNS:
* Maximum length - 63 characters
* Character set - `0-9`, `A-Z` (case insensitive), `-`

For more implementation details, please reference the open-source JavaScript DPNS client reference implementation found in the [js-dpns-client](https://github.com/dashevo/js-dpns-client) repository. Additionally, the DPNS data contract is available in the [dpns-contract](https://github.com/dashevo/dpns-contract/blob/master/src/schema/dpns-documents.json) repository.