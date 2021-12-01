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
  "body": "Since Dash Platform 0.18.0, `broadcastStateTransition` returns once the state transition has been accepted into the mempool instead of waiting until it is confirmed. \n\n**Note:** The [`waitForStateTransitionResult` endpoint](#waitforstatetransitionresult) should be used in conjunction with this one for instances where proof of block confirmation is required.",
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
| `prove` | Boolean | No | Set to `true` to receive a proof that contains the requested identity |

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
      "code": "# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"KVKPajj5rQh5LpoDLZk8dtHFbASadzzPvgLPFm4P9q0=\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentity",
      "language": "shell",
      "name": "gRPCurl"
    },
    {
      "code": "# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"KVKPajj5rQh5LpoDLZk8dtHFbASadzzPvgLPFm4P9q0=\",\n    \"prove\":true\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentity",
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
      "code": "{\n  id: <Buffer 29 52 8f 6a 38 f9 ad 08 79 2e 9a 03 2d 99 3c 76 d1 c5 6c 04 9a 77 3c cf be 02 cf 16 6e 0f f6 ad>,\n  balance: 48570,\n  revision: 0,\n  publicKeys: [\n    {\n      id: 0,\n      data: <Buffer 02 f2 3b 65 31 e0 46 01 e4 ee ed 53 c1 44 e0 6a 28 2e e0 56 40 8a 8b 22 97 9c a7 85 c8 11 b9 9c 59>,\n      type: 0\n    }\n  ]\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"identity\": \"AQAAAKRiaWRYIClSj2o4+a0IeS6aAy2ZPHbRxWwEmnc8z74CzxZuD/atZ2JhbGFuY2UZvbpocmV2aXNpb24AanB1YmxpY0tleXOBo2JpZABkZGF0YVghAvI7ZTHgRgHk7u1TwUTgaigu4FZAiosil5ynhcgRuZxZZHR5cGUA\",\n  \"metadata\": {\n    \"height\": \"4087\",\n    \"coreChainLockedHeight\": 602550\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    },
    {
      "code": "// The storeTreeProof contains the requested data\n{\n  \"proof\": {\n    \"rootTreeProof\": \"v+99FytmaUPDP65HthQllBL1JDXt2Zu/kzFEQRw66rQYEo4ebstz3CguuRKrBP2Cf9cMMDexKgMIX8a4ZnbtzoRRIZem6YRYjSO6e78TdtgwuuxbtaQ9t7uZI8EZI3q6\",\n    \"storeTreeProofs\": {\n      \"identitiesProof\": \"Ab3+9XLi6seGGvOeC+YPa6820TTQJg+/VLMTI1l+Aa8uAkoa3iCpnFGnKNsuN3kmkoZziG5966ZPSns0QhkuLhcMEAHqjSiFz+M0EUzxVwMmeYDzBJtbkAf1Dt5ZV0wHrPSy/gJQNWR9nfgU8mqYVkQZvn+cgnUW+ZfuJItL0Vihkm7sbhABLEnf8Xpnpkn4RBFx+7Ftc9+kA+AeukotY1n+Cry0f/kDIClSj2o4+a0IeS6aAy2ZPHbRxWwEmnc8z74CzxZuD/atAH4BAAAApGJpZFggKVKPajj5rQh5LpoDLZk8dtHFbASadzzPvgLPFm4P9q1nYmFsYW5jZRm9umhyZXZpc2lvbgBqcHVibGljS2V5c4GjYmlkAGRkYXRhWCEC8jtlMeBGAeTu7VPBROBqKC7gVkCKiyKXnKeFyBG5nFlkdHlwZQAQAYr7Kwts+/MCvdAEAfe0qlM939luA/bmFk/NWn3j+4XMERERAk89CgifO1qwuFxa+KOoekT9JZIRnwerQ3sSHCSWvqliEAHjXdp0pKgcIEmQaWfWG970qaHE8iQWEjp0z9WXkJYAxBECJq4BeR3ucca+V5J1neVZaj8NTzLC7tTtPzMDJ1udK3sQAVSpKcwG23rP77Wl0GF9QCtgi7AMuIA4W6ZDlpJ5guKKEQ==\"\n    },\n    \"signatureLlmqHash\": \"AAAA1p9iTq8xLCvbRQKqUT0TgOksMi4pRUwPp3mPhDI=\",\n    \"signature\": \"lk8TENYBLqhmBaw/IzyR8IjCM7aBhMne/EMwocLs0oQ+JyOKrNTJ8giIgc1aXAKCFDdqgpOMZNPnZcdgcnqJVUpRqIAebTlVdvIZODYpG8LZTA+cQtOqTwVkH3A3c+tr\"\n  },\n  \"metadata\": {\n    \"height\": \"4087\",\n    \"coreChainLockedHeight\": 602550\n  }\n}",
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
  "type": "info",
  "body": "**Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-platform-proofs) for details on decoding the data."
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Note: the hash must be done using all fields of the identity public key object - e.g.\n```json\n{\n  id: 0,\n  type: 0,\n  data: 'A/Wmv+LhxAjSBE19d3OJCUawUiYORF5HDDetZbvTDgQQ'\n}\n```\nWhen using the js-dpp library, the hash can be accessed via the [IdentityPublicKey object's](https://github.com/dashevo/platform/blob/master/packages/js-dpp/lib/identity/IdentityPublicKey.js) `hash` method (e.g. `identity.getPublicKeyById(0).hash()`).",
  "title": "Public key hash"
}
[/block]
** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst DashPlatformProtocol = require('@dashevo/dpp');\n\nconst client = new DAPIClient();\nconst dpp = new DashPlatformProtocol();\n\nconst publicKeyHash = '1e5debba672f105e6f0635fa21a6491d459b8d83';\nconst publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\ndpp.initialize().then(() => {\n  client.platform.getIdentitiesByPublicKeyHashes(publicKeysBuffer)\n    .then((response) => {\n      const retrievedIdentity = dpp.identity.createFromBuffer(response.identities[0]);\n      console.log(retrievedIdentity.toJSON());\n    });\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetIdentitiesByPublicKeyHashesRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst cbor = require('cbor');\nconst DashPlatformProtocol = require('@dashevo/dpp');\n\nconst dpp = new DashPlatformProtocol();\n\ndpp.initialize()\n  .then(() => {\n    const platformPromiseClient = new PlatformPromiseClient(\n      'http://seed-1.testnet.networks.dash.org:3010',\n    );\n\n    const publicKeyHash = '1e5debba672f105e6f0635fa21a6491d459b8d83';\n    const publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\n    const getIdentitiesByPublicKeyHashesRequest = new GetIdentitiesByPublicKeyHashesRequest();\n    getIdentitiesByPublicKeyHashesRequest.setPublicKeyHashesList(publicKeysBuffer);\n\n    platformPromiseClient.getIdentitiesByPublicKeyHashes(getIdentitiesByPublicKeyHashesRequest)\n      .then((response) => {\n        const identitiesResponse = response.getIdentitiesList();\n        const identities = identitiesResponse\n        \t.map((identity) => (identity.length > 0 ? Buffer.from(identity) : null));\n        console.log(dpp.identity.createFromBuffer(identityBuffers[0]).toJSON());\n      })\n      .catch((e) => console.error(e));\n  \t});",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `public_key_hashes` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n      \"public_key_hashes\":\"Hl3rumcvEF5vBjX6IaZJHUWbjYM=\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentitiesByPublicKeyHashes",
      "language": "shell",
      "name": "gRPCurl"
    },
    {
      "code": "# `public_key_hashes` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"public_key_hashes\":\"Hl3rumcvEF5vBjX6IaZJHUWbjYM=\",\n    \"prove\": true\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentitiesByPublicKeyHashes",
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
      "code": "{\n  protocolVersion: 1,\n  id: '3nJjUPiVbwvKRpZ6pLUwVjcFoEJdeqPqakobctF3uECU',\n  publicKeys: [\n    {\n      id: 0,\n      type: 0,\n      data: 'AvI7ZTHgRgHk7u1TwUTgaigu4FZAiosil5ynhcgRuZxZ'\n    }\n  ],\n  balance: 48570,\n  revision: 0\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"identities\": [\n    \"AQAAAKRiaWRYIClSj2o4+a0IeS6aAy2ZPHbRxWwEmnc8z74CzxZuD/atZ2JhbGFuY2UZvbpocmV2aXNpb24AanB1YmxpY0tleXOBo2JpZABkZGF0YVghAvI7ZTHgRgHk7u1TwUTgaigu4FZAiosil5ynhcgRuZxZZHR5cGUA\"\n  ],\n  \"metadata\": {\n    \"height\": \"4087\",\n    \"coreChainLockedHeight\": 602550\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    },
    {
      "code": "// The storeTreeProof contains the requested data\n{\n  \"proof\": {\n    \"rootTreeProof\": \"v+99FytmaUPDP65HthQllBL1JDXt2Zu/kzFEQRw66rS90a8Geh7WyM0bzw8iZZtO9TOcTRdGxiKyNu1w1SZtmoRRIZem6YRYjSO6e78TdtgwuuxbtaQ9t7uZI8EZI3q6\",\n    \"storeTreeProofs\": {\n      \"identitiesProof\": \"Ab3+9XLi6seGGvOeC+YPa6820TTQJg+/VLMTI1l+Aa8uAkoa3iCpnFGnKNsuN3kmkoZziG5966ZPSns0QhkuLhcMEAHqjSiFz+M0EUzxVwMmeYDzBJtbkAf1Dt5ZV0wHrPSy/gJQNWR9nfgU8mqYVkQZvn+cgnUW+ZfuJItL0Vihkm7sbhABLEnf8Xpnpkn4RBFx+7Ftc9+kA+AeukotY1n+Cry0f/kDIClSj2o4+a0IeS6aAy2ZPHbRxWwEmnc8z74CzxZuD/atAH4BAAAApGJpZFggKVKPajj5rQh5LpoDLZk8dtHFbASadzzPvgLPFm4P9q1nYmFsYW5jZRm9umhyZXZpc2lvbgBqcHVibGljS2V5c4GjYmlkAGRkYXRhWCEC8jtlMeBGAeTu7VPBROBqKC7gVkCKiyKXnKeFyBG5nFlkdHlwZQAQAYr7Kwts+/MCvdAEAfe0qlM939luA/bmFk/NWn3j+4XMERERAk89CgifO1qwuFxa+KOoekT9JZIRnwerQ3sSHCSWvqliEAHjXdp0pKgcIEmQaWfWG970qaHE8iQWEjp0z9WXkJYAxBECJq4BeR3ucca+V5J1neVZaj8NTzLC7tTtPzMDJ1udK3sQAVSpKcwG23rP77Wl0GF9QCtgi7AMuIA4W6ZDlpJ5guKKEQ==\",\n      \"publicKeyHashesToIdentityIdsProof\": \"AVVHdharvPK/abcB6ecVTNNxm3n21DO7rsQ8CYCtIDXOAvvzK1glOGZsp5eAfNCH334Wyu/+v8QKlESPYuNw73u6EAHiHt1xB1elAoisxa5A7aamXJFlnDVp+fO+39Dww8/ANxE=\"\n    },\n    \"signatureLlmqHash\": \"AAAA1p9iTq8xLCvbRQKqUT0TgOksMi4pRUwPp3mPhDI=\",\n    \"signature\": \"lk8TENYBLqhmBaw/IzyR8IjCM7aBhMne/EMwocLs0oQ+JyOKrNTJ8giIgc1aXAKCFDdqgpOMZNPnZcdgcnqJVUpRqIAebTlVdvIZODYpG8LZTA+cQtOqTwVkH3A3c+tr\"\n  },\n  \"metadata\": {\n    \"height\": \"4087\",\n    \"coreChainLockedHeight\": 602550\n  }\n}",
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
  "type": "info",
  "body": "**Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-platform-proofs) for details on decoding the data."
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Note: the hash must be done using all fields of the identity public key object - e.g.\n```json\n{\n  id: 0,\n  type: 0,\n  data: 'A/Wmv+LhxAjSBE19d3OJCUawUiYORF5HDDetZbvTDgQQ'\n}\n```\nWhen using the js-dpp library, the hash can be accessed via the [IdentityPublicKey object's](https://github.com/dashevo/platform/blob/master/packages/js-dpp/lib/identity/IdentityPublicKey.js) `hash` method (e.g. `identity.getPublicKeyById(0).hash()`).",
  "title": "Public key hash"
}
[/block]
** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\n\nconst client = new DAPIClient();\n\n// Get identity from hex public key hash\nconst publicKeyHash = '1e5debba672f105e6f0635fa21a6491d459b8d83';\nconst publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\nclient.platform.getIdentityIdsByPublicKeyHashes(publicKeysBuffer)\n  .then((response) => {\n    for (const r of response) {\n      console.log(`Identity ID: ${Identifier.from(r).toString()}`);\n    }\n  });",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetIdentityIdsByPublicKeyHashesRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst publicKeyHash = '1e5debba672f105e6f0635fa21a6491d459b8d83';\nconst publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\nconst getIdentityIdsByPublicKeyHashesRequest = new GetIdentityIdsByPublicKeyHashesRequest();\ngetIdentityIdsByPublicKeyHashesRequest.setPublicKeyHashesList(publicKeysBuffer);\n\nplatformPromiseClient.getIdentityIdsByPublicKeyHashes(getIdentityIdsByPublicKeyHashesRequest)\n  .then((response) => {\n    for (const r of response.getIdentityIdsList()) {\n      const id = Buffer.from(r);\n      console.log(`Identity ID: ${Identifier.from(id).toString()}`);\n    }\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `public_key_hashes` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"public_key_hashes\":\"Hl3rumcvEF5vBjX6IaZJHUWbjYM=\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentityIdsByPublicKeyHashes",
      "language": "shell",
      "name": "gRPCurl"
    },
    {
      "code": "# `public_key_hashes` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"public_key_hashes\":\"Hl3rumcvEF5vBjX6IaZJHUWbjYM=\",\n    \"prove\": true\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentityIdsByPublicKeyHashes",
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
      "code": "Identity ID: 3nJjUPiVbwvKRpZ6pLUwVjcFoEJdeqPqakobctF3uECU",
      "language": "text",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"identityIds\": [\n    \"KVKPajj5rQh5LpoDLZk8dtHFbASadzzPvgLPFm4P9q0=\"\n  ],\n  \"metadata\": {\n    \"height\": \"4086\",\n    \"coreChainLockedHeight\": 602550\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    },
    {
      "code": "// The storeTreeProof contains the requested data\n{\n  \"proof\": {\n    \"rootTreeProof\": \"vdGvBnoe1sjNG88PImWbTvUznE0XRsYisjbtcNUmbZoi9eaFSC/P2CNfdJ0+uFFYwUMpSE/op2o70wTCcKKFboRRIZem6YRYjSO6e78TdtgwuuxbtaQ9t7uZI8EZI3q6\",\n    \"storeTreeProofs\": {\n      \"publicKeyHashesToIdentityIdsProof\": \"ASl+f/bOOvYpB1UPrhD+OWx78EIJRDRlDQ3t81EFtxDwAxQeXeu6Zy8QXm8GNfohpkkdRZuNgwAgKVKPajj5rQh5LpoDLZk8dtHFbASadzzPvgLPFm4P9q0QAQw3DUjb35EbQujSCMbYJoBVowJeJYuF/n52Fs7SZxW7EQL78ytYJThmbKeXgHzQh99+Fsrv/r/ECpREj2LjcO97uhAB4h7dcQdXpQKIrMWuQO2mplyRZZw1afnzvt/Q8MPPwDcR\"\n    },\n    \"signatureLlmqHash\": \"AAAA1p9iTq8xLCvbRQKqUT0TgOksMi4pRUwPp3mPhDI=\",\n    \"signature\": \"lPD7ukoFs+mbUonBdnfn95pqFK2J06DBtjtMZro+KNY7pVy4waMx057mjc8boZoOBjT9Fo0XXqGrE9JLY7cGEITd6k3KpS2kMP9/SSgnm7L/7S9LRqPnMlUF5j2Ihusl\"\n  },\n  \"metadata\": {\n    \"height\": \"4086\",\n    \"coreChainLockedHeight\": 602550\n  }\n}",
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
  "type": "info",
  "body": "**Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-platform-proofs) for details on decoding the data."
}
[/block]
** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient();\n\nconst contractId = Identifier.from('7sGiwoT8QaMAqEcuhB9UXLYq4oNG3r87up8fK2vvADXc');\nclient.platform.getDataContract(contractId).then((response) => {\n  const contract = cbor.decode(response.dataContract);\n  console.dir(contract, { depth: 10 });\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetDataContractRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst contractId = Identifier.from('7sGiwoT8QaMAqEcuhB9UXLYq4oNG3r87up8fK2vvADXc');\nconst contractIdBuffer = Buffer.from(contractId);\nconst getDataContractRequest = new GetDataContractRequest();\ngetDataContractRequest.setId(contractIdBuffer);\n\nplatformPromiseClient.getDataContract(getDataContractRequest)\n  .then((response) => {\n    const contractResponse = response.getDataContract();\n    const contractBuffer = Buffer.from(contractResponse);\n  \t// Strip off protocol version (leading 4 bytes) and decode\n  \tconsole.dir(cbor.decode(contractBuffer.slice(4, contractBuffer.length)), { depth: 5 });\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"Zgbwte3d6DhC3X1bM3BWgJPWFLAA/Cl3SL5o3lpL+bc=\"\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDataContract",
      "language": "shell",
      "name": "gRPCurl"
    },
    {
      "code": "# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"Zgbwte3d6DhC3X1bM3BWgJPWFLAA/Cl3SL5o3lpL+bc=\",\n    \"prove\":true\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDataContract",
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
      "code": "{\n  '$id': Buffer(32) [Uint8Array] [\n    102,   6, 240, 181, 237, 221, 232,  56,\n     66, 221, 125,  91,  51, 112,  86, 128,\n    147, 214,  20, 176,   0, 252,  41, 119,\n     72, 190, 104, 222,  90,  75, 249, 183\n  ],\n  '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',\n  ownerId: Buffer(32) [Uint8Array] [\n    145, 119, 123, 174, 134,  75, 184,  15,\n    197, 162, 149, 223,  43, 213, 181, 107,\n     93, 222, 219, 162, 204, 166, 175,  99,\n     46,  49, 177,  86,  41,  54,  19, 232\n  ],\n  documents: {\n    note: {\n      type: 'object',\n      properties: { message: { type: 'string' } },\n      additionalProperties: false\n    }\n  }\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"dataContract\": \"AQAAAKRjJGlkWCBmBvC17d3oOELdfVszcFaAk9YUsAD8KXdIvmjeWkv5t2ckc2NoZW1heDRodHRwczovL3NjaGVtYS5kYXNoLm9yZy9kcHAtMC00LTAvbWV0YS9kYXRhLWNvbnRyYWN0Z293bmVySWRYIJF3e66GS7gPxaKV3yvVtWtd3tuizKavYy4xsVYpNhPoaWRvY3VtZW50c6Fkbm90ZaNkdHlwZWZvYmplY3RqcHJvcGVydGllc6FnbWVzc2FnZaFkdHlwZWZzdHJpbmd0YWRkaXRpb25hbFByb3BlcnRpZXP0\",\n  \"metadata\": {\n    \"height\": \"4126\",\n    \"coreChainLockedHeight\": 602597\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    },
    {
      "code": "// The storeTreeProof contains the requested data\n{\n  \"proof\": {\n    \"rootTreeProof\": \"UoUWcVFiOzWnTwE0g+mU5HoRsBjGYxCBMTawVvEiihQi9eaFSC/P2CNfdJ0+uFFYwUMpSE/op2o70wTCcKKFboRRIZem6YRYjSO6e78TdtgwuuxbtaQ9t7uZI8EZI3q6\",\n    \"storeTreeProofs\": {\n      \"dataContractsProof\": \"AWTLA9zhqX/Iv1xWjFje9RO1gvSDeR2Nozr0jJ54zC2WAtmQfNDX/4w3ZxzF6l+WrJ6zCXaoSgj+usc/W8Sq1wAZEAFw8dhrMeRmw/D5phVlzDMTz3+GPl6bragUf98TjetMHAKAHGP8oviChW1cszz/Z3X6JNqLu0cZvbKjVqg4XTOQ0BADIGYG8LXt3eg4Qt19WzNwVoCT1hSwAPwpd0i+aN5aS/m3AOcBAAAApGMkaWRYIGYG8LXt3eg4Qt19WzNwVoCT1hSwAPwpd0i+aN5aS/m3ZyRzY2hlbWF4NGh0dHBzOi8vc2NoZW1hLmRhc2gub3JnL2RwcC0wLTQtMC9tZXRhL2RhdGEtY29udHJhY3Rnb3duZXJJZFggkXd7roZLuA/FopXfK9W1a13e26LMpq9jLjGxVik2E+hpZG9jdW1lbnRzoWRub3Rlo2R0eXBlZm9iamVjdGpwcm9wZXJ0aWVzoWdtZXNzYWdloWR0eXBlZnN0cmluZ3RhZGRpdGlvbmFsUHJvcGVydGllc/QCajFnEzRfWdMz9eb04hV6NkJ7/WnRLZapbVLvbDjP+LAQAYChpQAdFW0BPcHA4K544kYqtHtZpMvE7zyN13z6i0DEEREC1f4FCxWur82ee+34+KBPfEyNPiVO5N6/Ri9fg0zTss4QATcPUokVmwwBNhZJYxh9hWOBbVkeUYlJnMoWMHNlOE0gEQL5aC0sNYefYm/7IxrFWDfk5UOpT1Xev55SxLVAcigOCRABwxylhTyqpev1DMfV8Z/QWCjr3bZWmWqafhooeOrG75ARAkLyTiNKYJZUXcOGs3Ba+UOX2LqBnK2Mh5ZKaDl1omAbEAHpUHg/nWE0DXbMghdB9OX5WB9sKyqQciNAlX3PrpDAqhECGn4Eb3qGrD88QyS5a6hjzlfCgURpHHU3bSmITL5IR+gQAbBk1WCyJTCDPtv+5KEJRdI9YliOesShZrZIJwcZrJpEERE=\"\n    },\n    \"signatureLlmqHash\": \"AAAA8+9g8wrkwRLpHCZTI3kZr3wCIEgIAyPgwXDe/JU=\",\n    \"signature\": \"E5kGj0PbY4aHm3+3Ps8s5oM5XkIc8gpt7zgjDvyjVXqlmFD1fMgISjKtownBrDCIEXr6TjoMfpPwyG98IXe5xfeiYoUH4ZF7qElh9xt0nd+OXcmu/wznf7TTwt0+FvD/\"\n  },\n  \"metadata\": {\n    \"height\": \"4158\",\n    \"coreChainLockedHeight\": 602638\n  }\n}",
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
  "type": "info",
  "body": "**Note**: When requesting proofs, the data requested will be encoded as part of the proof in the response. See the [Platform Proofs page](reference-platform-proofs) for details on decoding the data."
}
[/block]

** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient();\n\nconst contractId = Identifier.from('7sGiwoT8QaMAqEcuhB9UXLYq4oNG3r87up8fK2vvADXc');\nclient.platform.getDocuments(contractId, 'note', { limit: 10 }).then((response) => {\n  for (const rawData of response.documents) {\n    console.log(cbor.decode(rawData.slice(4, rawData.length)));\n  }\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetDocumentsRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst cbor = require('cbor');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.testnet.networks.dash.org:3010',\n);\n\nconst contractId = Identifier.from('7sGiwoT8QaMAqEcuhB9UXLYq4oNG3r87up8fK2vvADXc');\nconst contractIdBuffer = Buffer.from(contractId);\nconst getDocumentsRequest = new GetDocumentsRequest();\nconst type = 'note';\nconst limit = 10;\n\ngetDocumentsRequest.setDataContractId(contractIdBuffer);\ngetDocumentsRequest.setDocumentType(type);\n// getDocumentsRequest.setWhere(whereSerialized);\n// getDocumentsRequest.setOrderBy(orderBySerialized);\ngetDocumentsRequest.setLimit(limit);\n// getDocumentsRequest.setStartAfter(startAfter);\n// getDocumentsRequest.setStartAt(startAt);\n\nplatformPromiseClient.getDocuments(getDocumentsRequest)\n  .then((response) => {\n    for (const document of response.getDocumentsList()) {\n      const documentBuffer = Buffer.from(document);\n      // Strip off protocol version (leading 4 bytes) and decode\n      console.log(cbor.decode(documentBuffer.slice(4, documentBuffer.length)));\n    }\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# Request documents\n# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"data_contract_id\":\"Zgbwte3d6DhC3X1bM3BWgJPWFLAA/Cl3SL5o3lpL+bc=\",\n    \"document_type\":\"note\",\n    \"limit\":10\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDocuments",
      "language": "shell",
      "name": "Request (gRPCurl)"
    },
    {
      "code": "# Request documents\n# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"data_contract_id\":\"Zgbwte3d6DhC3X1bM3BWgJPWFLAA/Cl3SL5o3lpL+bc=\",\n    \"document_type\":\"note\",\n    \"limit\":10,\n    \"prove\":true\n    }' \\\n  seed-1.testnet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDocuments",
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
      "code": "{\n  '$id': <Buffer f6 28 4c ec 6a 42 e8 49 b5 97 91 80 dc 4e 6d c3 61 eb 16 f0 f1 17 06 5e 6b fc a8 2f 13 1f 47 ab>,\n  '$type': 'note',\n  message: 'Tutorial CI Test @ Wed, 27 Oct 2021 18:07:32 GMT',\n  '$ownerId': <Buffer 91 77 7b ae 86 4b b8 0f c5 a2 95 df 2b d5 b5 6b 5d de db a2 cc a6 af 63 2e 31 b1 56 29 36 13 e8>,\n  '$revision': 1,\n  '$dataContractId': <Buffer 66 06 f0 b5 ed dd e8 38 42 dd 7d 5b 33 70 56 80 93 d6 14 b0 00 fc 29 77 48 be 68 de 5a 4b f9 b7>\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"documents\": [\n    \"AQAAAKZjJGlkWCD2KEzsakLoSbWXkYDcTm3DYesW8PEXBl5r/KgvEx9Hq2UkdHlwZWRub3RlZ21lc3NhZ2V4MFR1dG9yaWFsIENJIFRlc3QgQCBXZWQsIDI3IE9jdCAyMDIxIDE4OjA3OjMyIEdNVGgkb3duZXJJZFggkXd7roZLuA/FopXfK9W1a13e26LMpq9jLjGxVik2E+hpJHJldmlzaW9uAW8kZGF0YUNvbnRyYWN0SWRYIGYG8LXt3eg4Qt19WzNwVoCT1hSwAPwpd0i+aN5aS/m3\"\n  ],\n  \"metadata\": {\n    \"height\": \"4181\",\n    \"coreChainLockedHeight\": 602674\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    },
    {
      "code": "// The storeTreeProof contains the requested data\n{\n  \"proof\": {\n    \"rootTreeProof\": \"OROtAnUrAkPl0DYUzlsMB9LhJZ06ru05bOPqkHVYo+31K6UWyw8x5nPaeBOdpn9MFo4KoyAeMuO6RJhg6uhR6g==\",\n    \"storeTreeProofs\": {\n      \"documentsProof\": \"Af3qHY9UTnuT73Ae0Z/XjDkekjlH1FfZguvcpoReGdogAjr5vfOseSD5G5S0Dkw1g34k0sczimEtHbGtYcyZ7icBEAGGyGfN3RmR58TRbLWh5UAS/+BLFIFYlQ2PJQ4h6NkWxgIPfn+kvIrzjO5P4NYObIRV0+xxCl0XKmHjzWHA3rXBqRABgJ8gC7AGyacDWJHJQKx+kdMeqmOYWyO99CBLFrJZnDgCHw+XhxBZjK7OJKbo3FXfiNzBN7YvAz3gxTqaIiWknNoQAVhIL5S6zg6v57CLmcP+ve+VDXdVexvBLGfImbrJ+Z1/AqLtr92YXdne0HIJNybRNY9W6dq1zPX8IpmLihMVt+4/EAE2BzKOaX9r6X+0zBVEAXuRHEI7jhpjmPhljWlAINlI/gIRmjuSsyX7cN/ADIM0LpiCR6xmd0h/x6y+DCo/8j8KqBABHrWz3C6kWbMLNPxATaDV6CiSti/3HFNa4fkCFzBs2F8DIPYoTOxqQuhJtZeRgNxObcNh6xbw8RcGXmv8qC8TH0erANgBAAAApmMkaWRYIPYoTOxqQuhJtZeRgNxObcNh6xbw8RcGXmv8qC8TH0erZSR0eXBlZG5vdGVnbWVzc2FnZXgwVHV0b3JpYWwgQ0kgVGVzdCBAIFdlZCwgMjcgT2N0IDIwMjEgMTg6MDc6MzIgR01UaCRvd25lcklkWCCRd3uuhku4D8Wild8r1bVrXd7bosymr2MuMbFWKTYT6GkkcmV2aXNpb24BbyRkYXRhQ29udHJhY3RJZFggZgbwte3d6DhC3X1bM3BWgJPWFLAA/Cl3SL5o3lpL+bcQARO6Fxi1e5WmcVHGtcGZVW//UkDG3e5s51hmYBmrw4CxEQJKYPVW7qYg9EU9ycHP1kpqW8gIlajI7snfor/ZTlcbWxAB/YH69QkKkpIg6ZV3EoEsr4CpY6Yg/LziAypgUbGeEwIRAhI5xb5+Cfsd2m8dJ0CXWWVIPPMNFzzip3H0rJvP9kQzEAGNmmazyIMJvTKmXDKxCAol5Cbgpp/3SBf6L4tyq5gZxhEREREREQ==\"\n    },\n    \"signatureLlmqHash\": \"AAAA1p9iTq8xLCvbRQKqUT0TgOksMi4pRUwPp3mPhDI=\",\n    \"signature\": \"k6o6k3BHGFcKU5fbdIH6wCu1dA+5WwgYlwMF9u1yvUeUn+KqzIZ+ijwlaQp5kSc+CVdFgMyVH4E5uysp+6fHSp8l1B47EdcHwe9CahdnlB4pGDA6RtCxCHLa5rrv6/eK\"\n  },\n  \"metadata\": {\n    \"height\": \"4181\",\n    \"coreChainLockedHeight\": 602674\n  }\n}",
      "language": "json",
      "name": "Response (gRPCurl with Proof)"
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
| `prove` | Boolean | Yes | Set to `true` to request a proof |

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

There are no recently deprecated endpoint, but the previous version of documentation can be [viewed here](https://dashplatform.readme.io/v0.20.0/docs/reference-dapi-endpoints-platform-endpoints).

# Code Reference
Implementation details related to the information on this page can be found in:
- The [Platform repository](https://github.com/dashevo/platform/tree/master/packages/dapi) `packages/dapi/lib/grpcServer/handlers/core` folder
- The [Platform repository](https://github.com/dashevo/platform/tree/master/packages/dapi-grpc) `packages/dapi-grpc/protos` folder