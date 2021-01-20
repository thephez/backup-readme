The purpose of this tutorial is to walk through the steps necessary to set up a masternode with Dash Platform services.
[block:callout]
{
  "type": "danger",
  "body": "Running a masternode requires familiarity with Dash Platform services. Improper configuration may impact testing so please exercise caution if running a masternode.",
  "title": "Advanced Topic"
}
[/block]
# Prerequisites
- Access to a Linux VPS (Ubuntu 18.04 LTS recommended) configured with a non-root user ([guide](https://docs.dash.org/en/stable/masternodes/setup.html#set-up-your-vps))
- [Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/) (v18.06.0+) and [docker-compose](https://docs.docker.com/compose/install/) (v1.25.0+) installed
- An installation of [NodeJS](https://nodejs.org/en/download/) v12+
- A copy of the [mn-bootstrap repository](https://github.com/dashevo/mn-bootstrap)

Clone the mn-bootstrap repository
[block:code]
{
  "codes": [
    {
      "code": "git clone -b master https://github.com/dashevo/mn-bootstrap.git\ncd mn-bootstrap\nnpm install # optional: install CLI dependencies\nsudo npm link # optional: link CLI for system-wide execution",
      "language": "shell"
    }
  ]
}
[/block]
# Masternode setup

First, obtain 1000 Evonet tDASH and create your protx registration transaction as described [here](https://docs.dash.org/en/stable/masternodes/setup.html#option-2-registering-from-dash-core-wallet). The [Dash Core Full Node tutorial](tutorial-connecting-to-evonet-dash-core-full-node#config-file) has an example configuration file that can be used to connect to Testnet with Dash Core.
[block:callout]
{
  "type": "info",
  "body": "The Dash required for the masternode collateral may be obtained from the [faucet](https://testnet-faucet.dash.org/) (it may take several requests to get the full amount)."
}
[/block]
## Set config options

Select the testnet config, set the IP address, and the BLS key used in the previous step:
[block:code]
{
  "codes": [
    {
      "code": "node bin/mn config:default testnet\nnode bin/mn config:set externalIp <your IP>\nnode bin/mn config:set core.masternode.operator.privateKey <your BLS private key>",
      "language": "shell"
    }
  ]
}
[/block]
## Start/Stop the masternode
[block:code]
{
  "codes": [
    {
      "code": "node bin/mn start\nnode bin/mn stop",
      "language": "shell"
    }
  ]
}
[/block]

# Full Node with Platform Services

A full node that runs all Platform services can be run by simply selecting the testnet config, setting the config as show below, and starting the node.
[block:code]
{
  "codes": [
    {
      "code": "bin/mn config:default testnet\nbin/mn config:set core.masternode.enable false\nbin/mn start",
      "language": "shell"
    }
  ]
}
[/block]
# Additional information

More comprehensive details of using the mn-bootstrap tool can be found in the [mn-bootstrap README](https://github.com/dashevo/mn-bootstrap/blob/master/README.md).