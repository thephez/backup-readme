# Usage

**Start Dash Core Daemon**
```bash 
dashd [options]
```

## Options:

```text
  -?
       Print this help message and exit

  -version
       Print version and exit

  -alertnotify=<cmd>
       Execute command when a relevant alert is received or we see a really
       long fork (%s in cmd is replaced by message)

  -blocknotify=<cmd>
       Execute command when the best block changes (%s in cmd is replaced by
       block hash)

  -assumevalid=<hex>
       If this block is in the chain assume that it and its ancestors are valid
       and potentially skip their script verification (0 to verify all,
       default:
       000000000000000c01e55cb55bcb15f700d79563efcbdecf8ab5aaee33dcfa49,
       testnet:
       0000019906db5219bdc6c9a0a94facf80356bcf1afebc0d356485711a294385f)

  -conf=<file>
       Specify configuration file. Relative paths will be prefixed by datadir
       location. (default: dash.conf)

  -daemon
       Run in the background as a daemon and accept commands

  -datadir=<dir>
       Specify data directory

  -dbcache=<n>
       Set database cache size in megabytes (4 to 16384, default: 300)

  -debuglogfile=<file>
       Specify location of debug log file. Relative paths will be prefixed by a
       net-specific datadir location. (default: debug.log)

  -loadblock=<file>
       Imports blocks from external blk000??.dat file on startup

  -maxorphantxsize=<n>
       Maximum total size of all orphan transactions in megabytes (default: 10)

  -maxmempool=<n>
       Keep the transaction memory pool below <n> megabytes (default: 300)

  -mempoolexpiry=<n>
       Do not keep transactions in the mempool longer than <n> hours (default:
       336)

  -persistmempool
       Whether to save the mempool on shutdown and load on restart (default: 1)

  -syncmempool
       Sync mempool from other nodes on start (default: 1)

  -blockreconstructionextratxn=<n>
       Extra transactions to keep in memory for compact block reconstructions
       (default: 100)

  -par=<n>
       Set the number of script verification threads (-4 to 16, 0 = auto, <0 =
       leave that many cores free, default: 0)

  -pid=<file>
       Specify pid file. Relative paths will be prefixed by a net-specific
       datadir location. (default: dashd.pid)

  -prune=<n>
       Reduce storage requirements by enabling pruning (deleting) of old
       blocks. This allows the pruneblockchain RPC to be called to
       delete specific blocks, and enables automatic pruning of old
       blocks if a target size in MiB is provided. This mode is
       incompatible with -txindex, -rescan and -disablegovernance=false.
       Warning: Reverting this setting requires re-downloading the
       entire blockchain. (default: 0 = disable pruning blocks, 1 =
       allow manual pruning via RPC, >945 = automatically prune block
       files to stay under the specified target size in MiB)

  -reindex-chainstate
       Rebuild chain state from the currently indexed blocks

  -reindex
       Rebuild chain state and block index from the blk*.dat files on disk

  -sysperms
       Create new files with system default permissions, instead of umask 077
       (only effective with disabled wallet functionality)

  -txindex
       Maintain a full transaction index, used by the getrawtransaction rpc
       call (default: 1)

  -addressindex
       Maintain a full address index, used to query for the balance, txids and
       unspent outputs for addresses (default: 0)

  -timestampindex
       Maintain a timestamp index for block hashes, used to query blocks hashes
       by a range of timestamps (default: 0)

  -spentindex
       Maintain a full spent index, used to query the spending txid and input
       index for an outpoint (default: 0)
```

## Connection options:

```text
  -addnode=<ip>
       Add a node to connect to and attempt to keep the connection open (see
       the `addnode` RPC command help for more info)

  -allowprivatenet
       Allow RFC1918 addresses to be relayed and connected to (default: 0)

  -banscore=<n>
       Threshold for disconnecting misbehaving peers (default: 100)

  -bantime=<n>
       Number of seconds to keep misbehaving peers from reconnecting (default:
       86400)

  -bind=<addr>
       Bind to given address and always listen on it. Use [host]:port notation
       for IPv6

  -connect=<ip>
       Connect only to the specified node(s); -connect=0 disables automatic
       connections (the rules for this peer are the same as for
       -addnode)

  -discover
       Discover own IP addresses (default: 1 when listening and no -externalip
       or -proxy)

  -dns
       Allow DNS lookups for -addnode, -seednode and -connect (default: 1)

  -dnsseed
       Query for peer addresses via DNS lookup, if low on addresses (default: 1
       unless -connect used)

  -enablebip61
       Send reject messages per BIP61 (default: 1)

  -externalip=<ip>
       Specify your own public address

  -forcednsseed
       Always query for peer addresses via DNS lookup (default: 0)

  -listen
       Accept connections from outside (default: 1 if no -proxy or -connect)

  -listenonion
       Automatically create Tor hidden service (default: 1)

  -maxconnections=<n>
       Maintain at most <n> connections to peers (temporary service connections
       excluded) (default: 125)

  -maxreceivebuffer=<n>
       Maximum per-connection receive buffer, <n>*1000 bytes (default: 5000)

  -maxsendbuffer=<n>
       Maximum per-connection send buffer, <n>*1000 bytes (default: 1000)

  -maxtimeadjustment
       Maximum allowed median peer time offset adjustment. Local perspective of
       time may be influenced by peers forward or backward by this
       amount. (default: 4200 seconds)

  -onion=<ip:port>
       Use separate SOCKS5 proxy to reach peers via Tor hidden services
       (default: -proxy)

  -onlynet=<net>
       Only connect to nodes in network <net> (ipv4, ipv6 or onion)

  -permitbaremultisig
       Relay non-P2SH multisig (default: 1)

  -peerbloomfilters
       Support filtering of blocks and transaction with bloom filters (default:
       1)

  -port=<port>
       Listen for connections on <port> (default: 9999 or testnet: 19999)

  -proxy=<ip:port>
       Connect through SOCKS5 proxy

  -proxyrandomize
       Randomize credentials for every proxy connection. This enables Tor
       stream isolation (default: 1)

  -seednode=<ip>
       Connect to a node to retrieve peer addresses, and disconnect

  -socketevents=<mode>
       Socket events mode, which must be one of: 'select', 'poll', 'epoll'
       (default: epoll)

  -timeout=<n>
       Specify connection timeout in milliseconds (minimum: 1, default: 5000)

  -torcontrol=<ip>:<port>
       Tor control port to use if onion listening enabled (default:
       127.0.0.1:9051)

  -torpassword=<pass>
       Tor control port password (default: empty)

  -upnp
       Use UPnP to map the listening port (default: 0)

  -whitebind=<addr>
       Bind to given address and whitelist peers connecting to it. Use
       [host]:port notation for IPv6

  -whitelist=<IP address or network>
       Whitelist peers connecting from the given IP address (e.g. 1.2.3.4) or
       CIDR notated network (e.g. 1.2.3.0/24). Can be specified multiple
       times. Whitelisted peers cannot be DoS banned and their
       transactions are always relayed, even if they are already in the
       mempool, useful e.g. for a gateway

  -maxuploadtarget=<n>
       Tries to keep outbound traffic under the given target (in MiB per 24h),
       0 = no limit (default: 0)
```

## Wallet options:

```text
  -disablewallet
       Do not load the wallet and disable wallet RPC calls

  -keypool=<n>
       Set key pool size to <n> (default: 1000)

  -fallbackfee=<amt>
       A fee rate (in DASH/kB) that will be used when fee estimation has
       insufficient data (default: 0.00001)

  -discardfee=<amt>
       The fee rate (in DASH/kB) that indicates your tolerance for discarding
       change by adding it to the fee (default: 0.0001). Note: An output
       is discarded if it is dust at this rate, but we will always
       discard up to the dust relay fee and a discard fee above that is
       limited by the fee estimate for the longest target

  -mintxfee=<amt>
       Fees (in DASH/kB) smaller than this are considered zero fee for
       transaction creation (default: 0.00001)

  -paytxfee=<amt>
       Fee (in DASH/kB) to add to transactions you send (default: 0.00)

  -rescan
       Rescan the block chain for missing wallet transactions on startup

  -salvagewallet
       Attempt to recover private keys from a corrupt wallet on startup

  -spendzeroconfchange
       Spend unconfirmed change when sending transactions (default: 1)

  -txconfirmtarget=<n>
       If paytxfee is not set, include enough fee so transactions begin
       confirmation on average within n blocks (default: 6)

  -usehd
       Use hierarchical deterministic key generation (HD) after BIP39/BIP44.
       Only has effect during wallet creation/first start (default: 0)

  -mnemonic=<text>
       User defined mnemonic for HD wallet (bip39). Only has effect during
       wallet creation/first start (default: randomly generated)

  -mnemonicpassphrase=<text>
       User defined mnemonic passphrase for HD wallet (BIP39). Only has effect
       during wallet creation/first start (default: empty string)

  -hdseed=<hex>
       User defined seed for HD wallet (should be in hex). Only has effect
       during wallet creation/first start (default: randomly generated)

  -upgradewallet
       Upgrade wallet to latest format on startup

  -wallet=<path>
       Specify wallet database path. Can be specified multiple times to load
       multiple wallets. Path is interpreted relative to <walletdir> if
       it is not absolute, and will be created if it does not exist (as
       a directory containing a wallet.dat file and log files). For
       backwards compatibility this will also accept names of existing
       data files in <walletdir>.)

  -walletbroadcast
       Make the wallet broadcast transactions (default: 1)

  -walletdir=<dir>
       Specify directory to hold wallets (default: <datadir>/wallets if it
       exists, otherwise <datadir>)

  -walletnotify=<cmd>
       Execute command when a wallet transaction changes (%s in cmd is replaced
       by TxID)

  -zapwallettxes=<mode>
       Delete all wallet transactions and only recover those parts of the
       blockchain through -rescan on startup (1 = keep tx meta data e.g.
       account owner and payment request information, 2 = drop tx meta
       data)

  -createwalletbackups=<n>
       Number of automatic wallet backups (default: 10)

  -walletbackupsdir=<dir>
       Specify full path to directory for automatic wallet backups (must exist)

  -keepass
       Use KeePass 2 integration using KeePassHttp plugin (default: 0)

  -keepassport=<port>
       Connect to KeePassHttp on port <port> (default: 19455)

  -keepasskey=<key>
       KeePassHttp key for AES encrypted communication with KeePass

  -keepassid=<id>
       KeePassHttp id for the established association

  -keepassname=<name>
       Name to construct url for KeePass entry that stores the wallet
       passphrase
```

##PrivateSend options:

```
  -enableprivatesend
       Enable use of PrivateSend for funds stored in this wallet (0-1, default:
       0)

  -privatesendautostart
       Start PrivateSend automatically (0-1, default: 0)

  -privatesendmultisession
       Enable multiple PrivateSend mixing sessions per block, experimental
       (0-1, default: 0)

  -privatesendsessions=<n>
       Use N separate masternodes in parallel to mix funds (1-10, default: 4)

  -privatesendrounds=<n>
       Use N separate masternodes for each denominated input to mix funds
       (2-16, default: 4)

  -privatesendamount=<n>
       Target PrivateSend balance (2-21000000, default: 1000)

  -privatesenddenomsgoal=<n>
       Try to create at least N inputs of each denominated amount (10-100000,
       default: 50)

  -privatesenddenomshardcap=<n>
       Create up to N inputs of each denominated amount (10-100000, default:
       300)
```

## ZeroMQ notification options:

```
  -zmqpubhashblock=<address>
       Enable publish hash block in <address>

  -zmqpubhashtx=<address>
       Enable publish hash transaction in <address>

  -zmqpubhashtxlock=<address>
       Enable publish hash transaction (locked via InstantSend) in <address>

  -zmqpubhashgovernancevote=<address>
       Enable publish hash of governance votes in <address>

  -zmqpubhashgovernanceobject=<address>
       Enable publish hash of governance objects (like proposals) in <address>

  -zmqpubhashinstantsenddoublespend=<address>
       Enable publish transaction hashes of attempted InstantSend double spend
       in <address>

  -zmqpubrawblock=<address>
       Enable publish raw block in <address>

  -zmqpubrawtx=<address>
       Enable publish raw transaction in <address>

  -zmqpubrawtxlock=<address>
       Enable publish raw transaction (locked via InstantSend) in <address>

  -zmqpubrawinstantsenddoublespend=<address>
       Enable publish raw transactions of attempted InstantSend double spend in
       <address>
```

## Debugging/Testing options:

