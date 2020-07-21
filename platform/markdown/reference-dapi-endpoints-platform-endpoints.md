[block:callout]
{
  "type": "info",
  "title": "gRPC Overview",
  "body": "Please refer to the [gRPC Overview](reference-dapi-endpoints-grpc-overview) for details regarding running the examples shown below, encoding/decoding the request/response data, and clients available for several languages."
}
[/block]
# Endpoint Details
[block:callout]
{
  "type": "danger",
  "title": "Work in Progress",
  "body": "This document is an work in progress. Details may be missing or incomplete and are subject to change."
}
[/block]
## applyStateTransition
Submits a [state transition](explanation-platform-protocol-state-transition) to the platform via DAPI to make a change to layer 2 data.

**Returns**: Nothing or error
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `state_transition` | Bytes (Base-64) | Yes | A [state transition](explanation-platform-protocol-state-transition) |

** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "# Submit an identity create State Transition\ngrpcurl -proto protos/platform.proto -plaintext \\\n  -d '{\n    \"state_transition\":\"pWR0eXBlAmlzaWduYXR1cmV4WEg3TWhFWDQ0Z3JzMVIwTE9XTU5IZjAxWFNpYVFQcUlVZ1JLRXQyMkxHVERsUlUrZ1BwQUlUZk5JUmhXd3IvYTVHd0lzWm1idGdYVVFxcVhjbW9lQWtUOD1qcHVibGljS2V5c4GkYmlkAGRkYXRheCxBdzh2UmYxeFFCTlVLbzNiY2llaHlaR2NhM0hBSThkY0ZvVWJTK3hLb0lITmR0eXBlAGlpc0VuYWJsZWT1bmxvY2tlZE91dFBvaW50eDBLT1VUSHB5YnFPek9DNnhEVUhFWm9uc1lNSVpqcGppTHFZNnkxYmlWNWxRQUFBQUFvcHJvdG9jb2xWZXJzaW9uAA==\"\n\n    }' \\\n  seed-1.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/applyStateTransition",
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
| `id` | String | Yes | An identity `id` |

** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient({\n  seeds: [{\n    service: 'seed-1.evonet.networks.dash.org',\n    port: 3000\n  }],\n});\n\nconst identityId = 'C7id2mah2RkiroiTy6h134hLgS6A47jhh5x91tvw16bz';\nclient.getIdentity(identityId).then((response) => {\n  const identity = cbor.decode(response);\n  console.log(identity);\n}",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "grpcurl -proto protos/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"C7id2mah2RkiroiTy6h134hLgS6A47jhh5x91tvw16bz\"\n    }' \\\n  seed-1.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentity",
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
      "code": "{\n  id: 'C7id2mah2RkiroiTy6h134hLgS6A47jhh5x91tvw16bz',\n  balance: 10026096,\n  publicKeys: [\n    {\n      id: 0,\n      data: 'A9/uZEcu2EtDODih7AYcwgfBViKE/Nkeo/x7UVa2RbUk',\n      type: 0,\n      isEnabled: true\n    }\n  ]\n\n}",
      "language": "json",
      "name": "Response (dapi-client)"
    },
    {
      "code": "{\n  \"identity\": \"o2JpZHgsQzdpZDJtYWgyUmtpcm9pVHk2aDEzNGhMZ1M2QTQ3amhoNXg5MXR2dzE2YnpnYmFsYW5jZRoAmPxwanB1YmxpY0tleXOBpGJpZABkZGF0YXgsQTkvdVpFY3UyRXRET0RpaDdBWWN3Z2ZCVmlLRS9Oa2VvL3g3VVZhMlJiVWtkdHlwZQBpaXNFbmFibGVk9Q==\"\n}",
      "language": "json",
      "name": "Response (gRPCurl)"
    }
  ]
}
[/block]
## getIdentityByFirstPublicKey
[block:callout]
{
  "type": "success",
  "title": "New in Dash Platform v0.13"
}
[/block]
**Returns**: [Identity](explanation-identity) information associated with the provided public key hash
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `public_key_hash` | Bytes | Yes | Public key hash (double-sha256) of an identity's first public key |


[block:callout]
{
  "type": "info",
  "body": "Note: the hash must be done using all fields of the identity public key object - e.g.\n```json\n{\n  id: 0,\n  data: 'A9/uZEcu2EtDODih7AYcwgfBViKE/Nkeo/x7UVa2RbUk',\n  type: 0,\n  isEnabled: true\n}\n```",
  "title": "Public key hash"
}
[/block]
** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient({\n  seeds: [{\n    service: 'seed-1.evonet.networks.dash.org',\n    port: 3000\n  }],\n});\n\nconst hash = '4e2736d0eecca645821089eb4b2422544e045655';\nclient.getIdentityByFirstPublicKey(hash).then((response) => {\n  const identity = cbor.decode(response);\n  console.log(identity);\n});\n",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    },
    {
      "code": "grpcurl -proto protos/platform.proto -plaintext \\\n  -d '{\n    \"public_key_hash\":\"4e2736d0eecca645821089eb4b2422544e045655\"\n    }' \\\n  seed-1.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentityByFirstPublicKey",
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
      "code": "{\n  id: 'C7id2mah2RkiroiTy6h134hLgS6A47jhh5x91tvw16bz',\n  balance: 10026096,\n  publicKeys: [\n    {\n\n      id: 0,\n      data: 'A9/uZEcu2EtDODih7AYcwgfBViKE/Nkeo/x7UVa2RbUk',\n      type: 0,\n      isEnabled: true\n    }\n  ]\n}",
      "language": "json",
      "name": "Response (dapi-client)"
    },
    {
      "code": "",
      "language": "text",
      "name": "Response (gRPCurl)"
    }
  ]
}
[/block]
## getIdentityIdByFirstPublicKey
[block:callout]
{
  "type": "success",
  "title": "New in Dash Platform v0.13"
}
[/block]
**Returns**: [Identity](explanation-identity) ID associated with the provided public key hash
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `public_key_hash` | Bytes | Yes | Public key hash (double-sha256) of an identity's first public key |

