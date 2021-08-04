Please refer to the [gRPC Overview](reference-dapi-endpoints-grpc-overview) for details regarding running the examples shown below, encoding/decoding the request/response data, and clients available for several languages.

# Endpoint Details
[block:callout]
{
  "type": "success",
  "body": "Dash Platform 0.20.0 introduced [proofs](https://github.com/dashevo/dapi-grpc/blob/v0.20.0/protos/platform/v0/platform.proto#L15-L20) to Platform gRPC endpoints so the data returned for a request can be verified as being valid. Full support is not yet available in the JavaScript client, but can be used via the low level via dapi-grpc library. Additional information about proofs is available on the [Platform Proofs page](reference-dapi-endpoints-platform-proofs).\n\nThis version also added some [additional metadata](https://github.com/dashevo/dapi-grpc/blob/v0.20.0/protos/platform/v0/platform.proto#L22-L25) to responses:\n - `height`: the last committed platform chain height\n - `coreChainLockedHeight`: height of the most recent ChainLock on the core chain",
  "title": "Added in Dash Platform 0.20.0"
}
[/block]
## broadcastStateTransition
[block:callout]
{
  "type": "info",
  "body": "Since Dash Platform 0.18.0 `broadcastStateTransition` returns once the state transition has been accepted into the mempool instead of waiting until it is confirmed. \n\n**Note:** The [`waitForStateTransitionResult` endpoint](#waitforstatetransitionresult) should be used in conjunction with this one for instances where proof of block confirmation is required.",
  "title": "Updated in Dash Platform 0.18.0"
}
[/block]
Broadcasts a [state transition](explanation-platform-protocol-state-transition) to the platform via DAPI to make a change to layer 2 data.

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
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst DashPlatformProtocol = require('@dashevo/dpp');\n\nconst client = new DAPIClient();\nconst dpp = new DashPlatformProtocol();\n\n// Data Contract Create State Transition (JSON)\nconst stateTransitionObject = {\n  protocolVersion: 0,\n  type: 0,\n  signature: 'HxAipUsLWQBE++C1suSRNQiQh91rI1LZbblvQhk2erUaIvRneAagxGYYsXXYNvEeO+lBzlF1a9KHGGTHgnO/8Ts=',\n  signaturePublicKeyId: 0,\n  dataContract: {\n    protocolVersion: 0,\n    '$id': 'CMc7RghKkHeHtFdwfSX5Hzy7CUdpCEJnwsbfHdsbmJ32',\n    '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',\n    ownerId: '8Z3ps3tNoGoPEDYerUNCd4yi7zDwgBh2ejgSMExxvkfD',\n    documents: {\n      note: {\n        properties: { message: { type: 'string' } },\n        additionalProperties: false,\n      },\n    },\n  },\n  entropy: '+RqUArypdL8f/gCMAo4b6c3CoQvxHzsQG0BdYrT5QT0=',\n};\n\n// Convert signature and entropy to buffer\nstateTransitionObject.signature = Buffer.from(stateTransitionObject.signature, 'base64');\nstateTransitionObject.entropy = Buffer.from(stateTransitionObject.entropy, 'base64');\n\ndpp.stateTransition.createFromObject(stateTransitionObject, { skipValidation: true })\n  .then((stateTransition) => {\n    client.platform.broadcastStateTransition(stateTransition.toBuffer())\n      .then(() => console.log('State Transition broadcast successfully'));\n  });",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    BroadcastStateTransitionRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst DashPlatformProtocol = require('@dashevo/dpp');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst dpp = new DashPlatformProtocol();\n\n// Data Contract Create State Transition (JSON)\nconst stateTransitionObject = {\n  protocolVersion: 0,\n  type: 0,\n  signature: 'HxAipUsLWQBE++C1suSRNQiQh91rI1LZbblvQhk2erUaIvRneAagxGYYsXXYNvEeO+lBzlF1a9KHGGTHgnO/8Ts=',\n  signaturePublicKeyId: 0,\n  dataContract: {\n    protocolVersion: 0,\n    '$id': 'CMc7RghKkHeHtFdwfSX5Hzy7CUdpCEJnwsbfHdsbmJ32',\n    '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',\n    ownerId: '8Z3ps3tNoGoPEDYerUNCd4yi7zDwgBh2ejgSMExxvkfD',\n    documents: {\n      note: {\n        properties: { message: { type: 'string' } },\n        additionalProperties: false,\n      },\n    },\n  },\n  entropy: '+RqUArypdL8f/gCMAo4b6c3CoQvxHzsQG0BdYrT5QT0=',\n};\n\n// Convert signature and entropy to buffer\nstateTransitionObject.signature = Buffer.from(stateTransitionObject.signature, 'base64');\nstateTransitionObject.entropy = Buffer.from(stateTransitionObject.entropy, 'base64');\n\nconst broadcastStateTransitionRequest = new BroadcastStateTransitionRequest();\n\ndpp.stateTransition.createFromObject(stateTransitionObject, { skipValidation: true })\n  .then((stateTransition) => {\n    console.log(stateTransition);\n    broadcastStateTransitionRequest.setStateTransition(stateTransition.toBuffer());\n\n    platformPromiseClient.broadcastStateTransition(broadcastStateTransitionRequest)\n      .then(() => console.log('State Transition broadcast successfully'))\n      .catch((e) => {\n        console.error(e);\n        console.error(e.metadata);\n      });\n  })\n  .catch((e) => console.error(e));\n",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# Submit an identity create State Transition\n# `state_transition` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"state_transition\":\"pWR0eXBlAmlzaWduYXR1cmV4WEg3TWhFWDQ0Z3JzMVIwTE9XTU5IZjAxWFNpYVFQcUlVZ1JLRXQyMkxHVERsUlUrZ1BwQUlUZk5JUmhXd3IvYTVHd0lzWm1idGdYVVFxcVhjbW9lQWtUOD1qcHVibGljS2V5c4GkYmlkAGRkYXRheCxBdzh2UmYxeFFCTlVLbzNiY2llaHlaR2NhM0hBSThkY0ZvVWJTK3hLb0lITmR0eXBlAGlpc0VuYWJsZWT1bmxvY2tlZE91dFBvaW50eDBLT1VUSHB5YnFPek9DNnhEVUhFWm9uc1lNSVpqcGppTHFZNnkxYmlWNWxRQUFBQUFvcHJvdG9jb2xWZXJzaW9uAA==\"\n\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/broadcastStateTransition",
      "language": "shell",
      "name": "gRPCurl"
    }
  ]
}
[/block]
**Response**: No response except on error

