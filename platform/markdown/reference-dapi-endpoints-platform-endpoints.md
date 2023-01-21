Please refer to the [gRPC Overview](reference-dapi-endpoints-grpc-overview) for details regarding running the examples shown below, encoding/decoding the request/response data, and clients available for several languages.

# Data Proofs and Metadata

Since Dash Platform 0.20.0, Platform gRPC endpoints can provide [proofs](https://github.com/dashevo/platform/blob/master/packages/dapi-grpc/protos/platform/v0/platform.proto#L16-L28) so the data returned for a request can be verified as being valid. Full support is not yet available in the JavaScript client, but can be used via the low level [dapi-grpc library](https://github.com/dashevo/platform/tree/master/packages/dapi-grpc). Additional information about proofs is available on the [Platform Proofs page](reference-platform-proofs).

Some [additional metadata](https://github.com/dashevo/platform/blob/master/packages/dapi-grpc/protos/platform/v0/platform.proto#L30-L33) is also provided with responses:
 - `height`: the last committed platform chain height
 - `coreChainLockedHeight`: height of the most recent ChainLock on the core chain

# Endpoint Details

## broadcastStateTransition

> 📘 
>
> **Note:** The [`waitForStateTransitionResult` endpoint](#waitforstatetransitionresult) should be used in conjunction with this one for instances where proof of block confirmation is required.

Broadcasts a [state transition](explanation-platform-protocol-state-transition) to the platform via DAPI to make a change to layer 2 data. Since Dash Platform 0.18.0, `broadcastStateTransition` returns once the state transition has been accepted into the mempool instead of waiting until it is confirmed. 

**Returns**: Nothing or error
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `state_transition` | Bytes (Base64) | Yes | A [state transition](explanation-platform-protocol-state-transition) |

** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst DashPlatformProtocol = require('@dashevo/dpp');\n\nconst client = new DAPIClient();\nconst dpp = new DashPlatformProtocol();\n\n// Data Contract Create State Transition (JSON)\n// Replace with your own state transition object before running\nconst stateTransitionObject = {\n  protocolVersion: 0,\n  type: 0,\n  signature: 'HxAipUsLWQBE++C1suSRNQiQh91rI1LZbblvQhk2erUaIvRneAagxGYYsXXYNvEeO+lBzlF1a9KHGGTHgnO/8Ts=',\n  signaturePublicKeyId: 0,\n  dataContract: {\n    protocolVersion: 0,\n    '$id': 'CMc7RghKkHeHtFdwfSX5Hzy7CUdpCEJnwsbfHdsbmJ32',\n    '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',\n    ownerId: '8Z3ps3tNoGoPEDYerUNCd4yi7zDwgBh2ejgSMExxvkfD',\n    documents: {\n      note: {\n        properties: { message: { type: 'string' } },\n        additionalProperties: false,\n      },\n    },\n  },\n  entropy: '+RqUArypdL8f/gCMAo4b6c3CoQvxHzsQG0BdYrT5QT0=',\n};\n\n// Convert signature and entropy to buffer\nstateTransitionObject.signature = Buffer.from(stateTransitionObject.signature, 'base64');\nstateTransitionObject.entropy = Buffer.from(stateTransitionObject.entropy, 'base64');\n\ndpp.stateTransition.createFromObject(stateTransitionObject, { skipValidation: true })\n  .then((stateTransition) => {\n    client.platform.broadcastStateTransition(stateTransition.toBuffer())\n      .then(() => console.log('State Transition broadcast successfully'));\n  });",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    BroadcastStateTransitionRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst DashPlatformProtocol = require('@dashevo/dpp');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst dpp = new DashPlatformProtocol();\n\n// Data Contract Create State Transition (JSON)\n// Replace with your own state transition object before running\nconst stateTransitionObject = {\n  protocolVersion: 0,\n  type: 0,\n  signature: 'HxAipUsLWQBE++C1suSRNQiQh91rI1LZbblvQhk2erUaIvRneAagxGYYsXXYNvEeO+lBzlF1a9KHGGTHgnO/8Ts=',\n  signaturePublicKeyId: 0,\n  dataContract: {\n    protocolVersion: 0,\n    '$id': 'CMc7RghKkHeHtFdwfSX5Hzy7CUdpCEJnwsbfHdsbmJ32',\n    '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',\n    ownerId: '8Z3ps3tNoGoPEDYerUNCd4yi7zDwgBh2ejgSMExxvkfD',\n    documents: {\n      note: {\n        properties: { message: { type: 'string' } },\n        additionalProperties: false,\n      },\n    },\n  },\n  entropy: '+RqUArypdL8f/gCMAo4b6c3CoQvxHzsQG0BdYrT5QT0=',\n};\n\n// Convert signature and entropy to buffer\nstateTransitionObject.signature = Buffer.from(stateTransitionObject.signature, 'base64');\nstateTransitionObject.entropy = Buffer.from(stateTransitionObject.entropy, 'base64');\n\nconst broadcastStateTransitionRequest = new BroadcastStateTransitionRequest();\n\ndpp.stateTransition.createFromObject(stateTransitionObject, { skipValidation: true })\n  .then((stateTransition) => {\n    console.log(stateTransition);\n    broadcastStateTransitionRequest.setStateTransition(stateTransition.toBuffer());\n\n    platformPromiseClient.broadcastStateTransition(broadcastStateTransitionRequest)\n      .then(() => console.log('State Transition broadcast successfully'))\n      .catch((e) => {\n        console.error(e);\n        console.error(e.metadata);\n      });\n  })\n  .catch((e) => console.error(e));\n",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# Submit an identity create State Transition\n# `state_transition` must be represented in base64\n# Replace `state_transition` with your own state transition object before running\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"state_transition\":\"pWR0eXBlAmlzaWduYXR1cmV4WEg3TWhFWDQ0Z3JzMVIwTE9XTU5IZjAxWFNpYVFQcUlVZ1JLRXQyMkxHVERsUlUrZ1BwQUlUZk5JUmhXd3IvYTVHd0lzWm1idGdYVVFxcVhjbW9lQWtUOD1qcHVibGljS2V5c4GkYmlkAGRkYXRheCxBdzh2UmYxeFFCTlVLbzNiY2llaHlaR2NhM0hBSThkY0ZvVWJTK3hLb0lITmR0eXBlAGlpc0VuYWJsZWT1bmxvY2tlZE91dFBvaW50eDBLT1VUSHB5YnFPek9DNnhEVUhFWm9uc1lNSVpqcGppTHFZNnkxYmlWNWxRQUFBQUFvcHJvdG9jb2xWZXJzaW9uAA==\"\n\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/broadcastStateTransition",
      "language": "shell",
      "name": "gRPCurl"
    }
  ]
}
[/block]
**Response**: No response except on error

