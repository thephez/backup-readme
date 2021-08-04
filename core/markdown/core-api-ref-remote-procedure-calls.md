# Overview

Dash Core provides a remote procedure call (RPC) interface for various administrative tasks, <<glossary:wallet>> operations, and queries about <<glossary:network>> and <<glossary:block chain>> data.

Open-source client libraries for the RPC interface are readily available in most modern programming languages, so you probably don't need to write your own from scratch. Dash Core also ships with its own compiled C++ RPC client, `dash-cli`, located in the `bin` directory alongside `dashd` and `dash-qt`. The `dash-cli` program can be used as a command-line interface (CLI) to Dash Core or for making RPC calls from applications written in languages lacking a suitable native client. The remainder of this section describes the Dash Core RPC protocol in detail.
[block:callout]
{
  "type": "info",
  "body": "The following subsections reference setting configuration values. See the [Examples Page](core-examples-introduction) for more information about setting Dash Core configuration values.",
  "title": "Dash Core Configuration"
}
[/block]
## Enabling RPC

If you start Dash Core using `dash-qt`, the RPC interface is disabled by default. To enable it, set `server=1` in `dash.conf` or supply the `-server` argument when invoking the program. If you start Dash Core using `dashd`, the RPC interface is enabled by default.

## Basic Security

The interface requires the user to provide a password for authenticating RPC requests. This password can be set either using the `rpcpassword` property in `dash.conf` or by supplying the `-rpcpassword` program argument. Optionally a username can be set using the `rpcuser` configuration value. 

## RPC-Auth Security

Alternatively, the authentication details can be provided using the `rpcauth` property. This removes the need to include a plaintext password in the dash.conf file by instead including a salt and hash of the password along with a username in the format:
`<USERNAME>:<SALT>$<HASH>`

``` shell
# Example dash.conf rpcauth entry
rpcauth=myuser:933fff1aaefa1fc5b3e981fd3ceacf03$f799757c0d36be8f1faa1dd3a01562b17ada82f2ff6c968c959103afda9e7c6f
```
[block:callout]
{
  "type": "info",
  "body": "The `rpcauth` option can be specified multiple times if multiple users are required."
}
[/block]
A canonical python script is included in Dash Core's repository under [share/rpcuser](https://github.com/dashpay/dash/tree/master/share/rpcauth) to generate the information required for the dash.conf file as well as the password required by clients using the rpcauth name.
[block:code]
{
  "codes": [
    {
      "code": "String to be appended to dash.conf:\nrpcauth=myuser:b87393f6957f80448f8a0aba5eb8cc00$f67a3321106b13acc2a8881c9eb64e7bbc6eeb4681261b2918cc54da8915be6e\nYour password:\n2-Cl0O92-MT-XavyEIkkV_hxqdC_7fag8w7EF7t3UVg=\n",
      "language": "text",
      "name": "Example output when running './rpcauth.py myuser'"
    }
  ]
}
[/block]
## Restricted Access Users
[block:callout]
{
  "type": "warning",
  "body": "This feature is only available on masternodes"
}
[/block]
As of Dash Core 0.17.0, an option is provided to add an RPC user that is restricted to a small subset of RPCs that will be used by Dash Platform. The `platform-user` configuration value sets the name of the RPC user to be restricted.

