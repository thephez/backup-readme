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
      "code": "grpcurl -proto protos/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"At44pvrZXLwjbJp415E2kjav49goGosRF3SB1WW1QJoG\"\n    }' \\\n  evonet.thephez.com:3010 \\\n  org.dash.platform.dapi.v0.Platform/getIdentity",
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
      "code": "{\n  \"identity\": \"o2JpZHgsQXQ0NHB2clpYTHdqYkpwNDE1RTJramF2NDlnb0dvc1JGM1NCMVdXMVFKb0dkdHlwZQFqcHVibGljS2V5c4GkYmlkAWRkYXRheCxBNkFKQWZSSnlLdU5vTnZ0MzN5Z1lmWWg2T0lZQTh0RjFzMkJRY1JBOVJOZ2R0eXBlAWlpc0VuYWJsZWT1\"\n}",
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
      "code": "grpcurl -proto protos/platform.proto -plaintext \\\n  -d '{\n    \"id\":\"77w8Xqn25HwJhjodrHW133aXhjuTsTv9ozQaYpSHACE3\"\n    }' \\\n  evonet.thephez.com:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDataContract",
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
      "code": "{\n  \"dataContract\": \"pGckc2NoZW1heDRodHRwczovL3NjaGVtYS5kYXNoLm9yZy9kcHAtMC00LTAvbWV0YS9kYXRhLWNvbnRyYWN0Z3ZlcnNpb24BaWRvY3VtZW50c6JmZG9tYWlupGdpbmRpY2Vzg6JmdW5pcXVl9Wpwcm9wZXJ0aWVzgaFobmFtZUhhc2hjYXNjoWpwcm9wZXJ0aWVzgqF4Gm5vcm1hbGl6ZWRQYXJlbnREb21haW5OYW1lY2FzY6Fvbm9ybWFsaXplZExhYmVsY2FzY6FqcHJvcGVydGllc4GhdHJlY29yZHMuZGFzaElkZW50aXR5Y2FzY2hyZXF1aXJlZIZobmFtZUhhc2hlbGFiZWxvbm9ybWFsaXplZExhYmVseBpub3JtYWxpemVkUGFyZW50RG9tYWluTmFtZWxwcmVvcmRlclNhbHRncmVjb3Jkc2pwcm9wZXJ0aWVzpmVsYWJlbKRkdHlwZWZzdHJpbmdncGF0dGVybngmXigoPyEtKVthLXpBLVowLTktXXswLDYyfVthLXpBLVowLTldKSRpbWF4TGVuZ3RoGD9rZGVzY3JpcHRpb254GURvbWFpbiBsYWJlbC4gZS5nLiAnVXNlUidncmVjb3Jkc6RkdHlwZWZvYmplY3RqcHJvcGVydGllc6FsZGFzaElkZW50aXR5pWR0eXBlZnN0cmluZ2dwYXR0ZXJueD9eWzEyMzQ1Njc4OUFCQ0RFRkdISktMTU5QUVJTVFVWV1hZWmFiY2RlZmdoaWprbW5vcHFyc3R1dnd4eXpdKyRpbWF4TGVuZ3RoGCxpbWluTGVuZ3RoGCprZGVzY3JpcHRpb254GWJhc2U1OCBpZGVudGl0eSBpZCBzdHJpbmdtbWluUHJvcGVydGllcwF0YWRkaXRpb25hbFByb3BlcnRpZXP0aG5hbWVIYXNopWR0eXBlZnN0cmluZ2dwYXR0ZXJubl5bQS1GYS1mMC05XSskaW1heExlbmd0aBhEaW1pbkxlbmd0aBhEa2Rlc2NyaXB0aW9ueEpEb3VibGUgc2hhLTI1NiBtdWx0aWhhc2ggb2YgdGhlIGZ1bGwgZG9tYWluIG5hbWUgaW4gYSBmb3JtIG9mIGEgaGV4IHN0cmluZ2xwcmVvcmRlclNhbHSlZHR5cGVmc3RyaW5nZ3BhdHRlcm54P15bMTIzNDU2Nzg5QUJDREVGR0hKS0xNTlBRUlNUVVZXWFlaYWJjZGVmZ2hpamttbm9wcXJzdHV2d3h5el0rJGltYXhMZW5ndGgYImltaW5MZW5ndGgYGWtkZXNjcmlwdGlvbnhKRG9tYWluIHByZS1vcmRlciBzYWx0LiBDdXJyZW50bHkgcmFuZG9tbHkgZ2VuZXJhdGVkIGJhc2U1OCBhZGRyZXNzIHN0cmluZy5vbm9ybWFsaXplZExhYmVspGR0eXBlZnN0cmluZ2dwYXR0ZXJueCBeKCg/IS0pW2EtejAtOS1dezAsNjJ9W2EtejAtOV0pJGltYXhMZW5ndGgYP2tkZXNjcmlwdGlvbngpRG9tYWluIGxhYmVsIGluIGEgbG93ZXIgY2FzZS4gZS5nLiAndXNlcid4Gm5vcm1hbGl6ZWRQYXJlbnREb21haW5OYW1lpGR0eXBlZnN0cmluZ2ltYXhMZW5ndGgYvmltaW5MZW5ndGgAa2Rlc2NyaXB0aW9ueDhBIGZ1bGwgcGFyZW50IGRvbWFpbiBuYW1lIGluIGxvd2VyIGNhc2UuIGUuZy4gJ2Rhc2gub3JnJ3RhZGRpdGlvbmFsUHJvcGVydGllc/RocHJlb3JkZXKkZ2luZGljZXOBomZ1bmlxdWX1anByb3BlcnRpZXOBoXBzYWx0ZWREb21haW5IYXNoY2FzY2hyZXF1aXJlZIFwc2FsdGVkRG9tYWluSGFzaGpwcm9wZXJ0aWVzoXBzYWx0ZWREb21haW5IYXNopWR0eXBlZnN0cmluZ2dwYXR0ZXJubl5bQS1GYS1mMC05XSskaW1heExlbmd0aBhEaW1pbkxlbmd0aBhEa2Rlc2NyaXB0aW9ueFFEb3VibGUgc2hhLTI1NiBtdWx0aWhhc2ggb2YgdGhlIGZ1bGwgZG9tYWluIG5hbWUgKyBzYWx0IGluIGEgZm9ybSBvZiBhIGhleCBzdHJpbmd0YWRkaXRpb25hbFByb3BlcnRpZXP0amNvbnRyYWN0SWR4LDc3dzhYcW4yNUh3Smhqb2RySFcxMzNhWGhqdVRzVHY5b3pRYVlwU0hBQ0Uz\"\n}\n",
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
      "code": "# Request one DPNS domain document\ngrpcurl -proto protos/platform.proto -plaintext \\\n  -d '{\n    \"data_contract_id\":\"77w8Xqn25HwJhjodrHW133aXhjuTsTv9ozQaYpSHACE3\",\n    \"document_type\":\"domain\",\n    \"limit\":1\n    }' \\\n  evonet.thephez.com:3010 \\\n  org.dash.platform.dapi.v0.Platform/getDocuments",
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
      "code": "{\n  \"documents\": [\n    \"q2QkcmV2AWUkdHlwZWZkb21haW5lbGFiZWxkZGFzaGckdXNlcklkeCw3N3c4WHFuMjVId0poam9kckhXMTMzYVhoanVUc1R2OW96UWFZcFNIQUNFM2dyZWNvcmRzoWxkYXNoSWRlbnRpdHl4LDc3dzhYcW4yNUh3Smhqb2RySFcxMzNhWGhqdVRzVHY5b3pRYVlwU0hBQ0UzaCRlbnRyb3B5eCJ5YVpRM1J1eHlKN2o3blZRN1g4RmdmVDdzTjZNakVMU2ZOaG5hbWVIYXNoeEQ1NjIwNjBmMDgzMzkzMmEyMTQ0NmFkYTliMGJiNzFhYzhlOGI0MGUyNjE4Zjk5ZjQ0MjA0ZDY2ODE1ZjZiZGYyNThjY2skY29udHJhY3RJZHgsNzd3OFhxbjI1SHdKaGpvZHJIVzEzM2FYaGp1VHNUdjlvelFhWXBTSEFDRTNscHJlb3JkZXJTYWx0eCJ5YnNhNFlOaUU5dW94Q3BwQmM4YVRranJzUTFXeTdOQ0szb25vcm1hbGl6ZWRMYWJlbGRkYXNoeBpub3JtYWxpemVkUGFyZW50RG9tYWluTmFtZWA=\"\n  ]\n}",
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