```
  -uacomment=<cmt>
       Append comment to the user agent string

  -debug=<category>
       Output debugging information (default: 0, supplying <category> is
       optional). If <category> is not supplied or if <category> = 1,
       output all debugging information. <category> can be: net, tor,
       mempool, http, bench, zmq, db, rpc, estimatefee, addrman,
       selectcoins, reindex, cmpctblock, rand, prune, proxy, mempoolrej,
       libevent, coindb, qt, leveldb, chainlocks, gobject, instantsend,
       keepass, llmq, llmq-dkg, llmq-sigs, mnpayments, mnsync,
       privatesend, spork, netconn.

  -debugexclude=<category>
       Exclude debugging information for a category. Can be used in conjunction
       with -debug=1 to output debug logs for all categories except one
       or more specified categories.

  -help-debug
       Show all debugging options (usage: --help -help-debug)

  -logips
       Include IP addresses in debug output (default: 0)

  -logtimestamps
       Prepend debug output with timestamp (default: 1)

  -maxtxfee=<amt>
       Maximum total fees (in DASH) to use in a single wallet transaction or
       raw transaction; setting this too low may abort large
       transactions (default: 0.10)

  -printtoconsole
       Send trace/debug info to console instead of debug.log file

  -printtodebuglog
       Send trace/debug info to debug.log file (default: 1)

  -shrinkdebugfile
       Shrink debug.log file on client startup (default: 1 when no -debug)
```

## Chain selection options:

```
  -testnet
       Use the test chain

  -devnet=<name>
       Use devnet chain with provided name

  -disablegovernance
       Disable governance validation (0-1, default: 0)

  -sporkaddr=<dashaddress>
       Override spork address. Only useful for regtest and devnet. Using this
       on mainnet or testnet will ban you.

  -minsporkkeys=<n>
       Overrides minimum spork signers to change spork value. Only useful for
       regtest and devnet. Using this on mainnet or testnet will ban
       you.
```

## Masternode options:

```
  -masternodeblsprivkey=<hex>
       Set the masternode BLS private key and enable the client to act as a
       masternode
```

## InstantSend options:

```
  -instantsendnotify=<cmd>
       Execute command when a wallet InstantSend transaction is successfully
       locked (%s in cmd is replaced by TxID)
```

## Node relay options:

```
  -bytespersigop
       Minimum bytes per sigop in transactions we relay and mine (default: 20)

  -datacarrier
       Relay and mine data carrier transactions (default: 1)

  -datacarriersize
       Maximum size of data in data carrier transactions we relay and mine
       (default: 83)

  -minrelaytxfee=<amt>
       Fees (in DASH/kB) smaller than this are considered zero fee for
       relaying, mining and transaction creation (default: 0.00001)

  -whitelistrelay
       Accept relayed transactions received from whitelisted peers even when
       not relaying transactions (default: 1)

  -whitelistforcerelay
       Force relay of transactions from whitelisted peers even if they violate
       local relay policy (default: 1)
```

## Block creation options:

```
  -blockmaxsize=<n>
       Set maximum block size in bytes (default: 2000000)

  -blockmintxfee=<amt>
       Set lowest fee rate (in DASH/kB) for transactions to be included in
       block creation. (default: 0.00001)
```

## RPC server options:

```
  -server
       Accept command line and JSON-RPC commands

  -rest
       Accept public REST requests (default: 0)

  -rpcbind=<addr>[:port]
       Bind to given address to listen for JSON-RPC connections. This option is
       ignored unless -rpcallowip is also passed. Port is optional and
       overrides -rpcport. Use [host]:port notation for IPv6. This
       option can be specified multiple times (default: 127.0.0.1 and
       ::1 i.e., localhost, or if -rpcallowip has been specified,
       0.0.0.0 and :: i.e., all addresses)

  -rpccookiefile=<loc>
       Location of the auth cookie. Relative paths will be prefixed by a
       net-specific datadir location. (default: data dir)

  -rpcuser=<user>
       Username for JSON-RPC connections

  -rpcpassword=<pw>
       Password for JSON-RPC connections

  -rpcauth=<userpw>
       Username and hashed password for JSON-RPC connections. The field
       <userpw> comes in the format: <USERNAME>:<SALT>$<HASH>. A
       canonical python script is included in share/rpcuser. The client
       then connects normally using the
       rpcuser=<USERNAME>/rpcpassword=<PASSWORD> pair of arguments. This
       option can be specified multiple times

  -rpcport=<port>
       Listen for JSON-RPC connections on <port> (default: 9998 or testnet:
       19998)

  -rpcallowip=<ip>
       Allow JSON-RPC connections from specified source. Valid for <ip> are a
       single IP (e.g. 1.2.3.4), a network/netmask (e.g.
       1.2.3.4/255.255.255.0) or a network/CIDR (e.g. 1.2.3.4/24). This
       option can be specified multiple times

  -rpcthreads=<n>
       Set the number of threads to service RPC calls (default: 4)
```