## getIdentity

**Returns**: [Identity](explanation-identity) information for the requested identity
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `id` | Bytes | Yes | An identity `id` |
| `prove` | Boolean | No | Set to `true` to receive a proof that contains the requested identity |

[block:callout]
{
  "type": "warning",
  "body": "**Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-dapi-endpoints-platform-proofs) for details on decoding the data."
}
[/block]

** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient();\n\nconst identityId = Identifier.from('BbSrAN8tKaoQgFxV9T1as8vKSTxRVYx5bWxDqJoVv5jQ');\nclient.platform.getIdentity(identityId).then((response) => {\n  const identity = cbor.decode(response.identity);\n  console.log(identity);\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n  },\n} = require('@dashevo/dapi-grpc');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst id = Identifier.from('BbSrAN8tKaoQgFxV9T1as8vKSTxRVYx5bWxDqJoVv5jQ');\nconst idBuffer = Buffer.from(id);\nconst getIdentityRequest = new GetIdentityRequest();\ngetIdentityRequest.setId(idBuffer);\ngetIdentityRequest.setProve(false);\n\nplatformPromiseClient.getIdentity(getIdentityRequest)\n  .then((response) => {\n    const identityResponse = response.getIdentity();\n    const identityBuffer = Buffer.from(identityResponse);\n    console.log(cbor.decode(identityBuffer));\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"nWfXvxOVHSgCWeZIKuYJp7UGzh3rT/ryj38KWJ+zITM=\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentity",
      "language": "shell",
      "name": "gRPCurl"
    },
    {
      "code": "# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"nWfXvxOVHSgCWeZIKuYJp7UGzh3rT/ryj38KWJ+zITM=\",\n    \"prove\":true\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentity",
      "language": "shell",
      "name": "gRPCurl with Proof"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  id: <Buffer 9d 67 d7 bf 13 95 1d 28 02 59 e6 48 2a e6 09 a7 b5 06 ce 1d eb 4f fa f2 8f 7f 0a 58 9f b3 21 33>,\n  balance: 10996072,\n  revision: 0,\n  publicKeys: [\n    {\n      id: 0,\n      data: <Buffer 03 17 4b f2 6e e7 cb 22 06 03 d6 3c a3 64 3d 2c 48 ff 69 43 1d ac c7 71 5b 2e b4 c1 e9 0d 3a 7e 49>,\n      type: 0\n    }\n  ],\n  protocolVersion: 0\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"identity\": \"pWJpZFggnWfXvxOVHSgCWeZIKuYJp7UGzh3rT/ryj38KWJ+zITNnYmFsYW5jZRoAp8loaHJldmlzaW9uAGpwdWJsaWNLZXlzgaNiaWQAZGRhdGFYIQMXS/Ju58siBgPWPKNkPSxI/2lDHazHcVsutMHpDTp+SWR0eXBlAG9wcm90b2NvbFZlcnNpb24A\",\n  \"metadata\": {\n    \"height\": 115,\n    \"coreChainLockedHeight\": 545261\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    },
    {
      "code": "// The storeTreeProof contains the requested data\n{\n  \"proof\": {\n    \"rootTreeProof\": \"AQAAAAO/730XK2ZpQ8M/rke2FCWUEvUkNe3Zm7+TMURBHDrqtPcn5nTrzcUK8RoqqZvgwYyvERWIxOuojAWh9JxFPs4on5yLLeYx9zIJIAv5uUESaTlcLrm5kWqtzGcwvrp3rg4BAw==\",\n    \"storeTreeProof\": \"AWyO60NAEaeZnR0hsk6K4NIzhxgC/SxLM4XGAGJpnptfAqshUDZ+353VykAJTytGq/tbOq3wOGwdkljpOZfB+6j/EAFKHml+7jyjG8o3OFVcNZkLd1ZGxu/vwiDRG4p0iXkVXgMgnWfXvxOVHSgCWeZIKuYJp7UGzh3rT/ryj38KWJ+zITMAjaViaWRYIJ1n178TlR0oAlnmSCrmCae1Bs4d60/68o9/ClifsyEzZ2JhbGFuY2UaAKfJaGhyZXZpc2lvbgBqcHVibGljS2V5c4GjYmlkAGRkYXRhWCEDF0vybufLIgYD1jyjZD0sSP9pQx2sx3FbLrTB6Q06fklkdHlwZQBvcHJvdG9jb2xWZXJzaW9uABABJwjSChrmnHIgWreCpNnrdgp5+5QBlr+oeoxemFrpoRcREQ==\",\n    \"signatureLlmqHash\": \"AAAAU76r1uJML75iJSdN/Ev94VMJtsl4KH9m/ULmCII=\",\n    \"signature\": \"lbOEbMLb2xr0Swq8k1yD2F2MOl8jwSH255O0ZNw2b3HEGv02j+zwE092pCzlJYNhBKk0orXtLSQwMHV51WQ/SKBQnYTlt4GAbyjall5fQJc5mzP5SVIx9MbBUEdwL8Jg\"\n  },\n  \"metadata\": {\n    \"height\": 116,\n    \"coreChainLockedHeight\": 545262\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl with Proof)"
    }
  ]
}
[/block]
## getIdentitiesByPublicKeyHashes

