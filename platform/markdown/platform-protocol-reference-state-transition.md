# State Transition Overview

 State transitions are the means for submitting data that creates, updates, or deletes platform data and results in a change to a new state. Each one must contain:
 - [Common fields](#common-fields) present in all state transitions
 - Additional fields specific to the type of action the state transition provides (e.g. [creating an identity](platform-protocol-reference-identity#identity-create-schema))

## Fees

State transition fees are paid via the credits established when an identity is created. Credits are created at a rate of [1000 credits/satoshi](https://github.com/dashevo/js-dpp/blob/v0.16.0/lib/identity/creditsConverter.js#L1). The current fee rate is [1 credit/byte](https://github.com/dashevo/js-dpp/blob/v0.16.0/lib/stateTransition/calculateStateTransitionFee.js#L1).

## Size

All serialized data (including state transitions) is limited to a maximum size of [16 KB](https://github.com/dashevo/js-dpp/blob/v0.16.0/lib/util/serializer.js#L5).

## Common Fields

All state transitions include the following fields:

| Field | Type | Description|
| - | - | - |
| protocolVersion | integer | The platform protocol version (currently `0`) |
| type | integer | State transition type:<br>`0` - [data contract](platform-protocol-reference-data-contract#data-contract-creation)<br>`1` - [documents batch](platform-protocol-reference-document#document-submission)<br>`2` - [identity create](platform-protocol-reference-identity#identity-creation)<br>`3` - [identity topup](platform-protocol-reference-identity#identity-topup) |
| signature | array of bytes | Signature of state transition data (65 bytes) |

Additionally, all state transitions except the identity create and topup state transitions include:

| Field | Type | Description|
| - | - | - |
| signaturePublicKeyId | integer | The `id` of the [identity public key](platform-protocol-reference-identity#identity-publickeys) that signed the state transition (`=> 0`)|


# State Transition Types

## Data Contract

| Field | Type | Description|
| - | - | - |
| dataContract | [data contract object](platform-protocol-reference-data-contract#data-contract-object) | Object containing valid [data contract](platform-protocol-reference-data-contract) details |
| entropy | array of bytes | Entropy used to generate the data contract ID (32 bytes) |

More detailed information about the `dataContract` object can be found in the [data contract section](platform-protocol-reference-data-contract).

### Entropy Generation

Entropy is included in [Data Contracts](platform-protocol-reference-data-contract#data-contract-creation) and [Documents](platform-protocol-reference-document#document-create-transition).

```javascript
// From the JavaScript reference implementation (js-dpp)
// generateEntropy.js
function generate() {
  return crypto.randomBytes(32);
}
```

## Documents Batch

| Field | Type | Description|
| - | - | - |
| ownerId | array of bytes | [Identity](platform-protocol-reference-identity) submitting the document(s) (32 bytes) |
| transitions | array of transition objects | Document `create`, `replace`, or `delete` transitions (up to 10 objects) |

More detailed information about the `transitions` array can be found in the [document section](platform-protocol-reference-document).

## Identity Create

| Field | Type | Description|
| - | - | - |
| lockedOutPoint | array of bytes | Lock [outpoint](https://dashcore.readme.io/docs/core-additional-resources-glossary#section-outpoint) from the layer 1 locking transaction (36 bytes) |
| publicKeys | array of keys | [Public key(s)](platform-protocol-reference-identity#identity-publickeys) associated with the identity (maximum number of keys: `10`)|

More detailed information about the `publicKeys` object can be found in the [identity section](platform-protocol-reference-identity).

## Identity TopUp

| Field | Type | Description|
| - | - | - |
| lockedOutPoint | array of bytes | Lock [outpoint](https://dashcore.readme.io/docs/core-additional-resources-glossary#section-outpoint) from the layer 1 locking transaction (36 bytes) |
| identityId | array of bytes | An [Identity ID](platform-protocol-reference-identity#identity-id) for the identity receiving the topup (can be any identity) (32 bytes) |


# State Transition Signing

State transitions must be signed by a private key associated with the identity creating the state transition.

The process to sign a state transition consists of the following steps:
1. Canonical CBOR encode the state transition data - this include all ST fields except the `signature` and `signaturePublicKeyId`
2. Sign the encoded data with a private key associated with the identity creating the state transition
3. Set the state transition `signature` to the value of the signature created in the previous step
4. For all state transitions _other than identity create or topup_, set the state transition`signaturePublicKeyId` to the [public key `id`](platform-protocol-reference-identity#public-key-id) corresponding to the key used to sign

## Signature Validation

The `signature` validation (see [js-dpp](https://github.com/dashevo/js-dpp/blob/v0.16.0/test/unit/stateTransition/validation/validateStateTransitionSignatureFactory.spec.js)) verifies that:

1. The identity has a public key
2. The identity's public key is of type `ECDSA`
3. The state transition signature is valid

The example test output below shows the necessary criteria:

```
validateStateTransitionSignatureFactory
  ✓ should pass properly signed state transition
  ✓ should return MissingPublicKeyError if the identity doesn't have a matching public key
  ✓ should return InvalidIdentityPublicKeyTypeError if type is not ECDSA_SECP256K1
  ✓ should return InvalidStateTransitionSignatureError if signature is invalid
```