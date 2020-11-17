Under current <<glossary:consensus rules>>, a <<glossary:block>> is not valid unless its serialized size is less than or equal to 2 MB. All fields described below are counted towards the serialized size.

| Bytes    | Name         | Data Type        | Description
| - | - | - | - |
| 80       | block header | block_header     | The <<glossary:block header>> in the format described in the [block header section](core-ref-block-chain-block-headers).
| *Varies* | txn_count    | <<glossary:compactSize uint>> | The total number of transactions in this block, including the coinbase transaction.
| *Varies* | txns         | <<glossary:raw transaction>>  | Every transaction in this block, one after another, in raw transaction format.  Transactions must appear in the data stream in the same order their TXIDs appeared in the first row of the merkle tree.  See the [merkle tree section](core-ref-block-chain-block-headers#merkle-trees) for details.

# Coinbase

The first transaction in a block must be a <<glossary:coinbase transaction>> which should collect and spend any <<glossary:transaction fee>> paid by transactions included in this block.

## Block Subsidy
Until the coin limit (~18 million Dash) is hit, all blocks are entitled to receive a block subsidy of newly created Dash value. The newly created value should be spent in the coinbase transaction.

The block subsidy declines by ~7.1% per year until all Dash is mined. Subsidy calculations are performed by the Dash Core [GetBlockSubsidy()](https://github.com/dashpay/dash/blob/v0.15.x/src/validation.cpp#L1012) function.

## Block Reward
Together, the transaction fees and block subsidy are called the <<glossary:block reward>>. A coinbase transaction is invalid if it tries to spend more value than is available from the block reward.

The block reward is divided into three parts: <<glossary:miner>>, <<glossary:masternode>>, and <<glossary:superblock>>. 

| Payee | Subsidy | Description |
| ----- | -------- | ----------- |
| Miner | 45% | Payment for mining
| Masternode | 45% | Payment for masternode services ([PrivateSend](core-guide-dash-features-privatesend), [InstantSend](core-guide-dash-features-instantsend), [Governance](https://docs.dash.org/en/stable/introduction/features.html#decentralized-governance), etc.)
| Superblock | 10% | Payment for maintenance/expansion of the ecosystem (Core development, marketing, integration, etc.)

### Block Reward Reallocation

Dash Core v0.16 includes logic to gradually adjust the block reward allocation if the BIP-9 activation threshold is met. This reallocation would eventually result in miners receiving 40% of the non-governance block subsidy and masternodes receiving 60% of it rather than the current 50/50 split.
[block:callout]
{
  "type": "info",
  "body": "The reward reallocation will be signaled via BIP-9 bit 5. The threshold percentage for this activation is [dynamic](https://github.com/dashpay/dash/pull/3692) and will decrease every 4032 blocks from 80% to 60%, over approximately 10 weeks. The first window for signaling will begin at block 1350720 on 2020-10-07.",
  "title": "Block Reward Reallocation BIP-9 Activation"
}
[/block]
**Activation Progress**

Progress can be seen on [this chart](http://178.254.23.111/~pub/16_adoption.png).

**Reward reallocation changes**

Reward reallocation changes would begin at the first superblock following activation and then occur every three superblock cycles (approximately once per quarter) until the reallocation is complete.

| Adjustment | Miner % | Masternode % | Change (%)
| :-: | :-: | :-: | :-:
| 0 (Current) | 50.0 | 50.0 | 0.00%
| 1 | 48.7 | 51.3 | 1.30%
| 2 | 47.4 | 52.6 | 1.30%
| 3 | 46.7 | 53.3 | 0.70%
| 4 | 46.0 | 54.0 | 0.70%
| 5 | 45.4 | 54.6 | 0.60%
| 6 | 44.8 | 55.2 | 0.60%
| 7 | 44.3 | 55.7 | 0.50%
| 8 | 43.8 | 56.2 | 0.50%
| 9 | 43.3 | 56.7 | 0.50%
| 10 | 42.8 | 57.2 | 0.50%
| 11 | 42.3 | 57.7 | 0.50%
| 12 | 41.8 | 58.2 | 0.50%
| 13 | 41.5 | 58.5 | 0.30%
| 14 | 41.2 | 58.8 | 0.30%
| 15 | 40.9 | 59.1 | 0.30%
| 16 | 40.6 | 59.4 | 0.30%
| 17 | 40.3 | 59.7 | 0.30%
| 18 | 40.1 | 59.9 | 0.20%
| 19 | 40.0 | 60.0 | 0.10%