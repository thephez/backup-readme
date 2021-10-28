# EstimatePriority
[block:callout]
{
  "type": "danger",
  "body": "**Warning:** **_Removed in Dash Core 0.14.0._**"
}
[/block]
The [`estimatepriority` RPC](core-api-ref-remote-procedure-calls-removed#estimatepriority) was removed in Dash Core 0.14.0. This should not to be confused with the [`prioritisetransaction` RPC](core-api-ref-remote-procedure-calls-mining#prioritisetransaction) which will remain supported for adding fee deltas to transactions.

*Parameter #1---how many blocks the transaction may wait before being included as a free high-priority transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
Blocks | number (int) | Required<br>(exactly 1) | The maximum number of blocks a transaction should have to wait before it is predicted to be included in a block based purely on its priority

*Result---the priority a transaction needs*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (real) | Required<br>(exactly 1) | The estimated priority the transaction should have in order to be included within the specified number of blocks.  If the node doesn't have enough information to make an estimate, the value `-1` will be returned

*Examples from Dash Core 0.12.2*

``` bash
dash-cli estimatepriority 6
```

Result:

``` json
718158904.10958910
```

Requesting data the node can't calculate yet:

``` bash
dash-cli estimatepriority 100
```

Result:

``` json
-1
```

*See also*

* [EstimateFee](/docs/core-api-ref-remote-procedure-calls-util#estimatefee): estimates the transaction fee per kilobyte that needs to be paid for a transaction to begin confirmation within a certain number of blocks.

# EstimateSmartPriority
[block:callout]
{
  "type": "danger",
  "body": "**Warning:** **_Removed in Dash Core 0.14.0._**"
}
[/block]
The [`estimatesmartpriority` RPC](core-api-ref-remote-procedure-calls-removed#estimatesmartpriority) was removed in Dash Core 0.14.0. This should not to be confused with the [`prioritisetransaction` RPC](core-api-ref-remote-procedure-calls-mining#prioritisetransaction) which will remain supported for adding fee deltas to transactions.

*Parameter #1---how many blocks the transaction may wait before being included as a free high-priority transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
Blocks | number (int) | Required<br>(exactly 1) | The maximum number of blocks a transaction should have to wait before it is predicted to be included in a block based purely on its priority

*Result---the priority a transaction needs*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | JSON Object containing estimate information
→<br>`priority` | number | Required<br>(exactly 1) | The estimated priority the transaction should be in order to be included within the specified number of blocks.  If the node doesn't have enough information to make an estimate, the value `-1` will be returned
→<br>`blocks` | number | Required<br>(exactly 1) | Block number where the estimate was found

*Examples from Dash Core 0.12.2*

``` bash
dash-cli estimatesmartpriority 6
```

Result:

``` json
{
  "priority": 718158904
  "blocks": 25
}
```

Requesting data the node can't calculate yet:

``` bash
dash-cli estimatesmartpriority 100
```

Result:

``` json
{
  "priority": -1,
  "blocks": 100
}
```

*See also*

* [EstimatePriority](/docs/core-api-ref-remote-procedure-calls-removed#estimatepriority): was removed in Dash Core 0.14.0.

# GetPoolInfo

The [`getpoolinfo` RPC](#getpoolinfo) returns an object containing CoinJoin pool related information.

*Parameters: none*

*Result---information about the CoinJoin pool*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | Information about the pool
→<br>`state` | string | Required<br>(exactly 1) | Pool state.  Will be one of the following:<br>• `IDLE` <br>• `QUEUE` <br>• `ACCEPTING_ENTRIES` <br>• `SIGNING` <br>• `ERROR` <br>• `SUCCESS` <br>• `UNKNOWN` <br>
→<br>`mixing_mode` | string | Required<br>(exactly 1) | One of the following:<br>• `normal` <br>• `multi-session` <br>
→<br>`queue` | number (int) | Required<br>(exactly 1) | Queue size
→<br>`entries` | number (int) | Required<br>(exactly 1) | The number of entries
→<br>`status` | string | Required<br>(exactly 1) | A more detailed description of the current state
→<br>`outpoint` | string (hex) | Optional<br>(exactly 1) | Previous output
→<br>`addr` | string | Optional<br>(exactly 1) | Address
→<br>`keys_left` | number (int) | Optional<br>(exactly 1) | The number of keys left in the local wallet
→<br>`warnings` | number (int) | Optional<br>(exactly 1) | Warnings related to local wallet

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet getpoolinfo
```

Result:
``` json
{
  "state": "IDLE",
  "mixing_mode": "normal",
  "queue": 0,
  "entries": 0,
  "status": "PrivateSend is idle.",
  "keys_left": 617,
  "warnings": ""
}
```

``` json
{
  "state": "QUEUE",
  "mixing_mode": "normal",
  "queue": 1,
  "entries": 0,
  "status": "Submitted to masternode, waiting in queue .",
  "outpoint": "e3a6b7878a7e9413898bb379b323c521676f9d460db17ec3bf42d9ac0c9a432f-1",
  "addr": "217.182.229.146:19999",
  "keys_left": 571,
  "warnings": ""
}
```

``` json
{
  "state": "ERROR",
  "mixing_mode": "normal",
  "queue": 0,
  "entries": 0,
  "status": "PrivateSend request incomplete: Session timed out. Will retry...",
  "keys_left": 571,
  "warnings": ""
}
```

*See also:*

# GetReceivedByAccount
[block:callout]
{
  "type": "danger",
  "body": "Warning: Removed in Dash Core 0.18.0."
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]
The [`getreceivedbyaccount` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#getreceivedbyaccount) returns the total amount received by addresses in a particular account from transactions with the specified number of confirmations.  It does not count coinbase transactions.

*Parameter #1---the account name*

Name | Type | Presence | Description
--- | --- | --- | ---
Account | string | Required<br>(exactly 1) | The name of the account containing the addresses to get.  For the default account, use an empty string (\\")"

*Parameter #2---the minimum number of confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations an externally-generated transaction must have before it is counted towards the balance.  Transactions generated by this node are counted immediately.  Typically, externally-generated transactions are payments to this wallet and transactions generated by this node are payments to other wallets.  Use `0` to count unconfirmed transactions.  Default is `1`

*Parameter #3---whether to include transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(exactly 1) | Add the balance from InstantSend locked transactions (default=false)

*Result---the number of dash received*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (dash) | Required<br>(exactly 1) | The number of dash received by the account.  May be `0`

*Example from Dash Core 0.12.2*

Get the dash received by the "doc test" account with six or more
confirmations:

``` bash
dash-cli -testnet getreceivedbyaccount "doc test" 6
```

Result:

``` json
0.30000000
```

*See also*

* [GetReceivedByAddress](/docs/core-api-ref-remote-procedure-calls-wallet#getreceivedbyaddress): returns the total amount received by the specified address in transactions with the specified number of confirmations. It does not count coinbase transactions.
* [GetAddressesByAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getaddressesbyaccount): returns a list of every address assigned to a particular account.
* [ListAccounts](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#listaccounts): lists accounts and their balances.


# ListAccounts
[block:callout]
{
  "type": "danger",
  "body": "Warning: Removed in Dash Core 0.18.0."
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]
The [`listaccounts` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#listaccounts) lists accounts and their balances.

*Parameter #1---the minimum number of confirmations a transaction must have*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations an externally-generated transaction must have before it is counted towards the balance.  Transactions generated by this node are counted immediately.  Typically, externally-generated transactions are payments to this wallet and transactions generated by this node are payments to other wallets.  Use `0` to count unconfirmed transactions.  Default is `1`

*Parameter #2--- whether to include transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(exactly 1) | Add the balance from InstantSend locked transactions

*Parameter #3---whether to include watch-only addresses in results*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Watch-Only | bool | Optional<br>(0 or 1) | If set to `true`, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.  If set to `false` (the default), treat watch-only addresses as if they didn't belong to this wallet

*Result---a list of accounts and their balances*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | A JSON array containing key/value pairs with account names and values.  Must include, at the very least, the default account (\\")"
→<br>Account : Balance | string : number (dash) | Required<br>(1 or more) | The name of an account as a string paired with the balance of the account as a number of dash.  The number of dash may be negative if the account has spent more dash than it received.  Accounts with zero balances and zero transactions will be displayed

*Example from Dash Core 0.13.0*

Display account balances with one confirmation and watch-only addresses
included. Add the balance of InstantSend locked transactions also.

``` bash
dash-cli -testnet listaccounts 1 true true
```

Result:

``` json
{
  "": -2941.30029732,
  "Watching": 8.50000000,
  "MN": 2000.25442744,
  "PS": 37.02970000,
  "Recv1": 3843.48167912,
}
```

*See also*

* [GetAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getaccount): returns the name of the account associated with the given address.
* [GetAddressesByAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getaddressesbyaccount): returns a list of every address assigned to a particular account.
* [ListReceivedByAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#listreceivedbyaccount): lists the total number of dash received by each account.
* [ListReceivedByLabel](core-api-ref-remote-procedure-calls-wallet#listreceivedbylabel): lists the total number of dash received by each label.

# ListReceivedByAccount
[block:callout]
{
  "type": "danger",
  "body": "Warning: Removed in Dash Core 0.18.0."
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]
The [`listreceivedbyaccount` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#listreceivedbyaccount) lists the total number of dash received by each account.

*Parameter #1---the minimum number of confirmations a transaction must have to be counted*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations an externally-generated transaction must have before it is counted towards the balance.  Transactions generated by this node are counted immediately.  Typically, externally-generated transactions are payments to this wallet and transactions generated by this node are payments to other wallets.  Use `0` to count unconfirmed transactions.  Default is `1`

*Parameter #2---whether to include transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(exactly 1) | Add the balance from InstantSend locked transactions

*Parameter #3---whether to include empty accounts*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Empty | bool | Optional<br>(0 or 1) | Set to `true` to display accounts which have never received a payment.  Set to `false` (the default) to only include accounts which have received a payment.  Any account which has received a payment will be displayed even if its current balance is `0`

*Parameter #4---whether to include watch-only addresses in results*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Watch-Only | bool | Optional<br>(0 or 1) | If set to `true`, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.  If set to `false` (the default), treat watch-only addresses as if they didn't belong to this wallet

*Result---account names, balances, and minimum confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array containing objects each describing an account.  At the very least, the default account (\\") will be included"
→<br>Account | object | Required<br>(1 or more) | An object describing an account
→ →<br>`involvesWatchonly` | bool | Optional<br>(0 or 1) | Set to `true` if the balance of this account includes a watch-only address which has received a spendable payment (that is, a payment with at least the specified number of confirmations and which is not an immature coinbase).  Otherwise not returned
→ →<br>`account` | string | Required<br>(exactly 1) | The name of the account
→ →<br>`amount` | number (dash) | Required<br>(exactly 1) | The total amount received by this account in dash
→ →<br>`confirmations` | number (int) | Required<br>(exactly 1) | The number of confirmations received by the last transaction received by this account.  May be `0`
→ →<br>`label` | string | Optional<br>(0 or 1) | A comment for the address/transaction

*Example from Dash Core 0.13.0*

Get the balances for all non-empty accounts, including transactions
which have been confirmed at least six times and InstantSend locked transactions:

``` bash
dash-cli -testnet listreceivedbyaccount 6 true false true
```

Result (edited to only show the first two results):

``` json
[
    {
        "account" : "",
        "amount" : 0.19960000,
        "confirmations" : 53601
    },
    {
        "account" : "doc test",
        "amount" : 0.30000000,
        "confirmations" : 8991
    }
]
```

*See also*

* [ListReceivedByAddress](/docs/core-api-ref-remote-procedure-calls-wallet#listreceivedbyaddress): lists the total number of dash received by each address.
* [ListReceivedByLabel](core-api-ref-remote-procedure-calls-wallet#listreceivedbylabel): lists the total number of dash received by each label.
* [GetReceivedByAccount](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#getreceivedbyaccount): returns the total amount received by addresses in a particular account from transactions with the specified number of confirmations.  It does not count coinbase transactions.
* [GetReceivedByAddress](/docs/core-api-ref-remote-procedure-calls-wallet#getreceivedbyaddress): returns the total amount received by the specified address in transactions with the specified number of confirmations. It does not count coinbase transactions.

# MasternodeBroadcast
[block:callout]
{
  "type": "danger",
  "body": "**Warning:** **_Removed in Dash Core 0.14.0._**"
}
[/block]
The [`masternodebroadcast` RPC](core-api-ref-remote-procedure-calls-removed#masternodebroadcast) was removed in Dash Core 0.14.0.

*Parameter #1---masternode broadcast command*

Name | Type | Presence | Description
--- | --- | --- | ---
`command` | string (hex) | Required<br>(exactly 1) | The command to use:<br>`create-alias`<br>`create-all`<br>`decode`<br>`relay`

## MNB create-alias

The `masternodebroadcast create-alias` RPC creates single remote masternode broadcast message by assigned alias configured in `masternode.conf`.

*Parameter #2---masternode alias*

Name | Type | Presence | Description
--- | --- | --- | ---
`alias` | string | Required<br>(exactly 1) | The masternode alias for creating the broadcast message

*Result---broadcast message*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | Object containing result data
→<br>`alias` | string | Required<br>(exactly 1) | Alias of the masternode
→<br>`result` | string | Required<br>(exactly 1) | Result of broadcast message create attempt
→<br>`hex` | string (hex) | Required<br>(exactly 1) | Masternode broadcast data

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet masternodebroadcast create-alias MN01
```

Result:
``` json
{
  "alias": "MN01",
  "result": "successful",
  "hex": "010fab7e86a6d7c483b836fe862c8a23f69aebadce7c58c48778a4fa6bd93fc8f60100000000ffffffff00000000000000000000ffff2d20ed4c4e1f210267fae84ef6aa6ab3d877b47932915a9b406566c873ea025986fc7e15a15fd2f24104341ab0d26ae967856213df205bf172418422a847f3a63941d8031234a64a143f5570a6010d2b5e1dff163c91316a65667f0ee1bfb0ff38edd0a695bea75de731411f8a9bf1e7818c7352c8a02bd31a4da1bb8d88e91c8a9c7151afc076b6a68f54c9087a981a780e6279e9d7b73940ee7aad65c28e4846573bffa74518443380dfde4d3c145a00000000401201000fab7e86a6d7c483b836fe862c8a23f69aebadce7c58c48778a4fa6bd93fc8f60100000000ffffffff69fc28f4772eaefd17cd1bab575aac752b5944ee3e7221df204b4d04000000004d3c145a00000000411bef1bdf25a500ae2af4052e8504e2f93ec365d5ed9d42e3c52b84714136060f9766068553c450a4b1c0b3d72740580f097f7e62c098addc55f71f016cfda24d7a0001000100"
}
```

## MNB create-all

The `masternodebroadcast create-all` RPC creates remote masternode broadcast messages for all masternodes configured in `masternode.conf`.

*Result---broadcast message(s)*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | Object containing result data
→<br>`overall` | string | Required<br>(exactly 1) | Summary of broadcast message creation success/failure
→<br>detail | object | Required<br>(exactly 1) | Object containing status details
→ →<br>status | object | Required<br>(1 or more) | Object containing status for each each masternode broadcast message creation attempt
→ → →<br>`alias` | string | Required<br>(exactly 1) | Alias of the masternode
→ → →<br>`result` | string | Required<br>(exactly 1) | Result - `successful` or `failed`
→ → →<br>`error` | string | Optional | Error message if failed
→<br>`hex` | string (hex) | Optional<br>(exactly 1) | Masternode broadcast data (if message(s) created successfully)

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet masternodebroadcast create-all
```

Result:
``` json
{
  "overall": "Successfully created broadcast messages for 1 masternodes, failed to create 0, total 1",
  "detail": {
    "status": {
      "alias": "MN01",
      "result": "successful"
    }
  },
  "hex": "010fab7e86a6d7c483b836fe862c8a23f69aebadce7c58c48778a4fa6bd93fc8f60100000000ffffffff00000000000000000000ffff2d20ed4c4e1f210267fae84ef6aa6ab3d877b47932915a9b406566c873ea025986fc7e15a15fd2f24104341ab0d26ae967856213df205bf172418422a847f3a63941d8031234a64a143f5570a6010d2b5e1dff163c91316a65667f0ee1bfb0ff38edd0a695bea75de731411f555444bd95d98b8407ff1b8cc595a3d284c30b9bbaca488a949bc53be08ca1021724527f9a15e9307c7391d9ad563dcc9ced6ae621ae7d6fe3e3c3ba81dce795d143145a00000000401201000fab7e86a6d7c483b836fe862c8a23f69aebadce7c58c48778a4fa6bd93fc8f60100000000ffffffff914dff1cc3dfc0729bb1f4e3f070d65d1fa41072da5290a54d472d0400000000d143145a00000000411c628109c911ef330aaa789bd621f8c7975290d196beef3ecdaa1133302daccdaa3df82b1f16d753fef884ce3a3eb28a7b621233c14496a010bb49f247190651100001000100"
}
```

## MNB decode

The `masternodebroadcast decode` RPC decodes a masternode broadcast message (deserializes from a hex string to JSON).

*Parameter #2---object data (hex)*

Name | Type | Presence | Description
--- | --- | --- | ---
`data-hex` | string (hex) | Required<br>(exactly 1) | The data (hex) of the masternode broadcast to decode

*Result---broadcast message(s)*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | Object containing result data
→<br>`outpoint` | string (hex) | Required<br>(exactly 1) | Masternode outpoint
→<br>`addr` | string | Required<br>(exactly 1) | Masternode IP address and port
→<br>`pubKeyCollateralAddress` | string (hex) | Required<br>(1 or more) | Masternode collateral public key address
→<br>`pubKeyMasternode` | string (hex) | Required<br>(exactly 1) | Masternode public key
→<br>`vchSig` | string (base64) | Required<br>(exactly 1) | Masternode signature
→<br>`sigTime` | int64_t | Required<br>(exactly 1) | Signature time as a Unix epoch
→<br>`protocolVersion` | int | Required<br>(exactly 1) | Dash protocol version
→<br>`nLastDsq` | int64_t | Required<br>(exactly 1) | Dsq count from the last [`dsq` message](core-ref-p2p-network-privatesend-messages#dsq) from this node
→<br>lastPing | object | Required<br>(exactly 1) | Ping object (`mnp` message)
→ →<br>`outpoint` | string (hex) | Required<br>(exactly 1) | Masternode outpoint
→ →<br>`blockHash` | string (hex) | Required<br>(exactly 1) | Block hash from 12 blocks prior to the current tip
→ →<br>`sigTime` | int64_t | Required<br>(exactly 1) | Signature time as a Unix epoch
→ →<br>`vchSig` | string (base64) | Required<br>(exactly 1) | Masternode signature
→<br>`overall` | string | Required<br>(exactly 1) | Summary of broadcast message creation success/failure

*Example from Dash Core 0.12.2*

``` bash
masternodebroadcast decode 010fab7e86a6d7c483b836fe862c8a23f69aebadce7c58c4\
8778a4fa6bd93fc8f60100000000ffffffff00000000000000000000ffff2d20ed4c4e1f2102\
67fae84ef6aa6ab3d877b47932915a9b406566c873ea025986fc7e15a15fd2f24104341ab0d2\
6ae967856213df205bf172418422a847f3a63941d8031234a64a143f5570a6010d2b5e1dff16\
3c91316a65667f0ee1bfb0ff38edd0a695bea75de731411f8a9bf1e7818c7352c8a02bd31a4d\
a1bb8d88e91c8a9c7151afc076b6a68f54c9087a981a780e6279e9d7b73940ee7aad65c28e48\
46573bffa74518443380dfde4d3c145a00000000401201000fab7e86a6d7c483b836fe862c8a\
23f69aebadce7c58c48778a4fa6bd93fc8f60100000000ffffffff69fc28f4772eaefd17cd1b\
ab575aac752b5944ee3e7221df204b4d04000000004d3c145a00000000411bef1bdf25a500ae\
2af4052e8504e2f93ec365d5ed9d42e3c52b84714136060f9766068553c450a4b1c0b3d72740\
580f097f7e62c098addc55f71f016cfda24d7a0001000100
```

Result:
``` json
{
  "36b753f9c8d328d405b8a909bbf4fd29c0d37aa48eae98fa1289b90e36e002c4": {
    "outpoint": "f6c83fd96bfaa47887c4587cceadeb9af6238a2c86fe36b883c4d7a6867eab0f-1",
    "addr": "45.32.237.76:19999",
    "pubKeyCollateralAddress": "yY6AmGopsZS31wy1JLHR9P6AC6owFaXwuh",
    "pubKeyMasternode": "yj25teTD6yjcNpQC7inq72tDgsivG6xLZM",
    "vchSig": "H4qb8eeBjHNSyKAr0xpNobuNiOkcipxxUa/Adramj1TJCHqYGngOYnnp17c5QO56rWXCjkhGVzv/p0UYRDOA394=",
    "sigTime": 1511275597,
    "protocolVersion": 70208,
    "nLastDsq": 0,
    "lastPing": {
      "outpoint": "f6c83fd96bfaa47887c4587cceadeb9af6238a2c86fe36b883c4d7a6867eab0f-1",
      "blockHash": "00000000044d4b20df21723eee44592b75ac5a57ab1bcd17fdae2e77f428fc69",
      "sigTime": 1511275597,
      "vchSig": "G+8b3yWlAK4q9AUuhQTi+T7DZdXtnULjxSuEcUE2Bg+XZgaFU8RQpLHAs9cnQFgPCX9+YsCYrdxV9x8BbP2iTXo="
    }
  },
  "overall": "Successfully decoded broadcast messages for 1 masternodes, failed to decode 0, total 1"
}
```

## MNB relay

The `masternodebroadcast relay` RPC relays a masternode broadcast message to the network.

*Parameter #2---object data (hex)*

Name | Type | Presence | Description
--- | --- | --- | ---
`data-hex` | string (hex) | Required<br>(exactly 1) | The data (hex) of the masternode broadcast to relay

*Result---broadcast message(s)*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | Object containing result data
→<br>Hash | string (hex) | Required<br>(1 or more) | Masternode broadcast hash
→ →<br>`outpoint` | string (hex) | Required<br>(exactly 1) | Masternode outpoint
→ →<br>`addr` | string | Required<br>(exactly 1) | Masternode IP address and port
→ →<br>Result | string (hex) | Required<br>(exactly 1) | Result - `successful` or `failed`
→ → →<br>`error` | string | Optional | Error message if failed
→<br>`overall` | string | Required<br>(exactly 1) | Summary of broadcast message creation success/failure

*Example from Dash Core 0.12.2*

``` bash
masternodebroadcast relay 010fab7e86a6d7c483b836fe862c8a23f69aebadce7c58c4\
8778a4fa6bd93fc8f60100000000ffffffff00000000000000000000ffff2d20ed4c4e1f2102\
67fae84ef6aa6ab3d877b47932915a9b406566c873ea025986fc7e15a15fd2f24104341ab0d2\
6ae967856213df205bf172418422a847f3a63941d8031234a64a143f5570a6010d2b5e1dff16\
3c91316a65667f0ee1bfb0ff38edd0a695bea75de731411f8a9bf1e7818c7352c8a02bd31a4d\
a1bb8d88e91c8a9c7151afc076b6a68f54c9087a981a780e6279e9d7b73940ee7aad65c28e48\
46573bffa74518443380dfde4d3c145a00000000401201000fab7e86a6d7c483b836fe862c8a\
23f69aebadce7c58c48778a4fa6bd93fc8f60100000000ffffffff69fc28f4772eaefd17cd1b\
ab575aac752b5944ee3e7221df204b4d04000000004d3c145a00000000411bef1bdf25a500ae\
2af4052e8504e2f93ec365d5ed9d42e3c52b84714136060f9766068553c450a4b1c0b3d72740\
580f097f7e62c098addc55f71f016cfda24d7a0001000100
```

Result:
``` json
{
  "36b753f9c8d328d405b8a909bbf4fd29c0d37aa48eae98fa1289b90e36e002c4": {
    "outpoint": "f6c83fd96bfaa47887c4587cceadeb9af6238a2c86fe36b883c4d7a6867eab0f-1",
    "addr": "45.32.237.76:19999",
    "36b753f9c8d328d405b8a909bbf4fd29c0d37aa48eae98fa1289b90e36e002c4": "successful"
  },
  "overall": "Successfully relayed broadcast messages for 1 masternodes, failed to relay 0, total 1"
}
```

*See also: none*

# Move
[block:callout]
{
  "type": "danger",
  "body": "Warning: Removed in Dash Core 0.18.0."
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**)"
}
[/block]
The [`move` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#move) moves a specified amount from one account in your wallet to another using an off-block-chain transaction.
[block:callout]
{
  "type": "warning",
  "body": "**Warning:** it's possible to move more funds than are in an account, giving the sending account a negative balance and giving the receiving account a balance that may exceed the number of dash in the wallet (or the number of dash in existence)."
}
[/block]
*Parameter #1---from account*

Name | Type | Presence | Description
--- | --- | --- | ---
From Account | string | Required<br>(exactly 1) | The name of the account to move the funds from

*Parameter #2---to account*

Name | Type | Presence | Description
--- | --- | --- | ---
To Account | string | Required<br>(exactly 1) | The name of the account to move the funds to

*Parameter #3---amount to move*

Name | Type | Presence | Description
--- | --- | --- | ---
Amount | number (dash) | Required<br>(exactly 1) | The amount of dash to move

*Parameter #4---an unused parameter*

Name | Type | Presence | Description
--- | --- | --- | ---
*Unused* | number (int) | Optional<br>(0 or 1) | This parameter is no longer used. If parameter #5 needs to be specified, this can be any integer

*Parameter #5---a comment*

Name | Type | Presence | Description
--- | --- | --- | ---
Comment | string | Optional<br>(0 or 1) | A comment to assign to this move payment

*Result---`true` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | bool | Required<br>(exactly 1) | Set to `true` if the move was successful

*Example from Dash Core 0.12.2*

Move 1 dash from "doc test" to "test1", giving the transaction the
comment "Example move":

``` bash
dash-cli -testnet move "doc test" "test1" 0.1 0 "Example move"
```

Result:

``` json
true
```

*See also*

* [ListAccounts](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#listaccounts): lists accounts and their balances.
* [SendFrom](/docs/core-api-ref-remote-procedure-calls-wallet-deprecated#sendfrom): spends an amount from a local account to a dash address.
* [SendToAddress](/docs/core-api-ref-remote-procedure-calls-wallet#sendtoaddress): spends an amount to a given address.

# SendFrom
[block:callout]
{
  "type": "danger",
  "body": "Warning: Removed in Dash Core 0.18.0."
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Requires <<glossary:wallet>> support (**unavailable on masternodes**). Requires an unlocked wallet or an unencrypted wallet."
}
[/block]
The [`sendfrom` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#sendfrom) spends an amount from a local account to a dash address.

*Parameter #1---from account*

Name | Type | Presence | Description
--- | --- | --- | ---
From Account | string | Required<br>(exactly 1) | The name of the account from which the dash should be spent.  Use an empty string (\\") for the default account"

*Parameter #2---to address*

Name | Type | Presence | Description
--- | --- | --- | ---
To Address | string | Required<br>(exactly 1) | A P2PKH or P2SH address to which the dash should be sent

*Parameter #3---amount to spend*

Name | Type | Presence | Description
--- | --- | --- | ---
Amount | number (dash) | Required<br>(exactly 1) | The amount to spend in dash.  Dash Core will ensure the account has sufficient dash to pay this amount (but the transaction fee paid is not included in the calculation, so an account can spend a total of its balance plus the transaction fee)

*Parameter #4---minimum confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations an incoming transaction must have for its outputs to be credited to this account's balance. Outgoing transactions are always counted, as are move transactions made with the [`move` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#move). If an account doesn't have a balance high enough to pay for this transaction, the payment will be rejected.  Use `0` to spend unconfirmed incoming payments. Default is `1`
[block:callout]
{
  "type": "warning",
  "body": "**Warning:** if account1 receives an unconfirmed payment and transfers it to account2 with the [`move` RPC](core-api-ref-remote-procedure-calls-wallet-deprecated#move), account2 will be able to spend those dash even if this parameter is set to `1` or higher."
}
[/block]
*Parameter #5---whether to add the balance from transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(0 or 1) | If set to `true`, add the balance from InstantSend locked transactions. If set to `false` (the default), InstantSend locked transaction balances are not included.

*Parameter #6---a comment*

Name | Type | Presence | Description
--- | --- | --- | ---
Comment | string | Optional<br>(0 or 1) | A locally-stored (not broadcast) comment assigned to this transaction.  Default is no comment

*Parameter #7---a comment about who the payment was sent to*

Name | Type | Presence | Description
--- | --- | --- | ---
Comment To | string | Optional<br>(0 or 1) | A locally-stored (not broadcast) comment assigned to this transaction.  Meant to be used for describing who the payment was sent to. Default is no comment

*Result---a TXID of the sent transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | The TXID of the sent transaction, encoded as hex in RPC byte order

*Example from Dash Core 0.12.2*

Spend 0.1 dash from the account "test" to the address indicated below
using only UTXOs with at least six confirmations, giving the
transaction the comment "Example spend" and labeling the spender
"Example.com":

``` bash
dash-cli -testnet sendfrom "test" \
            yhJays6zGUFKq1KS5V5WLbyk3cwCXyGrKd \
            0.1 \
            6 \
            false \
            "Example spend" \
            "Example.com"
```

Result:

``` text
cd64b9d55c63bf247f2eca32f978e340622107b607a46c422dabcdc20c0571fe
```

*See also*

* [SendToAddress](/docs/core-api-ref-remote-procedure-calls-wallet#sendtoaddress): spends an amount to a given address.
* [SendMany](/docs/core-api-ref-remote-procedure-calls-wallet#sendmany): creates and broadcasts a transaction which sends outputs to multiple addresses.

# SentinelPing
[block:callout]
{
  "type": "danger",
  "body": "**Warning:** **_Removed in Dash Core 0.14.0._**"
}
[/block]
The [`sentinelping` RPC](core-api-ref-remote-procedure-calls-removed#sentinelping) was removed in Dash Core 0.14.0.

*Parameter #1---sentinel version*

Name | Type | Presence | Description
--- | --- | --- | ---
Version | string | Required<br>(exactly 1) | Sentinel version in the form 'x.x.x'

*Result---the message signature*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | bool | Required<br>(exactly 1) | Ping result

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet sentinelping
```

Result:
``` json
true
```

*See also: none*