The `platform-user` configuration value must be set to a previously configured [rpcauth user](#rpc-auth-security).

Only the following RPCs are accessible to the restricted user:
- [`getbestblockhash`](core-api-ref-remote-procedure-calls-blockchain#getbestblockhash)
- [`getblockhash`](core-api-ref-remote-procedure-calls-blockchain#getblockhash)
- [`getblockcount`](core-api-ref-remote-procedure-calls-blockchain#getblockcount)
- [`getbestchainlock`](core-api-ref-remote-procedure-calls-blockchain#getbestchainlock)
- [`quorum sign 4`](core-api-ref-remote-procedure-calls-evo#quorum-sign) - The restricted user can only request quorum signatures from the Platform quorum (LLMQ type 4)
- [`quorum verify`](core-api-ref-remote-procedure-calls-evo#quorum-verify)
- [`verifyislock`](core-api-ref-remote-procedure-calls-evo#verifyislock)

 ## Default Connection Info

The Dash Core RPC service listens for HTTP `POST` requests on port 9998 in <<glossary:mainnet>> mode, 19998 in <<glossary:testnet>>, or 19898 in <<glossary:regression test mode>>. The port number can be changed by setting `rpcport` in `dash.conf`. By default the RPC service binds to your server's [localhost](https://en.wikipedia.org/wiki/Localhost) loopback network interface so it's not accessible from other servers. Authentication is implemented using [HTTP basic authentication](https://en.wikipedia.org/wiki/Basic_access_authentication). RPC HTTP requests must include a `Content-Type` header set to `text/plain` and a `Content-Length` header set to the size of the request body.

# Data Formats

The format of the request body and response data is based on [version 1.0 of the JSON-RPC specification](http://json-rpc.org/wiki/specification). 

## Request Format

Specifically, the HTTP `POST` data of a request must be a JSON object with the following format:

| Name                 | Type            | Presence                    | Description
|----------------------|-----------------|-----------------------------|----------------
| Request              | object          | Required<br>(exactly 1)     | The JSON-RPC request object
| → <br>`jsonrpc`      | number (real)   | Optional<br>(0 or 1)        | Version indicator for the JSON-RPC request. Currently ignored by Dash Core.
| → <br>`id`           | string          | Optional<br>(0 or 1)        | An arbitrary string that will be returned with the response.  May be omitted or set to an empty string ("")
| → <br>`method`       | string          | Required<br>(exactly 1)     | The RPC method name (e.g. `getblock`).  See the RPC section for a list of available methods.
| → <br>`params`       | array           | Optional<br>(0 or 1)        | An array containing positional parameter values for the RPC.  May be an empty array or omitted for RPC calls that don't have any required parameters.
| → <br>`params`       | object           | Optional<br>(0 or 1)        | Starting from Dash Core 0.12.3 / Bitcoin Core 0.14.0 (replaces the params array above) An object containing named parameter values for the RPC. May be an empty object or omitted for RPC calls that don’t have any required parameters.
| → → <br>Parameter    | *any*           | Optional<br>(0 or more)       | A parameter.  May be any JSON type allowed by the particular RPC method

In the table above and in other tables describing RPC input and output, we use the following conventions

* "→" indicates an argument that is the child of a JSON array or JSON object. For example, "→ → Parameter" above means Parameter is the child of the `params` array which itself is a child of the Request object.

* Plain-text names like "Request" are unnamed in the actual JSON object

* Code-style names like `params` are literal strings that appear in the JSON object.

* "Type" is the JSON data type and the specific Dash Core type.

* "Presence" indicates whether or not a field must be present within its containing array or object. Note that an optional object may still have required children.

## Response Format

The HTTP response data for a RPC request is a JSON object with the following format:

| Name                 | Type            | Presence                    | Description
|----------------------|-----------------|-----------------------------|----------------
| Response             | object          | Required<br>(exactly 1)     | The JSON-RPC response object.
| → <br>`result`       | *any*           | Required<br>(exactly 1)     | The RPC output whose type varies by call.  Has value `null` if an error occurred.
| → <br>`error`        | null/object     | Required<br>(exactly 1)     | An object describing the error if one occurred, otherwise `null`.
| → → <br>`code`        | number (int)    | Required<br>(exactly 1)     | The error code returned by the RPC function call. See [rpcprotocol.h](https://github.com/dashpay/dash/blob/v0.15.x/src/rpc/protocol.h) for a full list of error codes and their meanings.
| → → <br>`message`     | string          | Required<br>(exactly 1)     | A text description of the error.  May be an empty string ("").
| → <br>`id`           | string          | Required<br>(exactly 1)     | The value of `id` provided with the request. Has value `null` if the `id` field was omitted in the request.

# Example

As an example, here is the JSON-RPC request object for the hash of the <<glossary:genesis block>>:

```json
{
    "method": "getblockhash",
    "params": [0],
    "id": "foo"
}
```

The command to send this request using `dash-cli` is:
[block:code]
{
  "codes": [
    {
      "code": "dash-cli getblockhash 0",
      "language": "shell"
    }
  ]
}
[/block]
The command to send this request using `dash-cli` with named parameters is:
[block:code]
{
  "codes": [
    {
      "code": "dash-cli -named getblockhash height=0",
      "language": "shell"
    }
  ]
}
[/block]
Alternatively, we could `POST` this request using the cURL command-line program as follows:
[block:code]
{
  "codes": [
    {
      "code": "curl --user 'my_username:my_secret_password' --data-binary '''\n  {\n      \"method\": \"getblockhash\",\n      \"params\": [0],\n      \"id\": \"foo\"\n  }''' \\\n  --header 'Content-Type: text/plain;' localhost:9998",
      "language": "shell"
    }
  ]
}
[/block]
The HTTP response data for this request would be:

```json
{
    "result": "00000bafbc94add76cb75e2ec92894837288a481e5c005f6563d91623bf8bc2c",
    "error": null,
    "id": "foo"
}
```
[block:callout]
{
  "type": "info",
  "body": "Note: In order to minimize its size, the raw JSON response from Dash Core doesn't include any extraneous whitespace characters."
}
[/block]
Here whitespace has been added to make the object more readable. `dash-cli` also transforms the raw response to make it more human-readable. It:

- Adds whitespace indentation to JSON objects
- Expands escaped newline characters ("\n") into actual newlines
- Returns only the value of the `result` field if there's no error
- Strips the outer double-quotes around `result`s of type string
- Returns only the `error` field if there's an error

Continuing with the example above, the output from the `dash-cli` command would be simply:

```text
00000bafbc94add76cb75e2ec92894837288a481e5c005f6563d91623bf8bc2c
```

## RPCs with sub-commands

Dash Core has a number of RPC requests that use sub-commands to group access to related data under one RPC method name. Examples of this include the [`gobject`](core-api-ref-remote-procedure-calls-dash#gobject), [`masternode`](core-api-ref-remote-procedure-calls-dash#masternode), [`protx`](core-api-ref-remote-procedure-calls-evo#protx), and [`quorum`](core-api-ref-remote-procedure-calls-evo#quorum) RPCs. If using cURL, the sub-commands should be included in the requests `params` field as shown here:
[block:code]
{
  "codes": [
    {
      "code": "curl --user 'my_username:my_secret_password' --data-binary '''\n  {\n      \"method\": \"gobject\",\n      \"params\": [\"list\", \"valid\", \"proposals\"],\n      \"id\": \"foo\"\n  }''' \\\n  --header 'Content-Type: text/plain;' localhost:9998",
      "language": "shell"
    }
  ]
}
[/block]
## Error Handling

If there's an error processing a request, Dash Core sets the `result` field to `null` and provides information about the error in the  `error` field. For example, a request for the block hash at block height -1 would be met with the following response (again, whitespace added for clarity):

```json
{
    "result": null,
    "error": {
        "code": -8,
        "message": "Block height out of range"
    },
    "id": "foo"
}
```

If `dash-cli` encounters an error, it exits with a non-zero status code and outputs the `error` field as text to the process's standard error stream:

```text
error code: -8
error message:
Block height out of range
```

## Batch Requests

The RPC interface supports request batching as described in [version 2.0 of the JSON-RPC specification](http://www.jsonrpc.org/specification#batch). To initiate multiple RPC requests within a single HTTP request, a client can `POST` a JSON array filled with Request objects. The HTTP response data is then a JSON array filled with the corresponding Response objects. Depending on your usage pattern, request batching may provide significant performance gains. The `dash-cli` RPC client does not support batch requests.
[block:code]
{
  "codes": [
    {
      "code": "curl --user 'my_username:my_secret_password' --data-binary '''\n  [\n    {\n      \"method\": \"getblockhash\",\n      \"params\": [0],\n      \"id\": \"foo\"\n    },\n    {\n      \"method\": \"getblockhash\",\n      \"params\": [1],\n      \"id\": \"foo2\"\n    }\n  ]''' \\\n  --header 'Content-Type: text/plain;' localhost:9998",
      "language": "shell"
    }
  ]
}
[/block]
To keep this documentation compact and readable, the examples for each of the available RPC calls will be given as `dash-cli` commands:

```shell
dash-cli [options] <method name> <param1> <param2> ...
```

This translates into an JSON-RPC Request object of the form:

```json
{
    "method": "<method name>",
    "params": [ "<param1>", "<param2>", "..." ],
    "id": "foo"
}
```
[block:callout]
{
  "type": "warning",
  "body": "**Warning:** if you write programs using the JSON-RPC interface, you must ensure they handle high-precision real numbers correctly.  See the [Proper Money Handling](https://en.bitcoin.it/wiki/Proper_Money_Handling_%28JSON-RPC%29) Bitcoin Wiki article for details and example code.",
  "title": "High-precision real numbers"
}
[/block]