## getIdentity

> 🚧 Breaking changes
>
> As of Dash Platform 0.21 the `protocolVersion` is no longer included in the CBOR-encoded data. It is instead prepended to the data following CBOR encoding.

**Returns**: [Identity](explanation-identity) information for the requested identity
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `id` | Bytes | Yes | An identity `id` |
| `prove` | Boolean | No | Set to `true` to receive a proof that contains the requested identity |

> 📘 
>
> **Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-platform-proofs) for details on decoding the data.

** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient();\n\nconst identityId = Identifier.from('4EfA9Jrvv3nnCFdSf7fad59851iiTRZ6Wcu6YVJ4iSeF');\nclient.platform.getIdentity(identityId).then((response) => {\n  // Strip off protocol version (leading 4 bytes) and decode\n  const identity = cbor.decode(response.identity.slice(4, response.identity.length));\n  console.log(identity);\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n  },\n} = require('@dashevo/dapi-grpc');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst id = Identifier.from('4EfA9Jrvv3nnCFdSf7fad59851iiTRZ6Wcu6YVJ4iSeF');\nconst idBuffer = Buffer.from(id);\nconst getIdentityRequest = new GetIdentityRequest();\ngetIdentityRequest.setId(idBuffer);\ngetIdentityRequest.setProve(false);\n\nplatformPromiseClient.getIdentity(getIdentityRequest)\n  .then((response) => {\n    const identityResponse = response.getIdentity();\n    const identityBuffer = Buffer.from(identityResponse);\n    // Strip off protocol version (leading 4 bytes) and decode\n    console.log(cbor.decode(identityBuffer.slice(4, identityBuffer.length)));\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"MBLBm5jsADOt2zbNZLf1EGcPKjUaQwS19plBRChu/aw=\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentity",
      "language": "shell",
      "name": "gRPCurl"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  id: <Buffer 30 12 c1 9b 98 ec 00 33 ad db 36 cd 64 b7 f5 10 67 0f 2a 35 1a 43 04 b5 f6 99 41 44 28 6e fd ac>,\n  balance: 0,\n  revision: 0,\n  publicKeys: [\n    {\n      id: 0,\n      data: <Buffer 03 8b 74 ae a1 04 c1 94 63 b7 4b e5 fa e9 af 22 55 fe 42 01 3a ec d1 70 92 46 42 14 f2 86 7a c1 9b>,\n      type: 0,\n      purpose: 0,\n      readOnly: false,\n      securityLevel: 0\n    },\n    {\n      id: 1,\n      data: <Buffer 02 9d f4 53 a6 26 cd e5 01 f4 54 b4 4a 9b ee b8 e5 25 59 0c 94 ef 2e 78 74 39 93 00 66 51 fc ec 4f>,\n      type: 0,\n      purpose: 0,\n      readOnly: false,\n      securityLevel: 2\n    }\n  ]\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"identity\": \"AQAAAKRiaWRYIDASwZuY7AAzrds2zWS39RBnDyo1GkMEtfaZQUQobv2sZ2JhbGFuY2UAaHJldmlzaW9uAGpwdWJsaWNLZXlzgqZiaWQAZGRhdGFYIQOLdK6hBMGUY7dL5frpryJV/kIBOuzRcJJGQhTyhnrBm2R0eXBlAGdwdXJwb3NlAGhyZWFkT25sefRtc2VjdXJpdHlMZXZlbACmYmlkAWRkYXRhWCECnfRTpibN5QH0VLRKm+645SVZDJTvLnh0OZMAZlH87E9kdHlwZQBncHVycG9zZQBocmVhZE9ubHn0bXNlY3VyaXR5TGV2ZWwC\",\n  \"metadata\": {\n    \"height\": \"1190\",\n    \"coreChainLockedHeight\": 802899\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    }
  ]
}
[/block]
## getIdentitiesByPublicKeyHashes

> 👍 
> 
> In Dash Platform v0.23 the response is an array of identities instead of the previously provided array of CBOR-encoded arrays of identities.

**Returns**: [Identity](explanation-identity) an array of identities associated with the provided public key hashes
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `public_key_hashes` | Bytes | Yes | Public key hashes (sha256-ripemd160) of identity public keys |
| `prove` | Boolean | No | Set to `true` to receive a proof that contains the requested identities |

> 📘 
>
> **Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-platform-proofs) for details on decoding the data.

