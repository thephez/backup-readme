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
      "code": "# Submit a DPNS `preorder` State Transition\ngrpcurl -proto protos/platform.proto -plaintext \\\n  -d '{\n    \"state_transition\":\"pmR0eXBlAmdhY3Rpb25zgQFpZG9jdW1lbnRzgaZkJHJldgFlJHR5cGVocHJlb3JkZXJnJHVzZXJJZHgsR0pNVm51UzdYVFhkaWtnalFyRDR0TjVaSkNYem02eE12R0dyNVNkdGVjcDFoJGVudHJvcHl4InlVOXVta1Q0QnZjQWpQSmpGRVRGNW9CbUgzdEEyU3FKS2drJGNvbnRyYWN0SWR4LDJLZk1jTXhrdEtpbUp4QVpVZVp3WWtGVXNFY0FaaERLRXBRczhHTW5wVXNlcHNhbHRlZERvbWFpbkhhc2h4XjU2MmQ4Y2Q1YTQ1Nzg4ZWU0MWM3YzNiYWNhZGU5ODMwNGY0MTk0MzkyOTA4NDgxMzljOWZiZDU2MTI3NDY1NzM3NDJlNzQ2ODY1NzA2ODY1N2EzMzJlNjQ2MTczNjhpc2lnbmF0dXJleFhIMkxxMW5pM1cyR0Q0TXlqK3lzSHdOMExKRXdHSjExMTRaTHExL0dTalJxakliY2Z0VzcvUkpZVFozeFhnOW0wTTJ4SnVJSEwvMzVGUFVUdUkxUUFBSTg9b3Byb3RvY29sVmVyc2lvbgB0c2lnbmF0dXJlUHVibGljS2V5SWQB\"\n\n    }' \\\n  evonet.thephez.com:3010 \\\n  org.dash.platform.dapi.v0.Platform/applyStateTransition",
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
      "code": "grpcurl -proto protos/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"CbYVyoKnGxkHaBrucCAhPEBcpuz8j15cnYYiv1CEHBNx\"\n    }' \\\n  evonet.thephez.com:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentity",
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
      "code": "{\n  \"identity\": \"o2JpZHgsQ2JZVnlvS25HeGtIYUJydWNDQWhQRUJjcHV6OGoxNWNuWVlpdjFDRUhCTnhkdHlwZQFqcHVibGljS2V5c4GkYmlkAWRkYXRheCxBbXpSMkZNNGZZd0NtWnhHWjFOMnRhMkZmdUo5NU93K0xMQXJaREx1WUJqdGR0eXBlAWlpc0VuYWJsZWT1\"\n}",
      "language": "json",
      "name": "Response (truncated)"
    }
  ]
}
[/block]

