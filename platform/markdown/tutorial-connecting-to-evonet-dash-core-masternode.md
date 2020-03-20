The purpose of this tutorial is to walk through the steps necessary to set up a masternode with Dash Platform services.
[block:callout]
{
  "type": "danger",
  "body": "Running a masternode on Evonet requires familiarity with Dash Platform services. As with testnet, improper configuration may impact testing so please exercise caution if running a masternode.",
  "title": "Advanced Topic"
}
[/block]
# Prerequisites
- Access to a Linux VPS (Ubuntu 18.04 LTS recommended) configured with a non-root user ([guide](https://docs.dash.org/en/stable/masternodes/setup.html#set-up-your-vps))
- [Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/) (v18.06.0+) and [docker-compose](https://docs.docker.com/compose/install/) (v1.25.0+) installed

# Masternode setup
Clone the repository and edit the configuration file
[block:code]
{
  "codes": [
    {
      "code": "git clone https://github.com/dashevo/mn-bootstrap.git\ncd mn-bootstrap\ncp .env.evonet .env\nnano .env",
      "language": "shell"
    }
  ]
}
[/block]
Paste in appropriate values for `CORE_EXTERNAL_IP` and `CORE_MASTERNODE_BLS_PRIV_KEY`. Generate a new BLS key using `dash-cli bls generate` or from Dash Core debug console with `bls generate` if necessary.

# Start the masternode
[block:code]
{
  "codes": [
    {
      "code": "docker-compose up -d",
      "language": "text"
    }
  ]
}
[/block]
Then obtain 1000 Evonet tDASH and create your protx registration transaction as described [here](https://docs.dash.org/en/stable/masternodes/setup.html#option-2-registering-from-dash-core-wallet).