# Debug

The [`debug` RPC](core-api-ref-remote-procedure-calls-control#debug) changes the debug category from the console.

*Parameter #1---debug category*

Name | Type | Presence | Description
--- | --- | --- | ---
Debug category | string | Required<br>(1 or more) | The debug category to activate. Use a `+` to specify multiple categories. Categories will be one of the following:<br>• `0` - Disables all categories <br>• `1` or `all` - Enables all categories <br>• `addrman` <br>• `bench` <br>• `cmpctblock` <br>• `coindb` <br>• `db` <br>• `estimatefee` <br>• `http` <br>• `leveldb` <br>• `libevent` <br>• `mempool` <br>• `mempoolrej` <br>• `net` <br>• `proxy` <br>• `prune` <br>• `qt` <br>• `rand` <br>• `reindex` <br>• `rpc` <br>• `selectcoins` <br>• `tor` <br>• `zmq` <br>• `dash` (all subcategories)<br><br>The `dash` sub-categories can be enabled individually:<br>• `chainlocks` <br>• `gobject` <br>• `instantsend` <br>• `keepass` <br>• `llmq` <br>• `llmq-dkg` <br>• `llmq-sigs` <br>• `mnpayments` <br>• `mnsync` <br>• `privatesend` <br>• `spork` <br><br><br>Note: No error will be thrown even if the specified category doesn't match any of the above

*Example from Dash Core 0.15.0*

``` bash
dash-cli -testnet debug "net+mempool"
```

Result:

``` text
Debug mode: net+mempool
```

*See also*

* [Logging](/docs/core-api-ref-remote-procedure-calls-control#logging): gets and sets the logging configuration

# GetMemoryInfo

*Added in Dash Core 0.12.3 / Bitcoin Core 0.14.0*

The [`getmemoryinfo` RPC](core-api-ref-remote-procedure-calls-control#getmemoryinfo) returns information about memory usage.

*Parameter #1---mode*

Name | Type | Presence | Description
--- | --- | --- | ---
mode| string | Optional<br>Default: `stats` | *Added in Dash Core 0.15.0*<br><br>Determines what kind of information is returned.<br>- `stats` returns general statistics about memory usage in the daemon.<br>- `mallocinfo` returns an XML string describing low-level heap state (only available if compiled with glibc 2.10+).

*Result---information about memory usage*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object containing information about memory usage
→<br>`locked` | string : object | Required<br>(exactly 1) | An object containing information about locked memory manager
→→<br>`used` | number (int) | Required<br>(exactly 1) | Number of bytes used
→→<br>`free` | number (int) | Required<br>(exactly 1) | Number of bytes available in current arenas
→→<br>`total` | number (int) | Required<br>(exactly 1) | Total number of bytes managed
→→<br>`locked` | number (int) | Required<br>(exactly 1) | Amount of bytes that succeeded locking
→→<br>`chunks_used` | number (int) | Required<br>(exactly 1) | Number allocated chunks
→→<br>`chunks_free` | number (int) | Required<br>(exactly 1) | Number unused chunks

*Example from Dash Core 0.12.3*

``` bash
dash-cli getmemoryinfo
```

Result:

``` json
{
  "locked": {
    "used": 1146240,
    "free": 426624,
    "total": 1572864,
    "locked": 1572864,
    "chunks_used": 16368,
    "chunks_free": 7
  }
}
```

*See also*

* [GetMemPoolInfo](/docs/core-api-ref-remote-procedure-calls-blockchain#getmempoolinfo): returns information about the node's current transaction memory pool.

# Help

The [`help` RPC](core-api-ref-remote-procedure-calls-control#help) lists all available public RPC commands, or gets help for the specified RPC.  Commands which are unavailable will not be listed, such as wallet RPCs if wallet support is disabled.

*Parameter---the name of the RPC to get help for*

Name | Type | Presence | Description
--- | --- | --- | ---
RPC | string | Optional<br>(0 or 1) | The name of the RPC to get help for.  If omitted, Dash Core 0.10x will display an alphabetical list of commands; Dash Core 0.11.0 will display a categorized list of commands

*Result---a list of RPCs or detailed help for a specific RPC*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | The help text for the specified RPC or the list of commands.  The `dash-cli` command will parse this text and format it as human-readable text

*Example from Dash Core 0.12.2*

Command to get help about the [`help` RPC](core-api-ref-remote-procedure-calls-control#help):

``` bash
dash-cli -testnet help help
```

Result:

``` text
help ( "command" )

List all commands, or get help for a specified command.

Arguments:
1. "command"     (string, optional) The command to get help on

Result:
"text"     (string) The help text

```

*See also*

* The [RPC Quick Reference](core-api-ref-remote-procedure-call-quick-reference)

# Logging

The [`logging` RPC](core-api-ref-remote-procedure-calls-control#logging) gets and sets the logging configuration

*Parameter #1---include categories*

Name | Type | Presence | Description
--- | --- | --- | ---
`include` | array of strings | Optional<br>(0 or 1) | Enable debugging for these categories

*Parameter #2---exclude categories*

Name | Type | Presence | Description
--- | --- | --- | ---
`exclude` | array of strings | Optional<br>(0 or 1) | Enable debugging for these categories

The categories are:

| Type | Category |
| - | - |
| Special | • `0` - Disables all categories <br>• `1` or `all` - Enables all categories <br>• `dash` - Enables/disables all Dash categories |
| Standard | `addrman`, `bench` <br>`cmpctblock`, `coindb`, `db`, `estimatefee`, `http`, `leveldb`, `libevent`, `mempool`, `mempoolrej`, `net`, `proxy`, `prune`, `qt`, `rand`, `reindex`, `rpc`, `selectcoins`, `tor`, `zmq`|
| Dash | <br>`chainlocks`, `gobject`, `instantsend`, `keepass`, `llmq`, `llmq-dkg`, `llmq-sigs`, `mnpayments`, `mnsync`, `privatesend`, `spork` |

*Result---a list of the logging categories that are active*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | A JSON object show a list of the logging categories that are active

*Example from Dash Core 0.15.0*

Include a category in logging

``` bash
dash-cli -testnet logging '["llmq", "spork"]'
```

Result:
``` json
{
  "net": 0,
  "tor": 0,
  "mempool": 0,
  "http": 0,
  "bench": 0,
  "zmq": 0,
  "db": 0,
  "rpc": 0,
  "estimatefee": 0,
  "addrman": 0,
  "selectcoins": 0,
  "reindex": 0,
  "cmpctblock": 0,
  "rand": 0,
  "prune": 0,
  "proxy": 0,
  "mempoolrej": 0,
  "libevent": 0,
  "coindb": 0,
  "qt": 0,
  "leveldb": 0,
  "chainlocks": 0,
  "gobject": 0,
  "instantsend": 0,
  "keepass": 0,
  "llmq": 1,
  "llmq-dkg": 0,
  "llmq-sigs": 0,
  "mnpayments": 0,
  "mnsync": 0,
  "privatesend": 0,
  "spork": 1
}
```

Excluding a previously included category (without including any new ones):

``` bash
dash-cli -testnet logging '[]' '["spork"]'
```

Result:
``` json
{
  "net": 0,
  "tor": 0,
  "mempool": 0,
  "http": 0,
  "bench": 0,
  "zmq": 0,
  "db": 0,
  "rpc": 0,
  "estimatefee": 0,
  "addrman": 0,
  "selectcoins": 0,
  "reindex": 0,
  "cmpctblock": 0,
  "rand": 0,
  "prune": 0,
  "proxy": 0,
  "mempoolrej": 0,
  "libevent": 0,
  "coindb": 0,
  "qt": 0,
  "leveldb": 0,
  "chainlocks": 0,
  "gobject": 0,
  "instantsend": 0,
  "keepass": 0,
  "llmq": 1,
  "llmq-dkg": 0,
  "llmq-sigs": 0,
  "mnpayments": 0,
  "mnsync": 0,
  "privatesend": 0,
  "spork": 0
}
```

*See also*

* [Debug](/docs/core-api-ref-remote-procedure-calls-control#debug): changes the debug category from the console.

# Stop

The [`stop` RPC](core-api-ref-remote-procedure-calls-control#stop) safely shuts down the Dash Core server.

*Parameters: none*

*Result---the server is safely shut down*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | The string \Dash Core server stopping\""

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet stop
```

Result:

``` text
Dash Core server stopping
```

*See also: none*

# Uptime

The [`uptime` RPC](core-api-ref-remote-procedure-calls-control#uptime) returns the total uptime of the server.

*Parameters: none*

*Result*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (int) | Required<br>(exactly 1) | The number of seconds that the server has been running

*Example from Dash Core 0.15.0*

``` bash
dash-cli -testnet uptime
```

Result:
``` text
5500
```

*See also: none*