> 📘 Public key hash
>
> Note: the hash must be done using all fields of the identity public key object - e.g.
> 
> ```json
> {
>   id: 0,
>   type: 0,
>   purpose: 0,
>   securityLevel: 0,
>   data: 'A2GTAJk9eAWkMXVCb+rRKXH99POtR5OaW6zqZl7/yozp',
>   readOnly: false
> }
> ```
> 
> When using the js-dpp library, the hash can be accessed via the [IdentityPublicKey object's](https://github.com/dashevo/platform/blob/master/packages/js-dpp/lib/identity/IdentityPublicKey.js) `hash` method (e.g. `identity.getPublicKeyById(0).hash()`).

** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst DashPlatformProtocol = require('@dashevo/dpp');\n\nconst client = new DAPIClient();\nconst dpp = new DashPlatformProtocol();\n\nconst publicKeyHash = 'f307874782c54f1fb4faf687b6831465469aae15';\nconst publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\ndpp.initialize().then(() => {\n  client.platform.getIdentitiesByPublicKeyHashes(publicKeysBuffer)\n    .then((response) => {\n      const retrievedIdentity = dpp.identity.createFromBuffer(response.identities[0]);\n      console.log(retrievedIdentity.toJSON());\n    });\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetIdentitiesByPublicKeyHashesRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst DashPlatformProtocol = require('@dashevo/dpp');\n\nconst dpp = new DashPlatformProtocol();\n\ndpp.initialize()\n  .then(() => {\n    const platformPromiseClient = new PlatformPromiseClient(\n      'http://seed-1.testnet.networks.dash.org:3010',\n    );\n\n    const publicKeyHash = 'f307874782c54f1fb4faf687b6831465469aae15';\n    const publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\n    const getIdentitiesByPublicKeyHashesRequest = new GetIdentitiesByPublicKeyHashesRequest();\n    getIdentitiesByPublicKeyHashesRequest.setPublicKeyHashesList(publicKeysBuffer);\n\n    platformPromiseClient.getIdentitiesByPublicKeyHashes(getIdentitiesByPublicKeyHashesRequest)\n      .then((response) => {\n        const identitiesResponse = response.getIdentitiesList();\n      \tconsole.log(dpp.identity.createFromBuffer(Buffer.from(identitiesResponse[0])).toJSON());\n      })\n      .catch((e) => console.error(e));\n  \t});",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `public_key_hashes` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n      \"public_key_hashes\":\"8weHR4LFTx+0+vaHtoMUZUaarhU=\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentitiesByPublicKeyHashes",
      "language": "shell",
      "name": "gRPCurl"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  protocolVersion: 1,\n  id: '4EfA9Jrvv3nnCFdSf7fad59851iiTRZ6Wcu6YVJ4iSeF',\n  publicKeys: [\n    {\n      id: 0,\n      type: 0,\n      purpose: 0,\n      securityLevel: 0,\n      data: 'A4t0rqEEwZRjt0vl+umvIlX+QgE67NFwkkZCFPKGesGb',\n      readOnly: false\n    },\n    {\n      id: 1,\n      type: 0,\n      purpose: 0,\n      securityLevel: 2,\n      data: 'Ap30U6YmzeUB9FS0SpvuuOUlWQyU7y54dDmTAGZR/OxP',\n      readOnly: false\n    }\n  ],\n  balance: 0,\n  revision: 0\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"identities\": [\n    \"AQAAAKRiaWRYIDASwZuY7AAzrds2zWS39RBnDyo1GkMEtfaZQUQobv2sZ2JhbGFuY2UAaHJldmlzaW9uAGpwdWJsaWNLZXlzgqZiaWQAZGRhdGFYIQOLdK6hBMGUY7dL5frpryJV/kIBOuzRcJJGQhTyhnrBm2R0eXBlAGdwdXJwb3NlAGhyZWFkT25sefRtc2VjdXJpdHlMZXZlbACmYmlkAWRkYXRhWCECnfRTpibN5QH0VLRKm+645SVZDJTvLnh0OZMAZlH87E9kdHlwZQBncHVycG9zZQBocmVhZE9ubHn0bXNlY3VyaXR5TGV2ZWwC\"\n  ],\n  \"metadata\": {\n    \"height\": \"1191\",\n    \"coreChainLockedHeight\": 802900\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    }
  ]
}
[/block]
##getIdentityIdsByPublicKeyHashes

>❗️Endpoint removed
>
> The `getIdentityIdsByPublicKeyHashes` endpoint was [removed](https://github.com/dashevo/platform/pull/437) in Dash Platform v0.23. Use the [`getIdentitiesByPublicKeyHashes` endpoint](reference-dapi-endpoints-platform-endpoints#getidentitiesbypublickeyhashes) to retrieve identity information.
> 
> Details of the removed endpoint can be seen in the [previous version](https://dashplatform.readme.io/v0.22.0/docs/reference-dapi-endpoints-platform-endpoints#getidentityidsbypublickeyhashes) of the documentation.

## getDataContract

**Returns**: [Data Contract](explanation-platform-protocol-data-contract) information for the requested data contract
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `id` | Bytes | Yes | A data contract `id` |
| `prove` | Boolean | No | Set to `true` to receive a proof that contains the requested data contract |

> 📘 
>
> **Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-platform-proofs) for details on decoding the data.

** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient();\n\nconst contractId = Identifier.from('GWRSAVFMjXx8HpQFaNJMqBV7MBgMK4br5UESsB4S31Ec');\nclient.platform.getDataContract(contractId).then((response) => {\n  const contract = cbor.decode(response.dataContract);\n  console.dir(contract, { depth: 10 });\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetDataContractRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst contractId = Identifier.from('GWRSAVFMjXx8HpQFaNJMqBV7MBgMK4br5UESsB4S31Ec');\nconst contractIdBuffer = Buffer.from(contractId);\nconst getDataContractRequest = new GetDataContractRequest();\ngetDataContractRequest.setId(contractIdBuffer);\n\nplatformPromiseClient.getDataContract(getDataContractRequest)\n  .then((response) => {\n    const contractResponse = response.getDataContract();\n    const contractBuffer = Buffer.from(contractResponse);\n  \t// Strip off protocol version (leading 4 bytes) and decode\n  \tconsole.dir(cbor.decode(contractBuffer.slice(4, contractBuffer.length)), { depth: 5 });\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"5mjGWa9mruHnLBht3ntbfgodcSoJxA1XIfYiv1PFMVU=\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDataContract",
      "language": "shell",
      "name": "gRPCurl"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  '$id': Buffer(32) [Uint8Array] [\n    230, 104, 198,  89, 175, 102, 174, 225,\n    231,  44,  24, 109, 222, 123,  91, 126,\n     10,  29, 113,  42,   9, 196,  13,  87,\n     33, 246,  34, 191,  83, 197,  49,  85\n  ],\n  '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',\n  ownerId: Buffer(32) [Uint8Array] [\n     48,  18, 193, 155, 152, 236,   0,  51,\n    173, 219,  54, 205, 100, 183, 245,  16,\n    103,  15,  42,  53,  26,  67,   4, 181,\n    246, 153,  65,  68,  40, 110, 253, 172\n  ],\n  version: 1,\n  documents: {\n    domain: {\n      type: 'object',\n      indices: [\n        {\n          name: 'parentNameAndLabel',\n          unique: true,\n          properties: [ [Object], [Object] ]\n        },\n        {\n          name: 'dashIdentityId',\n          unique: true,\n          properties: [ [Object] ]\n        },\n        { name: 'dashAlias', properties: [ [Object] ] }\n      ],\n      '$comment': \"In order to register a domain you need to create a preorder. The preorder step is needed to prevent man-in-the-middle attacks. normalizedLabel + '.' + normalizedParentDomain must not be longer than 253 chars length as defined by RFC 1035. Domain documents are immutable: modification and deletion are restricted\",\n      required: [\n        'label',\n        'normalizedLabel',\n        'normalizedParentDomainName',\n        'preorderSalt',\n        'records',\n        'subdomainRules'\n      ],\n      properties: {\n        label: {\n          type: 'string',\n          pattern: '^[a-zA-Z0-9][a-zA-Z0-9-]{0,61}[a-zA-Z0-9]$',\n          maxLength: 63,\n          minLength: 3,\n          description: \"Domain label. e.g. 'Bob'.\"\n        },\n        records: {\n          type: 'object',\n          '$comment': 'Constraint with max and min properties ensure that only one identity record is used - either a `dashUniqueIdentityId` or a `dashAliasIdentityId`',\n          properties: {\n            dashAliasIdentityId: [Object],\n            dashUniqueIdentityId: [Object]\n          },\n          maxProperties: 1,\n          minProperties: 1,\n          additionalProperties: false\n        },\n        preorderSalt: {\n          type: 'array',\n          maxItems: 32,\n          minItems: 32,\n          byteArray: true,\n          description: 'Salt used in the preorder document'\n        },\n        subdomainRules: {\n          type: 'object',\n          required: [ 'allowSubdomains' ],\n          properties: { allowSubdomains: [Object] },\n          description: 'Subdomain rules allow domain owners to define rules for subdomains',\n          additionalProperties: false\n        },\n        normalizedLabel: {\n          type: 'string',\n          pattern: '^[a-z0-9][a-z0-9-]{0,61}[a-z0-9]$',\n          '$comment': 'Must be equal to the label in lowercase. This property will be deprecated due to case insensitive indices',\n          maxLength: 63,\n          description: \"Domain label in lowercase for case-insensitive uniqueness validation. e.g. 'bob'\"\n        },\n        normalizedParentDomainName: {\n          type: 'string',\n          pattern: '^$|^[[a-z0-9][a-z0-9-\\\\.]{0,61}[a-z0-9]$',\n          '$comment': 'Must either be equal to an existing domain or empty to create a top level domain. Only the data contract owner can create top level domains.',\n          maxLength: 63,\n          minLength: 0,\n          description: \"A full parent domain name in lowercase for case-insensitive uniqueness validation. e.g. 'dash'\"\n        }\n      },\n      additionalProperties: false\n    },\n    preorder: {\n      type: 'object',\n      indices: [\n        { name: 'saltedHash', unique: true, properties: [ [Object] ] }\n      ],\n      '$comment': 'Preorder documents are immutable: modification and deletion are restricted',\n      required: [ 'saltedDomainHash' ],\n      properties: {\n        saltedDomainHash: {\n          type: 'array',\n          maxItems: 32,\n          minItems: 32,\n          byteArray: true,\n          description: 'Double sha-256 of the concatenation of a 32 byte random salt and a normalized domain name'\n        }\n      },\n      additionalProperties: false\n    }\n  }\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"dataContract\": \"AQAAAKVjJGlkWCDmaMZZr2au4ecsGG3ee1t+Ch1xKgnEDVch9iK/U8UxVWckc2NoZW1heDRodHRwczovL3NjaGVtYS5kYXNoLm9yZy9kcHAtMC00LTAvbWV0YS9kYXRhLWNvbnRyYWN0Z293bmVySWRYIDASwZuY7AAzrds2zWS39RBnDyo1GkMEtfaZQUQobv2sZ3ZlcnNpb24BaWRvY3VtZW50c6JmZG9tYWlupmR0eXBlZm9iamVjdGdpbmRpY2Vzg6NkbmFtZXJwYXJlbnROYW1lQW5kTGFiZWxmdW5pcXVl9Wpwcm9wZXJ0aWVzgqF4Gm5vcm1hbGl6ZWRQYXJlbnREb21haW5OYW1lY2FzY6Fvbm9ybWFsaXplZExhYmVsY2FzY6NkbmFtZW5kYXNoSWRlbnRpdHlJZGZ1bmlxdWX1anByb3BlcnRpZXOBoXgccmVjb3Jkcy5kYXNoVW5pcXVlSWRlbnRpdHlJZGNhc2OiZG5hbWVpZGFzaEFsaWFzanByb3BlcnRpZXOBoXgbcmVjb3Jkcy5kYXNoQWxpYXNJZGVudGl0eUlkY2FzY2gkY29tbWVudHkBN0luIG9yZGVyIHRvIHJlZ2lzdGVyIGEgZG9tYWluIHlvdSBuZWVkIHRvIGNyZWF0ZSBhIHByZW9yZGVyLiBUaGUgcHJlb3JkZXIgc3RlcCBpcyBuZWVkZWQgdG8gcHJldmVudCBtYW4taW4tdGhlLW1pZGRsZSBhdHRhY2tzLiBub3JtYWxpemVkTGFiZWwgKyAnLicgKyBub3JtYWxpemVkUGFyZW50RG9tYWluIG11c3Qgbm90IGJlIGxvbmdlciB0aGFuIDI1MyBjaGFycyBsZW5ndGggYXMgZGVmaW5lZCBieSBSRkMgMTAzNS4gRG9tYWluIGRvY3VtZW50cyBhcmUgaW1tdXRhYmxlOiBtb2RpZmljYXRpb24gYW5kIGRlbGV0aW9uIGFyZSByZXN0cmljdGVkaHJlcXVpcmVkhmVsYWJlbG9ub3JtYWxpemVkTGFiZWx4Gm5vcm1hbGl6ZWRQYXJlbnREb21haW5OYW1lbHByZW9yZGVyU2FsdGdyZWNvcmRzbnN1YmRvbWFpblJ1bGVzanByb3BlcnRpZXOmZWxhYmVspWR0eXBlZnN0cmluZ2dwYXR0ZXJueCpeW2EtekEtWjAtOV1bYS16QS1aMC05LV17MCw2MX1bYS16QS1aMC05XSRpbWF4TGVuZ3RoGD9pbWluTGVuZ3RoA2tkZXNjcmlwdGlvbngZRG9tYWluIGxhYmVsLiBlLmcuICdCb2InLmdyZWNvcmRzpmR0eXBlZm9iamVjdGgkY29tbWVudHiQQ29uc3RyYWludCB3aXRoIG1heCBhbmQgbWluIHByb3BlcnRpZXMgZW5zdXJlIHRoYXQgb25seSBvbmUgaWRlbnRpdHkgcmVjb3JkIGlzIHVzZWQgLSBlaXRoZXIgYSBgZGFzaFVuaXF1ZUlkZW50aXR5SWRgIG9yIGEgYGRhc2hBbGlhc0lkZW50aXR5SWRganByb3BlcnRpZXOic2Rhc2hBbGlhc0lkZW50aXR5SWSnZHR5cGVlYXJyYXloJGNvbW1lbnR4I011c3QgYmUgZXF1YWwgdG8gdGhlIGRvY3VtZW50IG93bmVyaG1heEl0ZW1zGCBobWluSXRlbXMYIGlieXRlQXJyYXn1a2Rlc2NyaXB0aW9ueD1JZGVudGl0eSBJRCB0byBiZSB1c2VkIHRvIGNyZWF0ZSBhbGlhcyBuYW1lcyBmb3IgdGhlIElkZW50aXR5cGNvbnRlbnRNZWRpYVR5cGV4IWFwcGxpY2F0aW9uL3guZGFzaC5kcHAuaWRlbnRpZmllcnRkYXNoVW5pcXVlSWRlbnRpdHlJZKdkdHlwZWVhcnJheWgkY29tbWVudHgjTXVzdCBiZSBlcXVhbCB0byB0aGUgZG9jdW1lbnQgb3duZXJobWF4SXRlbXMYIGhtaW5JdGVtcxggaWJ5dGVBcnJhefVrZGVzY3JpcHRpb254PklkZW50aXR5IElEIHRvIGJlIHVzZWQgdG8gY3JlYXRlIHRoZSBwcmltYXJ5IG5hbWUgdGhlIElkZW50aXR5cGNvbnRlbnRNZWRpYVR5cGV4IWFwcGxpY2F0aW9uL3guZGFzaC5kcHAuaWRlbnRpZmllcm1tYXhQcm9wZXJ0aWVzAW1taW5Qcm9wZXJ0aWVzAXRhZGRpdGlvbmFsUHJvcGVydGllc/RscHJlb3JkZXJTYWx0pWR0eXBlZWFycmF5aG1heEl0ZW1zGCBobWluSXRlbXMYIGlieXRlQXJyYXn1a2Rlc2NyaXB0aW9ueCJTYWx0IHVzZWQgaW4gdGhlIHByZW9yZGVyIGRvY3VtZW50bnN1YmRvbWFpblJ1bGVzpWR0eXBlZm9iamVjdGhyZXF1aXJlZIFvYWxsb3dTdWJkb21haW5zanByb3BlcnRpZXOhb2FsbG93U3ViZG9tYWluc6NkdHlwZWdib29sZWFuaCRjb21tZW50eE9Pbmx5IHRoZSBkb21haW4gb3duZXIgaXMgYWxsb3dlZCB0byBjcmVhdGUgc3ViZG9tYWlucyBmb3Igbm9uIHRvcC1sZXZlbCBkb21haW5za2Rlc2NyaXB0aW9ueFtUaGlzIG9wdGlvbiBkZWZpbmVzIHdobyBjYW4gY3JlYXRlIHN1YmRvbWFpbnM6IHRydWUgLSBhbnlvbmU7IGZhbHNlIC0gb25seSB0aGUgZG9tYWluIG93bmVya2Rlc2NyaXB0aW9ueEJTdWJkb21haW4gcnVsZXMgYWxsb3cgZG9tYWluIG93bmVycyB0byBkZWZpbmUgcnVsZXMgZm9yIHN1YmRvbWFpbnN0YWRkaXRpb25hbFByb3BlcnRpZXP0b25vcm1hbGl6ZWRMYWJlbKVkdHlwZWZzdHJpbmdncGF0dGVybnghXlthLXowLTldW2EtejAtOS1dezAsNjF9W2EtejAtOV0kaCRjb21tZW50eGlNdXN0IGJlIGVxdWFsIHRvIHRoZSBsYWJlbCBpbiBsb3dlcmNhc2UuIFRoaXMgcHJvcGVydHkgd2lsbCBiZSBkZXByZWNhdGVkIGR1ZSB0byBjYXNlIGluc2Vuc2l0aXZlIGluZGljZXNpbWF4TGVuZ3RoGD9rZGVzY3JpcHRpb254UERvbWFpbiBsYWJlbCBpbiBsb3dlcmNhc2UgZm9yIGNhc2UtaW5zZW5zaXRpdmUgdW5pcXVlbmVzcyB2YWxpZGF0aW9uLiBlLmcuICdib2IneBpub3JtYWxpemVkUGFyZW50RG9tYWluTmFtZaZkdHlwZWZzdHJpbmdncGF0dGVybngnXiR8XltbYS16MC05XVthLXowLTktXC5dezAsNjF9W2EtejAtOV0kaCRjb21tZW50eIxNdXN0IGVpdGhlciBiZSBlcXVhbCB0byBhbiBleGlzdGluZyBkb21haW4gb3IgZW1wdHkgdG8gY3JlYXRlIGEgdG9wIGxldmVsIGRvbWFpbi4gT25seSB0aGUgZGF0YSBjb250cmFjdCBvd25lciBjYW4gY3JlYXRlIHRvcCBsZXZlbCBkb21haW5zLmltYXhMZW5ndGgYP2ltaW5MZW5ndGgAa2Rlc2NyaXB0aW9ueF5BIGZ1bGwgcGFyZW50IGRvbWFpbiBuYW1lIGluIGxvd2VyY2FzZSBmb3IgY2FzZS1pbnNlbnNpdGl2ZSB1bmlxdWVuZXNzIHZhbGlkYXRpb24uIGUuZy4gJ2Rhc2gndGFkZGl0aW9uYWxQcm9wZXJ0aWVz9GhwcmVvcmRlcqZkdHlwZWZvYmplY3RnaW5kaWNlc4GjZG5hbWVqc2FsdGVkSGFzaGZ1bmlxdWX1anByb3BlcnRpZXOBoXBzYWx0ZWREb21haW5IYXNoY2FzY2gkY29tbWVudHhKUHJlb3JkZXIgZG9jdW1lbnRzIGFyZSBpbW11dGFibGU6IG1vZGlmaWNhdGlvbiBhbmQgZGVsZXRpb24gYXJlIHJlc3RyaWN0ZWRocmVxdWlyZWSBcHNhbHRlZERvbWFpbkhhc2hqcHJvcGVydGllc6Fwc2FsdGVkRG9tYWluSGFzaKVkdHlwZWVhcnJheWhtYXhJdGVtcxggaG1pbkl0ZW1zGCBpYnl0ZUFycmF59WtkZXNjcmlwdGlvbnhZRG91YmxlIHNoYS0yNTYgb2YgdGhlIGNvbmNhdGVuYXRpb24gb2YgYSAzMiBieXRlIHJhbmRvbSBzYWx0IGFuZCBhIG5vcm1hbGl6ZWQgZG9tYWluIG5hbWV0YWRkaXRpb25hbFByb3BlcnRpZXP0\",\n  \"metadata\": {\n    \"height\": \"1191\",\n    \"coreChainLockedHeight\": 802900\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    }
  ]
}
[/block]

## getDocuments

**Returns**: [Document](explanation-platform-protocol-document) information for the requested document(s)
**Parameters**:

> 🚧 * Parameter constraints
>
> The `where`, `order_by`, `limit`, `start_at`, and `start_after` parameters must comply with the limits defined on the [Query Syntax](reference-query-syntax) page.
>
> Additionally, note that `where` and `order_by` must be [CBOR](https://tools.ietf.org/html/rfc7049) encoded.

| Name | Type | Required | Description |
| - | - | - | - |
| `data_contract_id` | Bytes | Yes | A data contract `id` |
| `document_type` | String | Yes | A document type defined by the data contract (e.g. `preorder` or `domain` for the DPNS contract) |
| `where` * | Bytes | No | Where clause to filter the results (**must be CBOR encoded**) |
| `order_by` * | Bytes | No | Sort records by the field(s) provided (**must be CBOR encoded**) |
| `limit` | Integer | No | Maximum number of results to return |
| ---------- | | | |
| _One_ of the following: | | | |
| `start_at` | Integer | No | Return records beginning with the index provided |
| `start_after` | Integer | No | Return records beginning after the index provided |
| ---------- | | | |
| `prove` | Boolean | No | Set to `true` to receive a proof that contains the requested document(s) |

> 📘 
>
> **Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-platform-proofs) for details on decoding the data


** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient();\n\nconst contractId = Identifier.from('GWRSAVFMjXx8HpQFaNJMqBV7MBgMK4br5UESsB4S31Ec');\nclient.platform.getDocuments(contractId, 'domain', { limit: 10 }).then((response) => {\n  for (const rawData of response.documents) {\n    console.log(cbor.decode(rawData.slice(4, rawData.length)));\n  }\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetDocumentsRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst cbor = require('cbor');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst contractId = Identifier.from('GWRSAVFMjXx8HpQFaNJMqBV7MBgMK4br5UESsB4S31Ec');\nconst contractIdBuffer = Buffer.from(contractId);\nconst getDocumentsRequest = new GetDocumentsRequest();\nconst type = 'domain';\nconst limit = 10;\n\ngetDocumentsRequest.setDataContractId(contractIdBuffer);\ngetDocumentsRequest.setDocumentType(type);\n// getDocumentsRequest.setWhere(whereSerialized);\n// getDocumentsRequest.setOrderBy(orderBySerialized);\ngetDocumentsRequest.setLimit(limit);\n// getDocumentsRequest.setStartAfter(startAfter);\n// getDocumentsRequest.setStartAt(startAt);\n\nplatformPromiseClient.getDocuments(getDocumentsRequest)\n  .then((response) => {\n    for (const document of response.getDocumentsList()) {\n      const documentBuffer = Buffer.from(document);\n      // Strip off protocol version (leading 4 bytes) and decode\n      console.log(cbor.decode(documentBuffer.slice(4, documentBuffer.length)));\n    }\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# Request documents\n# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"data_contract_id\":\"5mjGWa9mruHnLBht3ntbfgodcSoJxA1XIfYiv1PFMVU=\",\n    \"document_type\":\"domain\",\n    \"limit\":1\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDocuments",
      "language": "shell",
      "name": "Request (gRPCurl)"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  '$id': <Buffer 16 e7 55 dd 8a 97 0c 10 ee 11 55 c4 e7 ed b2 01 fe 82 80 49 38 1a b7 e7 14 1f 6f 4a 7d 95 ab a4>,\n  '$type': 'domain',\n  label: 'RT-First-00000',\n  records: {\n    dashUniqueIdentityId: <Buffer e9 17 11 54 b9 91 b0 aa 1a 5a a4 d7 78 70 99 37 6e 04 8f 43 a7 f3 00 45 7c d4 5d 3c bc 4a db 13>\n  },\n  '$ownerId': <Buffer e9 17 11 54 b9 91 b0 aa 1a 5a a4 d7 78 70 99 37 6e 04 8f 43 a7 f3 00 45 7c d4 5d 3c bc 4a db 13>,\n  '$revision': 1,\n  preorderSalt: <Buffer 80 a7 a7 93 1a 88 8f 9e 32 41 63 9c 8c 04 c5 94 48 b7 3b 32 7f bc dc 58 97 04 fb 37 df 40 7c a2>,\n  subdomainRules: { allowSubdomains: false },\n  '$dataContractId': <Buffer e6 68 c6 59 af 66 ae e1 e7 2c 18 6d de 7b 5b 7e 0a 1d 71 2a 09 c4 0d 57 21 f6 22 bf 53 c5 31 55>,\n  normalizedLabel: 'rt-first-00000',\n  normalizedParentDomainName: 'dash'\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"documents\": [\n    \"AQAAAKxjJGlkWCDX8sU/RqkXq25bOaLXvCYLZJKJRTdE0eDU8mqNjv83z2UkdHlwZWZkb21haW5lbGFiZWxkZGFzaGdyZWNvcmRzoXNkYXNoQWxpYXNJZGVudGl0eUlkWCAwEsGbmOwAM63bNs1kt/UQZw8qNRpDBLX2mUFEKG79rGgkb3duZXJJZFggMBLBm5jsADOt2zbNZLf1EGcPKjUaQwS19plBRChu/axpJHJldmlzaW9uAWokY3JlYXRlZEF0GwAAAXrOSnqVbHByZW9yZGVyU2FsdFgg4LUIxaNoJaIGaTofQUqhPtvs9DxB48eZ6p5ze0+aoiZuc3ViZG9tYWluUnVsZXOhb2FsbG93U3ViZG9tYWluc/VvJGRhdGFDb250cmFjdElkWCDmaMZZr2au4ecsGG3ee1t+Ch1xKgnEDVch9iK/U8UxVW9ub3JtYWxpemVkTGFiZWxkZGFzaHgabm9ybWFsaXplZFBhcmVudERvbWFpbk5hbWVg\"\n  ],\n  \"metadata\": {\n    \"height\": \"409\",\n    \"coreChainLockedHeight\": 802152\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    }
  ]
}
[/block]
## waitForStateTransitionResult

**Returns**: The state transition hash and either a proof that the state transition was confirmed in a block or an error. 
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `state_transition_hash` | Bytes | Yes | Hash of the state transition |
| `prove` | Boolean | Yes | Set to `true` to request a proof |

> 📘 
>
> **Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-platform-proofs) for details on decoding the data.


** Example Request and Response**
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst DashPlatformProtocol = require('@dashevo/dpp');\nconst crypto = require('crypto');\n\nconst client = new DAPIClient();\nconst dpp = new DashPlatformProtocol();\n\n// Replace with your own state transition object before running\nconst stateTransitionObject = {\n  protocolVersion: 0,\n  type: 0,\n  signature: 'HxAipUsLWQBE++C1suSRNQiQh91rI1LZbblvQhk2erUaIvRneAagxGYYsXXYNvEeO+lBzlF1a9KHGGTHgnO/8Ts=',\n  signaturePublicKeyId: 0,\n  dataContract: {\n    protocolVersion: 0,\n    '$id': 'CMc7RghKkHeHtFdwfSX5Hzy7CUdpCEJnwsbfHdsbmJ32',\n    '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',\n    ownerId: '8Z3ps3tNoGoPEDYerUNCd4yi7zDwgBh2ejgSMExxvkfD',\n    documents: {\n      note: {\n        properties: { message: { type: 'string' } },\n        additionalProperties: false,\n      },\n    },\n  },\n  entropy: '+RqUArypdL8f/gCMAo4b6c3CoQvxHzsQG0BdYrT5QT0=',\n};\n\n// Convert signature and entropy to buffer\nstateTransitionObject.signature = Buffer.from(stateTransitionObject.signature, 'base64');\nstateTransitionObject.entropy = Buffer.from(stateTransitionObject.entropy, 'base64');\n\ndpp.stateTransition.createFromObject(stateTransitionObject, { skipValidation: true })\n  .then((stateTransition) => {\n    //  Calculate state transition hash\n    const hash = crypto.createHash('sha256')\n      .update(stateTransition.toBuffer())\n      .digest();\n\n    console.log(`Requesting proof of state transition with hash:\\n\\t${hash.toString('hex')}`);\n\n    client.platform.waitForStateTransitionResult(hash, { prove: true })\n      .then((response) => {\n        console.log(response);\n      });\n  });\n",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    WaitForStateTransitionResultRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst DashPlatformProtocol = require('@dashevo/dpp');\nconst crypto = require('crypto');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst dpp = new DashPlatformProtocol();\n\n// Replace with your own state transition object before running\nconst stateTransitionObject = {\n  protocolVersion: 0,\n  type: 0,\n  signature: 'HxAipUsLWQBE++C1suSRNQiQh91rI1LZbblvQhk2erUaIvRneAagxGYYsXXYNvEeO+lBzlF1a9KHGGTHgnO/8Ts=',\n  signaturePublicKeyId: 0,\n  dataContract: {\n    protocolVersion: 0,\n    '$id': 'CMc7RghKkHeHtFdwfSX5Hzy7CUdpCEJnwsbfHdsbmJ32',\n    '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',\n    ownerId: '8Z3ps3tNoGoPEDYerUNCd4yi7zDwgBh2ejgSMExxvkfD',\n    documents: {\n      note: {\n        properties: { message: { type: 'string' } },\n        additionalProperties: false,\n      },\n    },\n  },\n  entropy: '+RqUArypdL8f/gCMAo4b6c3CoQvxHzsQG0BdYrT5QT0=',\n};\n\n// Convert signature and entropy to buffer\nstateTransitionObject.signature = Buffer.from(stateTransitionObject.signature, 'base64');\nstateTransitionObject.entropy = Buffer.from(stateTransitionObject.entropy, 'base64');\n\ndpp.stateTransition.createFromObject(stateTransitionObject, { skipValidation: true })\n  .then((stateTransition) => {\n    //  Calculate state transition hash\n    const hash = crypto.createHash('sha256')\n      .update(stateTransition.toBuffer())\n      .digest();\n\n    const waitForStateTransitionResultRequest = new WaitForStateTransitionResultRequest();\n    waitForStateTransitionResultRequest.setStateTransitionHash(hash);\n    waitForStateTransitionResultRequest.setProve(true);\n\n    console.log(`Requesting proof of state transition with hash:\\n\\t${hash.toString('hex')}`);\n\n    platformPromiseClient.waitForStateTransitionResult(waitForStateTransitionResultRequest)\n      .then((response) => {\n        const rootTreeProof = Buffer.from(response.getProof().getRootTreeProof());\n        const storeTreeProof = Buffer.from(response.getProof().getStoreTreeProof());\n        console.log(`Root tree proof: ${rootTreeProof.toString('hex')}`);\n        console.log(`Store tree proof: ${storeTreeProof.toString('hex')}`);\n      })\n  \t\t.catch((e) => console.error(e));\n  });",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `state_transition_hash` must be represented in base64\n# Replace `state_transition_hash` with your own before running\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"state_transition_hash\":\"wEiwFu9WvAtylrwTph5v0uXQm743N+75C+C9DhmZBkw=\",\n    \"prove\": \"true\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/waitForStateTransitionResult",
      "language": "shell",
      "name": "Request (gRPCurl)"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  proof: {\n    rootTreeProof: <Buffer 01 00 00 00 03 26 e0 35 e0 31 82 7e 7c 27 b0 91 23 41 ed d2 11 bf 3b 90 54 70 11 2c 68 5a 8e 76 8c 68 bb 39 21 3d cf 46 6d 09 d0 7a 28 e3 e9 0b 2b 0e ... 17 more bytes>,\n    storeTreeProof: <Buffer 01 0b ee 31 ce ca 2a bd 44 6a db d4 9f 13 4a 7d 70 25 96 a9 b9 02 6e c4 e1 90 95 f7 a1 b4 c9 de 1f e4 63 e6 ce f7 58 3a 5b c3 10 01 78 9b 4f 98 9a c9 ... 526 more bytes>\n  }\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "Requesting proof of state transition with hash:\n        8ae93b89c272455f3ce8d01dba99a3a28c9550262a602c6ba44de08e545d3aa9\nRoot tree proof: 010000000326e035e031827e7c27b0912341edd211bf3b905470112c685a8e768c68bb39213dcf466d09d07a28e3e90b2b0e1d1510dede30214f68e32f8cf498220101\nStore tree proof: 010bee31ceca2abd446adbd49f134a7d702596a9b9026ec4e19095f7a1b4c9de1fe463e6cef7583a5bc31001789b4f989ac9f8f524f1247fed372502d8c54a3c026072d5239f074422621673c250d1c74eadbb304a10013fb54a99ef641b9a7585d6d28dd443875e435a35022d92a0711f56ae23bc13f4a630a1455970451e3f1001fe2b060fca69ce2eb3d784cec28c0f575690f131026df252af068635bdf08f5448ea67c23d9a9a02831001a7df0a9f392682d7d7d0a29bd43d932c16b0d6530320a8b7df4cadb6cdfd5b5d1bb31cbb488d241e91d60cc1341cd686a3fbb6291f19e800a5632469645820a8b7df4cadb6cdfd5b5d1bb31cbb488d241e91d60cc1341cd686a3fbb6291f196724736368656d61783468747470733a2f2f736368656d612e646173682e6f72672f6470702d302d342d302f6d6574612f646174612d636f6e7472616374676f776e657249645820703796bfd3e2bbd54505a8e04929bb05b8aecfb1cd5c013ef8a8b84511770e0c69646f63756d656e7473a1646e6f7465a26a70726f70657274696573a1676d657373616765a1647479706566737472696e67746164646974696f6e616c50726f70657274696573f46f70726f746f636f6c56657273696f6e001001d94cd40044b8485e962d80e57c1992c77a182a1611111102abe7e3f7231ed71b8c4fd7ee09d4a1f970a51cd4100139312feadce145313ef34f720e940892d0ed2405111102245c6aaaf192b51207333be85ab77c9c9393af47100128ef7d4479e4f488373d92280fe8e52a9a48a6621111",
      "language": "text",
      "name": "Response (dapi-grpc)"
    },
    {
      "code": "{\n  \"proof\": {\n    \"merkleProof\": \"AQAAAAAAAAJHAV8NeFRgf0cWRiCIIVsbJcNs+bALwShVQEkXV2jRCueIAkeoHTKeU60+Jm2oiHbGSkOL8ui06Nj27SLz2raMF29iEAHKnCrDvO67hcJV79tfQwqQSFcxJOek9Fa/x3oYvwQrQQL8lxVOSAkZSC1qhI8LJa6PtR+u8TifMMmRCRY2dXrSUBABihjPBDgD9SM/d9JgWkYyT+sUp4FdgotmwHhZIB1rJVUCkaeftxRkQ/B0FU26ojDJirY/SwZ8RcU1/3pnbJbA+5EQBCBkeRNzJuVLjOKnDOmG48wnSzjHg5lLdlA5JSBD79rQxQAFAgEBAAAYUfu7+1iqqIwjkszndZ6Vm8pli6pjt4XxvvgKCcE3ygI+G8nIsflGKoZQOaWGcFBmXDzAwJqjGlMxSIWNSEgoQBACAQd7xuP4BzKoaf75MNyQQC6Q7e94Vg2IdQe2LFJysA4QAZ2mq+ad/rNJ7n2fPiJgHi4NMT9Wht6Kb5J8qF20hajJEQKz2VnGsgYNwMjY9kadWb63Tjk3nTqFttgjLnoz4PCpQBABpdWVJ9sfU+o+OEcUuFcDOV+y4gYFoao3kVNLZ+Yz7Y4RAuVHBoF21TyNc9DUDHmAfcaJf6K+/VzqIzfnJ7iGqXN+EAGvNlkFsWw+DjFg+MLjzo2MzUaWk7sA63+rG3FQJ7LO2hERERECPImih8uZnpEfew+qLeKrdEig5TR6g5VsfuHI9U2WuvsQAU7qdWenHDisDf3TNzJQytKJYaeyhmy2LEo11zZVwsoREQIAAAAAAAAA/wMBAAD6APcBAAAApGJpZFggZHkTcyblS4zipwzphuPMJ0s4x4OZS3ZQOSUgQ+/a0MVnYmFsYW5jZRo7mmxAaHJldmlzaW9uAGpwdWJsaWNLZXlzgqZiaWQAZGRhdGFYIQLlz7I9IuqDAf1fp2xiyvGiApsvgANo2ldfmrWv6MsfG2R0eXBlAGdwdXJwb3NlAGhyZWFkT25sefRtc2VjdXJpdHlMZXZlbACmYmlkAWRkYXRhWCECrq7odM9OoHGEyM1D19ZAaEPf50OKLwsxL2D4SpnLZllkdHlwZQBncHVycG9zZQBocmVhZE9ubHn0bXNlY3VyaXR5TGV2ZWwCAAEAAAAAAAAA0QQBAAAkAgEgbaBpE46QX8+EXS6Sl5CG4r+JuiXVDhxZeZy/TS8qnQEAUw2uiB7scatAs99mQfB5VfVb0lMSywDMHXpmUgNfONsC4OEPflWGlZAqBSOhaKD0/SfPJHbOOMEfCjTtBV1jjgwQARdKA3tf2c8gP3H7tRKcRMHXfljTH/4L8L63tbZk5FX/EQL+TedZ8kFHQEuNY8e9pfmaLI36Y7rHe1hAjVBSh9U95BABfPfYq74T0N0ygKE4spKvSQkekrnH0Ge8Ot0FEDsq2rcR\",\n    \"signatureLlmqHash\": \"AAAA1h0X9yUNsXpD0/iKlsPvVb+VezkZAIkQIzmGqoc=\",\n    \"signature\": \"jeByZ8qlZvID/C3LVVy/mZGHlRu2QhN3MZO09hCOjAH0gn1tqrAX6BXaJf6qRLw9APv0+nInObRF3JhstvsByPK8QOHCCl9M3NpcgI/HCECpqMMG8S9DPtJYI6HwQO5I\"\n  },\n  \"metadata\": {\n    \"height\": \"1221\",\n    \"coreChainLockedHeight\": 802939\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    }
  ]
}
[/block]
# Deprecated Endpoints

The `getIdentityIdsByPublicKeyHashes` was deprecated and removed in Dash Platform v0.23. The previous version of documentation can be [viewed here](https://dashplatform.readme.io/v0.22.0/docs/reference-dapi-endpoints-platform-endpoints).

# Code Reference
Implementation details related to the information on this page can be found in:
- The [Platform repository](https://github.com/dashevo/platform/tree/master/packages/dapi) `packages/dapi/lib/grpcServer/handlers/core` folder
- The [Platform repository](https://github.com/dashevo/platform/tree/master/packages/dapi-grpc) `packages/dapi-grpc/protos` folder