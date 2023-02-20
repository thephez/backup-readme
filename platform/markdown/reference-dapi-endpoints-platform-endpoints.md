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

```javascript JavaScript (dapi-client)
const DAPIClient = require('@dashevo/dapi-client');
const DashPlatformProtocol = require('@dashevo/dpp');

const client = new DAPIClient();
const dpp = new DashPlatformProtocol();

// Data Contract Create State Transition (JSON)
// Replace with your own state transition object before running
const stateTransitionObject = {
  protocolVersion: 0,
  type: 0,
  signature: 'HxAipUsLWQBE++C1suSRNQiQh91rI1LZbblvQhk2erUaIvRneAagxGYYsXXYNvEeO+lBzlF1a9KHGGTHgnO/8Ts=',
  signaturePublicKeyId: 0,
  dataContract: {
    protocolVersion: 0,
    '$id': 'CMc7RghKkHeHtFdwfSX5Hzy7CUdpCEJnwsbfHdsbmJ32',
    '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',
    ownerId: '8Z3ps3tNoGoPEDYerUNCd4yi7zDwgBh2ejgSMExxvkfD',
    documents: {
      note: {
        properties: { message: { type: 'string' } },
        additionalProperties: false,
      },
    },
  },
  entropy: '+RqUArypdL8f/gCMAo4b6c3CoQvxHzsQG0BdYrT5QT0=',
};

// Convert signature and entropy to buffer
stateTransitionObject.signature = Buffer.from(stateTransitionObject.signature, 'base64');
stateTransitionObject.entropy = Buffer.from(stateTransitionObject.entropy, 'base64');

dpp.stateTransition.createFromObject(stateTransitionObject, { skipValidation: true })
  .then((stateTransition) => {
    client.platform.broadcastStateTransition(stateTransition.toBuffer())
      .then(() => console.log('State Transition broadcast successfully'));
  });
```
```javascript JavaScript (dapi-grpc)
const {
  v0: {
    PlatformPromiseClient,
    BroadcastStateTransitionRequest,
  },
} = require('@dashevo/dapi-grpc');
const DashPlatformProtocol = require('@dashevo/dpp');

const platformPromiseClient = new PlatformPromiseClient(
  'http://seed-1.testnet.networks.dash.org:3010',
);

const dpp = new DashPlatformProtocol();

// Data Contract Create State Transition (JSON)
// Replace with your own state transition object before running
const stateTransitionObject = {
  protocolVersion: 0,
  type: 0,
  signature: 'HxAipUsLWQBE++C1suSRNQiQh91rI1LZbblvQhk2erUaIvRneAagxGYYsXXYNvEeO+lBzlF1a9KHGGTHgnO/8Ts=',
  signaturePublicKeyId: 0,
  dataContract: {
    protocolVersion: 0,
    '$id': 'CMc7RghKkHeHtFdwfSX5Hzy7CUdpCEJnwsbfHdsbmJ32',
    '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',
    ownerId: '8Z3ps3tNoGoPEDYerUNCd4yi7zDwgBh2ejgSMExxvkfD',
    documents: {
      note: {
        properties: { message: { type: 'string' } },
        additionalProperties: false,
      },
    },
  },
  entropy: '+RqUArypdL8f/gCMAo4b6c3CoQvxHzsQG0BdYrT5QT0=',
};

// Convert signature and entropy to buffer
stateTransitionObject.signature = Buffer.from(stateTransitionObject.signature, 'base64');
stateTransitionObject.entropy = Buffer.from(stateTransitionObject.entropy, 'base64');

const broadcastStateTransitionRequest = new BroadcastStateTransitionRequest();

dpp.stateTransition.createFromObject(stateTransitionObject, { skipValidation: true })
  .then((stateTransition) => {
    console.log(stateTransition);
    broadcastStateTransitionRequest.setStateTransition(stateTransition.toBuffer());

    platformPromiseClient.broadcastStateTransition(broadcastStateTransitionRequest)
      .then(() => console.log('State Transition broadcast successfully'))
      .catch((e) => {
        console.error(e);
        console.error(e.metadata);
      });
  })
  .catch((e) => console.error(e));
```
```shell gRPCurl
# Submit an identity create State Transition
# `state_transition` must be represented in base64
# Replace `state_transition` with your own state transition object before running
grpcurl -proto protos/platform/v0/platform.proto -plaintext \
  -d '{
    "state_transition":"pWR0eXBlAmlzaWduYXR1cmV4WEg3TWhFWDQ0Z3JzMVIwTE9XTU5IZjAxWFNpYVFQcUlVZ1JLRXQyMkxHVERsUlUrZ1BwQUlUZk5JUmhXd3IvYTVHd0lzWm1idGdYVVFxcVhjbW9lQWtUOD1qcHVibGljS2V5c4GkYmlkAGRkYXRheCxBdzh2UmYxeFFCTlVLbzNiY2llaHlaR2NhM0hBSThkY0ZvVWJTK3hLb0lITmR0eXBlAGlpc0VuYWJsZWT1bmxvY2tlZE91dFBvaW50eDBLT1VUSHB5YnFPek9DNnhEVUhFWm9uc1lNSVpqcGppTHFZNnkxYmlWNWxRQUFBQUFvcHJvdG9jb2xWZXJzaW9uAA=="

    }' \
  seed-1.testnet.networks.dash.org:3010 \
  org.dash.platform.dapi.v0.Platform/broadcastStateTransition
```

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

