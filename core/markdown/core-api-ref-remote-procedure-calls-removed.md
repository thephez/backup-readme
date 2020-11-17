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

# GetInfo
[block:callout]
{
  "type": "danger",
  "body": "**Warning:** **_Removed in Dash Core 0.16.0._**",
  "title": ""
}
[/block]
The [`getinfo` RPC](core-api-ref-remote-procedure-calls-removed#getinfo) prints various information about the node and the network.

*Parameters: none*

*Result---information about the node and network*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | Information about this node and the network
→<br>`deprecation-warning` | string | Required<br>(exactly 1) | Warning that the getinfo command is deprecated and will be removed in a future version
→<br>`version` | number (int) | Required<br>(exactly 1) | This node's version of Bitcoin Core in its internal integer format.  For example, Dash Core 0.12.2 has the integer version number 120200
→<br>`protocolversion` | number (int) | Required<br>(exactly 1) | The protocol version number used by this node.  See the [protocol versions section](core-ref-p2p-network-protocol-versions) for more information
→<br>`walletversion` | number (int) | Optional<br>(0 or 1) | The version number of the wallet.  Only returned if wallet support is enabled
→<br>`balance` | number (duffs) | Optional<br>(0 or 1) | The total balance of the wallet in duffs.  Only returned if wallet support is enabled
→<br>`privatesend_balance` | number (duffs) | Optional<br>(0 or 1) | The PrivateSend balance of the wallet in duffs.  Only returned if wallet support is enabled  (Added in Dash Core 0.11.0)
→<br>`blocks` | number (int) | Required<br>(exactly 1) | The number of blocks in the local best block chain.  A new node with only the hardcoded genesis block will return `0`
→<br>`timeoffset` | number (int) | Required<br>(exactly 1) | The offset of the node's clock from the computer's clock (both in UTC) in seconds.  The offset may be up to 4200 seconds (70 minutes)
→<br>`connections` | number (int) | Required<br>(exactly 1) | The total number of open connections (both outgoing and incoming) between this node and other nodes
→<br>`proxy` | string | Required<br>(exactly 1) | The hostname/IP address and port number of the proxy, if set, or an empty string if unset
→<br>`difficulty` | number (real) | Required<br>(exactly 1) | The difficulty of the highest-height block in the local best block chain
→<br>`testnet` | bool | Required<br>(exactly 1) | Set to `true` if this node is on testnet; set to `false` if this node is on mainnet or a regtest
→<br>`keypoololdest` | number (int) | Optional<br>(0 or 1) | The date as Unix epoch time when the oldest key in the wallet key pool was created; useful for only scanning blocks created since this date for transactions.  Only returned if wallet support is enabled
→<br>`keypoolsize` | number (int) | Optional<br>(0 or 1) | The number of keys in the wallet keypool.  Only returned if wallet support is enabled
→<br>`unlocked_until` | number (int) | Optional<br>(0 or 1) | The Unix epoch time when the wallet will automatically re-lock.  Only displayed if wallet encryption is enabled.  Set to `0` if wallet is currently locked
→<br>`paytxfee` | number (duffs) | Optional<br>(0 or 1) | The minimum fee to pay per kilobyte of transaction; may be `0`.  Only returned if wallet support is enabled
→<br>`relayfee` | number (duffs) | Required<br>(exactly 1) | The minimum fee per kilobyte a transaction must pay in order for this node to accept it into its memory pool
→<br>`errors` | string | Required<br>(exactly 1) | A plain-text description of any errors this node has encountered or detected.  If there are no errors, an empty string will be returned.  This is not related to the JSON-RPC `error` field

*Example from Dash Core 0.15.0 with wallet support enabled*

``` bash
dash-cli -testnet getinfo
```

Result:

``` json
{
  "deprecation-warning": "WARNING: getinfo is deprecated and will be fully removed in a future version. Projects should transition to using getblockchaininfo, getnetworkinfo, and getwalletinfo.",
  "version": 140100,
  "protocolversion": 70215,
  "walletversion": 61000,
  "balance": 0.00000000,
  "privatesend_balance": 0.00000000,
  "blocks": 0,
  "timeoffset": 0,
  "connections": 0,
  "proxy": "",
  "difficulty": 0.000244140625,
  "testnet": true,
  "keypoololdest": 1507579068,
  "keypoolsize": 617,
  "unlocked_until": 0,
  "paytxfee": 0.00000000,
  "relayfee": 0.00010000,
  "errors": ""
}
```

*See also*

* [GetBlockChainInfo](/docs/core-api-ref-remote-procedure-calls-blockchain#getblockchaininfo): provides information about the current state of the block chain.
* [GetMemPoolInfo](/docs/core-api-ref-remote-procedure-calls-blockchain#getmempoolinfo): returns information about the node's current transaction memory pool.
* [GetMiningInfo](/docs/core-api-ref-remote-procedure-calls-mining#getmininginfo): returns various mining-related information.
* [GetNetworkInfo](/docs/core-api-ref-remote-procedure-calls-network#getnetworkinfo): returns information about the node's connection to the network.
* [GetWalletInfo](/docs/core-api-ref-remote-procedure-calls-wallet#getwalletinfo): provides information about the wallet.

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