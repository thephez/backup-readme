# Identity Overview

Identities are a low-level construct that provide the foundation for user-facing functionality on the platform. An identity is a public key (or set of public keys) recorded on the platform chain that can be used to prove ownership of data. Please see the [Identity DIP](https://github.com/dashpay/dips/blob/master/dip-0011.md) for additional information.

Identities consist of three components that are described in further detail in the following sections:

| Field | Type | Description|
| - | - | - |
| protocolVersion | integer | The protocol version |
| id | array of bytes | The identity id (32 bytes) |
| publicKeys | array of keys | Public key(s) associated with the identity |
| balance | integer | Credit balance associated with the identity |
| revision | integer | Identity update revision |

Each identity must comply with this JSON-Schema definition established in [js-dpp](https://github.com/dashevo/platform/blob/v0.21.5/packages/js-dpp/schema/identity/identity.json):

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "protocolVersion": {
      "type": "integer",
      "$comment": "Maximum is the latest protocol version"
    },
    "id": {
      "type": "array",
      "byteArray": true,
      "minItems": 32,
      "maxItems": 32,
      "contentMediaType": "application/x.dash.dpp.identifier"
    },
    "publicKeys": {
      "type": "array",
      "minItems": 1,
      "maxItems": 32,
      "uniqueItems": true
    },
    "balance": {
      "type": "integer",
      "minimum": 0
    },
    "revision": {
      "type": "integer",
      "minimum": 0,
      "description": "Identity update revision"
  }
},
  "required": [
    "protocolVersion",
    "id",
    "publicKeys",
    "balance",
    "revision"
  ]
}
```

**Example Identity**

```json
{
  "protocolVersion": 0,
  "id": "4ZJsE1Yg8AosmC4hAeo3GJgso4N9pCoa6eCTDeXsvdhn",
  "publicKeys": [
    {
      "id": 0,
      "type": 0,
      "data": "Ao57Lp0174Svimn3OW+JUxOu/JhjhgRjBWzx9Gu/hyjo"
    }
  ],
  "balance": 0,
  "revision": 0
}
```

## Identity id

The identity `id` is calculated by Base58 encoding the double sha256 hash of the [outpoint](https://dashcore.readme.io/docs/core-additional-resources-glossary#section-outpoint) used to fund the identity creation.

`id = base58(sha256(sha256(<identity create funding output>)))`

### Example id creation

```javascript
// From the JavaScript reference implementation (js-dpp)
// IdentityCreateTransition.js
    this.identityId = new Identifier(
      hash(this.lockedOutPoint),
    );
```

**Note:** The identity `id` uses the Dash Platform specific `application/x.dash.dpp.identifier` content media type. For additional information, please refer to the [js-dpp PR 252](https://github.com/dashevo/js-dpp/pull/252) that introduced it and [Identifier.js](https://github.com/dashevo/platform/blob/v0.21.5/packages/js-dpp/lib/identifier/Identifier.js).

## Identity publicKeys

The identity `publicKeys` array stores information regarding each public key associated with the identity. Each identity must have at least one public key.

**Note:** Any public key(s) assigned to an identity must be unique (not already used by any identity).

Each item in the `publicKeys` array consists of an object containing:

| Field | Type | Description|
| - | - | - |
| id | integer | The key id (all public keys must be unique) |
| type | integer | Type of key (default: 0 - ECDSA) |
| data | array of bytes | Public key (ECDSA: 33 bytes; BLS: 48 bytes) |

Each identity public key must comply with this JSON-Schema definition established in [js-dpp](https://github.com/dashevo/platform/blob/v0.21.5/packages/js-dpp/schema/identity/publicKey.json):

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "id": {
      "type": "integer",
      "minimum": 0,
      "description": "Public key ID",
      "$comment": "Must be unique for the identity. It can’t be changed after adding a key. Included when signing state transitions to indicate which identity key was used to sign."
    },
    "type": {
      "type": "integer",
      "enum": [
        0,
        1
      ],
      "description": "Public key type. 0 - ECDSA Secp256k1, 1 - BLS 12-381",
      "$comment": "It can't be changed after adding a key"
    },
    "data": true
  },
  "allOf": [
    {
      "if": {
        "properties": {
          "type": {
            "const": 0
          }
        }
      },
      "then": {
        "properties": {
          "data": {
            "type": "array",
            "byteArray": true,
            "minItems": 33,
            "maxItems": 33,
            "description": "Raw ECDSA public key",
            "$comment": "It must be a valid key of the specified type and unique for the identity. It can’t be changed after adding a key"
          }
        }
      }
    },
    {
      "if": {
        "properties": {
          "type": {
            "const": 1
          }
        }
      },
      "then": {
        "properties": {
          "data": {
            "type": "array",
            "byteArray": true,
            "minItems": 48,
            "maxItems": 48,
            "description": "Raw BLS public key",
            "$comment": "It must be a valid key of the specified type and unique for the identity. It can’t be changed after adding a key"
          }
        }
      }
    }
  ],
  "required": [
    "id",
    "type",
    "data"
  ],
  "additionalProperties": false
}
```