```javascript JavaScript (dapi-client)
const DAPIClient = require('@dashevo/dapi-client');
const Identifier = require('@dashevo/dpp/lib/Identifier');
const cbor = require('cbor');

const client = new DAPIClient();

const identityId = Identifier.from('4EfA9Jrvv3nnCFdSf7fad59851iiTRZ6Wcu6YVJ4iSeF');
client.platform.getIdentity(identityId).then((response) => {
  // Strip off protocol version (leading 4 bytes) and decode
  const identity = cbor.decode(response.identity.slice(4, response.identity.length));
  console.log(identity);
});
```
```javascript JavaScript (dapi-grpc)
const {
  v0: {
    PlatformPromiseClient,
  },
} = require('@dashevo/dapi-grpc');
const Identifier = require('@dashevo/dpp/lib/Identifier');
const cbor = require('cbor');

const platformPromiseClient = new PlatformPromiseClient(
  'http://seed-1.testnet.networks.dash.org:3010',
);

const id = Identifier.from('4EfA9Jrvv3nnCFdSf7fad59851iiTRZ6Wcu6YVJ4iSeF');
const idBuffer = Buffer.from(id);
const getIdentityRequest = new GetIdentityRequest();
getIdentityRequest.setId(idBuffer);
getIdentityRequest.setProve(false);

platformPromiseClient.getIdentity(getIdentityRequest)
  .then((response) => {
    const identityResponse = response.getIdentity();
    const identityBuffer = Buffer.from(identityResponse);
    // Strip off protocol version (leading 4 bytes) and decode
    console.log(cbor.decode(identityBuffer.slice(4, identityBuffer.length)));
  })
  .catch((e) => console.error(e));
```
```shell gRPCurl
# `id` must be represented in base64
grpcurl -proto protos/platform/v0/platform.proto -plaintext \
  -d '{
    "id":"MBLBm5jsADOt2zbNZLf1EGcPKjUaQwS19plBRChu/aw="
    }' \
  seed-1.testnet.networks.dash.org:3010 \
  org.dash.platform.dapi.v0.Platform/getIdentity
```

```json Response (JavaScript)
{
  id: <Buffer 30 12 c1 9b 98 ec 00 33 ad db 36 cd 64 b7 f5 10 67 0f 2a 35 1a 43 04 b5 f6 99 41 44 28 6e fd ac>,
  balance: 0,
  revision: 0,
  publicKeys: [
    {
      id: 0,
      data: <Buffer 03 8b 74 ae a1 04 c1 94 63 b7 4b e5 fa e9 af 22 55 fe 42 01 3a ec d1 70 92 46 42 14 f2 86 7a c1 9b>,
      type: 0,
      purpose: 0,
      readOnly: false,
      securityLevel: 0
    },
    {
      id: 1,
      data: <Buffer 02 9d f4 53 a6 26 cd e5 01 f4 54 b4 4a 9b ee b8 e5 25 59 0c 94 ef 2e 78 74 39 93 00 66 51 fc ec 4f>,
      type: 0,
      purpose: 0,
      readOnly: false,
      securityLevel: 2
    }
  ]
}
```
```json Response (gRPCurl)
{
  "identity": "AQAAAKRiaWRYIDASwZuY7AAzrds2zWS39RBnDyo1GkMEtfaZQUQobv2sZ2JhbGFuY2UAaHJldmlzaW9uAGpwdWJsaWNLZXlzgqZiaWQAZGRhdGFYIQOLdK6hBMGUY7dL5frpryJV/kIBOuzRcJJGQhTyhnrBm2R0eXBlAGdwdXJwb3NlAGhyZWFkT25sefRtc2VjdXJpdHlMZXZlbACmYmlkAWRkYXRhWCECnfRTpibN5QH0VLRKm+645SVZDJTvLnh0OZMAZlH87E9kdHlwZQBncHVycG9zZQBocmVhZE9ubHn0bXNlY3VyaXR5TGV2ZWwC",
  "metadata": {
    "height": "1190",
    "coreChainLockedHeight": 802899
  }
}
```

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

```javascript JavaScript (dapi-client)
const DAPIClient = require('@dashevo/dapi-client');
const DashPlatformProtocol = require('@dashevo/dpp');

const client = new DAPIClient();
const dpp = new DashPlatformProtocol();

const publicKeyHash = 'f307874782c54f1fb4faf687b6831465469aae15';
const publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];

dpp.initialize().then(() => {
  client.platform.getIdentitiesByPublicKeyHashes(publicKeysBuffer)
    .then((response) => {
      const retrievedIdentity = dpp.identity.createFromBuffer(response.identities[0]);
      console.log(retrievedIdentity.toJSON());
    });
});
```
```javascript JavaScript (dapi-grpc)
const {
  v0: {
    PlatformPromiseClient,
    GetIdentitiesByPublicKeyHashesRequest,
  },
} = require('@dashevo/dapi-grpc');
const DashPlatformProtocol = require('@dashevo/dpp');

const dpp = new DashPlatformProtocol();

dpp.initialize()
  .then(() => {
    const platformPromiseClient = new PlatformPromiseClient(
      'http://seed-1.testnet.networks.dash.org:3010',
    );

    const publicKeyHash = 'f307874782c54f1fb4faf687b6831465469aae15';
    const publicKeysBuffer = [Buffer.from(publicKeyHash, 'hex')];

    const getIdentitiesByPublicKeyHashesRequest = new GetIdentitiesByPublicKeyHashesRequest();
    getIdentitiesByPublicKeyHashesRequest.setPublicKeyHashesList(publicKeysBuffer);

    platformPromiseClient.getIdentitiesByPublicKeyHashes(getIdentitiesByPublicKeyHashesRequest)
      .then((response) => {
        const identitiesResponse = response.getIdentitiesList();
      	console.log(dpp.identity.createFromBuffer(Buffer.from(identitiesResponse[0])).toJSON());
      })
      .catch((e) => console.error(e));
  	});
```
```shell gRPCurl
# `public_key_hashes` must be represented in base64
grpcurl -proto protos/platform/v0/platform.proto -plaintext \
  -d '{
      "public_key_hashes":"8weHR4LFTx+0+vaHtoMUZUaarhU="
    }' \
  seed-1.testnet.networks.dash.org:3010 \
  org.dash.platform.dapi.v0.Platform/getIdentitiesByPublicKeyHashes
```