**Returns**: [Identity](explanation-identity) information associated with the provided public key hashes
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `public_key_hashes` | Bytes | Yes | Public key hashes (double-sha256) of identity public keys |
| `prove` | Boolean | No | Set to `true` to receive a proof that contains the requested identities |

[block:callout]
{
  "type": "warning",
  "body": "**Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-dapi-endpoints-platform-proofs) for details on decoding the data."
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Note: the hash must be done using all fields of the identity public key object - e.g.\n```json\n{\n  id: 0,\n  type: 0,\n  data: 'A/Wmv+LhxAjSBE19d3OJCUawUiYORF5HDDetZbvTDgQQ'\n}\n```\nWhen using the js-dpp library, the hash can be accessed via the [IdentityPublicKey object's](https://github.com/dashevo/js-dpp/blob/master/lib/identity/IdentityPublicKey.js) `hash` method (e.g. `identity.getPublicKeyById(0).hash()`).",
  "title": "Public key hash"
}
[/block]
** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst DashPlatformProtocol = require('@dashevo/dpp');\n\nconst client = new DAPIClient();\nconst dpp = new DashPlatformProtocol();\n\nconst publicKeyHash = 'bfb0b2a54a8d020b6c68fa4e20cbedcafc8422e0';\nconst publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\ndpp.initialize().then(() => {\n  client.platform.getIdentitiesByPublicKeyHashes(publicKeysBuffer)\n    .then((response) => {\n      const retrievedIdentity = dpp.identity.createFromBuffer(response.identities[0]);\n      console.log(retrievedIdentity.toJSON());\n    });\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetIdentitiesByPublicKeyHashesRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst cbor = require('cbor');\nconst DashPlatformProtocol = require('@dashevo/dpp');\n\nconst dpp = new DashPlatformProtocol();\n\ndpp.initialize()\n  .then(() => {\n    const platformPromiseClient = new PlatformPromiseClient(\n      'http://seed-1.testnet.networks.dash.org:3010',\n    );\n\n    const publicKeyHash = 'bfb0b2a54a8d020b6c68fa4e20cbedcafc8422e0';\n    const publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\n    const getIdentitiesByPublicKeyHashesRequest = new GetIdentitiesByPublicKeyHashesRequest();\n    getIdentitiesByPublicKeyHashesRequest.setPublicKeyHashesList(publicKeysBuffer);\n\n    platformPromiseClient.getIdentitiesByPublicKeyHashes(getIdentitiesByPublicKeyHashesRequest)\n      .then((response) => {\n        const identitiesResponse = response.getIdentitiesList();\n        const identities = identitiesResponse\n          .map((identity) => (identity.length > 0 ? cbor.decode(Buffer.from(identity)) : null));\n        console.log(dpp.identity.createFromObject(identities[0]).toJSON());\n      })\n      .catch((e) => console.error(e));\n  \t});",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `public_key_hashes` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"public_key_hashes\":\"v7CypUqNAgtsaPpOIMvtyvyEIuA=\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentitiesByPublicKeyHashes",
      "language": "shell",
      "name": "gRPCurl"
    },
    {
      "code": "# `public_key_hashes` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"public_key_hashes\":\"v7CypUqNAgtsaPpOIMvtyvyEIuA=\",\n    \"prove\": true\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentitiesByPublicKeyHashes",
      "language": "shell",
      "name": "gRPCurl with Proof"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  protocolVersion: 0,\n  id: 'BbSrAN8tKaoQgFxV9T1as8vKSTxRVYx5bWxDqJoVv5jQ',\n  publicKeys: [\n    {\n      id: 0,\n      type: 0,\n      data: 'AxdL8m7nyyIGA9Y8o2Q9LEj/aUMdrMdxWy60wekNOn5J'\n    }\n  ],\n  balance: 10996072,\n  revision: 0\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"identities\": [\n    \"pWJpZFggnWfXvxOVHSgCWeZIKuYJp7UGzh3rT/ryj38KWJ+zITNnYmFsYW5jZRoAp8loaHJldmlzaW9uAGpwdWJsaWNLZXlzgaNiaWQAZGRhdGFYIQMXS/Ju58siBgPWPKNkPSxI/2lDHazHcVsutMHpDTp+SWR0eXBlAG9wcm90b2NvbFZlcnNpb24A\"\n  ],\n  \"metadata\": {\n    \"height\": 121,\n    \"coreChainLockedHeight\": 545274\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    },
    {
      "code": "// The storeTreeProof contains the requested data\n{\n  \"proof\": {\n    \"rootTreeProof\": \"AQAAAAO/730XK2ZpQ8M/rke2FCWUEvUkNe3Zm7+TMURBHDrqtPcn5nTrzcUK8RoqqZvgwYyvERWIxOuojAWh9JxFPs4on5yLLeYx9zIJIAv5uUESaTlcLrm5kWqtzGcwvrp3rg4BAw==\",\n    \"storeTreeProof\": \"AWyO60NAEaeZnR0hsk6K4NIzhxgC/SxLM4XGAGJpnptfAqshUDZ+353VykAJTytGq/tbOq3wOGwdkljpOZfB+6j/EAFKHml+7jyjG8o3OFVcNZkLd1ZGxu/vwiDRG4p0iXkVXgMgnWfXvxOVHSgCWeZIKuYJp7UGzh3rT/ryj38KWJ+zITMAjaViaWRYIJ1n178TlR0oAlnmSCrmCae1Bs4d60/68o9/ClifsyEzZ2JhbGFuY2UaAKfJaGhyZXZpc2lvbgBqcHVibGljS2V5c4GjYmlkAGRkYXRhWCEDF0vybufLIgYD1jyjZD0sSP9pQx2sx3FbLrTB6Q06fklkdHlwZQBvcHJvdG9jb2xWZXJzaW9uABABJwjSChrmnHIgWreCpNnrdgp5+5QBlr+oeoxemFrpoRcREQ==\",\n    \"signatureLlmqHash\": \"AAAAU76r1uJML75iJSdN/Ev94VMJtsl4KH9m/ULmCII=\",\n    \"signature\": \"CWqlvjvKTbQIeVPElzK2aOoktOzN2CHOjIXb8JH6HhwtdmtuD5ePY60B9GO8OGwdDjj03Cy6pEa0xksdrWWfDgRVZLFKWPXLGWqldUdJDtAMmhVFk4LaDk5JnaA7fB3f\"\n  },\n  \"metadata\": {\n    \"height\": 121,\n    \"coreChainLockedHeight\": 545274\n  }\n}\n",
      "language": "json",
      "name": "Response (gRPCurl with Proof)"
    }
  ]
}
[/block]
##getIdentityIdsByPublicKeyHashes

**Returns**: [Identity](explanation-identity) ID(s) associated with the provided public key hashes
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `public_key_hashes` | Bytes | Yes | Public key hash (double-sha256) of an identity's public key |
| `prove` | Boolean | No | Set to `true` to receive a proof that contains the requested identity IDs |

[block:callout]
{
  "type": "warning",
  "body": "**Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-dapi-endpoints-platform-proofs) for details on decoding the data."
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Note: the hash must be done using all fields of the identity public key object - e.g.\n```json\n{\n  id: 0,\n  type: 0,\n  data: 'A/Wmv+LhxAjSBE19d3OJCUawUiYORF5HDDetZbvTDgQQ'\n}\n```\nWhen using the js-dpp library, the hash can be accessed via the [IdentityPublicKey object's](https://github.com/dashevo/js-dpp/blob/master/lib/identity/IdentityPublicKey.js) `hash` method (e.g. `identity.getPublicKeyById(0).hash()`).",
  "title": "Public key hash"
}
[/block]
** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\n\nconst client = new DAPIClient();\n\n// Get identity from hex public key hash\nconst publicKeyHash = 'bfb0b2a54a8d020b6c68fa4e20cbedcafc8422e0';\nconst publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\nclient.platform.getIdentityIdsByPublicKeyHashes(publicKeysBuffer)\n  .then((response) => {\n    for (const r of response) {\n      console.log(`Identity ID: ${Identifier.from(r).toString()}`);\n    }\n  });",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetIdentityIdsByPublicKeyHashesRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst publicKeyHash = 'bfb0b2a54a8d020b6c68fa4e20cbedcafc8422e0';\nconst publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\nconst getIdentityIdsByPublicKeyHashesRequest = new GetIdentityIdsByPublicKeyHashesRequest();\ngetIdentityIdsByPublicKeyHashesRequest.setPublicKeyHashesList(publicKeysBuffer);\n\nplatformPromiseClient.getIdentityIdsByPublicKeyHashes(getIdentityIdsByPublicKeyHashesRequest)\n  .then((response) => {\n    for (const r of response.getIdentityIdsList()) {\n      const id = Buffer.from(r);\n      console.log(`Identity ID: ${Identifier.from(id).toString()}`);\n    }\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `public_key_hashes` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"public_key_hashes\":\"v7CypUqNAgtsaPpOIMvtyvyEIuA=\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentityIdsByPublicKeyHashes",
      "language": "shell",
      "name": "gRPCurl"
    },
    {
      "code": "# `public_key_hashes` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"public_key_hashes\":\"v7CypUqNAgtsaPpOIMvtyvyEIuA=\",\n    \"prove\": true\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentityIdsByPublicKeyHashes",
      "language": "shell",
      "name": "gRPCurl with Proof"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "Identity ID: BbSrAN8tKaoQgFxV9T1as8vKSTxRVYx5bWxDqJoVv5jQ",
      "language": "text",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"identityIds\": [\n    \"nWfXvxOVHSgCWeZIKuYJp7UGzh3rT/ryj38KWJ+zITM=\"\n  ],\n  \"metadata\": {\n    \"height\": 126,\n    \"coreChainLockedHeight\": 545280\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    },
    {
      "code": "// The storeTreeProof contains the requested data\n{\n  \"proof\": {\n    \"rootTreeProof\": \"AQAAAAPx389OUoMhPwNQz096/f4Ng4i2iIG260YscRpZcUMmKR7NvS3i3YD4jguNQuAcb+uPhaG29YHrd2EG45KSLM8Tn5yLLeYx9zIJIAv5uUESaTlcLrm5kWqtzGcwvrp3rg4BBQ==\",\n    \"storeTreeProof\": \"ATeFN5n8xiKvWvMdvzysqD5u3DkaJAPvATjxds7IrqGlAo4LrtGX49t4RVNwm0NwwqysyHYeIke8adCFBPWk5squEAMUh1EY/fMYJqrQ3x52Svn/D0nJ/1IAIIUxI566SJ8tHo+d5bbQBlOESYAZy+tdC39j2P85E22uAxS/sLKlSo0CC2xo+k4gy+3K/IQi4AAgnWfXvxOVHSgCWeZIKuYJp7UGzh3rT/ryj38KWJ+zITMQAQJ61gkGg2cKVHGmUNUFZ04yHdDjj89QyjTdejs9b1pJERE=\",\n    \"signatureLlmqHash\": \"AAAAABuz53KkQFcbIKei9yoOTb6Nj1/skLoLHj3T+Lc=\",\n    \"signature\": \"Dm3nKYPP3trGIodvTgzm74qJk/NyGUOHQJrgJYW3z93NH0sRkil9U7y0dbQ8bJImBn/qMbpoqq9eIikYk/RV3MjX22GIf+b/ItSH/XQ6DaKce/B69sGTeFW1MmvPiUNX\"\n  },\n  \"metadata\": {\n    \"height\": 126,\n    \"coreChainLockedHeight\": 545280\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl with Proof)"
    }
  ]
}
[/block]
## getDataContract

