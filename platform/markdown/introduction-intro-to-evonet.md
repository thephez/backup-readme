[block:callout]
{
  "type": "danger",
  "title": "Dash Platform on Testnet",
  "body": "As of Dash Platform v0.17, platform services are now deployed on the Dash testnet. Evonet is currently still online but it is still using Dash Platform v0.16. Please refer to [this page](intro-to-testnet) for testnet information."
}
[/block]
# Overview

Evonet is the initial development network provided for experimentation and evaluation of Dash Platform features. This marks the first release of the core Dash Platform components in a freely accessible network. 
[block:callout]
{
  "type": "warning",
  "body": "As a development network, Evonet may be subject to frequent updates and changes that break backwards compatibility.",
  "title": "Evonet Stability"
}
[/block]

# Network Details

## Infrastructure
Dash Core Group provides the core Evonet infrastructure consisting of 50 masternodes running Dash Core along with the platform services that provide the [decentralized API (DAPI)](explanation-dapi) and [storage (Drive)](explanation-drive) functionality.

Evonet also includes a [block explorer](http://insight.evonet.networks.dash.org:3001/insight/) for the core blockchain and a [test Dash faucet](http://faucet.evonet.networks.dash.org/) that dispenses funds to users/developers experimenting on the network.

## Features

With this initial public release of the core Dash Platform components, the features available on the platform include:
- [Dash Platform Name Service](explanation-dpns) (DPNS): a data contract and supporting logic for name registration
- [Identities](explanation-identity): creation of identities
- [Names](explanation-dpns): creation of DPNS names that link to an identity

In the near future, the DashPay data contract will also be registered on Evonet.

[block:callout]
{
  "type": "success",
  "title": "Evonet Updated",
  "body": "Registration of third-party data contracts is now enabled on Evonet. See this [tutorial](tutorial-register-a-data-contract) for details."
}
[/block]

# Getting involved

This network is open for all who are interested in testing and interacting with Dash Platform. To learn how to connect, please jump to the [Connecting to a Network tutorial](tutorial-connecting-to-evonet).