```json Response (JavaScript)
{
  protocolVersion: 1,
  id: '4EfA9Jrvv3nnCFdSf7fad59851iiTRZ6Wcu6YVJ4iSeF',
  publicKeys: [
    {
      id: 0,
      type: 0,
      purpose: 0,
      securityLevel: 0,
      data: 'A4t0rqEEwZRjt0vl+umvIlX+QgE67NFwkkZCFPKGesGb',
      readOnly: false
    },
    {
      id: 1,
      type: 0,
      purpose: 0,
      securityLevel: 2,
      data: 'Ap30U6YmzeUB9FS0SpvuuOUlWQyU7y54dDmTAGZR/OxP',
      readOnly: false
    }
  ],
  balance: 0,
  revision: 0
}
```
```json Response (gRPCurl)
{
  "identities": [
    "AQAAAKRiaWRYIDASwZuY7AAzrds2zWS39RBnDyo1GkMEtfaZQUQobv2sZ2JhbGFuY2UAaHJldmlzaW9uAGpwdWJsaWNLZXlzgqZiaWQAZGRhdGFYIQOLdK6hBMGUY7dL5frpryJV/kIBOuzRcJJGQhTyhnrBm2R0eXBlAGdwdXJwb3NlAGhyZWFkT25sefRtc2VjdXJpdHlMZXZlbACmYmlkAWRkYXRhWCECnfRTpibN5QH0VLRKm+645SVZDJTvLnh0OZMAZlH87E9kdHlwZQBncHVycG9zZQBocmVhZE9ubHn0bXNlY3VyaXR5TGV2ZWwC"
  ],
  "metadata": {
    "height": "1191",
    "coreChainLockedHeight": 802900
  }
}
```

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

```javascript JavaScript (dapi-client)
const DAPIClient = require('@dashevo/dapi-client');
const Identifier = require('@dashevo/dpp/lib/Identifier');
const cbor = require('cbor');

const client = new DAPIClient();

const contractId = Identifier.from('GWRSAVFMjXx8HpQFaNJMqBV7MBgMK4br5UESsB4S31Ec');
client.platform.getDataContract(contractId).then((response) => {
  const contract = cbor.decode(response.dataContract);
  console.dir(contract, { depth: 10 });
});
```
```javascript JavaScript (dapi-grpc)
const {
  v0: {
    PlatformPromiseClient,
    GetDataContractRequest,
  },
} = require('@dashevo/dapi-grpc');
const Identifier = require('@dashevo/dpp/lib/Identifier');
const cbor = require('cbor');

const platformPromiseClient = new PlatformPromiseClient(
  'http://seed-1.testnet.networks.dash.org:3010',
);

const contractId = Identifier.from('GWRSAVFMjXx8HpQFaNJMqBV7MBgMK4br5UESsB4S31Ec');
const contractIdBuffer = Buffer.from(contractId);
const getDataContractRequest = new GetDataContractRequest();
getDataContractRequest.setId(contractIdBuffer);

platformPromiseClient.getDataContract(getDataContractRequest)
  .then((response) => {
    const contractResponse = response.getDataContract();
    const contractBuffer = Buffer.from(contractResponse);
  	// Strip off protocol version (leading 4 bytes) and decode
  	console.dir(cbor.decode(contractBuffer.slice(4, contractBuffer.length)), { depth: 5 });
  })
  .catch((e) => console.error(e));
```
```shell gRPCurl
# `id` must be represented in base64
grpcurl -proto protos/platform/v0/platform.proto -plaintext \
  -d '{
    "id":"5mjGWa9mruHnLBht3ntbfgodcSoJxA1XIfYiv1PFMVU="
    }' \
  seed-1.testnet.networks.dash.org:3010 \
  org.dash.platform.dapi.v0.Platform/getDataContract
```

