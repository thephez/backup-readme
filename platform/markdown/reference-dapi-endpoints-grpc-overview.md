[block:callout]
{
  "type": "danger",
  "title": "Work in Progress",
  "body": "This document is an work in progress. Details may be missing or incomplete and are subject to change."
}
[/block]
The gRPC endpoints provide access to information from Dash Platform (layer 2) as well as streaming of events related to blocks and transactions/transitions.

# Connecting to gRPC

## Auto-generated Clients

Clients for a number of languages are built automatically from the protocol definitions and are available in the `clients` folder of the [dapi-grpc repository](https://github.com/dashevo/dapi-grpc/). The protocol definitions are available in the `protos` folder of the repository.
[block:callout]
{
  "type": "info",
  "title": "Pull Requests welcome",
  "body": "Pull requests are welcome to add support for additional languages that are not currently being built."
}
[/block]
## Command line

The examples shown in the endpoint details pages use a command-line tool named [gRPCurl](https://github.com/fullstorydev/grpcurl) that allows interacting with gRPC servers in a similar way as `curl` does for the [JSON-RPCs](reference-dapi-endpoints-json-rpc-endpoints). Additional information may be found in the [gRPC documentation](https://grpc.io/docs/guides/).

To use gRPCurl as shown in the detailed examples, clone the [dapi-grpc](https://github.com/dashevo/dapi-grpc) repository and execute the example requests from the root directory of that repository as shown in this example:
[block:code]
{
  "codes": [
    {
      "code": "# Clone the dapi-grpc repository\ngit clone https://github.com/dashevo/dapi-grpc.git\ncd dapi-grpc\n\n# Execute gRPCurl command\ngrpcurl -plaintext -proto protos/...",
      "language": "shell"
    }
  ]
}
[/block]
# Data Encoding

The data submitted/received from the gRPC endpoints is encoded using both [CBOR](https://tools.ietf.org/html/rfc7049) and Base64. Data is first encoded with CBOR and the resulting output is then encoded in Base64 before being sent. 
[block:callout]
{
  "type": "warning",
  "title": "Canonical Encoding",
  "body": "Canonical encoding is used for state transitions, identities, data contracts, and documents. This puts the object's data fields in a sorted order to ensure the same hash is produced every time regardless of the actual order received by the encoder. Reproducible hashes are necessary to support validation of request/response data."
}
[/block]
Libraries such as [`cbor` (JavaScript)](https://www.npmjs.com/package/cbor) and [`cbor2` (Python)](https://pypi.org/project/cbor2/) can be used to encode/decode data for DAPI gRPC endpoints.

The examples below use the response from a [`getIdentity` gPRC request](#section-get-identity) to demonstrate how to both encode data for sending and decode received data:
[block:code]
{
  "codes": [
    {
      "code": "from base64 import b64decode, b64encode\nimport json\nimport cbor2\n\ngrpc_identity_response = 'o2JpZHgsQ2JZVnlvS25HeGtIYUJydWNDQWhQRUJjcHV6OGoxNWNuWVlpdjFDRUhCTnhkdHlwZQFqcHVibGljS2V5c4GkYmlkAWRkYXRheCxBbXpSMkZNNGZZd0NtWnhHWjFOMnRhMkZmdUo5NU93K0xMQXJaREx1WUJqdGR0eXBlAWlpc0VuYWJsZWT1'\n\nidentity_cbor = b64decode(grpc_identity_response)\nidentity = cbor2.loads(identity_cbor)\n\nprint('Identity details:\\n{}\\n'.format(json.dumps(identity, indent=2)))",
      "language": "python",
      "name": "Python - Decode Identity"
    },
    {
      "code": "from base64 import b64decode, b64encode\nimport json\nimport cbor2\n\n# Encode an identity\nidentity = {\n  \"id\": \"CbYVyoKnGxkHaBrucCAhPEBcpuz8j15cnYYiv1CEHBNx\",  \n  \"type\": 1,\n  \"publicKeys\": [\n    {\n      \"id\": 1,\n      \"data\": \"AmzR2FM4fYwCmZxGZ1N2ta2FfuJ95Ow+LLArZDLuYBjt\",\n      \"type\": 1,\n      \"isEnabled\": True\n    }\n  ]\n}\n\nidentity_cbor = cbor2.dumps(identity)\nidentity_grpc = b64encode(identity_cbor)\nprint('Identity gRPC data: {}'.format(identity_grpc))\n",
      "language": "python",
      "name": "Python - Encode Identity"
    }
  ]
}
[/block]