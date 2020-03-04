# Overview

Dash aims to be the most user-friendly and scalable payments-focused cryptocurrency in the world. The Dash <<glossary:network>> features instant transaction confirmation, double spend protection, anonymity similar to that of physical cash, a self-governing, self-funding model driven by incentivized full <<glossary:nodes>> and a clear [roadmap](https://www.dash.org/roadmap/) for future development.

While Dash is based on Bitcoin and compatible with many key components of the Bitcoin ecosystem, its two-tier network structure offers significant improvements in transaction speed, anonymity and governance. This section of the documentation describes these key features that set Dash apart in the blockchain economy.

# Lite Mode

Lite mode provides a way to run Dash Core full nodes with Dash-specific functionality disabled. Masternodes **_cannot_** be run in lite mode since they are paid to provide the services that lite mode disables. Disabled items include: [InstantSend](core-guide-dash-features-instantsend), [ChainLocks](core-guide-dash-features-chainlocks), [PrivateSend](core-guide-dash-features-privatesend), Masternode-related information, and [Governance](core-guide-dash-features-governance) details.
[block:callout]
{
  "type": "danger",
  "title": "Lite mode effects",
  "body": "Since nodes running in lite mode do not execute InstantSend/ChainLock logic, they are unaware of transaction lock status and will always show `false` for the lock status returned in RPC responses (e.g. [GetTransaction's](core-api-ref-remote-procedure-calls-wallet#section-get-transaction) `chainlock`, `instantlock`, and `instantlock_internal` fields)."
}
[/block]
Lite mode is enable by setting `litemode=1` in the `dash.conf` file or by running Dash Core with the command line parameter `-litemode=1`.