```json Response (JavaScript)
{
  '$id': Buffer(32) [Uint8Array] [
    230, 104, 198,  89, 175, 102, 174, 225,
    231,  44,  24, 109, 222, 123,  91, 126,
     10,  29, 113,  42,   9, 196,  13,  87,
     33, 246,  34, 191,  83, 197,  49,  85
  ],
  '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',
  ownerId: Buffer(32) [Uint8Array] [
     48,  18, 193, 155, 152, 236,   0,  51,
    173, 219,  54, 205, 100, 183, 245,  16,
    103,  15,  42,  53,  26,  67,   4, 181,
    246, 153,  65,  68,  40, 110, 253, 172
  ],
  version: 1,
  documents: {
    domain: {
      type: 'object',
      indices: [
        {
          name: 'parentNameAndLabel',
          unique: true,
          properties: [ [Object], [Object] ]
        },
        {
          name: 'dashIdentityId',
          unique: true,
          properties: [ [Object] ]
        },
        { name: 'dashAlias', properties: [ [Object] ] }
      ],
      '$comment': "In order to register a domain you need to create a preorder. The preorder step is needed to prevent man-in-the-middle attacks. normalizedLabel + '.' + normalizedParentDomain must not be longer than 253 chars length as defined by RFC 1035. Domain documents are immutable: modification and deletion are restricted",
      required: [
        'label',
        'normalizedLabel',
        'normalizedParentDomainName',
        'preorderSalt',
        'records',
        'subdomainRules'
      ],
      properties: {
        label: {
          type: 'string',
          pattern: '^[a-zA-Z0-9][a-zA-Z0-9-]{0,61}[a-zA-Z0-9]$',
          maxLength: 63,
          minLength: 3,
          description: "Domain label. e.g. 'Bob'."
        },
        records: {
          type: 'object',
          '$comment': 'Constraint with max and min properties ensure that only one identity record is used - either a `dashUniqueIdentityId` or a `dashAliasIdentityId`',
          properties: {
            dashAliasIdentityId: [Object],
            dashUniqueIdentityId: [Object]
          },
          maxProperties: 1,
          minProperties: 1,
          additionalProperties: false
        },
        preorderSalt: {
          type: 'array',
          maxItems: 32,
          minItems: 32,
          byteArray: true,
          description: 'Salt used in the preorder document'
        },
        subdomainRules: {
          type: 'object',
          required: [ 'allowSubdomains' ],
          properties: { allowSubdomains: [Object] },
          description: 'Subdomain rules allow domain owners to define rules for subdomains',
          additionalProperties: false
        },
        normalizedLabel: {
          type: 'string',
          pattern: '^[a-z0-9][a-z0-9-]{0,61}[a-z0-9]$',
          '$comment': 'Must be equal to the label in lowercase. This property will be deprecated due to case insensitive indices',
          maxLength: 63,
          description: "Domain label in lowercase for case-insensitive uniqueness validation. e.g. 'bob'"
        },
        normalizedParentDomainName: {
          type: 'string',
          pattern: '^$|^[[a-z0-9][a-z0-9-\\.]{0,61}[a-z0-9]$',
          '$comment': 'Must either be equal to an existing domain or empty to create a top level domain. Only the data contract owner can create top level domains.',
          maxLength: 63,
          minLength: 0,
          description: "A full parent domain name in lowercase for case-insensitive uniqueness validation. e.g. 'dash'"
        }
      },
      additionalProperties: false
    },
    preorder: {
      type: 'object',
      indices: [
        { name: 'saltedHash', unique: true, properties: [ [Object] ] }
      ],
      '$comment': 'Preorder documents are immutable: modification and deletion are restricted',
      required: [ 'saltedDomainHash' ],
      properties: {
        saltedDomainHash: {
          type: 'array',
          maxItems: 32,
          minItems: 32,
          byteArray: true,
          description: 'Double sha-256 of the concatenation of a 32 byte random salt and a normalized domain name'
        }
      },
      additionalProperties: false
    }
  }
}
```
```json Response (gRPCurl)
{
  "dataContract": "AQAAAKVjJGlkWCDmaMZZr2au4ecsGG3ee1t+Ch1xKgnEDVch9iK/U8UxVWckc2NoZW1heDRodHRwczovL3NjaGVtYS5kYXNoLm9yZy9kcHAtMC00LTAvbWV0YS9kYXRhLWNvbnRyYWN0Z293bmVySWRYIDASwZuY7AAzrds2zWS39RBnDyo1GkMEtfaZQUQobv2sZ3ZlcnNpb24BaWRvY3VtZW50c6JmZG9tYWlupmR0eXBlZm9iamVjdGdpbmRpY2Vzg6NkbmFtZXJwYXJlbnROYW1lQW5kTGFiZWxmdW5pcXVl9Wpwcm9wZXJ0aWVzgqF4Gm5vcm1hbGl6ZWRQYXJlbnREb21haW5OYW1lY2FzY6Fvbm9ybWFsaXplZExhYmVsY2FzY6NkbmFtZW5kYXNoSWRlbnRpdHlJZGZ1bmlxdWX1anByb3BlcnRpZXOBoXgccmVjb3Jkcy5kYXNoVW5pcXVlSWRlbnRpdHlJZGNhc2OiZG5hbWVpZGFzaEFsaWFzanByb3BlcnRpZXOBoXgbcmVjb3Jkcy5kYXNoQWxpYXNJZGVudGl0eUlkY2FzY2gkY29tbWVudHkBN0luIG9yZGVyIHRvIHJlZ2lzdGVyIGEgZG9tYWluIHlvdSBuZWVkIHRvIGNyZWF0ZSBhIHByZW9yZGVyLiBUaGUgcHJlb3JkZXIgc3RlcCBpcyBuZWVkZWQgdG8gcHJldmVudCBtYW4taW4tdGhlLW1pZGRsZSBhdHRhY2tzLiBub3JtYWxpemVkTGFiZWwgKyAnLicgKyBub3JtYWxpemVkUGFyZW50RG9tYWluIG11c3Qgbm90IGJlIGxvbmdlciB0aGFuIDI1MyBjaGFycyBsZW5ndGggYXMgZGVmaW5lZCBieSBSRkMgMTAzNS4gRG9tYWluIGRvY3VtZW50cyBhcmUgaW1tdXRhYmxlOiBtb2RpZmljYXRpb24gYW5kIGRlbGV0aW9uIGFyZSByZXN0cmljdGVkaHJlcXVpcmVkhmVsYWJlbG9ub3JtYWxpemVkTGFiZWx4Gm5vcm1hbGl6ZWRQYXJlbnREb21haW5OYW1lbHByZW9yZGVyU2FsdGdyZWNvcmRzbnN1YmRvbWFpblJ1bGVzanByb3BlcnRpZXOmZWxhYmVspWR0eXBlZnN0cmluZ2dwYXR0ZXJueCpeW2EtekEtWjAtOV1bYS16QS1aMC05LV17MCw2MX1bYS16QS1aMC05XSRpbWF4TGVuZ3RoGD9pbWluTGVuZ3RoA2tkZXNjcmlwdGlvbngZRG9tYWluIGxhYmVsLiBlLmcuICdCb2InLmdyZWNvcmRzpmR0eXBlZm9iamVjdGgkY29tbWVudHiQQ29uc3RyYWludCB3aXRoIG1heCBhbmQgbWluIHByb3BlcnRpZXMgZW5zdXJlIHRoYXQgb25seSBvbmUgaWRlbnRpdHkgcmVjb3JkIGlzIHVzZWQgLSBlaXRoZXIgYSBgZGFzaFVuaXF1ZUlkZW50aXR5SWRgIG9yIGEgYGRhc2hBbGlhc0lkZW50aXR5SWRganByb3BlcnRpZXOic2Rhc2hBbGlhc0lkZW50aXR5SWSnZHR5cGVlYXJyYXloJGNvbW1lbnR4I011c3QgYmUgZXF1YWwgdG8gdGhlIGRvY3VtZW50IG93bmVyaG1heEl0ZW1zGCBobWluSXRlbXMYIGlieXRlQXJyYXn1a2Rlc2NyaXB0aW9ueD1JZGVudGl0eSBJRCB0byBiZSB1c2VkIHRvIGNyZWF0ZSBhbGlhcyBuYW1lcyBmb3IgdGhlIElkZW50aXR5cGNvbnRlbnRNZWRpYVR5cGV4IWFwcGxpY2F0aW9uL3guZGFzaC5kcHAuaWRlbnRpZmllcnRkYXNoVW5pcXVlSWRlbnRpdHlJZKdkdHlwZWVhcnJheWgkY29tbWVudHgjTXVzdCBiZSBlcXVhbCB0byB0aGUgZG9jdW1lbnQgb3duZXJobWF4SXRlbXMYIGhtaW5JdGVtcxggaWJ5dGVBcnJhefVrZGVzY3JpcHRpb254PklkZW50aXR5IElEIHRvIGJlIHVzZWQgdG8gY3JlYXRlIHRoZSBwcmltYXJ5IG5hbWUgdGhlIElkZW50aXR5cGNvbnRlbnRNZWRpYVR5cGV4IWFwcGxpY2F0aW9uL3guZGFzaC5kcHAuaWRlbnRpZmllcm1tYXhQcm9wZXJ0aWVzAW1taW5Qcm9wZXJ0aWVzAXRhZGRpdGlvbmFsUHJvcGVydGllc/RscHJlb3JkZXJTYWx0pWR0eXBlZWFycmF5aG1heEl0ZW1zGCBobWluSXRlbXMYIGlieXRlQXJyYXn1a2Rlc2NyaXB0aW9ueCJTYWx0IHVzZWQgaW4gdGhlIHByZW9yZGVyIGRvY3VtZW50bnN1YmRvbWFpblJ1bGVzpWR0eXBlZm9iamVjdGhyZXF1aXJlZIFvYWxsb3dTdWJkb21haW5zanByb3BlcnRpZXOhb2FsbG93U3ViZG9tYWluc6NkdHlwZWdib29sZWFuaCRjb21tZW50eE9Pbmx5IHRoZSBkb21haW4gb3duZXIgaXMgYWxsb3dlZCB0byBjcmVhdGUgc3ViZG9tYWlucyBmb3Igbm9uIHRvcC1sZXZlbCBkb21haW5za2Rlc2NyaXB0aW9ueFtUaGlzIG9wdGlvbiBkZWZpbmVzIHdobyBjYW4gY3JlYXRlIHN1YmRvbWFpbnM6IHRydWUgLSBhbnlvbmU7IGZhbHNlIC0gb25seSB0aGUgZG9tYWluIG93bmVya2Rlc2NyaXB0aW9ueEJTdWJkb21haW4gcnVsZXMgYWxsb3cgZG9tYWluIG93bmVycyB0byBkZWZpbmUgcnVsZXMgZm9yIHN1YmRvbWFpbnN0YWRkaXRpb25hbFByb3BlcnRpZXP0b25vcm1hbGl6ZWRMYWJlbKVkdHlwZWZzdHJpbmdncGF0dGVybnghXlthLXowLTldW2EtejAtOS1dezAsNjF9W2EtejAtOV0kaCRjb21tZW50eGlNdXN0IGJlIGVxdWFsIHRvIHRoZSBsYWJlbCBpbiBsb3dlcmNhc2UuIFRoaXMgcHJvcGVydHkgd2lsbCBiZSBkZXByZWNhdGVkIGR1ZSB0byBjYXNlIGluc2Vuc2l0aXZlIGluZGljZXNpbWF4TGVuZ3RoGD9rZGVzY3JpcHRpb254UERvbWFpbiBsYWJlbCBpbiBsb3dlcmNhc2UgZm9yIGNhc2UtaW5zZW5zaXRpdmUgdW5pcXVlbmVzcyB2YWxpZGF0aW9uLiBlLmcuICdib2IneBpub3JtYWxpemVkUGFyZW50RG9tYWluTmFtZaZkdHlwZWZzdHJpbmdncGF0dGVybngnXiR8XltbYS16MC05XVthLXowLTktXC5dezAsNjF9W2EtejAtOV0kaCRjb21tZW50eIxNdXN0IGVpdGhlciBiZSBlcXVhbCB0byBhbiBleGlzdGluZyBkb21haW4gb3IgZW1wdHkgdG8gY3JlYXRlIGEgdG9wIGxldmVsIGRvbWFpbi4gT25seSB0aGUgZGF0YSBjb250cmFjdCBvd25lciBjYW4gY3JlYXRlIHRvcCBsZXZlbCBkb21haW5zLmltYXhMZW5ndGgYP2ltaW5MZW5ndGgAa2Rlc2NyaXB0aW9ueF5BIGZ1bGwgcGFyZW50IGRvbWFpbiBuYW1lIGluIGxvd2VyY2FzZSBmb3IgY2FzZS1pbnNlbnNpdGl2ZSB1bmlxdWVuZXNzIHZhbGlkYXRpb24uIGUuZy4gJ2Rhc2gndGFkZGl0aW9uYWxQcm9wZXJ0aWVz9GhwcmVvcmRlcqZkdHlwZWZvYmplY3RnaW5kaWNlc4GjZG5hbWVqc2FsdGVkSGFzaGZ1bmlxdWX1anByb3BlcnRpZXOBoXBzYWx0ZWREb21haW5IYXNoY2FzY2gkY29tbWVudHhKUHJlb3JkZXIgZG9jdW1lbnRzIGFyZSBpbW11dGFibGU6IG1vZGlmaWNhdGlvbiBhbmQgZGVsZXRpb24gYXJlIHJlc3RyaWN0ZWRocmVxdWlyZWSBcHNhbHRlZERvbWFpbkhhc2hqcHJvcGVydGllc6Fwc2FsdGVkRG9tYWluSGFzaKVkdHlwZWVhcnJheWhtYXhJdGVtcxggaG1pbkl0ZW1zGCBpYnl0ZUFycmF59WtkZXNjcmlwdGlvbnhZRG91YmxlIHNoYS0yNTYgb2YgdGhlIGNvbmNhdGVuYXRpb24gb2YgYSAzMiBieXRlIHJhbmRvbSBzYWx0IGFuZCBhIG5vcm1hbGl6ZWQgZG9tYWluIG5hbWV0YWRkaXRpb25hbFByb3BlcnRpZXP0",
  "metadata": {
    "height": "1191",
    "coreChainLockedHeight": 802900
  }
}
```

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

