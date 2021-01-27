Dash Platform consensus is provided by Drive which uses Tenderdash to operate the platform chain. Tenderdash handles state transition ordering, block creation, and network communication related to the platform chain while Dash Platform Protocol ensures the consistency and integrity of the data itself.

# Tenderdash

Tenderdash is a fork of Tendermint modified to meet Dash Platform requirements. As with Tendermint, Tenderdash provides Byzantine Fault Tolerant (BFT) State Machine Replication via blocks containing transactions. Consensus is arrived at when more than 2/3 of the set of validator nodes vote for a proposed block and commit to it. Information regarding the voting is then included in the subsequent block to provide proof of the block commitment.
[block:callout]
{
  "type": "info",
  "title": "",
  "body": "Additional information about Tendermint is available in the <a href=\"https://docs.tendermint.com/master/spec/#overview\" target=\"_blank\">Tendermint Core spec</a>."
}
[/block]
In Tenderdash the validator set is comprised of a subset of masternodes that are members of one of the 100 member long-living masternode quorums (LLMQs). 

Additionally, it is being updated to integrate some improvements that leverage Dash's long-living masternode quorums (LLMQ). ***Specifically ... validator sets using LLMQs, BLS signing, ...***