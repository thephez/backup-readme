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
  "title": "New in Dash Platform v0.15.0",
  "body": "Previously named `applyStateTransition`"
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
      "code": "# Submit an identity create State Transition\n# `state_transition` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"state_transition\":\"pWR0eXBlAmlzaWduYXR1cmV4WEg3TWhFWDQ0Z3JzMVIwTE9XTU5IZjAxWFNpYVFQcUlVZ1JLRXQyMkxHVERsUlUrZ1BwQUlUZk5JUmhXd3IvYTVHd0lzWm1idGdYVVFxcVhjbW9lQWtUOD1qcHVibGljS2V5c4GkYmlkAGRkYXRheCxBdzh2UmYxeFFCTlVLbzNiY2llaHlaR2NhM0hBSThkY0ZvVWJTK3hLb0lITmR0eXBlAGlpc0VuYWJsZWT1bmxvY2tlZE91dFBvaW50eDBLT1VUSHB5YnFPek9DNnhEVUhFWm9uc1lNSVpqcGppTHFZNnkxYmlWNWxRQUFBQUFvcHJvdG9jb2xWZXJzaW9uAA==\"\n\n    }' \\\n  seed-1.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/broadcastStateTransition",
      "language": "shell",
      "name": "Request (gRPCurl)"
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
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n  },\n} = require('@dashevo/dapi-grpc');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed.evonet.networks.dash.org:3010',\n);\n\nconst id = Identifier.from('BhC9M3fQHyUCyuxH4WHdhn1VGgJ4JTLmer8qmTTHkYTe');\nconst idBuffer = Buffer.from(id);\nconst getIdentityRequest = new GetIdentityRequest();\ngetIdentityRequest.setId(idBuffer);\n\nplatformPromiseClient.getIdentity(getIdentityRequest)\n  .then((response) => {\n    const identityResponse = response.getIdentity();\n    const identityBuffer = Buffer.from(identityResponse);\n    console.log(cbor.decode(identityBuffer));\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"nuCzumg/s5LC1SplgbztDxV9AfZBr1wNklM4zK8xZ90=\"\n    }' \\\n  seed-1.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentity",
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
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst DashPlatformProtocol = require('@dashevo/dpp');\n\nconst client = new DAPIClient();\nconst dpp = new DashPlatformProtocol();\n\nconst publicKeyHash = '2251b999499564dba481e36e0bfeeacfe5aa582e';\nconst publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\nclient.platform.getIdentitiesByPublicKeyHashes(publicKeysBuffer)\n  .then((response) => {\n    const retrievedIdentity = dpp.identity.createFromBuffer(response[0]);\n    console.log(retrievedIdentity.toJSON());\n  });",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetIdentitiesByPublicKeyHashesRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst cbor = require('cbor');\nconst DashPlatformProtocol = require('@dashevo/dpp');\n\nconst dpp = new DashPlatformProtocol();\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed.evonet.networks.dash.org:3010',\n);\n\nconst publicKeyHash = '2251b999499564dba481e36e0bfeeacfe5aa582e';\nconst publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\nconst getIdentitiesByPublicKeyHashesRequest = new GetIdentitiesByPublicKeyHashesRequest();\ngetIdentitiesByPublicKeyHashesRequest.setPublicKeyHashesList(publicKeysBuffer);\n\nplatformPromiseClient.getIdentitiesByPublicKeyHashes(getIdentitiesByPublicKeyHashesRequest)\n  .then((response) => {\n    const identitiesResponse = response.getIdentitiesList();\n    const identities = identitiesResponse\n      .map((identity) => (identity.length > 0 ? cbor.decode(Buffer.from(identity)) : null));\n    console.log(dpp.identity.createFromObject(identities[0]).toJSON());\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `public_key_hashes` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"public_key_hashes\":\"IlG5mUmVZNukgeNuC/7qz+WqWC4=\"\n    }' \\\n  seed-1.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentitiesByPublicKeyHashes",
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
      "code": "{\n  protocolVersion: 0,\n  id: 'BhC9M3fQHyUCyuxH4WHdhn1VGgJ4JTLmer8qmTTHkYTe',\n  publicKeys: [\n    {\n      id: 0,\n      type: 0,\n      data: 'A/Wmv+LhxAjSBE19d3OJCUawUiYORF5HDDetZbvTDgQQ'\n    }\n  ],\n  balance: 10991631,\n  revision: 0\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"identities\": [\n    \"pWJpZFggnuCzumg/s5LC1SplgbztDxV9AfZBr1wNklM4zK8xZ91nYmFsYW5jZRoAp7gPaHJldmlzaW9uAGpwdWJsaWNLZXlzgaNiaWQAZGRhdGFYIQP1pr/i4cQI0gRNfXdziQlGsFImDkReRww3rWW70w4EEGR0eXBlAG9wcm90b2NvbFZlcnNpb24A\"\n  ]\n}",
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
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\n\nconst client = new DAPIClient();\n\n// Get identity from hex public key hash\nconst publicKeyHash = '2251b999499564dba481e36e0bfeeacfe5aa582e';\nconst publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\nclient.platform.getIdentityIdsByPublicKeyHashes(publicKeysBuffer)\n  .then((response) => {\n    for (const r of response) {\n      console.log(`Identity ID: ${Identifier.from(r).toString()}`);\n    }\n  });",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetIdentityIdsByPublicKeyHashesRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed.evonet.networks.dash.org:3010',\n);\n\nconst publicKeyHash = '2251b999499564dba481e36e0bfeeacfe5aa582e';\nconst publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];\n\nconst getIdentityIdsByPublicKeyHashesRequest = new GetIdentityIdsByPublicKeyHashesRequest();\ngetIdentityIdsByPublicKeyHashesRequest.setPublicKeyHashesList(publicKeysBuffer);\n\nplatformPromiseClient.getIdentityIdsByPublicKeyHashes(getIdentityIdsByPublicKeyHashesRequest)\n  .then((response) => {\n    for (const r of response.getIdentityIdsList()) {\n      const id = Buffer.from(r);\n      console.log(`Identity ID: ${Identifier.from(id).toString()}`);\n    }\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `public_key_hashes` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"public_key_hashes\":\"IlG5mUmVZNukgeNuC/7qz+WqWC4=\"\n    }' \\\n  seed-1.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentityIdsByPublicKeyHashes",
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
      "code": "Identity ID: BhC9M3fQHyUCyuxH4WHdhn1VGgJ4JTLmer8qmTTHkYTe",
      "language": "text",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"identityIds\": [\n    \"nuCzumg/s5LC1SplgbztDxV9AfZBr1wNklM4zK8xZ90=\"\n  ]\n}",
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
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient();\n\nconst contractId = Identifier.from('Q894cs83D8REQNo7mAetj1wPJK2W3svrwqaN61aP25W');\nclient.platform.getDataContract(contractId).then((response) => {\n  const contract = cbor.decode(response);\n  console.dir(contract, {depth:10});\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetDataContractRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed.evonet.networks.dash.org:3010',\n);\n\nconst contractId = Identifier.from('Q894cs83D8REQNo7mAetj1wPJK2W3svrwqaN61aP25W');\nconst contractIdBuffer = Buffer.from(contractId);\nconst getDataContractRequest = new GetDataContractRequest();\ngetDataContractRequest.setId(contractIdBuffer);\n\nplatformPromiseClient.getDataContract(getDataContractRequest)\n  .then((response) => {\n    const contractResponse = response.getDataContract();\n    const contractBuffer = Buffer.from(contractResponse);\n    console.dir(cbor.decode(contractBuffer), { depth: 5 });\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"BexuQvZ7zeNK51eNqJDUMOCGHzPC+wfHrnqjqsg/CWk=\"\n    }' \\\n  seed-1.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDataContract",
      "language": "shell"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  '$id': Buffer(32) [Uint8Array] [\n      5, 236, 110,  66, 246, 123, 205, 227,\n     74, 231,  87, 141, 168, 144, 212,  48,\n    224, 134,  31,  51, 194, 251,   7, 199,\n    174, 122, 163, 170, 200,  63,   9, 105\n  ],\n  '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',\n  ownerId: Buffer(32) [Uint8Array] [\n    158, 224, 179, 186, 104,  63, 179, 146,\n    194, 213,  42, 101, 129, 188, 237,  15,\n     21, 125,   1, 246,  65, 175,  92,  13,\n    146,  83,  56, 204, 175,  49, 103, 221\n  ],\n  documents: {\n    note: {\n      properties: { message: { type: 'string' } },\n      additionalProperties: false\n    }\n  },\n  protocolVersion: 0\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"dataContract\": \"pWMkaWRYIAXsbkL2e83jSudXjaiQ1DDghh8zwvsHx656o6rIPwlpZyRzY2hlbWF4NGh0dHBzOi8vc2NoZW1hLmRhc2gub3JnL2RwcC0wLTQtMC9tZXRhL2RhdGEtY29udHJhY3Rnb3duZXJJZFggnuCzumg/s5LC1SplgbztDxV9AfZBr1wNklM4zK8xZ91pZG9jdW1lbnRzoWRub3Rlompwcm9wZXJ0aWVzoWdtZXNzYWdloWR0eXBlZnN0cmluZ3RhZGRpdGlvbmFsUHJvcGVydGllc/RvcHJvdG9jb2xWZXJzaW9uAA==\"\n}\n",
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
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient();\n\nconst contractId = Identifier.from('Q894cs83D8REQNo7mAetj1wPJK2W3svrwqaN61aP25W');\nclient.platform.getDocuments(contractId, 'note', { limit: 1 }).then((response) => {\n  for (const rawData of response) {\n    console.log(cbor.decode(rawData));\n  }\n});",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "const {\n  v0: {\n    PlatformPromiseClient,\n    GetDocumentsRequest,\n  },\n} = require('@dashevo/dapi-grpc');\nconst cbor = require('cbor');\nconst Identifier = require('@dashevo/dpp/lib/Identifier');\n\nconst platformPromiseClient = new PlatformPromiseClient(\n  'http://seed-1.evonet.networks.dash.org:3010',\n);\n\nconst contractId = Identifier.from('Q894cs83D8REQNo7mAetj1wPJK2W3svrwqaN61aP25W');\nconst contractIdBuffer = Buffer.from(contractId);\nconst getDocumentsRequest = new GetDocumentsRequest();\nconst type = 'note';\nconst limit = 1;\n\ngetDocumentsRequest.setDataContractId(contractIdBuffer);\ngetDocumentsRequest.setDocumentType(type);\n// getDocumentsRequest.setWhere(whereSerialized);\n// getDocumentsRequest.setOrderBy(orderBySerialized);\ngetDocumentsRequest.setLimit(limit);\n// getDocumentsRequest.setStartAfter(startAfter);\n// getDocumentsRequest.setStartAt(startAt);\n\nplatformPromiseClient.getDocuments(getDocumentsRequest)\n  .then((response) => {\n    for (const document of response.getDocumentsList()) {\n      console.log(cbor.decode(Buffer.from(document)));\n    }\n  })\n  .catch((e) => console.error(e));",
      "language": "javascript",
      "name": "JavaScript (dapi-grpc)"
    },
    {
      "code": "# Request one DPNS document\n# `id` must be represented in base64\ngrpcurl -proto protos/platform/v0/platform.proto -plaintext \\\n  -d '{\n    \"data_contract_id\":\"BexuQvZ7zeNK51eNqJDUMOCGHzPC+wfHrnqjqsg/CWk=\",\n    \"document_type\":\"note\",\n    \"limit\":1\n    }' \\\n  seed-1.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDocuments",
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
      "code": "{\n  '$id': <Buffer 31 39 11 9a bc 87 b9 d7 a8 f0 76 d0 bb 85 9f 0b 7c cc 99 ed 21 65 1c 4c 79 14 9d c1 35 07 39 d6>,\n  '$type': 'note',\n  message: 'Tutorial Test @ Thu, 29 Oct 2020 20:39:40 GMT',\n  '$ownerId': <Buffer 9e e0 b3 ba 68 3f b3 92 c2 d5 2a 65 81 bc ed 0f 15 7d 01 f6 41 af 5c 0d 92 53 38 cc af 31 67 dd>,\n  '$revision': 1,\n  '$dataContractId': <Buffer 05 ec 6e 42 f6 7b cd e3 4a e7 57 8d a8 90 d4 30 e0 86 1f 33 c2 fb 07 c7 ae 7a a3 aa c8 3f 09 69>,\n  '$protocolVersion': 0\n}",
      "language": "json",
      "name": "Response (JavaScript)"
    },
    {
      "code": "{\n  \"documents\": [\n    \"p2MkaWRYIDE5EZq8h7nXqPB20LuFnwt8zJntIWUcTHkUncE1BznWZSR0eXBlZG5vdGVnbWVzc2FnZXgtVHV0b3JpYWwgVGVzdCBAIFRodSwgMjkgT2N0IDIwMjAgMjA6Mzk6NDAgR01UaCRvd25lcklkWCCe4LO6aD+zksLVKmWBvO0PFX0B9kGvXA2SUzjMrzFn3WkkcmV2aXNpb24BbyRkYXRhQ29udHJhY3RJZFggBexuQvZ7zeNK51eNqJDUMOCGHzPC+wfHrnqjqsg/CWlwJHByb3RvY29sVmVyc2lvbgA=\"\n  ]\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    }
  ]
}
[/block]

# Deprecated Endpoints

## applyStateTransition
[block:callout]
{
  "type": "danger",
  "title": "Deprecated RPC",
  "body": "Renamed to [`broadcastStateTransition`](#broadcaststatetransition) in Dash Platform v0.15.0"
}
[/block]

## getIdentityByFirstPublicKey
[block:callout]
{
  "type": "danger",
  "title": "Removed in Dash Platform v0.16",
  "body": "Similar functionality available with [getIdentitiesByPublicKeyHashes](#getidentitiesbypublickeyhashes)"
}
[/block]
## getIdentityIdByFirstPublicKey
[block:callout]
{
  "type": "danger",
  "title": "Removed in Dash Platform v0.16",
  "body": "Similar functionality available with [getIdentityIdsByPublicKeyHashes](#getidentityidsbypublickeyhashes)"
}
[/block]
# Code Reference
Implementation details related to the information on this page can be found in:
- The [DAPI repository](https://github.com/dashevo/dapi) `lib/grpcServer/handlers/platform` folder
- The [dapi-grpc repository](https://github.com/dashevo/dapi-grpc/) `protos` folder