```javascript JavaScript (dapi-client)
const DAPIClient = require('@dashevo/dapi-client');
const Identifier = require('@dashevo/dpp/lib/Identifier');
const cbor = require('cbor');

const client = new DAPIClient();

const contractId = Identifier.from('GWRSAVFMjXx8HpQFaNJMqBV7MBgMK4br5UESsB4S31Ec');
client.platform.getDocuments(contractId, 'domain', { limit: 10 }).then((response) => {
  for (const rawData of response.documents) {
    console.log(cbor.decode(rawData.slice(4, rawData.length)));
  }
});
```
```javascript JavaScript (dapi-grpc)
const {
  v0: {
    PlatformPromiseClient,
    GetDocumentsRequest,
  },
} = require('@dashevo/dapi-grpc');
const cbor = require('cbor');
const Identifier = require('@dashevo/dpp/lib/Identifier');

const platformPromiseClient = new PlatformPromiseClient(
  'http://seed-1.testnet.networks.dash.org:3010',
);

const contractId = Identifier.from('GWRSAVFMjXx8HpQFaNJMqBV7MBgMK4br5UESsB4S31Ec');
const contractIdBuffer = Buffer.from(contractId);
const getDocumentsRequest = new GetDocumentsRequest();
const type = 'domain';
const limit = 10;

getDocumentsRequest.setDataContractId(contractIdBuffer);
getDocumentsRequest.setDocumentType(type);
// getDocumentsRequest.setWhere(whereSerialized);
// getDocumentsRequest.setOrderBy(orderBySerialized);
getDocumentsRequest.setLimit(limit);
// getDocumentsRequest.setStartAfter(startAfter);
// getDocumentsRequest.setStartAt(startAt);

platformPromiseClient.getDocuments(getDocumentsRequest)
  .then((response) => {
    for (const document of response.getDocumentsList()) {
      const documentBuffer = Buffer.from(document);
      // Strip off protocol version (leading 4 bytes) and decode
      console.log(cbor.decode(documentBuffer.slice(4, documentBuffer.length)));
    }
  })
  .catch((e) => console.error(e));
```
```shell Request (gRPCurl)
# Request documents
# `id` must be represented in base64
grpcurl -proto protos/platform/v0/platform.proto -plaintext \
  -d '{
    "data_contract_id":"5mjGWa9mruHnLBht3ntbfgodcSoJxA1XIfYiv1PFMVU=",
    "document_type":"domain",
    "limit":1
    }' \
  seed-1.testnet.networks.dash.org:3010 \
  org.dash.platform.dapi.v0.Platform/getDocuments
```

