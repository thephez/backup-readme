Please refer to the [gRPC Overview](reference-dapi-endpoints-grpc-overview) for details regarding running the examples shown below, encoding/decoding the request/response data, and clients available for several languages.

# Data Proofs and Metadata

Since Dash Platform 0.20.0, Platform gRPC endpoints can provide [proofs](https://github.com/dashevo/platform/blob/master/packages/dapi-grpc/protos/platform/v0/platform.proto#L16-L28) so the data returned for a request can be verified as being valid. Full support is not yet available in the JavaScript client, but can be used via the low level [dapi-grpc library](https://github.com/dashevo/platform/tree/master/packages/dapi-grpc). Additional information about proofs is available on the [Platform Proofs page](reference-platform-proofs).

Some [additional metadata](https://github.com/dashevo/platform/blob/master/packages/dapi-grpc/protos/platform/v0/platform.proto#L30-L33) is also provided with responses:
 - `height`: the last committed platform chain height
 - `coreChainLockedHeight`: height of the most recent ChainLock on the core chain

# Endpoint Details

## broadcastStateTransition
[block:callout]
{
  "type": "info",
  "body": "**Note:** The [`waitForStateTransitionResult` endpoint](#waitforstatetransitionresult) should be used in conjunction with this one for instances where proof of block confirmation is required.",
  "title": ""
}
[/block]
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
[block:callout]
{
  "type": "warning",
  "body": "As of Dash Platform 0.21 the `protocolVersion` is no longer included in the CBOR-encoded data. It is instead prepended to the data following CBOR encoding.",
  "title": "Breaking changes"
}
[/block]
**Returns**: [Identity](explanation-identity) information for the requested identity
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `id` | Bytes | Yes | An identity `id` |
| `prove` | Boolean | No | **Not available in Platform v0.22.0**<br>Set to `true` to receive a proof that contains the requested identity |

[block:callout]
{
  "type": "info",
  "body": "**Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-platform-proofs) for details on decoding the data."
}
[/block]

** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient();\n\nconst identityId = Identifier.from('3nJjUPiVbwvKRpZ6pLUwVjcFoEJdeqPqakobctF3uECU');\nclient.platform.getIdentity(identityId).then((response) => {\n  // Strip off protocol version (leading 4 bytes) and decode\n  const identity = cbor.decode(response.identity.slice(4, response.identity.length));\n  console.log(identity);\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n  },\n} = require('@dashevo/dapi-grpc');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst id = Identifier.from('3nJjUPiVbwvKRpZ6pLUwVjcFoEJdeqPqakobctF3uECU');\nconst idBuffer = Buffer.from(id);\nconst getIdentityRequest = new GetIdentityRequest();\ngetIdentityRequest.setId(idBuffer);\ngetIdentityRequest.setProve(false);\n\nplatformPromiseClient.getIdentity(getIdentityRequest)\n  .then((response) => {\n    const identityResponse = response.getIdentity();\n    const identityBuffer = Buffer.from(identityResponse);\n    // Strip off protocol version (leading 4 bytes) and decode\n    console.log(cbor.decode(identityBuffer.slice(4, identityBuffer.length)));\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"/W2nISbC47UB7vVZNicsUAF8DjPPl40jSO74HyUdCB0=\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentity",
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
      "code": "{\n  id: <Buffer fd 6d a7 21 26 c2 e3 b5 01 ee f5 59 36 27 2c 50 01 7c 0e 33 cf 97 8d 23 48 ee f8 1f 25 1d 08 1d>,\n  balance: 10994722,\n  revision: 0,\n  publicKeys: [\n    {\n      id: 0,\n      data: <Buffer 03 61 93 00 99 3d 78 05 a4 31 75 42 6f ea d1 29 71 fd f4 f3 ad 47 93 9a 5b ac ea 66 5e ff ca 8c e9>,\n      type: 0,\n      purpose: 0,\n      readOnly: false,\n      securityLevel: 0\n    }\n  ]\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"identity\": \"AQAAAKRiaWRYIP1tpyEmwuO1Ae71WTYnLFABfA4zz5eNI0ju+B8lHQgdZ2JhbGFuY2UaAKfEImhyZXZpc2lvbgBqcHVibGljS2V5c4GmYmlkAGRkYXRhWCEDYZMAmT14BaQxdUJv6tEpcf30861Hk5pbrOpmXv/KjOlkdHlwZQBncHVycG9zZQBocmVhZE9ubHn0bXNlY3VyaXR5TGV2ZWwA\",\n  \"metadata\": {\n    \"height\": \"7216\",\n    \"coreChainLockedHeight\": 696039\n  }\n}\n",
      "language": "json",
      "name": "Response (gRPCurl)"
    }
  ]
}
[/block]
## getIdentitiesByPublicKeyHashes
[block:callout]
{
  "type": "success",
  "body": "In Dash Platform v0.23 the response is an array of identities instead of the previously provided array of CBOR-encoded arrays of identities."
}
[/block]
**Returns**: [Identity](explanation-identity) an array of identities associated with the provided public key hashes
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `public_key_hashes` | Bytes | Yes | Public key hashes (sha256-ripemd160) of identity public keys |
| `prove` | Boolean | No | **Not available in Platform v0.22.0**<br>Set to `true` to receive a proof that contains the requested identities |

[block:callout]
{
  "type": "info",
  "body": "**Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-platform-proofs) for details on decoding the data."
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Note: the hash must be done using all fields of the identity public key object - e.g.\n```json\n{\n  id: 0,\n  type: 0,\n  purpose: 0,\n  securityLevel: 0,\n  data: 'A2GTAJk9eAWkMXVCb+rRKXH99POtR5OaW6zqZl7/yozp',\n  readOnly: false\n}\n```\nWhen using the js-dpp library, the hash can be accessed via the [IdentityPublicKey object's](https://github.com/dashevo/platform/blob/master/packages/js-dpp/lib/identity/IdentityPublicKey.js) `hash` method (e.g. `identity.getPublicKeyById(0).hash()`).",
  "title": "Public key hash"
}
[/block]
** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst DashPlatformProtocol = require('@dashevo/dpp');\n\nconst client = new DAPIClient();\nconst dpp = new DashPlatformProtocol();\n\nconst publicKeyHash = 'b51b2072970377a20a0bf3931e66a046a587a22a';\nconst publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\ndpp.initialize().then(() => {\n  client.platform.getIdentitiesByPublicKeyHashes(publicKeysBuffer)\n    .then((response) => {\n      const retrievedIdentity = dpp.identity.createFromBuffer(response.identities[0]);\n      console.log(retrievedIdentity.toJSON());\n    });\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetIdentitiesByPublicKeyHashesRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst DashPlatformProtocol = require('@dashevo/dpp');\n\nconst dpp = new DashPlatformProtocol();\n\ndpp.initialize()\n  .then(() => {\n    const platformPromiseClient = new PlatformPromiseClient(\n      'http://seed-1.testnet.networks.dash.org:3010',\n    );\n\n    const publicKeyHash = 'b51b2072970377a20a0bf3931e66a046a587a22a';\n    const publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\n    const getIdentitiesByPublicKeyHashesRequest = new GetIdentitiesByPublicKeyHashesRequest();\n    getIdentitiesByPublicKeyHashesRequest.setPublicKeyHashesList(publicKeysBuffer);\n\n    platformPromiseClient.getIdentitiesByPublicKeyHashes(getIdentitiesByPublicKeyHashesRequest)\n      .then((response) => {\n        const identitiesResponse = response.getIdentitiesList();\n      \tconsole.log(dpp.identity.createFromBuffer(Buffer.from(identitiesResponse[0])).toJSON());\n      })\n      .catch((e) => console.error(e));\n  \t});",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `public_key_hashes` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n      \"public_key_hashes\":\"tRsgcpcDd6IKC/OTHmagRqWHoio=\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentitiesByPublicKeyHashes",
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
      "code": "{\n  protocolVersion: 1,\n  id: 'J4H8FjbF6c5Mkv2o5TUDi1ufVBLJqXiDQjgbnXRTMNBe',\n  publicKeys: [\n    {\n      id: 0,\n      type: 0,\n      purpose: 0,\n      securityLevel: 0,\n      data: 'A2GTAJk9eAWkMXVCb+rRKXH99POtR5OaW6zqZl7/yozp',\n      readOnly: false\n    }\n  ],\n  balance: 10994722,\n  revision: 0\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"identities\": [\n    \"gViiAQAAAKRiaWRYIP1tpyEmwuO1Ae71WTYnLFABfA4zz5eNI0ju+B8lHQgdZ2JhbGFuY2UaAKfEImhyZXZpc2lvbgBqcHVibGljS2V5c4GmYmlkAGRkYXRhWCEDYZMAmT14BaQxdUJv6tEpcf30861Hk5pbrOpmXv/KjOlkdHlwZQBncHVycG9zZQBocmVhZE9ubHn0bXNlY3VyaXR5TGV2ZWwA\"\n  ],\n  \"metadata\": {\n    \"height\": \"7219\",\n    \"coreChainLockedHeight\": 696041\n  }\n}",
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
| `prove` | Boolean | No | **Not available in Platform v0.22.0**<br>Set to `true` to receive a proof that contains the requested data contract |

[block:callout]
{
  "type": "info",
  "body": "**Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-platform-proofs) for details on decoding the data."
}
[/block]
** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient();\n\nconst contractId = Identifier.from('DSbnfEe8fU43dgyktHVMKFBVYSsLKHG3mAdN4x5dd9eg');\nclient.platform.getDataContract(contractId).then((response) => {\n  const contract = cbor.decode(response.dataContract);\n  console.dir(contract, { depth: 10 });\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetDataContractRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst contractId = Identifier.from('DSbnfEe8fU43dgyktHVMKFBVYSsLKHG3mAdN4x5dd9eg');\nconst contractIdBuffer = Buffer.from(contractId);\nconst getDataContractRequest = new GetDataContractRequest();\ngetDataContractRequest.setId(contractIdBuffer);\n\nplatformPromiseClient.getDataContract(getDataContractRequest)\n  .then((response) => {\n    const contractResponse = response.getDataContract();\n    const contractBuffer = Buffer.from(contractResponse);\n  \t// Strip off protocol version (leading 4 bytes) and decode\n  \tconsole.dir(cbor.decode(contractBuffer.slice(4, contractBuffer.length)), { depth: 5 });\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"uNsY7l8uGhINKREVl5zk6/I22cHD0vD/HTdfhOGXhEk=\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDataContract",
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
      "code": "{\n  '$id': Buffer(32) [Uint8Array] [\n    184, 219,  24, 238,  95,  46,  26,  18,\n     13,  41,  17,  21, 151, 156, 228, 235,\n    242,  54, 217, 193, 195, 210, 240, 255,\n     29,  55,  95, 132, 225, 151, 132,  73\n  ],\n  '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',\n  ownerId: Buffer(32) [Uint8Array] [\n    253, 109, 167, 33,  38, 194, 227, 181,\n      1, 238, 245, 89,  54,  39,  44,  80,\n      1, 124,  14, 51, 207, 151, 141,  35,\n     72, 238, 248, 31,  37,  29,   8,  29\n  ],\n  version: 2,\n  documents: {\n    note: {\n      type: 'object',\n      properties: { author: { type: 'string' }, message: { type: 'string' } },\n      additionalProperties: false\n    }\n  }\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"dataContract\": \"AQAAAKVjJGlkWCC42xjuXy4aEg0pERWXnOTr8jbZwcPS8P8dN1+E4ZeESWckc2NoZW1heDRodHRwczovL3NjaGVtYS5kYXNoLm9yZy9kcHAtMC00LTAvbWV0YS9kYXRhLWNvbnRyYWN0Z293bmVySWRYIP1tpyEmwuO1Ae71WTYnLFABfA4zz5eNI0ju+B8lHQgdZ3ZlcnNpb24CaWRvY3VtZW50c6Fkbm90ZaNkdHlwZWZvYmplY3RqcHJvcGVydGllc6JmYXV0aG9yoWR0eXBlZnN0cmluZ2dtZXNzYWdloWR0eXBlZnN0cmluZ3RhZGRpdGlvbmFsUHJvcGVydGllc/Q=\",\n  \"metadata\": {\n    \"height\": \"7220\",\n    \"coreChainLockedHeight\": 696042\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    }
  ]
}
[/block]

## getDocuments

**Returns**: [Document](explanation-platform-protocol-document) information for the requested document(s)
**Parameters**:
[block:callout]
{
  "type": "warning",
  "title": "* Parameter constraints",
  "body": "The `where`, `order_by`, `limit`, `start_at`, and `start_after` parameters must comply with the limits defined on the [Query Syntax](reference-query-syntax) page.\n\nAdditionally, note that `where` and `order_by` must be [CBOR](https://tools.ietf.org/html/rfc7049) encoded."
}
[/block]
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
| `prove` | Boolean | No | **Not available in Platform v0.22.0**<br>Set to `true` to receive a proof that contains the requested document(s) |

[block:callout]
{
  "type": "info",
  "body": "**Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-platform-proofs) for details on decoding the data."
}
[/block]

** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient();\n\nconst contractId = Identifier.from('DSbnfEe8fU43dgyktHVMKFBVYSsLKHG3mAdN4x5dd9eg');\nclient.platform.getDocuments(contractId, 'note', { limit: 10 }).then((response) => {\n  for (const rawData of response.documents) {\n    console.log(cbor.decode(rawData.slice(4, rawData.length)));\n  }\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetDocumentsRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst cbor = require('cbor');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst contractId = Identifier.from('DSbnfEe8fU43dgyktHVMKFBVYSsLKHG3mAdN4x5dd9eg');\nconst contractIdBuffer = Buffer.from(contractId);\nconst getDocumentsRequest = new GetDocumentsRequest();\nconst type = 'note';\nconst limit = 10;\n\ngetDocumentsRequest.setDataContractId(contractIdBuffer);\ngetDocumentsRequest.setDocumentType(type);\n// getDocumentsRequest.setWhere(whereSerialized);\n// getDocumentsRequest.setOrderBy(orderBySerialized);\ngetDocumentsRequest.setLimit(limit);\n// getDocumentsRequest.setStartAfter(startAfter);\n// getDocumentsRequest.setStartAt(startAt);\n\nplatformPromiseClient.getDocuments(getDocumentsRequest)\n  .then((response) => {\n    for (const document of response.getDocumentsList()) {\n      const documentBuffer = Buffer.from(document);\n      // Strip off protocol version (leading 4 bytes) and decode\n      console.log(cbor.decode(documentBuffer.slice(4, documentBuffer.length)));\n    }\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# Request documents\n# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"data_contract_id\":\"uNsY7l8uGhINKREVl5zk6/I22cHD0vD/HTdfhOGXhEk=\",\n    \"document_type\":\"note\",\n    \"limit\":10\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDocuments",
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
      "code": "{\n  '$id': <Buffer 16 97 f2 36 96 6a 3a 6d af 29 d9 de c0 b4 42 7a 6b 65 0b 3d ed 36 74 a6 a6 a1 98 78 8d 7e c9 4a>,\n  '$type': 'note',\n  message: 'Tutorial CI Test @ Mon, 28 Mar 2022 14:23:59 GMT',\n  '$ownerId': <Buffer fd 6d a7 21 26 c2 e3 b5 01 ee f5 59 36 27 2c 50 01 7c 0e 33 cf 97 8d 23 48 ee f8 1f 25 1d 08 1d>,\n  '$revision': 1,\n  '$dataContractId': <Buffer b8 db 18 ee 5f 2e 1a 12 0d 29 11 15 97 9c e4 eb f2 36 d9 c1 c3 d2 f0 ff 1d 37 5f 84 e1 97 84 49>\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"documents\": [\n    \"AQAAAKZjJGlkWCAWl/I2lmo6ba8p2d7AtEJ6a2ULPe02dKamoZh4jX7JSmUkdHlwZWRub3RlZ21lc3NhZ2V4MFR1dG9yaWFsIENJIFRlc3QgQCBNb24sIDI4IE1hciAyMDIyIDE0OjIzOjU5IEdNVGgkb3duZXJJZFgg/W2nISbC47UB7vVZNicsUAF8DjPPl40jSO74HyUdCB1pJHJldmlzaW9uAW8kZGF0YUNvbnRyYWN0SWRYILjbGO5fLhoSDSkRFZec5OvyNtnBw9Lw/x03X4Thl4RJ\"\n  ],\n  \"metadata\": {\n    \"height\": \"7220\",\n    \"coreChainLockedHeight\": 696042\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    }
  ]
}
[/block]
## waitForStateTransitionResult
[block:callout]
{
  "type": "info",
  "body": "",
  "title": "Added in Dash Platform 0.18.0"
}
[/block]
**Returns**: The state transition hash and either a proof that the state transition was confirmed in a block or an error. 
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `state_transition_hash` | Bytes | Yes | Hash of the state transition |
| `prove` | Boolean | Yes | **Not available in Platform v0.22.0**<br>Set to `true` to request a proof |

[block:callout]
{
  "type": "info",
  "body": "**Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-platform-proofs) for details on decoding the data."
}
[/block]

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
      "code": "# `state_transition_hash` must be represented in base64\n# Replace `state_transition_hash` with your own before running\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"state_transition_hash\":\"iuk7icJyRV886NAdupmjooyVUCYqYCxrpE3gjlRdOqk=\",\n    \"prove\": \"true\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/waitForStateTransitionResult",
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
      "code": "// NOT AVAILABLE IN PLATFORM v0.22\n{\n  \"proof\": {\n    \"rootTreeProof\": \"AQAAAAMm4DXgMYJ+fCewkSNB7dIRvzuQVHARLGhajnaMaLs5IT3PRm0J0Hoo4+kLKw4dFRDe3jAhT2jjL4z0mCIBAQ==\",\n    \"storeTreeProof\": \"AQvuMc7KKr1EatvUnxNKfXAllqm5Am7E4ZCV96G0yd4f5GPmzvdYOlvDEAF4m0+Ymsn49STxJH/tNyUC2MVKPAJgctUjnwdEImIWc8JQ0cdOrbswShABP7VKme9kG5p1hdbSjdRDh15DWjUCLZKgcR9WriO8E/SmMKFFWXBFHj8QAf4rBg/Kac4us9eEzsKMD1dWkPExAm3yUq8GhjW98I9USOpnwj2amgKDEAGn3wqfOSaC19fQopvUPZMsFrDWUwMgqLffTK22zf1bXRuzHLtIjSQekdYMwTQc1oaj+7YpHxnoAKVjJGlkWCCot99MrbbN/VtdG7Mcu0iNJB6R1gzBNBzWhqP7tikfGWckc2NoZW1heDRodHRwczovL3NjaGVtYS5kYXNoLm9yZy9kcHAtMC00LTAvbWV0YS9kYXRhLWNvbnRyYWN0Z293bmVySWRYIHA3lr/T4rvVRQWo4EkpuwW4rs+xzVwBPviouEURdw4MaWRvY3VtZW50c6Fkbm90ZaJqcHJvcGVydGllc6FnbWVzc2FnZaFkdHlwZWZzdHJpbmd0YWRkaXRpb25hbFByb3BlcnRpZXP0b3Byb3RvY29sVmVyc2lvbgAQAdlM1ABEuEheli2A5XwZksd6GCoWERERAqvn4/cjHtcbjE/X7gnUoflwpRzUEAE5MS/q3OFFMT7zT3IOlAiS0O0kBRERAiRcaqrxkrUSBzM76Fq3fJyTk69HEAEo731EeeT0iDc9kigP6OUqmkimYhER\"\n  }\n}\n",
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