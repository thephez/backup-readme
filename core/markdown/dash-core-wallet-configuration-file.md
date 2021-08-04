To use `dashd` and `dash-cli`, you will need to add a RPC password to your `dash.conf` file. Both programs will read from the same file if both run on the same system as the same user, so any long random password will work:

``` text
rpcpassword=change_this_to_a_long_random_password
```

You should also make the `dash.conf` file only readable to its owner.  On Linux, Mac OSX, and other Unix-like systems, this can be accomplished by running the following command in the Dash Core application directory:

``` text
chmod 0600 dash.conf
```

For development, it's safer and cheaper to use Dash's test network (<<glossary:testnet>>), <<glossary:regression test mode>> (regtest), or a developer network  (<<glossary:devnet>>) described below.

Questions about Dash use are best sent to the [Dash forum](https://www.dash.org/forum/categories/dash-support.61/) and [Discord channels](http://www.dashchat.org).

# Example Testnet Config

```text
testnet=1

# RPC Settings
rpcuser=user
rpcpassword=pass
rpcallowip=127.0.0.1
#----
listen=1
server=1

# Index Settings
txindex=1
addressindex=1
timestampindex=1
spentindex=1

[test]
rpcport=19998
```

# Configuration sections for different networks
[block:callout]
{
  "type": "success",
  "title": "Added in Dash Core 0.16"
}
[/block]
Since Dash Core 0.16 it is possible for a single configuration file to set different options for different networks. This is done by using sections or by prefixing the option with the network as shown below:

[block:callout]
{
  "type": "danger",
  "title": "Valid Section Names",
  "body": "Please note that the only valid section names are **`[main]`**, **`[test]`**, **`[regtest]`**, and **`[devnet]`**."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "# Enable RPC server for all networks\nserver=1\n\n[main]\n# Set custom mainnet ports\nport=9989\nrpcport=9988\n# Set custom mainnet RPC auth\nrpcuser=mainnetuser\nrpcpassword=mainnetpass\n\n[test]\n# Set custom testnet RPC auth\nrpcuser=testnetuser\nrpcpassword=testnetpass\n\n# Enabling indexing\ntxindex=1\naddressindex=1\ntimestampindex=1\nspentindex=1\n\n[regtest]\nmempoolsize=20\n\n[devnet]\nport=21999\nrpcport=21998",
      "language": "text",
      "name": "Example dash.conf"
    }
  ]
}
[/block]
With this configuration file, dashd, dash-qt, or dash-cli can be run with the `-conf=<configuration file>` along with the `-testnet`,  `-regtest`, or `-devnet=<devnet name>` parameter to select the correct settings.
[block:callout]
{
  "type": "danger",
  "body": "The following options will only apply to mainnet **_unless they are in a section_** (e.g., `[test]`): `addnode=`, `connect=`, `port=`, `bind=`, `rpcport=`, `rpcbind=` and `wallet=`. \nThe options to choose a network (`regtest=` and `testnet=`) must be specified outside of sections."
}
[/block]