```json Response (JavaScript)
{
  '$id': <Buffer 16 e7 55 dd 8a 97 0c 10 ee 11 55 c4 e7 ed b2 01 fe 82 80 49 38 1a b7 e7 14 1f 6f 4a 7d 95 ab a4>,
  '$type': 'domain',
  label: 'RT-First-00000',
  records: {
    dashUniqueIdentityId: <Buffer e9 17 11 54 b9 91 b0 aa 1a 5a a4 d7 78 70 99 37 6e 04 8f 43 a7 f3 00 45 7c d4 5d 3c bc 4a db 13>
  },
  '$ownerId': <Buffer e9 17 11 54 b9 91 b0 aa 1a 5a a4 d7 78 70 99 37 6e 04 8f 43 a7 f3 00 45 7c d4 5d 3c bc 4a db 13>,
  '$revision': 1,
  preorderSalt: <Buffer 80 a7 a7 93 1a 88 8f 9e 32 41 63 9c 8c 04 c5 94 48 b7 3b 32 7f bc dc 58 97 04 fb 37 df 40 7c a2>,
  subdomainRules: { allowSubdomains: false },
  '$dataContractId': <Buffer e6 68 c6 59 af 66 ae e1 e7 2c 18 6d de 7b 5b 7e 0a 1d 71 2a 09 c4 0d 57 21 f6 22 bf 53 c5 31 55>,
  normalizedLabel: 'rt-first-00000',
  normalizedParentDomainName: 'dash'
}
```
```json Response (gRPCurl)
{
  "documents": [
    "AQAAAKxjJGlkWCDX8sU/RqkXq25bOaLXvCYLZJKJRTdE0eDU8mqNjv83z2UkdHlwZWZkb21haW5lbGFiZWxkZGFzaGdyZWNvcmRzoXNkYXNoQWxpYXNJZGVudGl0eUlkWCAwEsGbmOwAM63bNs1kt/UQZw8qNRpDBLX2mUFEKG79rGgkb3duZXJJZFggMBLBm5jsADOt2zbNZLf1EGcPKjUaQwS19plBRChu/axpJHJldmlzaW9uAWokY3JlYXRlZEF0GwAAAXrOSnqVbHByZW9yZGVyU2FsdFgg4LUIxaNoJaIGaTofQUqhPtvs9DxB48eZ6p5ze0+aoiZuc3ViZG9tYWluUnVsZXOhb2FsbG93U3ViZG9tYWluc/VvJGRhdGFDb250cmFjdElkWCDmaMZZr2au4ecsGG3ee1t+Ch1xKgnEDVch9iK/U8UxVW9ub3JtYWxpemVkTGFiZWxkZGFzaHgabm9ybWFsaXplZFBhcmVudERvbWFpbk5hbWVg"
  ],
  "metadata": {
    "height": "409",
    "coreChainLockedHeight": 802152
  }
}
```

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

