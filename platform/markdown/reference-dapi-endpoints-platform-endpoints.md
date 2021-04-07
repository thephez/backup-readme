[block:callout]
{
  "type": "info",
  "title": "gRPC Overview",
  "body": "Please refer to the [gRPC Overview](reference-dapi-endpoints-grpc-overview) for details regarding running the examples shown below, encoding/decoding the request/response data, and clients available for several languages."
}
[/block]
# Endpoint Details

## broadcastStateTransition
[block:callout]
{
  "type": "success",
  "body": "Since Dash Platform 0.18.0 `broadcastStateTransition` returns once the state transition has been accepted into the mempool instead of waiting until it is confirmed. \n\n**Note:** The [`waitForStateTransitionResult` endpoint](#waitforstatetransitionresult) should be used in conjunction with this one for instances where proof of block confirmation is required.",
  "title": "Updated in Dash Platform 0.18.0"
}
[/block]
Broadcasts a [state transition](explanation-platform-protocol-state-transition) to the platform via DAPI to make a change to layer 2 data.

**Returns**: Nothing or error
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `state_transition` | Bytes | Yes | A [state transition](explanation-platform-protocol-state-transition) |

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

** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient();\n\nconst identityId = Identifier.from('BhC9M3fQHyUCyuxH4WHdhn1VGgJ4JTLmer8qmTTHkYTe');\nclient.platform.getIdentity(identityId).then((response) => {\n  const identity = cbor.decode(response);\n  console.log(identity);\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n  },\n} = require('@dashevo/dapi-grpc');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst id = Identifier.from('BhC9M3fQHyUCyuxH4WHdhn1VGgJ4JTLmer8qmTTHkYTe');\nconst idBuffer = Buffer.from(id);\nconst getIdentityRequest = new GetIdentityRequest();\ngetIdentityRequest.setId(idBuffer);\n\nplatformPromiseClient.getIdentity(getIdentityRequest)\n  .then((response) => {\n    const identityResponse = response.getIdentity();\n    const identityBuffer = Buffer.from(identityResponse);\n    console.log(cbor.decode(identityBuffer));\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"nuCzumg/s5LC1SplgbztDxV9AfZBr1wNklM4zK8xZ90=\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentity",
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
      "code": "{\n  id: <Buffer 9e e0 b3 ba 68 3f b3 92 c2 d5 2a 65 81 bc ed 0f 15 7d 01 f6 41 af 5c 0d 92 53 38 cc af 31 67 dd>,\n  balance: 10994307,\n  revision: 0,\n  publicKeys: [\n    {\n      id: 0,\n      data: <Buffer 03 f5 a6 bf e2 e1 c4 08 d2 04 4d 7d 77 73 89 09 46 b0 52 26 0e 44 5e 47 0c 37 ad 65 bb d3 0e 04 10>,\n      type: 0\n    }\n  ],\n  protocolVersion: 0\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"identity\": \"pWJpZFggnuCzumg/s5LC1SplgbztDxV9AfZBr1wNklM4zK8xZ91nYmFsYW5jZRoAp7gPaHJldmlzaW9uAGpwdWJsaWNLZXlzgaNiaWQAZGRhdGFYIQP1pr/i4cQI0gRNfXdziQlGsFImDkReRww3rWW70w4EEGR0eXBlAG9wcm90b2NvbFZlcnNpb24A\"\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
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
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst DashPlatformProtocol = require('@dashevo/dpp');\n\nconst client = new DAPIClient();\nconst dpp = new DashPlatformProtocol();\n\nconst publicKeyHash = 'afa1783993cb690ccd4ab53765589437ccca83e7';\nconst publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\nclient.platform.getIdentitiesByPublicKeyHashes(publicKeysBuffer)\n  .then((response) => {\n    const retrievedIdentity = dpp.identity.createFromBuffer(response[0]);\n    console.log(retrievedIdentity.toJSON());\n  });",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetIdentitiesByPublicKeyHashesRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst cbor = require('cbor');\nconst DashPlatformProtocol = require('@dashevo/dpp');\n\nconst dpp = new DashPlatformProtocol();\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst publicKeyHash = 'afa1783993cb690ccd4ab53765589437ccca83e7';\nconst publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\nconst getIdentitiesByPublicKeyHashesRequest = new GetIdentitiesByPublicKeyHashesRequest();\ngetIdentitiesByPublicKeyHashesRequest.setPublicKeyHashesList(publicKeysBuffer);\n\nplatformPromiseClient.getIdentitiesByPublicKeyHashes(getIdentitiesByPublicKeyHashesRequest)\n  .then((response) => {\n    const identitiesResponse = response.getIdentitiesList();\n    const identities = identitiesResponse\n      .map((identity) => (identity.length > 0 ? cbor.decode(Buffer.from(identity)) : null));\n    console.log(dpp.identity.createFromObject(identities[0]).toJSON());\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `public_key_hashes` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"public_key_hashes\":\"r6F4OZPLaQzNSrU3ZViUN8zKg+c=\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentitiesByPublicKeyHashes",
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
      "code": "{\n  protocolVersion: 0,\n  id: 'BhC9M3fQHyUCyuxH4WHdhn1VGgJ4JTLmer8qmTTHkYTe',\n  publicKeys: [\n    {\n      id: 0,\n      type: 0,\n      data: 'A/Wmv+LhxAjSBE19d3OJCUawUiYORF5HDDetZbvTDgQQ'\n    }\n  ],\n  balance: 10991320,\n  revision: 0\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"identities\": [\n    \"pWJpZFggmxU33DGz26TMBLW0pjNE06+RWmmbfCrKbOXHXBFrPfZnYmFsYW5jZRoAp8IhaHJldmlzaW9uAGpwdWJsaWNLZXlzgaNiaWQAZGRhdGFYIQKqExozt8o2EV9+p4es4sdvNLHb+uSzmJt3l+UHScAFmWR0eXBlAG9wcm90b2NvbFZlcnNpb24A\"\n  ]\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
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
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\n\nconst client = new DAPIClient();\n\n// Get identity from hex public key hash\nconst publicKeyHash = 'afa1783993cb690ccd4ab53765589437ccca83e7';\nconst publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\nclient.platform.getIdentityIdsByPublicKeyHashes(publicKeysBuffer)\n  .then((response) => {\n    for (const r of response) {\n      console.log(`Identity ID: ${Identifier.from(r).toString()}`);\n    }\n  });",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetIdentityIdsByPublicKeyHashesRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst publicKeyHash = 'afa1783993cb690ccd4ab53765589437ccca83e7';\nconst publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\nconst getIdentityIdsByPublicKeyHashesRequest = new GetIdentityIdsByPublicKeyHashesRequest();\ngetIdentityIdsByPublicKeyHashesRequest.setPublicKeyHashesList(publicKeysBuffer);\n\nplatformPromiseClient.getIdentityIdsByPublicKeyHashes(getIdentityIdsByPublicKeyHashesRequest)\n  .then((response) => {\n    for (const r of response.getIdentityIdsList()) {\n      const id = Buffer.from(r);\n      console.log(`Identity ID: ${Identifier.from(id).toString()}`);\n    }\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `public_key_hashes` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"public_key_hashes\":\"r6F4OZPLaQzNSrU3ZViUN8zKg+c=\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentityIdsByPublicKeyHashes",
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
      "code": "Identity ID: Bbqt9aQgKGJeYeYvmKQUTSh1UbhGQV15sN5Jq7AjSGms",
      "language": "text",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"identityIds\": [\n    \"mxU33DGz26TMBLW0pjNE06+RWmmbfCrKbOXHXBFrPfY=\"\n  ]\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
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

** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient();\n\nconst contractId = Identifier.from('6Ti3c7nvD1gDf4gFi8a3FfZVhVLiRsGLnQ7nCAF74osi');\nclient.platform.getDataContract(contractId).then((response) => {\n  const contract = cbor.decode(response);\n  console.dir(contract, { depth:10 });\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetDataContractRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst contractId = Identifier.from('6Ti3c7nvD1gDf4gFi8a3FfZVhVLiRsGLnQ7nCAF74osi');\nconst contractIdBuffer = Buffer.from(contractId);\nconst getDataContractRequest = new GetDataContractRequest();\ngetDataContractRequest.setId(contractIdBuffer);\n\nplatformPromiseClient.getDataContract(getDataContractRequest)\n  .then((response) => {\n    const contractResponse = response.getDataContract();\n    const contractBuffer = Buffer.from(contractResponse);\n    console.dir(cbor.decode(contractBuffer), { depth: 5 });\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"USHx/8H6lZvZGKbGltP9oW4QgnGP61q70JD+h8rb/E0=\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDataContract",
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
      "code": "{\n  '$id': Buffer(32) [Uint8Array] [\n     81,  33, 241, 255, 193, 250, 149,\n    155, 217,  24, 166, 198, 150, 211,\n    253, 161, 110,  16, 130, 113, 143,\n    235,  90, 187, 208, 144, 254, 135,\n    202, 219, 252,  77\n  ],\n  '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',\n  ownerId: Buffer(32) [Uint8Array] [\n    155,  21,  55, 220,  49, 179, 219, 164,\n    204,   4, 181, 180, 166,  51,  68, 211,\n    175, 145,  90, 105, 155, 124,  42, 202,\n    108, 229, 199,  92,  17, 107,  61, 246\n  ],\n  documents: {\n    note: {\n      properties: { message: { type: 'string' } },\n      additionalProperties: false\n    }\n  },\n  protocolVersion: 0\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"dataContract\": \"pWMkaWRYIFEh8f/B+pWb2RimxpbT/aFuEIJxj+tau9CQ/ofK2/xNZyRzY2hlbWF4NGh0dHBzOi8vc2NoZW1hLmRhc2gub3JnL2RwcC0wLTQtMC9tZXRhL2RhdGEtY29udHJhY3Rnb3duZXJJZFggmxU33DGz26TMBLW0pjNE06+RWmmbfCrKbOXHXBFrPfZpZG9jdW1lbnRzoWRub3Rlompwcm9wZXJ0aWVzoWdtZXNzYWdloWR0eXBlZnN0cmluZ3RhZGRpdGlvbmFsUHJvcGVydGllc/RvcHJvdG9jb2xWZXJzaW9uAA==\"\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    }
  ]
}
[/block]

## getDocuments

**Returns**: [Document](explanation-platform-protocol-document) information for the requested document(s)
**Parameters**:

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
[block:callout]
{
  "type": "warning",
  "title": "* Parameter constraints",
  "body": "The `where`, `order_by`, `limit`, `start_at`, and `start_after` parameters must comply with the limits defined on the [Query Syntax](reference-query-syntax) page.\n\nAdditionally, note that `where` and `order_by` must be [CBOR](https://tools.ietf.org/html/rfc7049) encoded."
}
[/block]

** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient();\n\nconst contractId = Identifier.from('6Ti3c7nvD1gDf4gFi8a3FfZVhVLiRsGLnQ7nCAF74osi');\nclient.platform.getDocuments(contractId, 'note', { limit: 10 }).then((response) => {\n  for (const rawData of response) {\n    console.log(cbor.decode(rawData));\n  }\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetDocumentsRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst cbor = require('cbor');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst contractId = Identifier.from('6Ti3c7nvD1gDf4gFi8a3FfZVhVLiRsGLnQ7nCAF74osi');\nconst contractIdBuffer = Buffer.from(contractId);\nconst getDocumentsRequest = new GetDocumentsRequest();\nconst type = 'note';\nconst limit = 10;\n\ngetDocumentsRequest.setDataContractId(contractIdBuffer);\ngetDocumentsRequest.setDocumentType(type);\n// getDocumentsRequest.setWhere(whereSerialized);\n// getDocumentsRequest.setOrderBy(orderBySerialized);\ngetDocumentsRequest.setLimit(limit);\n// getDocumentsRequest.setStartAfter(startAfter);\n// getDocumentsRequest.setStartAt(startAt);\n\nplatformPromiseClient.getDocuments(getDocumentsRequest)\n  .then((response) => {\n    for (const document of response.getDocumentsList()) {\n      console.log(cbor.decode(Buffer.from(document)));\n    }\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# Request one DPNS document\n# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"data_contract_id\":\"USHx/8H6lZvZGKbGltP9oW4QgnGP61q70JD+h8rb/E0=\",\n    \"document_type\":\"note\",\n    \"limit\":10\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDocuments",
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
      "code": "{\n  '$id': <Buffer ce 41 d0 51 36 0d 22 ee 58 ad 29 1b 94 bc ed 2c 43 a6 ce 3f d5 5b 9e cc ad 66 65 97 27 09 71 13>,\n  '$type': 'note',\n  message: 'Tutorial Test @ Wed, 30 Dec 2020 21:12:38 GMT',\n  '$ownerId': <Buffer 9b 15 37 dc 31 b3 db a4 cc 04 b5 b4 a6 33 44 d3 af 91 5a 69 9b 7c 2a ca 6c e5 c7 5c 11 6b 3d f6>,\n  '$revision': 1,\n  '$dataContractId': <Buffer 51 21 f1 ff c1 fa 95 9b d9 18 a6 c6 96 d3 fd a1 6e 10 82 71 8f eb 5a bb d0 90 fe 87 ca db fc 4d>,\n  '$protocolVersion': 0\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"documents\": [\n    \"p2MkaWRYIM5B0FE2DSLuWK0pG5S87SxDps4/1VuezK1mZZcnCXETZSR0eXBlZG5vdGVnbWVzc2FnZXgtVHV0b3JpYWwgVGVzdCBAIFdlZCwgMzAgRGVjIDIwMjAgMjE6MTI6MzggR01UaCRvd25lcklkWCCbFTfcMbPbpMwEtbSmM0TTr5FaaZt8Ksps5cdcEWs99mkkcmV2aXNpb24BbyRkYXRhQ29udHJhY3RJZFggUSHx/8H6lZvZGKbGltP9oW4QgnGP61q70JD+h8rb/E1wJHByb3RvY29sVmVyc2lvbgA=\"\n  ]\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
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

There are no recently deprecated endpoint, but the previous version of documentation can be [viewed here](https://dashplatform.readme.io/v0.17.0/docs/reference-dapi-endpoints-platform-endpoints).

# Code Reference
Implementation details related to the information on this page can be found in:
- The [DAPI repository](https://github.com/dashevo/dapi) `lib/grpcServer/handlers/platform` folder
- The [dapi-grpc repository](https://github.com/dashevo/dapi-grpc/) `protos` folder