[block:html]
{
  "html": "<div></div>\n<!--\nFull identity result:\no2JpZHgsQ2JZVnlvS25HeGtIYUJydWNDQWhQRUJjcHV6OGoxNWNuWVlpdjFDRUhCTnhkdHlwZQFqcHVibGljS2V5c4GkYmlkAWRkYXRheCxBbXpSMkZNNGZZd0NtWnhHWjFOMnRhMkZmdUo5NU93K0xMQXJaREx1WUJqdGR0eXBlAWlpc0VuYWJsZWT1\n-->\n<style></style>"
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
      "code": "grpcurl -proto protos/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"2KfMcMxktKimJxAZUeZwYkFUsEcAZhDKEpQs8GMnpUse\"\n    }' \\\n  evonet.thephez.com:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDataContract",
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
      "code": "{\n  \"dataContract\": \"pGckc2NoZW1heDRodHRwczovL3NjaGVtYS5kYXNoLm9yZy9kcHAtMC00LTAvbWV0YS9kYXRhLWNvbnRyYWN0Z3ZlcnNpb24BaWRvY3VtZW50c6JmZG9tYWlupGdpbmRpY2Vzg6JmdW5pcXVl9Wpwcm9wZXJ0aWVzgaFobmFtZUhhc2hjYXNjoWpwcm9wZXJ0aWVzgqF4Gm5vcm1hbGl6ZWRQYXJlbnREb21haW5OYW1lY2FzY6Fvbm9ybWFsaXplZExhYmVsY2FzY6FqcHJvcGVydGllc4GhdHJlY29yZHMuZGFzaElkZW50aXR5Y2FzY2hyZXF1aXJlZIZobmFtZUhhc2hlbGFiZWxvbm9ybWFsaXplZExhYmVseBpub3JtYWxpemVkUGFyZW50RG9tYWluTmFtZWxwcmVvcmRlclNhbHRncmVjb3Jkc2pwcm9wZXJ0aWVzpmVsYWJlbKJkdHlwZWZzdHJpbmdncGF0dGVybngmXigoPyEtKVthLXpBLVowLTktXXswLDYyfVthLXpBLVowLTldKSRncmVjb3Jkc6RkdHlwZWZvYmplY3RqcHJvcGVydGllc6FsZGFzaElkZW50aXR5pGR0eXBlZnN0cmluZ2dwYXR0ZXJueD9eWzEyMzQ1Njc4OUFCQ0RFRkdISktMTU5QUVJTVFVWV1hZWmFiY2RlZmdoaWprbW5vcHFyc3R1dnd4eXpdKyRpbWF4TGVuZ3RoGCxpbWluTGVuZ3RoGCptbWluUHJvcGVydGllcwF0YWRkaXRpb25hbFByb3BlcnRpZXP0aG5hbWVIYXNoomR0eXBlZnN0cmluZ2ltaW5MZW5ndGgBbHByZW9yZGVyU2FsdKJkdHlwZWZzdHJpbmdpbWluTGVuZ3RoAW9ub3JtYWxpemVkTGFiZWyiZHR5cGVmc3RyaW5nZ3BhdHRlcm54IF4oKD8hLSlbYS16MC05LV17MCw2Mn1bYS16MC05XSkkeBpub3JtYWxpemVkUGFyZW50RG9tYWluTmFtZaJkdHlwZWZzdHJpbmdpbWluTGVuZ3RoAHRhZGRpdGlvbmFsUHJvcGVydGllc/RocHJlb3JkZXKkZ2luZGljZXOBomZ1bmlxdWX1anByb3BlcnRpZXOBoXBzYWx0ZWREb21haW5IYXNoY2FzY2hyZXF1aXJlZIFwc2FsdGVkRG9tYWluSGFzaGpwcm9wZXJ0aWVzoXBzYWx0ZWREb21haW5IYXNoomR0eXBlZnN0cmluZ2ltaW5MZW5ndGgBdGFkZGl0aW9uYWxQcm9wZXJ0aWVz9Gpjb250cmFjdElkeCwyS2ZNY014a3RLaW1KeEFaVWVad1lrRlVzRWNBWmhES0VwUXM4R01ucFVzZQ==\"\n}",
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
      "code": "# Request one DPNS domain document\ngrpcurl -proto protos/platform.proto -plaintext \\\n  -d '{\n    \"data_contract_id\":\"2KfMcMxktKimJxAZUeZwYkFUsEcAZhDKEpQs8GMnpUse\",\n    \"document_type\":\"domain\",\n    \"limit\":1\n    }' \\\n  evonet.thephez.com:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDocuments",
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
      "code": "{\n  \"documents\": [\n    \"q2QkcmV2AWUkdHlwZWZkb21haW5lbGFiZWxkZGFzaGckdXNlcklkeCwyS2ZNY014a3RLaW1KeEFaVWVad1lrRlVzRWNBWmhES0VwUXM4R01ucFVzZWdyZWNvcmRzoWxkYXNoSWRlbnRpdHl4LDJLZk1jTXhrdEtpbUp4QVpVZVp3WWtGVXNFY0FaaERLRXBRczhHTW5wVXNlaCRlbnRyb3B5eCJ5TGY3b214aHVVQVYybXh0VUNmaG1hTUFZZWdLZTJSRUJaaG5hbWVIYXNobDU2MDQ2NDYxNzM2OGskY29udHJhY3RJZHgsMktmTWNNeGt0S2ltSnhBWlVlWndZa0ZVc0VjQVpoREtFcFFzOEdNbnBVc2VscHJlb3JkZXJTYWx0eCJ5Z244S20zVXhhU1E4QU54S2NIU1J4dDNKNldYNzkyWlcyb25vcm1hbGl6ZWRMYWJlbGRkYXNoeBpub3JtYWxpemVkUGFyZW50RG9tYWluTmFtZWA=\"\n  ]\n}\n",
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