### Public Key `id`

Each public key in an identity's `publicKeys` array must be assigned a unique index number (`id`).

### Public Key `type`

The `type` field indicates the algorithm used to derive the key.

| Type | Description |
| :-: | - |
| 0 | ECDSA (default) |
| 1 | BLS (currently unused)|

### Public Key `data`

The `data` field contains the compressed public key.

#### Example data encode/decode

**Encode**

```javascript
// From the JavaScript reference implementation (js-dpp)
// AbstractStateTransitionIdentitySigned.js
pubKeyBase = new PublicKey({
  ...privateKeyModel.toPublicKey().toObject(),
  compressed: true,
})
  .toBuffer();
```

**Decode**

```javascript
// From the JavaScript reference implementation (js-dpp)
// validatePublicKeysFactory.js
const dataHex = rawPublicKey.data.toString('hex');
```

## Identity balance

Each identity has a balance of credits established by value locked via a layer 1 lock transaction. This credit balance is used to pay the fees associated with state transitions.

# Identity State Transition Details

There are two identity-related state transitions: [identity create](#identity-creation) and [identity topup](#identity-topup). Details are provided in this section including information about [asset locking](#asset-lock) and [signing](#identity-state-transition-signing) required for both state transitions.

## Identity Creation

Identities are created on the platform by submitting the identity information in an identity create state transition.

| Field | Type | Description|
| - | - | - |
| protocolVersion | integer | The protocol version (currently `1`) |
| type | integer | State transition type (`2` for identity create) |
| assetLockProof | object | [Asset lock proof object](#asset-lock) proving the layer 1 locking transaction exists and is locked |
| publicKeys | array of keys | [Public key(s)](#identity-publickeys) associated with the identity |
| signature | array of bytes | Signature of state transition data (65 bytes) |

Each identity must comply with this JSON-Schema definition established in [js-dpp](https://github.com/dashevo/platform/blob/v0.21.5/packages/js-dpp/schema/identity/stateTransition/identityCreate.json):

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "protocolVersion": {
      "type": "integer",
      "$comment": "Maximum is the latest protocol version"
    },
    "type": {
      "type": "integer",
      "const": 2
    },
    "assetLockProof": {
      "type": "object"
    },
    "publicKeys": {
      "type": "array",
      "minItems": 1,
      "maxItems": 10,
      "uniqueItems": true
    },
    "signature": {
      "type": "array",
      "byteArray": true,
      "minItems": 65,
      "maxItems": 65
    }
  },
  "additionalProperties": false,
  "required": [
    "protocolVersion",
    "type",
    "assetLockProof",
    "publicKeys",
    "signature"
  ]
}
```

**Example State Transition**

```json
{
  "protocolVersion": 0,
  "type": 2,
  "signature": "Hzr8+TKH8dQ6jGpIEJkL4ZwAyEz1kXZvpMvJEREMrGNYaFcz1DeI4kdiEPAQlhHlxIEclpBV/UUqx/31t+q3f+g=",
  "assetLockProof": {
    "type": 0,
    "instantLock": "AccIHRPGVv1zaEfUuv+zMrEgMrAHmcv/ga8RcrxMJ+iwAQAAACpJWiHcEKX2be4a4yuJk+1CgdCXwlm8NV5rnIddtK9mkvj/BgcP2xnj1tpbwbWIbtoKhD7/lIEgzCOLbUh6AgFoYnwdhuzbV6CBr6johaSUBBwDiWpcL/IunPOXt3coYE+VBtMxDi4zUJYt9/honbtk+0R9e2wWz6msdoRSsaSI",
    "transaction": "0300000001c7081d13c656fd736847d4baffb332b12032b00799cbff81af1172bc4c27e8b0010000006a47304402202ee1794aef90a2bb4c3864ff907b8fcba1e35bdf8eb7cd0e13be35ff03ec76d9022007dedf1f82f971f0c8aef72c64fb4ef19e9e93e10e24ab9aa1d1a1afbf8071b10121034cd9086e5c478520e951de2b0c7921e509e3075ca7ec8ca50520cabc584b0decffffffff021027000000000000166a1415b79fb7696556717d80358a0eb91d1b87683f2018eecf2c020000001976a914ca4ca9236b42ee5704bae2c5127211cc1b077bf488ac00000000",
    "outputIndex": 0
  },
  "publicKeys": [
    {
      "id": 0,
      "type": 0,
      "data": "Axm/d8nCzwgE4WpFSWVlGLj6mWggkTmva0U8yQJYn1XS"
    }
  ]
}
```

## Identity TopUp

Identity credit balances are increased by submitting the topup information in an identity topup state transition.

| Field | Type | Description|
| - | - | - |
| protocolVersion | integer | The protocol version (currently `1`) |
| type | integer | State transition type (`3` for identity topup) |
| assetLockProof | object | [Asset lock proof object](#asset-lock) proving the layer 1 locking transaction exists and is locked |
| identityId | array of bytes | An [Identity ID](#identity-id) for the identity receiving the topup (can be any identity) (32 bytes) |
| signature | array of bytes | Signature of state transition data (65 bytes) |

Each identity must comply with this JSON-Schema definition established in [js-dpp](https://github.com/dashevo/platform/blob/v0.21.5/packages/js-dpp/schema/identity/stateTransition/identityTopUp.json):

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "protocolVersion": {
      "type": "integer",
      "$comment": "Maximum is the latest protocol version"
    },
    "type": {
      "type": "integer",
      "const": 3
    },
    "assetLockProof": {
      "type": "object"
    },
    "identityId": {
      "type": "array",
      "byteArray": true,
      "minItems": 32,
      "maxItems": 32,
      "contentMediaType": "application/x.dash.dpp.identifier"
    },
    "signature": {
      "type": "array",
      "byteArray": true,
      "minItems": 65,
      "maxItems": 65
    }
  },
  "additionalProperties": false,
  "required": [
    "protocolVersion",
    "type",
    "assetLockProof",
    "identityId",
    "signature"
  ]
}
```

**Example State Transition**

```json
{
   "protocolVersion": 0,
   "type": 3,
   "signature": "H600QTquykuG5H6XeZPaAUfnMCIKcsLha/0hlTPm/WSofsx7TH26/Xxl65E4d2mQ2ntxBaaaomGaxX/8l9inJXo=",
   "identityId": "Dnda8JuiiFAFWkaiCdfvEgfbazaccTpmV3EkfJiMHXar",
   "assetLockProof": {
      "type": 0,
      "instantLock": "ASpJWiHcEKX2be4a4yuJk+1CgdCXwlm8NV5rnIddtK9mAQAAAJoriHitzrMlv/PjjTw10sP+F/PndylOV5igJzELzPu7E1+hOPXjpuMg8T4BCRD1pdAE/ysDJMAqeycSpGJNiaxyu4REBiHWBR8FDE2qkCo2EThpWTqIF9jqhH5oMyLNPaB60mWNRrfipXm7B/dBlOs4ugeAFr8RrzCPayD2bfob",
      "transaction": "03000000012a495a21dc10a5f66dee1ae32b8993ed4281d097c259bc355e6b9c875db4af66010000006b483045022100c0f0efeb48b5cfc33031062d4111b70056a9fb5b162b27afe690a4b0582badad02205688e74e98d51210f93f48b75862934d35a797f00b46f6329b6ce7dca4e1151f012103d90d4da6b8310e7c7a4be9fa1ff75d530def3f57dc60d5995a472b0bfeccbd0bffffffff02e803000000000000166a140ecb1591376a48e36049484d2063e4202a17fa6348e6cf2c020000001976a914e3ff5db1da7e6966c54aa7d82d98a4fc5fce428888ac00000000",
      "outputIndex": 0
   }
}
```

## Asset Lock
[block:callout]
{
  "type": "info",
  "body": "Updated in Dash Platform 0.19"
}
[/block]
The [identity create](#identity-creation) and [identity topup](#identity-topup) state transitions both include an asset lock proof object. This object references the layer 1 lock transaction and includes proof that the transaction is locked.

Currently there are two types of asset lock proofs: InstantSend and ChainLock. Transactions almost always receive InstantSend locks, so the InstantSend asset lock proof is the predominate type.

### InstantSend Asset Lock Proof

The InstantSend asset lock proof is used for transactions that have received an InstantSend lock.

| Field | Type | Description|
| - | - | - |
| type | integer | The asset lock proof type (`0` for InstantSend locks) |
| instantLock | array of bytes | The InstantSend lock ([`islock`](https://dashcore.readme.io/docs/core-ref-p2p-network-instantsend-messages#islock)) |
| transaction | array of bytes | The asset lock transaction |
| outputIndex | integer | Index of the transaction output to be used |

Asset locks using an InstantSend lock as proof must comply with this JSON-Schema definition established in [js-dpp](https://github.com/dashevo/platform/blob/v0.21.5/packages/js-dpp/schema/identity/stateTransition/assetLockProof/instantAssetLockProof.json):

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "type": {
      "type": "integer",
      "const": 0
    },
    "instantLock": {
      "type": "array",
      "byteArray": true,
      "minItems": 165,
      "maxItems": 100000
    },
    "transaction": {
      "type": "array",
      "byteArray": true,
      "minItems": 1,
      "maxItems": 100000
    },
    "outputIndex": {
      "type": "integer",
      "minimum": 0
    }
  },
  "additionalProperties": false,
  "required": [
    "type",
    "instantLock",
    "transaction",
    "outputIndex"
  ]
}
```

### ChainLock Asset Lock Proof
[block:callout]
{
  "type": "success",
  "body": "Added in Dash Platform v0.19"
}
[/block]
The ChainLock asset lock proof is used for transactions that have note received an InstantSend lock, but have been included in a block that has received a ChainLock.

| Field | Type | Description|
| - | - | - |
| type | array of bytes | The type of asset lock proof (`1` for ChainLocks) |
| coreChainLockedHeight | integer | Height of the ChainLocked Core block containing the transaction  |
| outPoint | object | The  [outpoint](https://dashcore.readme.io/docs/core-additional-resources-glossary#outpoint) being used as the asset lock |

Asset locks using a ChainLock as proof must comply with this JSON-Schema definition established in [js-dpp](https://github.com/dashevo/platform/blob/v0.21.5/packages/js-dpp/schema/identity/stateTransition/assetLockProof/chainAssetLockProof.json):

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "type": {
      "type": "integer",
      "const": 1
    },
    "coreChainLockedHeight":  {
      "type": "integer",
      "minimum": 1,
      "maximum": 4294967295
    },
    "outPoint": {
      "type": "array",
      "byteArray": true,
      "minItems": 36,
      "maxItems": 36
    }
  },
  "additionalProperties": false,
  "required": [
    "type",
    "coreChainLockedHeight",
    "outPoint"
  ]
}
```

## Identity State Transition Signing

**Note:** The identity create and topup state transition signatures are unique in that they must be signed by the private key used in the layer 1 locking transaction. All other state transitions will be signed by a private key of the identity submitting them.

The process to sign an identity create state transition consists of the following steps:

1. Canonical CBOR encode the state transition data - this include all ST fields except the `signature`
2. Sign the encoded data with private key associated with a lock transaction public key
3. Set the state transition `signature` to the value of the signature created in the previous step

### Code snipits related to signing

```javascript
// From js-dpp
// AbstractStateTransition.js
// toBuffer encodes the object (excluding the signature-related fields) with canonical CBOR
const data = this.toBuffer({ skipSignature: true });
const privateKeyModel = new PrivateKey(privateKey);

this.setSignature(sign(data, privateKeyModel));

// From dashcore-lib
// signer.js
/**
* @param {Buffer} data
* @param {string|PrivateKey} privateKey
* @return {Buffer}
*/
function sign(data, privateKey) {
	var hash = doubleSha(data);
	return signHash(hash, privateKey);
}

/**
* Sign hash.
* @param {Buffer} hash
* @param {string|PrivateKey} privateKey
* @return {Buffer} - 65-bit compact signature
*/
function signHash(hash, privateKey) {
	if (typeof privateKey === 'string') {
		privateKey = new PrivateKey(privateKey);
	}

	var ecdsa = new ECDSA();
	ecdsa.hashbuf = hash;
	ecdsa.privkey = privateKey;
	ecdsa.pubkey = privateKey.toPublicKey();
	ecdsa.signRandomK();
	ecdsa.calci();
	return ecdsa.sig.toCompact();
}
```