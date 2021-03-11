# Overview

Identities in Dash Platform provide the foundation for names as a way for users to interact and identify one another that is familiar and easy to use, especially compared to traditional cryptocurrency identifiers such as public key hashes.

Identities are separate from names and can be thought of as a lower-level construct that provides the foundation for various user-facing functionality. Simply, it is a public key recorded on the platform chain that controls a user profile and signs documents. Each identity also has a balance of [credits](#credits) that is established by locking funds on layer 1. These credits are used to pay fees associated with the [state transitions](explanation-platform-protocol-state-transition) used to perform actions on the platform.
[block:callout]
{
  "type": "success",
  "title": "Identities DIP",
  "body": "The [Identities Dash Improvement Proposal (DIP)](https://github.com/dashpay/dips/blob/master/dip-0011.md) is now available with more extensive background information and details."
}
[/block]
# Identity Management

In order to [create an identity](#identity-create-process), a user needs to pay the network for storing a user's public key on the platform chain. Since new users may not have existing Dash funds, there will be an invitation process in the future to allow users to create an identity despite lacking their own funds. The invitation process will effectively separate the funding and registration steps that are required for any new identity to be created.

Once an identity is created, its credit balance is used to pay for platform activity (e.g. use of applications). The [topup process ](#identity-balance-topup-process) provides a way to add additional funds to the balance.

## Identity Create Process
[block:callout]
{
  "type": "info",
  "title": "Evonet Faucet",
  "body": "On Evonet, a [test Dash faucet](http://faucet.evonet.networks.dash.org/) is available. It dispenses small amounts to enable all users to directly acquire the funds necessary to create an identity and username."
}
[/block]
First, a sponsor (which could be a business, another person, or even the same user who is creating the identity) spends Dash in a transaction to create an invitation. The transaction contains one or more outputs which lock some Dash funds to establish credits within Dash platform.

After the transaction is broadcast and confirmed, the sponsor sends information about the invitation to the new user. This may be done as a hyperlink that the core wallet understands, or as a QR code that a mobile wallet can scan. Once the user has the transaction data from the sponsor, they can use it to fund an identity create state transition within Dash platform.

Users who already have Dash funds can act as their own sponsor if they wish, using the same steps listed here.

## Identity Balance Topup Process
[block:callout]
{
  "type": "success",
  "body": "Added in Dash Platform Protocol v0.13"
}
[/block]
The identity balance topup process works in a similar way to the initial identity creation funding. As with identity creation, a lock transaction is created on the layer 1 core blockchain. This lock transaction is then referenced in the identity topup state transition which increases the identity's balance by the designated amount.

# Credits
[block:callout]
{
  "type": "success",
  "title": "Added in Dash Platform Protocol v0.13",
  "body": "DPP v0.13 introduced the initial implementation of credits. Future releases will expand the functionality available."
}
[/block]
As mentioned above, credits provide the mechanism for paying fees that cover the cost of platform usage. Once a user locks Dash on the core blockchain and proves ownership of the locked value in an identity create or topup transaction, their credit balance increases by that amount. As they perform platform actions, these credits are deducted to pay the associated fees.
[block:callout]
{
  "type": "info",
  "body": "As of Dash Platform Protocol v0.13, credits deducted to pay state transition fees cannot be converted by masternodes back into Dash. This aspect of the credit system will come in a future release."
}
[/block]