```javascript JavaScript (dapi-client)
const DAPIClient = require('@dashevo/dapi-client');
const DashPlatformProtocol = require('@dashevo/dpp');
const crypto = require('crypto');

const client = new DAPIClient();
const dpp = new DashPlatformProtocol();

// Replace with your own state transition object before running
const stateTransitionObject = {
  protocolVersion: 0,
  type: 0,
  signature: 'HxAipUsLWQBE++C1suSRNQiQh91rI1LZbblvQhk2erUaIvRneAagxGYYsXXYNvEeO+lBzlF1a9KHGGTHgnO/8Ts=',
  signaturePublicKeyId: 0,
  dataContract: {
    protocolVersion: 0,
    '$id': 'CMc7RghKkHeHtFdwfSX5Hzy7CUdpCEJnwsbfHdsbmJ32',
    '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',
    ownerId: '8Z3ps3tNoGoPEDYerUNCd4yi7zDwgBh2ejgSMExxvkfD',
    documents: {
      note: {
        properties: { message: { type: 'string' } },
        additionalProperties: false,
      },
    },
  },
  entropy: '+RqUArypdL8f/gCMAo4b6c3CoQvxHzsQG0BdYrT5QT0=',
};

// Convert signature and entropy to buffer
stateTransitionObject.signature = Buffer.from(stateTransitionObject.signature, 'base64');
stateTransitionObject.entropy = Buffer.from(stateTransitionObject.entropy, 'base64');

dpp.stateTransition.createFromObject(stateTransitionObject, { skipValidation: true })
  .then((stateTransition) => {
    //  Calculate state transition hash
    const hash = crypto.createHash('sha256')
      .update(stateTransition.toBuffer())
      .digest();

    console.log(`Requesting proof of state transition with hash:\n\t${hash.toString('hex')}`);

    client.platform.waitForStateTransitionResult(hash, { prove: true })
      .then((response) => {
        console.log(response);
      });
  });
```
```javascript JavaScript (dapi-grpc)
const {
  v0: {
    PlatformPromiseClient,
    WaitForStateTransitionResultRequest,
  },
} = require('@dashevo/dapi-grpc');
const DashPlatformProtocol = require('@dashevo/dpp');
const crypto = require('crypto');

const platformPromiseClient = new PlatformPromiseClient(
  'http://seed-1.testnet.networks.dash.org:3010',
);

const dpp = new DashPlatformProtocol();

// Replace with your own state transition object before running
const stateTransitionObject = {
  protocolVersion: 0,
  type: 0,
  signature: 'HxAipUsLWQBE++C1suSRNQiQh91rI1LZbblvQhk2erUaIvRneAagxGYYsXXYNvEeO+lBzlF1a9KHGGTHgnO/8Ts=',
  signaturePublicKeyId: 0,
  dataContract: {
    protocolVersion: 0,
    '$id': 'CMc7RghKkHeHtFdwfSX5Hzy7CUdpCEJnwsbfHdsbmJ32',
    '$schema': 'https://schema.dash.org/dpp-0-4-0/meta/data-contract',
    ownerId: '8Z3ps3tNoGoPEDYerUNCd4yi7zDwgBh2ejgSMExxvkfD',
    documents: {
      note: {
        properties: { message: { type: 'string' } },
        additionalProperties: false,
      },
    },
  },
  entropy: '+RqUArypdL8f/gCMAo4b6c3CoQvxHzsQG0BdYrT5QT0=',
};

// Convert signature and entropy to buffer
stateTransitionObject.signature = Buffer.from(stateTransitionObject.signature, 'base64');
stateTransitionObject.entropy = Buffer.from(stateTransitionObject.entropy, 'base64');

dpp.stateTransition.createFromObject(stateTransitionObject, { skipValidation: true })
  .then((stateTransition) => {
    //  Calculate state transition hash
    const hash = crypto.createHash('sha256')
      .update(stateTransition.toBuffer())
      .digest();

    const waitForStateTransitionResultRequest = new WaitForStateTransitionResultRequest();
    waitForStateTransitionResultRequest.setStateTransitionHash(hash);
    waitForStateTransitionResultRequest.setProve(true);

    console.log(`Requesting proof of state transition with hash:\n\t${hash.toString('hex')}`);

    platformPromiseClient.waitForStateTransitionResult(waitForStateTransitionResultRequest)
      .then((response) => {
        const rootTreeProof = Buffer.from(response.getProof().getRootTreeProof());
        const storeTreeProof = Buffer.from(response.getProof().getStoreTreeProof());
        console.log(`Root tree proof: ${rootTreeProof.toString('hex')}`);
        console.log(`Store tree proof: ${storeTreeProof.toString('hex')}`);
      })
  		.catch((e) => console.error(e));
  });
```
```shell Request (gRPCurl)
# `state_transition_hash` must be represented in base64
# Replace `state_transition_hash` with your own before running
grpcurl -proto protos/platform/v0/platform.proto -plaintext \
  -d '{
    "state_transition_hash":"wEiwFu9WvAtylrwTph5v0uXQm743N+75C+C9DhmZBkw=",
    "prove": "true"
    }' \
  seed-1.testnet.networks.dash.org:3010 \
  org.dash.platform.dapi.v0.Platform/waitForStateTransitionResult
```

