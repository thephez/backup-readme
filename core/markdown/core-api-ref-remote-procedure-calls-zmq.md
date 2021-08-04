# GetZmqNotifications
[block:callout]
{
  "type": "success",
  "body": "Added in Dash Core 0.17.0"
}
[/block]
The [`getzmqnotifications` RPC](core-api-ref-remote-procedure-calls-blockchain#getblockchaininfo) returns information about the active ZeroMQ notifications.

*Parameters: none*

*Result---A JSON array of objects providing information about the enabled ZMQ notifications*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | Array of objects containing Information about the enabled ZMQ notifications
→<br>Notification | object | Required<br>(0 or more) | The name of the block chain. Either `main` for mainnet, `test` for testnet, `regtest` for regtest, or `devnet-<name>` for devnets
→ →<br>`type` | string | Required<br>(exactly 1) | Type of notification
→ →<br>`address` | string | Required<br>(exactly 1) | Address of the publisher

*Example from Dash Core 0.17.0*

``` bash
dash-cli -testnet getzmqnotifications
```

Result:

``` json
[
  {
    "type": "pubhashblock",
    "address": "tcp://0.0.0.0:20009"
  },
  {
    "type": "pubhashchainlock",
    "address": "tcp://0.0.0.0:20009"
  },
  {
    "type": "pubhashgovernanceobject",
    "address": "tcp://0.0.0.0:20009"
  }
]
```

*See also: none*