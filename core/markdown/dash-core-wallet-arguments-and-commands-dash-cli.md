The `dash-cli` application provides a command-line option for accessing Dash Core RPCs.

# Usage

```bash Send a command
dash-cli [options] <command> [params]
```
```bash Send a command (with named arguments)
  dash-cli [options] -named <command> [name=value] ... 
```
```bash List commands
dash-cli [options] help
```
```bash Get help for a command
dash-cli [options] help <command>
```

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
```

## Chain selection options:

```text
  -testnet
       Use the test chain

  -devnet=<name>
       Use devnet chain with provided name

  -regtest
       Enter regression test mode, which uses a special chain in which blocks
       can be solved instantly. This is intended for regression testing
       tools and app development.

  -named
       Pass named instead of positional arguments (default: false)

  -rpcconnect=<ip>
       Send commands to node running on <ip> (default: 127.0.0.1)

  -rpcport=<port>
       Connect to JSON-RPC on <port> (default: 9998 or testnet: 19998)

  -rpcwait
       Wait for RPC server to start

  -rpcuser=<user>
       Username for JSON-RPC connections

  -rpcpassword=<pw>
       Password for JSON-RPC connections

  -rpcclienttimeout=<n>
       Timeout in seconds during HTTP requests, or 0 for no timeout. (default:
       900)

  -stdinrpcpass
       Read RPC password from standard input as a single line.  When combined
       with -stdin, the first line from standard input is used for the
       RPC password.

  -stdin
       Read extra arguments from standard input, one per line until EOF/Ctrl-D
       (recommended for sensitive information such as passphrases). 
       When combined with -stdinrpcpass, the first line from standard
       input is used for the RPC password.

  -rpcwallet=<walletname>
       Send RPC for non-default wallet on RPC server (needs to exactly match
       corresponding -wallet option passed to dashd)
```