Dash Platform consensus is provided by Drive which uses a fork of Tendermint called Tenderdash to operate the platform chain. Tenderdash handles state transition ordering, block creation, and network communication related to the platform chain while Dash Platform Protocol ensures the consistency and integrity of the data itself.

# Tendermint

Tendermint provides Byzantine Fault Tolerant (BFT) State Machine Replication via blocks containing transactions. Consensus is arrived at when more than 2/3 of the set of validator nodes vote for a proposed block and commit to it. Information regarding the voting is then included in the subsequent block to provide proof of the block commitment.
[block:callout]
{
  "type": "info",
  "title": "",
  "body": "Additional information about Tendermint is available in the <a href=\"https://docs.tendermint.com/master/spec/#overview\" target=\"_blank\">Tendermint Core spec</a>."
}
[/block]
# Tenderdash

[Tenderdash](https://github.com/dashevo/tenderdash) is a fork of [Tendermint](https://docs.tendermint.com/master/introduction/what-is-tendermint.html) modified to meet Dash Platform requirements. As with Tendermint, Tenderdash provides Byzantine Fault Tolerant (BFT) State Machine Replication via blocks containing transactions. Additionally, it is being updated to integrate some improvements that leverage Dash's long-living masternode quorums (LLMQ).

## Validator Set Rotation

Rather than having a static validator set, Tenderdash will periodically change to a new set of validator nodes. These validator sets are comprised of a subset of masternodes that are members of one of the 100 member long-living masternode quorums (LLMQs). This rotation process balances the performance and security of platform chain like InstantSend and ChainLock quorum changes do on the core chain.

## Threshold Signing

Tenderdash also takes advantage of LLMQ BLS threshold signing to produce block signatures for platform chain. This results in more compact block headers since only a single BLS threshold signature is required instead of individual signatures from each validator. Notably this means that the block signatures can be easily verified by any client using the deterministic masternode list.