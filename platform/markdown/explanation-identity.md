# Overview

Identities in Dash Platform provide the foundation for names as a way for users to interact and identify one another that is familiar and easy to use, especially compared to traditional cryptocurrency identifiers such as public key hashes.

Identities are separate from names and can be thought of as a lower-level construct that provides the foundation for various user-facing functionality. Simply, it is a public key recorded on the platform chain that controls a user profile and signs documents. Each identity also has a balance of credits that is established by locking funds on layer 1. These credits are used to pay fees associated with [state transitions](explanation-platform-protocol-state-transition).

In order to create an identity, a user needs to pay the network for storing a user's public key on the platform chain. Since new users may not have existing Dash funds, there may be an invitation process in the future to allow users to register an identity despite lacking their own funds. The invitation process would effectively separate the funding and registration steps that are required for any new identity to be created.

# The Identity Registration Process
[block:callout]
{
  "type": "info",
  "title": "Evonet Faucet",
  "body": "On Evonet, a [test Dash faucet](http://faucet.evonet.networks.dash.org/) is available. It dispenses small amounts to enable all users to directly acquire the funds necessary to register an identity and username."
}
[/block]
First, a sponsor (which could be a business, another person, or even the same user who is registering) spends Dash to create an <a class="glossary" href="reference-glossary#section-invitation-lock-transaction" title="A special transaction with outputs for funding creation of new dash platform identities">invitation lock transaction</a>. The transaction contains one or more outputs which burns some Dash funds to establish credits within Dash platform. (In the future those Dash will be locked instead of burned, so that Masternodes who have been paid credits can unlock the original Dash to receive their payment.) Credits are spent to fund identity registrations and other actions on the platform. In order to prevent a privacy risk by linking the sponsors wallet and the new identities, the invitation lock transaction may be created with PrivateSend funds.

After the invitation lock transaction is broadcast and confirmed, the sponsor sends information about the transaction to the new user. This may be done as a hyperlink that the core wallet understands, or as a QR code that a mobile wallet can scan. Once the user has the transaction data from the sponsor, they can use it to fund an identity registration state transition within Dash platform.

Users who already have Dash funds can act as their own sponsor if they wish, using the same steps listed here.
[block:callout]
{
  "type": "info",
  "body": "As of Dash Platform Protocol v0.12.0, there are no longer multiple identity types. Any identity is now able to create one or more data contracts."
}
[/block]