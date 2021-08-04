The `dash-cli` application provides a command-line option for accessing Dash Core RPCs.

# Usage

```bash Send command
dash-cli [options] <command> [params]
```
```bash Send command (with named arguments)
  dash-cli [options] -named <command> [name=value] ... 
```
```bash List commands
dash-cli [options] help
```
```bash Get help for command
dash-cli [options] help <command>
```
[block:callout]
{
  "type": "info",
  "title": "RPC Details",
  "body": "The list of RPCs can be found [here](core-api-ref-remote-procedure-call-quick-reference) with links to more detailed information. Using dash-cli, this information is available using the `dash-cli [options] help` and `dash-cli [options] help <command>` commands."
}
[/block]
## Options:

```text
  -?
       This help message

  -conf=<file>
       Specify configuration file. Relative paths will be prefixed by datadir
       location. (default: dash.conf)

  -datadir=<dir>
       Specify data directory

  -getinfo
       Get general information from the remote server. Note that unlike
       server-side RPC calls, the results of -getinfo is the result of
       multiple non-atomic requests. Some entries in the result may
       represent results from different states (e.g. wallet balance may
       be as of a different block from the chain state reported)

  -named
       Pass named instead of positional arguments (default: false)

  -rpcclienttimeout=<n>
       Timeout in seconds during HTTP requests, or 0 for no timeout. (default:
       900)

  -rpcconnect=<ip>
       Send commands to node running on <ip> (default: 127.0.0.1)

  -rpcpassword=<pw>
       Password for JSON-RPC connections

  -rpcport=<port>
       Connect to JSON-RPC on <port> (default: 9998 or testnet: 19998)

  -rpcuser=<user>
       Username for JSON-RPC connections

  -rpcwait
       Wait for RPC server to start

  -rpcwallet=<walletname>
       Send RPC for non-default wallet on RPC server (needs to exactly match
       corresponding -wallet option passed to dashd)

  -stdin
       Read extra arguments from standard input, one per line until EOF/Ctrl-D
       (recommended for sensitive information such as passphrases). 
       When combined with -stdinrpcpass, the first line from standard
       input is used for the RPC password.

  -stdinrpcpass
       Read RPC password from standard input as a single line.  When combined
       with -stdin, the first line from standard input is used for the
       RPC password.
```

## Chain selection options:

```text
  -devnet=<name>
       Use devnet chain with provided name

  -regtest
       Enter regression test mode, which uses a special chain in which blocks
       can be solved instantly. This is intended for regression testing
       tools and app development.

  -testnet
       Use the test chain
```