**Returns**: [Data Contract](explanation-platform-protocol-data-contract) information for the requested data contract
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `id` | Bytes | Yes | A data contract `id` |
| `prove` | Boolean | No | Set to `true` to receive a proof that contains the requested data contract |

[block:callout]
{
  "type": "warning",
  "body": "**Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-dapi-endpoints-platform-proofs) for details on decoding the data."
}
[/block]
** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient();\n\nconst contractId = Identifier.from('2aSUdJhasGANowpgjxARDY8N94dJYQNmp7VbH2WEXkmj');\nclient.platform.getDataContract(contractId).then((response) => {\n  const contract = cbor.decode(response.dataContract);\n  console.dir(contract, { depth: 10 });\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetDataContractRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst contractId = Identifier.from('2aSUdJhasGANowpgjxARDY8N94dJYQNmp7VbH2WEXkmj');\nconst contractIdBuffer = Buffer.from(contractId);\nconst getDataContractRequest = new GetDataContractRequest();\ngetDataContractRequest.setId(contractIdBuffer);\n\nplatformPromiseClient.getDataContract(getDataContractRequest)\n  .then((response) => {\n    const contractResponse = response.getDataContract();\n    const contractBuffer = Buffer.from(contractResponse);\n    console.dir(cbor.decode(contractBuffer), { depth: 5 });\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"F2yo52VQu4BQJomGoOrPEyrPVkOifemB5Eswr1oCRQ4=\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDataContract",
      "language": "shell",
      "name": "gRPCurl"
    },
    {
      "code": "# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"F2yo52VQu4BQJomGoOrPEyrPVkOifemB5Eswr1oCRQ4=\",\n    \"prove\":true\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDataContract",
      "language": "shell",
      "name": "gRPCurl with Proof"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  '$id': Buffer(32) [Uint8Array] [\n     23, 108, 168, 231, 101,  80, 187, 128,\n     80,  38, 137, 134, 160, 234, 207,  19,\n     42, 207,  86,  67, 162, 125, 233, 129,\n    228,  75,  48, 175,  90,   2,  69,  14\n  ],\n  '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',\n  ownerId: Buffer(32) [Uint8Array] [\n    157, 103, 215, 191,  19, 149,  29,  40,\n      2,  89, 230,  72,  42, 230,   9, 167,\n    181,   6, 206,  29, 235,  79, 250, 242,\n    143, 127,  10,  88, 159, 179,  33,  51\n  ],\n  documents: {\n    note: {\n      type: 'object',\n      properties: { message: { type: 'string' } },\n      additionalProperties: false\n    }\n  },\n  protocolVersion: 0\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"dataContract\": \"pWMkaWRYIBdsqOdlULuAUCaJhqDqzxMqz1ZDon3pgeRLMK9aAkUOZyRzY2hlbWF4NGh0dHBzOi8vc2NoZW1hLmRhc2gub3JnL2RwcC0wLTQtMC9tZXRhL2RhdGEtY29udHJhY3Rnb3duZXJJZFggnWfXvxOVHSgCWeZIKuYJp7UGzh3rT/ryj38KWJ+zITNpZG9jdW1lbnRzoWRub3Rlo2R0eXBlZm9iamVjdGpwcm9wZXJ0aWVzoWdtZXNzYWdloWR0eXBlZnN0cmluZ3RhZGRpdGlvbmFsUHJvcGVydGllc/RvcHJvdG9jb2xWZXJzaW9uAA==\",\n  \"metadata\": {\n    \"height\": 129,\n    \"coreChainLockedHeight\": 545286\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    },
    {
      "code": "// The storeTreeProof contains the requested data\n{\n  \"proof\": {\n    \"rootTreeProof\": \"AQAAAAMue8f0VKLw60DfGDnhpCHhLJj5g7fILfFlHx0Uvb7i8B7NvS3i3YD4jguNQuAcb+uPhaG29YHrd2EG45KSLM8Tn5yLLeYx9zIJIAv5uUESaTlcLrm5kWqtzGcwvrp3rg4BAQ==\",\n    \"storeTreeProof\": \"AyAXbKjnZVC7gFAmiYag6s8TKs9WQ6J96YHkSzCvWgJFDgD0pWMkaWRYIBdsqOdlULuAUCaJhqDqzxMqz1ZDon3pgeRLMK9aAkUOZyRzY2hlbWF4NGh0dHBzOi8vc2NoZW1hLmRhc2gub3JnL2RwcC0wLTQtMC9tZXRhL2RhdGEtY29udHJhY3Rnb3duZXJJZFggnWfXvxOVHSgCWeZIKuYJp7UGzh3rT/ryj38KWJ+zITNpZG9jdW1lbnRzoWRub3Rlo2R0eXBlZm9iamVjdGpwcm9wZXJ0aWVzoWdtZXNzYWdloWR0eXBlZnN0cmluZ3RhZGRpdGlvbmFsUHJvcGVydGllc/RvcHJvdG9jb2xWZXJzaW9uAAKebAzm7RJJgwTqwNdlS3NJee8Xt5BYqDoWBIf3sSWG1BACRMS8jo36RpN7TSh79+gQL+UYqhYT7Waqmrb6cuysD4cQAQqdbDdz4OKqh5J81U6ndGXvGUb0V3NkxHagzQ7Vcb7GEQ==\",\n    \"signatureLlmqHash\": \"AAAAABuz53KkQFcbIKei9yoOTb6Nj1/skLoLHj3T+Lc=\",\n    \"signature\": \"hy3QXOZ+pcUbqL1wjGtLeFF8wTgTr0/locJKH4ITLvU9y5dh9l6Kllj21rmpGUHCEuqc6hvni7uWHSwZCo914agNFStRFmJIQxdb37oTIXvo6JFg4dOgXpJe/s+EfwLD\"\n  },\n  \"metadata\": {\n    \"height\": 130,\n    \"coreChainLockedHeight\": 545289\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl with Proof)"
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
| `prove` | Boolean | No | Set to `true` to receive a proof that contains the requested document(s) |

[block:callout]
{
  "type": "warning",
  "body": "**Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-dapi-endpoints-platform-proofs) for details on decoding the data."
}
[/block]

** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient();\n\nconst contractId = Identifier.from('2aSUdJhasGANowpgjxARDY8N94dJYQNmp7VbH2WEXkmj');\nclient.platform.getDocuments(contractId, 'note', { limit: 10 }).then((response) => {\n  for (const rawData of response) {\n    console.log(cbor.decode(rawData));\n  }\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetDocumentsRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst cbor = require('cbor');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst contractId = Identifier.from('2aSUdJhasGANowpgjxARDY8N94dJYQNmp7VbH2WEXkmj');\nconst contractIdBuffer = Buffer.from(contractId);\nconst getDocumentsRequest = new GetDocumentsRequest();\nconst type = 'note';\nconst limit = 10;\n\ngetDocumentsRequest.setDataContractId(contractIdBuffer);\ngetDocumentsRequest.setDocumentType(type);\n// getDocumentsRequest.setWhere(whereSerialized);\n// getDocumentsRequest.setOrderBy(orderBySerialized);\ngetDocumentsRequest.setLimit(limit);\n// getDocumentsRequest.setStartAfter(startAfter);\n// getDocumentsRequest.setStartAt(startAt);\n\nplatformPromiseClient.getDocuments(getDocumentsRequest)\n  .then((response) => {\n    for (const document of response.getDocumentsList()) {\n      console.log(cbor.decode(Buffer.from(document)));\n    }\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# Request one DPNS document\n# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"data_contract_id\":\"F2yo52VQu4BQJomGoOrPEyrPVkOifemB5Eswr1oCRQ4=\",\n    \"document_type\":\"note\",\n    \"limit\":10\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDocuments",
      "language": "shell",
      "name": "Request (gRPCurl)"
    },
    {
      "code": "# Request one DPNS document\n# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"data_contract_id\":\"F2yo52VQu4BQJomGoOrPEyrPVkOifemB5Eswr1oCRQ4=\",\n    \"document_type\":\"note\",\n    \"limit\":10,\n    \"prove\":true\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDocuments",
      "language": "shell",
      "name": "Request (gRPCurl with Proof)"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  '$id': <Buffer 3c a1 b4 cc cf 7a cd c2 d5 a6 d5 3a 3b 69 66 ad 75 c6 24 d0 45 05 aa e6 22 8e f5 83 25 9f 0a d7>,\n  '$type': 'note',\n  message: 'Tutorial CI Test @ Tue, 27 Jul 2021 13:06:35 GMT',\n  '$ownerId': <Buffer 9d 67 d7 bf 13 95 1d 28 02 59 e6 48 2a e6 09 a7 b5 06 ce 1d eb 4f fa f2 8f 7f 0a 58 9f b3 21 33>,\n  '$revision': 1,\n  '$dataContractId': <Buffer 17 6c a8 e7 65 50 bb 80 50 26 89 86 a0 ea cf 13 2a cf 56 43 a2 7d e9 81 e4 4b 30 af 5a 02 45 0e>,\n  '$protocolVersion': 0\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"documents\": [\n    \"p2MkaWRYIDyhtMzPes3C1abVOjtpZq11xiTQRQWq5iKO9YMlnwrXZSR0eXBlZG5vdGVnbWVzc2FnZXgwVHV0b3JpYWwgQ0kgVGVzdCBAIFR1ZSwgMjcgSnVsIDIwMjEgMTM6MDY6MzUgR01UaCRvd25lcklkWCCdZ9e/E5UdKAJZ5kgq5gmntQbOHetP+vKPfwpYn7MhM2kkcmV2aXNpb24BbyRkYXRhQ29udHJhY3RJZFggF2yo52VQu4BQJomGoOrPEyrPVkOifemB5Eswr1oCRQ5wJHByb3RvY29sVmVyc2lvbgA=\"\n  ],\n  \"metadata\": {\n    \"height\": 132,\n    \"coreChainLockedHeight\": 545298\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    },
    {
      "code": "// The storeTreeProof contains the requested data\n{\n  \"proof\": {\n    \"rootTreeProof\": \"AQAAAAI3K3b38xhhvsh49mtplZhoX9g81EXJEjxwHeuku+QsJdUkWafhJfD3uDjsl+ZmSHUDrjn7xwbwAozYDr1Uv6b4AQI=\",\n    \"storeTreeProof\": \"AXNwJkbsrxIoxhurh+nOi7+LkOqQidT6rmHnqJqr0KfkAin9QcwUR4nCi2beKYCsCrARr2lAa2YnJY2BJu2lM2MbEAEraKbQuKTiu9dsfWhOKKFL3+XuAOO4s8EMWSMItNzOsQMgPKG0zM96zcLVptU6O2lmrXXGJNBFBarmIo71gyWfCtcA5qdjJGlkWCA8obTMz3rNwtWm1To7aWatdcYk0EUFquYijvWDJZ8K12UkdHlwZWRub3RlZ21lc3NhZ2V4MFR1dG9yaWFsIENJIFRlc3QgQCBUdWUsIDI3IEp1bCAyMDIxIDEzOjA2OjM1IEdNVGgkb3duZXJJZFggnWfXvxOVHSgCWeZIKuYJp7UGzh3rT/ryj38KWJ+zITNpJHJldmlzaW9uAW8kZGF0YUNvbnRyYWN0SWRYIBdsqOdlULuAUCaJhqDqzxMqz1ZDon3pgeRLMK9aAkUOcCRwcm90b2NvbFZlcnNpb24AEAF05KHTyyYguECyveRfD5nB+umrpuE+NUtdZ+bLsim0VRERAsf6E4Sx5AJ0bCpD6L4YbAeocPL1FqWExMMpKk/LYiYrEAFWv4FD9lfbosegLN5/13ajFH2I7XJ9gD2r3kx8KKRyMBE=\",\n    \"signatureLlmqHash\": \"AAAAABuz53KkQFcbIKei9yoOTb6Nj1/skLoLHj3T+Lc=\",\n    \"signature\": \"iHkJflUfSQ98L6gPVFu03s+ZuMM6L4VXgFs6KifrkLLFAhYPI8goe63AZKiePCoTDv/aaEr0oWTQT53WNrRuEabI0ABn8KZE4BndkTTn0FmIgQ7T6eg+pqFuTtrptBqM\"\n  },\n  \"metadata\": {\n    \"height\": 132,\n    \"coreChainLockedHeight\": 545298\n  }\n}\n",
      "language": "json",
      "name": "Response (gRPCurl with Proof)"
    }
  ]
}
[/block]
## waitForStateTransitionResult
[block:callout]
{
  "type": "success",
  "body": "",
  "title": "Added in Dash Platform 0.18.0"
}
[/block]
**Returns**: The state transition hash and either a proof that the state transition was confirmed in a block or an error. 
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `state_transition_hash` | Bytes | Yes | Hash of the state transition |
| `prove` | Boolean | Yes | Set to `true` to request a proof |

[block:callout]
{
  "type": "warning",
  "body": "**Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-dapi-endpoints-platform-proofs) for details on decoding the data."
}
[/block]

** Example Request and Response**
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst DashPlatformProtocol = require('@dashevo/dpp');\nconst crypto = require('crypto');\n\nconst client = new DAPIClient();\nconst dpp = new DashPlatformProtocol();\n\nconst stateTransitionObject = {\n  protocolVersion: 0,\n  type: 0,\n  signature: 'HxAipUsLWQBE++C1suSRNQiQh91rI1LZbblvQhk2erUaIvRneAagxGYYsXXYNvEeO+lBzlF1a9KHGGTHgnO/8Ts=',\n  signaturePublicKeyId: 0,\n  dataContract: {\n    protocolVersion: 0,\n    '$id': 'CMc7RghKkHeHtFdwfSX5Hzy7CUdpCEJnwsbfHdsbmJ32',\n    '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',\n    ownerId: '8Z3ps3tNoGoPEDYerUNCd4yi7zDwgBh2ejgSMExxvkfD',\n    documents: {\n      note: {\n        properties: { message: { type: 'string' } },\n        additionalProperties: false,\n      },\n    },\n  },\n  entropy: '+RqUArypdL8f/gCMAo4b6c3CoQvxHzsQG0BdYrT5QT0=',\n};\n\n// Convert signature and entropy to buffer\nstateTransitionObject.signature = Buffer.from(stateTransitionObject.signature, 'base64');\nstateTransitionObject.entropy = Buffer.from(stateTransitionObject.entropy, 'base64');\n\ndpp.stateTransition.createFromObject(stateTransitionObject, { skipValidation: true })\n  .then((stateTransition) => {\n    //  Calculate state transition hash\n    const hash = crypto.createHash('sha256')\n      .update(stateTransition.toBuffer())\n      .digest();\n\n    console.log(`Requesting proof of state transition with hash:\\n\\t${hash.toString('hex')}`);\n\n    client.platform.waitForStateTransitionResult(hash, { prove: true })\n      .then((response) => {\n        console.log(response);\n      });\n  });\n",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    WaitForStateTransitionResultRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst DashPlatformProtocol = require('@dashevo/dpp');\nconst crypto = require('crypto');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst dpp = new DashPlatformProtocol();\n\nconst stateTransitionObject = {\n  protocolVersion: 0,\n  type: 0,\n  signature: 'HxAipUsLWQBE++C1suSRNQiQh91rI1LZbblvQhk2erUaIvRneAagxGYYsXXYNvEeO+lBzlF1a9KHGGTHgnO/8Ts=',\n  signaturePublicKeyId: 0,\n  dataContract: {\n    protocolVersion: 0,\n    '$id': 'CMc7RghKkHeHtFdwfSX5Hzy7CUdpCEJnwsbfHdsbmJ32',\n    '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',\n    ownerId: '8Z3ps3tNoGoPEDYerUNCd4yi7zDwgBh2ejgSMExxvkfD',\n    documents: {\n      note: {\n        properties: { message: { type: 'string' } },\n        additionalProperties: false,\n      },\n    },\n  },\n  entropy: '+RqUArypdL8f/gCMAo4b6c3CoQvxHzsQG0BdYrT5QT0=',\n};\n\n// Convert signature and entropy to buffer\nstateTransitionObject.signature = Buffer.from(stateTransitionObject.signature, 'base64');\nstateTransitionObject.entropy = Buffer.from(stateTransitionObject.entropy, 'base64');\n\ndpp.stateTransition.createFromObject(stateTransitionObject, { skipValidation: true })\n  .then((stateTransition) => {\n    //  Calculate state transition hash\n    const hash = crypto.createHash('sha256')\n      .update(stateTransition.toBuffer())\n      .digest();\n\n    const waitForStateTransitionResultRequest = new WaitForStateTransitionResultRequest();\n    waitForStateTransitionResultRequest.setStateTransitionHash(hash);\n    waitForStateTransitionResultRequest.setProve(true);\n\n    console.log(`Requesting proof of state transition with hash:\\n\\t${hash.toString('hex')}`);\n\n    platformPromiseClient.waitForStateTransitionResult(waitForStateTransitionResultRequest)\n      .then((response) => {\n        const rootTreeProof = Buffer.from(response.getProof().getRootTreeProof());\n        const storeTreeProof = Buffer.from(response.getProof().getStoreTreeProof());\n        console.log(`Root tree proof: ${rootTreeProof.toString('hex')}`);\n        console.log(`Store tree proof: ${storeTreeProof.toString('hex')}`);\n      })\n  \t\t.catch((e) => console.error(e));\n  });",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `state_transition_hash` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"state_transition_hash\":\"iuk7icJyRV886NAdupmjooyVUCYqYCxrpE3gjlRdOqk=\",\n    \"prove\": \"true\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/waitForStateTransitionResult",
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
      "code": "{\n  \"proof\": {\n    \"rootTreeProof\": \"AQAAAAMm4DXgMYJ+fCewkSNB7dIRvzuQVHARLGhajnaMaLs5IT3PRm0J0Hoo4+kLKw4dFRDe3jAhT2jjL4z0mCIBAQ==\",\n    \"storeTreeProof\": \"AQvuMc7KKr1EatvUnxNKfXAllqm5Am7E4ZCV96G0yd4f5GPmzvdYOlvDEAF4m0+Ymsn49STxJH/tNyUC2MVKPAJgctUjnwdEImIWc8JQ0cdOrbswShABP7VKme9kG5p1hdbSjdRDh15DWjUCLZKgcR9WriO8E/SmMKFFWXBFHj8QAf4rBg/Kac4us9eEzsKMD1dWkPExAm3yUq8GhjW98I9USOpnwj2amgKDEAGn3wqfOSaC19fQopvUPZMsFrDWUwMgqLffTK22zf1bXRuzHLtIjSQekdYMwTQc1oaj+7YpHxnoAKVjJGlkWCCot99MrbbN/VtdG7Mcu0iNJB6R1gzBNBzWhqP7tikfGWckc2NoZW1heDRodHRwczovL3NjaGVtYS5kYXNoLm9yZy9kcHAtMC00LTAvbWV0YS9kYXRhLWNvbnRyYWN0Z293bmVySWRYIHA3lr/T4rvVRQWo4EkpuwW4rs+xzVwBPviouEURdw4MaWRvY3VtZW50c6Fkbm90ZaJqcHJvcGVydGllc6FnbWVzc2FnZaFkdHlwZWZzdHJpbmd0YWRkaXRpb25hbFByb3BlcnRpZXP0b3Byb3RvY29sVmVyc2lvbgAQAdlM1ABEuEheli2A5XwZksd6GCoWERERAqvn4/cjHtcbjE/X7gnUoflwpRzUEAE5MS/q3OFFMT7zT3IOlAiS0O0kBRERAiRcaqrxkrUSBzM76Fq3fJyTk69HEAEo731EeeT0iDc9kigP6OUqmkimYhER\"\n  }\n}\n",
      "language": "json",
      "name": "Response (gRPCurl)"
    }
  ]
}
[/block]
# Deprecated Endpoints

There are no recently deprecated endpoint, but the previous version of documentation can be [viewed here](https://dashplatform.readme.io/v0.19.0/docs/reference-dapi-endpoints-platform-endpoints).

# Code Reference
Implementation details related to the information on this page can be found in:
- The [DAPI repository](https://github.com/dashevo/dapi) `lib/grpcServer/handlers/platform` folder
- The [dapi-grpc repository](https://github.com/dashevo/dapi-grpc/) `protos` folder