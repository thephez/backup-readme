# GenerateToAddress
[block:callout]
{
  "type": "warning",
  "title": "Limited Support",
  "body": "Since Dash Core 0.14, this RPC is not available in the official Windows/Mac binaries. The Linux binary and binaries self-compiled (with the appropriate options) continue to support this feature. See [PR #2778](https://github.com/dashpay/dash/pull/2778) for additional details."
}
[/block]

*Added in Dash Core 0.12.3 / Bitcoin Core 0.13.0*

The [`generatetoaddress` RPC](core-api-ref-remote-procedure-calls-generating#generatetoaddress) mines blocks immediately to a specified address.

*Parameter #1---the number of blocks to generate*

Name | Type | Presence | Description
--- | --- | --- | ---
Blocks | number (int) | Required<br>(exactly 1) | The number of blocks to generate.  The RPC call will not return until all blocks have been generated or the maximum number of iterations has been reached

*Parameter #2---a transaction identifier (TXID)*

Name | Type | Presence | Description
--- | --- | --- | ---
Address | string (base58) | Required<br>(exactly 1) | The address that will receive the newly generated Dash

*Parameter #3---the maximum number of iterations to try*

Name | Type | Presence | Description
--- | --- | --- | ---
Maxtries | number (int) | Optional<br>(0 or 1) | The maximum number of iterations that are tried to create the requested number of blocks.  Default is `1000000`

*Result---the generated block header hashes*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array containing the block header hashes of the generated blocks (may be empty if used with `generate 0`)
→<br>Header Hashes | string (hex) | Required<br>(1 or more) | The hashes of the headers of the blocks generated, as hex in RPC byte order

*Example from Dash Core 0.12.3*

Using regtest mode, generate 2 blocks with maximal 500000 iterations:

```bash
dash-cli -regtest generatetoaddress 2 "yaQzdWrDVYGncLKSKG4bHQ\
ML9UdAe726QN" 500000
```

Result:

```json
[
  "34726c518d1688a9c56b3399e892089d3a639b43de194517c07da2b168a3a89c",
  "1f030abe2bb323b8895542e3a85ed8386bd92c67af9d19fe9c163a4c5f5ef149"
]
```

*See also*

* [Generate](/docs/core-api-ref-remote-procedure-calls-generating#generate): mines blocks immediately (before the RPC call returns).
* [GetMiningInfo](/docs/core-api-ref-remote-procedure-calls-mining#getmininginfo): returns various mining-related information.
* [GetBlockTemplate](/docs/core-api-ref-remote-procedure-calls-mining#getblocktemplate): gets a block template or proposal for use with mining software.