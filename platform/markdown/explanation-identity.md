# Overview

Identities in Dash Platform provide a way for users to interact and identify one another that is familiar and easy to use, especially compared to traditional cryptocurrency identifiers such as public key hashes. Identities are separate from profiles, which contains identifying details like a username, display name, and avatar URL. Identities can be thought of as a lower-level construct that provide the foundation for various user-facing functionality. Simply, it is a public key recorded on the platform chain that controls a user profile and signs documents. 

In order to create an identity, a user needs to pay the network for storing a user's public key on the platform chain. Since new users may not have existing Dash funds, there will be an invitation process in the future to allow users to register an identity despite lacking their own funds. The invitation process will effectively separate the funding and registration steps that are required for any new identity to be created.

# The Identity Registration Process
[block:callout]
{
  "type": "info",
  "title": "Evonet Faucet",
  "body": "On Evonet, a [test Dash faucet](http://devnet-evonet-1117662964.us-west-2.elb.amazonaws.com/) is available. It dispenses small amounts to enable all users to directly acquire the funds necessary to register an identity and username."
}
[/block]
## User Registration

First, a sponsor (which could be a business, another person, or even the same user who is registering) spends Dash to create an <a class="glossary" href="reference-glossary#section-invitation-lock-transaction" title="A special transaction with outputs for funding creation of new dash platform identities">invitation lock transaction</a>. The transaction contains one or more outputs which burns some Dash funds to establish credits within Dash platform. (In the future those Dash will be locked instead of burned, so that Masternodes who have been paid credits can unlock the original Dash to receive their payment.) Credits are spent to fund identity registrations. In order to prevent a privacy risk by linking the sponsors wallet and the new identities, the invitation lock transaction will normally be created with PrivateSend funds.

After the invitation lock transaction is broadcast and confirmed, the sponsor sends information about the transaction to the new user. This will usually be done as a hyperlink that the core wallet understands, or as a QR code that a mobile wallet can scan. Once the user has the transaction data from the sponsor, they can use it to fund an identity registration state transition within Dash platform.

Users who already have Dash funds can act as their own sponsor if they wish, using the same steps listed here.

## Application Registration

There is a second kind of identity registration which is to create a Dash Platform application identity. The first step in registering a new application is creating an identity. This is used to manage the application. The process is very similar to registering a user identity, although usually an application developer will fund their own registration instead of using a sponsor.