[block:callout]
{
  "type": "info",
  "body": "Note: the hash must be done using all fields of the identity public key object - e.g.\n```json\n{\n  id: 0,\n  data: 'A9/uZEcu2EtDODih7AYcwgfBViKE/Nkeo/x7UVa2RbUk',\n  type: 0,\n  isEnabled: true\n}\n```",
  "title": "Public key hash"
}
[/block]

** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "grpcurl -proto protos/platform.proto -plaintext \\\n  -d '{\n    \"public_key_hash\":\"4e2736d0eecca645821089eb4b2422544e045655\"\n    }' \\\n  seed-1.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentityIdByFirstPublicKey",
      "language": "shell",
      "name": "Request (gRPCurl)"
    },
    {
      "code": "const DAPIClient = require('@dashevo/dapi-client');\nconst cbor = require('cbor');\n\nconst client = new DAPIClient({\n  seeds: [{\n    service: 'seed-1.evonet.networks.dash.org',\n    port: 3000\n  }],\n});\n\nconst hash = '4e2736d0eecca645821089eb4b2422544e045655';\nclient.getIdentityIdByFirstPublicKey(hash).then((response) => {\n  console.log(response);\n});\n",
      "language": "javascript",
      "name": "JavaScript (dapi-client)"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "",
      "language": "text",
      "name": "Response (gRPCurl)"
    },
    {
      "code": "C7id2mah2RkiroiTy6h134hLgS6A47jhh5x91tvw16bz",
      "language": "text",
      "name": "Response (dapi-client)"
    }
  ]
}
[/block]
## getDataContract

**Returns**: [Data Contract](explanation-platform-protocol-data-contract) information for the requested data contract
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `id` | String | Yes | A data contract `id` |

** Example Request and Response **
[block:code]
{
  "codes": [
    {
      "code": "grpcurl -proto protos/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"ARQGUnPH3YMK8FZuqwUjnTWEF6Zu4Cf3sT6e1Ruu1RXk\"\n    }' \\\n  seed-1.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDataContract",
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
      "code": "{\n  \"dataContract\": \"pGMkaWR4LEFSUUdVblBIM1lNSzhGWnVxd1VqblRXRUY2WnU0Q2Yzc1Q2ZTFSdXUxUlhrZyRzY2hlbWF4NGh0dHBzOi8vc2NoZW1hLmRhc2gub3JnL2RwcC0wLTQtMC9tZXRhL2RhdGEtY29udHJhY3Rnb3duZXJJZHgsR2I5WUVITVl4Y01pU25CaHRCUVFtdHRtQW9xTjFhc2k2SEtReFJSN1lkWEFpZG9jdW1lbnRzoWRub3Rlompwcm9wZXJ0aWVzoWdtZXNzYWdloWR0eXBlZnN0cmluZ3RhZGRpdGlvbmFsUHJvcGVydGllc/Q=\"\n}\n",
      "language": "json",
      "name": "Response"
    }
  ]
}
[/block]

## getDocuments

**Returns**: [Document](explanation-platform-protocol-document) information for the requested document(s)
**Parameters**:

| Name | Type | Required | Description |
| - | - | - | - |
| `data_contract_id` | String | Yes | A data contract `id` |
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
      "code": "# Request one DPNS domain document\ngrpcurl -proto protos/platform.proto -plaintext \\\n  -d '{\n    \"data_contract_id\":\"ARQGUnPH3YMK8FZuqwUjnTWEF6Zu4Cf3sT6e1Ruu1RXk\",\n    \"document_type\":\"note\",\n    \"limit\":1\n    }' \\\n  seed-1.evonet.networks.dash.org:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDocuments",
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
      "code": "{\n  \"documents\": [\n    \"pmMkaWR4LEhUYkNWZlRoU2gySmZiU1RCQmVjZlVpbWlONVp3VEU5bVZpb2pYQVYyZ1QxZSR0eXBlZG5vdGVnbWVzc2FnZXgtVHV0b3JpYWwgVGVzdCBAIE1vbiwgMTUgSnVuIDIwMjAgMjA6MDM6MDIgR01UaCRvd25lcklkeCxHYjlZRUhNWXhjTWlTbkJodEJRUW10dG1Bb3FOMWFzaTZIS1F4UlI3WWRYQWkkcmV2aXNpb24BbyRkYXRhQ29udHJhY3RJZHgsQVJRR1VuUEgzWU1LOEZadXF3VWpuVFdFRjZadTRDZjNzVDZlMVJ1dTFSWGs=\"\n  ]\n}\n",
      "language": "json",
      "name": "Response"
    }
  ]
}
[/block]
# Code Reference
Implementation details related to the information on this page can be found in:
- The [DAPI repository](https://github.com/dashevo/dapi) `lib/grpcServer/handlers/platform` folder
- The [dapi-grpc repository](https://github.com/dashevo/dapi-grpc/) `protos` folder