```json Response (JavaScript)
{
  proof: {
    rootTreeProof: <Buffer 01 00 00 00 03 26 e0 35 e0 31 82 7e 7c 27 b0 91 23 41 ed d2 11 bf 3b 90 54 70 11 2c 68 5a 8e 76 8c 68 bb 39 21 3d cf 46 6d 09 d0 7a 28 e3 e9 0b 2b 0e ... 17 more bytes>,
    storeTreeProof: <Buffer 01 0b ee 31 ce ca 2a bd 44 6a db d4 9f 13 4a 7d 70 25 96 a9 b9 02 6e c4 e1 90 95 f7 a1 b4 c9 de 1f e4 63 e6 ce f7 58 3a 5b c3 10 01 78 9b 4f 98 9a c9 ... 526 more bytes>
  }
}
```
```text Response (dapi-grpc)
Requesting proof of state transition with hash:
        8ae93b89c272455f3ce8d01dba99a3a28c9550262a602c6ba44de08e545d3aa9
Root tree proof: 010000000326e035e031827e7c27b0912341edd211bf3b905470112c685a8e768c68bb39213dcf466d09d07a28e3e90b2b0e1d1510dede30214f68e32f8cf498220101
Store tree proof: 010bee31ceca2abd446adbd49f134a7d702596a9b9026ec4e19095f7a1b4c9de1fe463e6cef7583a5bc31001789b4f989ac9f8f524f1247fed372502d8c54a3c026072d5239f074422621673c250d1c74eadbb304a10013fb54a99ef641b9a7585d6d28dd443875e435a35022d92a0711f56ae23bc13f4a630a1455970451e3f1001fe2b060fca69ce2eb3d784cec28c0f575690f131026df252af068635bdf08f5448ea67c23d9a9a02831001a7df0a9f392682d7d7d0a29bd43d932c16b0d6530320a8b7df4cadb6cdfd5b5d1bb31cbb488d241e91d60cc1341cd686a3fbb6291f19e800a5632469645820a8b7df4cadb6cdfd5b5d1bb31cbb488d241e91d60cc1341cd686a3fbb6291f196724736368656d61783468747470733a2f2f736368656d612e646173682e6f72672f6470702d302d342d302f6d6574612f646174612d636f6e7472616374676f776e657249645820703796bfd3e2bbd54505a8e04929bb05b8aecfb1cd5c013ef8a8b84511770e0c69646f63756d656e7473a1646e6f7465a26a70726f70657274696573a1676d657373616765a1647479706566737472696e67746164646974696f6e616c50726f70657274696573f46f70726f746f636f6c56657273696f6e001001d94cd40044b8485e962d80e57c1992c77a182a1611111102abe7e3f7231ed71b8c4fd7ee09d4a1f970a51cd4100139312feadce145313ef34f720e940892d0ed2405111102245c6aaaf192b51207333be85ab77c9c9393af47100128ef7d4479e4f488373d92280fe8e52a9a48a6621111
```
```json Response (gRPCurl)
{
  "proof": {
    "merkleProof": "AQAAAAAAAAJHAV8NeFRgf0cWRiCIIVsbJcNs+bALwShVQEkXV2jRCueIAkeoHTKeU60+Jm2oiHbGSkOL8ui06Nj27SLz2raMF29iEAHKnCrDvO67hcJV79tfQwqQSFcxJOek9Fa/x3oYvwQrQQL8lxVOSAkZSC1qhI8LJa6PtR+u8TifMMmRCRY2dXrSUBABihjPBDgD9SM/d9JgWkYyT+sUp4FdgotmwHhZIB1rJVUCkaeftxRkQ/B0FU26ojDJirY/SwZ8RcU1/3pnbJbA+5EQBCBkeRNzJuVLjOKnDOmG48wnSzjHg5lLdlA5JSBD79rQxQAFAgEBAAAYUfu7+1iqqIwjkszndZ6Vm8pli6pjt4XxvvgKCcE3ygI+G8nIsflGKoZQOaWGcFBmXDzAwJqjGlMxSIWNSEgoQBACAQd7xuP4BzKoaf75MNyQQC6Q7e94Vg2IdQe2LFJysA4QAZ2mq+ad/rNJ7n2fPiJgHi4NMT9Wht6Kb5J8qF20hajJEQKz2VnGsgYNwMjY9kadWb63Tjk3nTqFttgjLnoz4PCpQBABpdWVJ9sfU+o+OEcUuFcDOV+y4gYFoao3kVNLZ+Yz7Y4RAuVHBoF21TyNc9DUDHmAfcaJf6K+/VzqIzfnJ7iGqXN+EAGvNlkFsWw+DjFg+MLjzo2MzUaWk7sA63+rG3FQJ7LO2hERERECPImih8uZnpEfew+qLeKrdEig5TR6g5VsfuHI9U2WuvsQAU7qdWenHDisDf3TNzJQytKJYaeyhmy2LEo11zZVwsoREQIAAAAAAAAA/wMBAAD6APcBAAAApGJpZFggZHkTcyblS4zipwzphuPMJ0s4x4OZS3ZQOSUgQ+/a0MVnYmFsYW5jZRo7mmxAaHJldmlzaW9uAGpwdWJsaWNLZXlzgqZiaWQAZGRhdGFYIQLlz7I9IuqDAf1fp2xiyvGiApsvgANo2ldfmrWv6MsfG2R0eXBlAGdwdXJwb3NlAGhyZWFkT25sefRtc2VjdXJpdHlMZXZlbACmYmlkAWRkYXRhWCECrq7odM9OoHGEyM1D19ZAaEPf50OKLwsxL2D4SpnLZllkdHlwZQBncHVycG9zZQBocmVhZE9ubHn0bXNlY3VyaXR5TGV2ZWwCAAEAAAAAAAAA0QQBAAAkAgEgbaBpE46QX8+EXS6Sl5CG4r+JuiXVDhxZeZy/TS8qnQEAUw2uiB7scatAs99mQfB5VfVb0lMSywDMHXpmUgNfONsC4OEPflWGlZAqBSOhaKD0/SfPJHbOOMEfCjTtBV1jjgwQARdKA3tf2c8gP3H7tRKcRMHXfljTH/4L8L63tbZk5FX/EQL+TedZ8kFHQEuNY8e9pfmaLI36Y7rHe1hAjVBSh9U95BABfPfYq74T0N0ygKE4spKvSQkekrnH0Ge8Ot0FEDsq2rcR",
    "signatureLlmqHash": "AAAA1h0X9yUNsXpD0/iKlsPvVb+VezkZAIkQIzmGqoc=",
    "signature": "jeByZ8qlZvID/C3LVVy/mZGHlRu2QhN3MZO09hCOjAH0gn1tqrAX6BXaJf6qRLw9APv0+nInObRF3JhstvsByPK8QOHCCl9M3NpcgI/HCECpqMMG8S9DPtJYI6HwQO5I"
  },
  "metadata": {
    "height": "1221",
    "coreChainLockedHeight": 802939
  }
}
```

# Deprecated Endpoints

The `getIdentityIdsByPublicKeyHashes` was deprecated and removed in Dash Platform v0.23. The previous version of documentation can be [viewed here](https://dashplatform.readme.io/v0.22.0/docs/reference-dapi-endpoints-platform-endpoints).

# Code Reference
Implementation details related to the information on this page can be found in:
- The [Platform repository](https://github.com/dashevo/platform/tree/master/packages/dapi) `packages/dapi/lib/grpcServer/handlers/core` folder
- The [Platform repository](https://github.com/dashevo/platform/tree/master/packages/dapi-grpc) `packages/dapi-grpc/protos` folder