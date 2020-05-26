# [Addressindex RPCs](core-api-ref-remote-procedure-calls-address-index)

These RPCs are all Dash-specific and not found in Bitcoin Core

* [GetAddressBalance](core-api-ref-remote-procedure-calls-address-index#getaddressbalance): returns the balance for address(es).
* [GetAddressDeltas](core-api-ref-remote-procedure-calls-address-index#getaddressdeltas): returns all changes for an address.
* [GetAddressMempool](core-api-ref-remote-procedure-calls-address-index#getaddressmempool): returns all mempool deltas for an address.
* [GetAddressTxids](core-api-ref-remote-procedure-calls-address-index#getaddresstxids): returns the txids for an address(es).
* [GetAddressUtxos](core-api-ref-remote-procedure-calls-address-index#getaddressutxos): returns all unspent outputs for an address.

# [Block Chain RPCs](core-api-ref-remote-procedure-calls-blockchain)

* [GetBestBlockHash](core-api-ref-remote-procedure-calls-blockchain#getbestblockhash): returns the header hash of the most recent block on the best block chain.
* [GetBestChainLock](core-api-ref-remote-procedure-calls-blockchain#getbestchainlock): returns the block hash of the best chainlock. **_New in Dash Core 0.15.0_**
* [GetBlock](core-api-ref-remote-procedure-calls-blockchain#getblock): gets a block with a particular header hash from the local block database either as a JSON object or as a serialized block. **Updated in Dash Core 0.16.0**
* [GetBlockChainInfo](core-api-ref-remote-procedure-calls-blockchain#getblockchaininfo): provides information about the current state of the block chain. **Updated in Dash Core 0.16.0**
* [GetBlockCount](core-api-ref-remote-procedure-calls-blockchain#getblockcount): returns the number of blocks in the local best block chain.
* [GetBlockHash](core-api-ref-remote-procedure-calls-blockchain#getblockhash): returns the header hash of a block at the given height in the local best block chain.
* [GetBlockHashes](core-api-ref-remote-procedure-calls-blockchain#getblockhashes): returns array of hashes of blocks within the timestamp range provided (requires `timestampindex` to be enabled). New in Dash Core 0.12.1
* [GetBlockHeader](core-api-ref-remote-procedure-calls-blockchain#getblockheader): gets a block header with a particular header hash from the local block database either as a JSON object or as a serialized block header. **Updated in Dash Core 0.16.0**
* [GetBlockHeaders](core-api-ref-remote-procedure-calls-blockchain#getblockheaders): returns an array of items with information about the requested number of blockheaders starting from the requested hash. New in Dash Core 0.12.1
* [GetBlockStats](core-api-ref-remote-procedure-calls-blockchain#getblockstats): computes per block statistics for a given window. **_New in Dash Core 0.15.0_**
* [GetChainTips](core-api-ref-remote-procedure-calls-blockchain#getchaintips): returns information about the highest-height block (tip) of each local block chain. *Updated in Dash Core 0.12.3*
* [GetChainTxStats](core-api-ref-remote-procedure-calls-blockchain#getchaintx-stats): compute statistics about the total number and rate of transactions in the chain. **Updated in Dash Core 0.16.0**
* [GetDifficulty](core-api-ref-remote-procedure-calls-blockchain#getdifficulty): returns the proof-of-work difficulty as a multiple of the minimum difficulty.
* [GetMemPoolAncestors](core-api-ref-remote-procedure-calls-blockchain#getmempoolancestors): returns all in-mempool ancestors for a transaction in the mempool. _Updated in Dash Core 0.14.0_
* [GetMemPoolDescendants](core-api-ref-remote-procedure-calls-blockchain#getmempooldescendants): returns all in-mempool descendants for a transaction in the mempool. _Updated in Dash Core 0.14.0_
* [GetMemPoolEntry](core-api-ref-remote-procedure-calls-blockchain#getmempoolentry): returns mempool data for given transaction (must be in mempool). _Updated in Dash Core 0.14.0_
* [GetMemPoolInfo](core-api-ref-remote-procedure-calls-blockchain#getmempoolinfo): returns information about the node's current transaction memory pool. **Updated in Dash Core 0.16.0**
* [GetRawMemPool](core-api-ref-remote-procedure-calls-blockchain#getrawmempool): returns all transaction identifiers (TXIDs) in the memory pool as a JSON array, or detailed information about each transaction in the memory pool as a JSON object. **_Updated in Dash Core 0.15.0_**
* [GetMerkleBlocks](core-api-ref-remote-procedure-calls-blockchain#getmerkleblocks): returns an array of hex-encoded merkleblocks for <count> blocks starting from <hash> which match <filter>. **_New in Dash Core 0.15.0_**
* [GetSpecialTxes](core-api-ref-remote-procedure-calls-blockchain#getspecialtxes): returns an array of special transactions found in the specified block **New in Dash Core 0.13.1**
* [GetSpentInfo](core-api-ref-remote-procedure-calls-blockchain#getspentinfo): returns the txid and index where an output is spent (requires `spentindex` to be enabled). New in Dash Core 0.12.1
* [GetTxOut](core-api-ref-remote-procedure-calls-blockchain#gettxout): returns details about an unspent transaction output (UTXO). **_Updated in Dash Core 0.15.0_**
* [GetTxOutProof](core-api-ref-remote-procedure-calls-blockchain#gettxoutproof): returns a hex-encoded proof that one or more specified transactions were included in a block.
* [GetTxOutSetInfo](core-api-ref-remote-procedure-calls-blockchain#gettxoutsetinfo): returns statistics about the confirmed unspent transaction output (UTXO) set. Note that this call may take some time and that it only counts outputs from confirmed transactions---it does not count outputs from the memory pool. **_Updated in Dash Core 0.15.0_**
* [PreciousBlock](core-api-ref-remote-procedure-calls-blockchain#preciousblock): treats a block as if it were received before others with the same work. *New in Dash Core 0.12.3*
* [PruneBlockChain](core-api-ref-remote-procedure-calls-blockchain#pruneblockchain): prunes the blockchain up to a specified height or timestamp. *New in Dash Core 0.12.3*
* [SaveMemPool](core-api-ref-remote-procedure-calls-blockchain#savemempool): dumps the mempool to disk. **New in Dash Core 0.16.0**
* [VerifyChain](core-api-ref-remote-procedure-calls-blockchain#verifychain): verifies each entry in the local block chain database.
* [VerifyTxOutProof](core-api-ref-remote-procedure-calls-blockchain#verifytxoutproof): verifies that a proof points to one or more transactions in a block, returning the transactions the proof commits to and throwing an RPC error if the block is not in our best block chain.

# [Control RPCs](core-api-ref-remote-procedure-calls-control)

* [Debug](core-api-ref-remote-procedure-calls-control#debug): changes the debug category from the console. _Updated in Dash Core 0.14.0_
* [GetMemoryInfo](core-api-ref-remote-procedure-calls-control#getmemoryinfo): returns information about memory usage. **_Updated in Dash Core 0.15.0_**
* [Help](core-api-ref-remote-procedure-calls-control#help): lists all available public RPC commands, or gets help for the specified RPC.  Commands which are unavailable will not be listed, such as wallet RPCs if wallet support is disabled.
* [Logging](core-api-ref-remote-procedure-calls-control#logging): gets and sets the logging configuration **_New in Dash Core 0.15.0_**
* [Stop](core-api-ref-remote-procedure-calls-control#stop): safely shuts down the Dash Core server.
* [Uptime](core-api-ref-remote-procedure-calls-control#uptime): returns the total uptime of the server. **_New in Dash Core 0.15.0_**

# [Dash RPCs](core-api-ref-remote-procedure-calls-dash)

* [GetGovernanceInfo](core-api-ref-remote-procedure-calls-dash#getgovernanceinfo): returns an object containing governance parameters. _Updated in Dash Core 0.14.0_
* [GetPoolInfo](core-api-ref-remote-procedure-calls-dash#getpoolinfo): returns an object containing mixing pool related information. **_Deprecated in 0.15.0_**
* [GetPrivateSendInfo](core-api-ref-remote-procedure-calls-dash#getprivatesendinfo): returns an object containing an information about PrivateSend settings and state. **_New in Dash Core 0.15.0_**
* [GetSuperblockBudget](core-api-ref-remote-procedure-calls-dash#getsuperblockbudget): returns the absolute maximum sum of superblock payments allowed.
* [GObject](core-api-ref-remote-procedure-calls-dash#gobject): provides a set of commands for managing governance objects and displaying information about them. **_Updated in Dash Core 0.15.0_**
* [Masternode](core-api-ref-remote-procedure-calls-dash#masternode): provides a set of commands for managing masternodes and displaying information about them. _Updated in Dash Core 0.14.0_
* [MasternodeList](core-api-ref-remote-procedure-calls-dash#masternodelist): returns a list of masternodes in different modes. _Updated in Dash Core 0.14.0_
* [MnSync](core-api-ref-remote-procedure-calls-dash#mnsync): returns the sync status, updates to the next step or resets it entirely. _Updated in Dash Core 0.14.0_
* [PrivateSend](core-api-ref-remote-procedure-calls-dash#privatesend): controls the mixing process. *Updated in Dash Core 0.12.3*
* [Spork](core-api-ref-remote-procedure-calls-dash#spork): reads or updates spork settings on the network.
* [VoteRaw](core-api-ref-remote-procedure-calls-dash#voteraw): compiles and relays a governance vote with provided external signature instead of signing vote internally

# [Evolution RPCs](core-api-ref-remote-procedure-calls-evo)

* [BLS](core-api-ref-remote-procedure-calls-evo#bls): provides a set of commands to execute BLS-related actions. _Updated in Dash Core 0.14.0_
* [ProTx](core-api-ref-remote-procedure-calls-evo#protx): provides a set of commands to execute ProTx related actions. **Updated in Dash Core 0.16.0**
* [Quorum](core-api-ref-remote-procedure-calls-evo#quorum): provides a set of commands for quorums (LLMQs). **Updated in Dash Core 0.16.0**

# [Generating RPCs](core-api-ref-remote-procedure-calls-generating)

* [Generate](core-api-ref-remote-procedure-calls-generating#generate): mines blocks immediately (before the RPC call returns). *Updated in Dash Core 0.12.3*
* [GenerateToAddress](core-api-ref-remote-procedure-calls-generating#generatetoaddress): mines blocks immediately to a specified address. *New in Dash Core 0.12.3*

# [Mining RPCs](core-api-ref-remote-procedure-calls-mining)

* [GetBlockTemplate](core-api-ref-remote-procedure-calls-mining#getblocktemplate): gets a block template or proposal for use with mining software. *Updated in Dash Core 0.13.0*
* [GetMiningInfo](core-api-ref-remote-procedure-calls-mining#getmininginfo): returns various mining-related information. **Updated in Dash Core 0.16.0**
* [GetNetworkHashPS](core-api-ref-remote-procedure-calls-mining#getnetworkhashps): returns the estimated network hashes per second based on the last n blocks.
* [PrioritiseTransaction](core-api-ref-remote-procedure-calls-mining#prioritisetransaction): adds virtual priority or fee to a transaction, allowing it to be accepted into blocks mined by this node (or miners which use this node) with a lower priority or fee. (It can also remove virtual priority or fee, requiring the transaction have a higher priority or fee to be accepted into a locally-mined block.) _Updated in Dash Core 0.14.0_
* [SubmitBlock](core-api-ref-remote-procedure-calls-mining#submitblock): accepts a block, verifies it is a valid addition to the block chain, and broadcasts it to the network. Extra parameters are ignored by Dash Core but may be used by mining pools or other programs.

# [Network RPCs](core-api-ref-remote-procedure-calls-network)

* [AddNode](core-api-ref-remote-procedure-calls-network#addnode): attempts to add or remove a node from the addnode list, or to try a connection to a node once.
* [ClearBanned](core-api-ref-remote-procedure-calls-network#clearbanned): clears list of banned nodes.
* [DisconnectNode](core-api-ref-remote-procedure-calls-network#disconnectnode): immediately disconnects from a specified node. **_Updated in Dash Core 0.15.0_**
* [GetAddedNodeInfo](core-api-ref-remote-procedure-calls-network#getaddednodeinfo): returns information about the given added node, or all added nodes (except onetry nodes). Only nodes which have been manually added using the [`addnode` RPC](core-api-ref-remote-procedure-calls-network#addnode) will have their information displayed. *Updated in Dash Core 0.12.3*
* [GetConnectionCount](core-api-ref-remote-procedure-calls-network#getconnectioncount): returns the number of connections to other nodes.
* [GetNetTotals](core-api-ref-remote-procedure-calls-network#getnettotals): returns information about network traffic, including bytes in, bytes out, and the current time.
* [GetNetworkInfo](core-api-ref-remote-procedure-calls-network#getnetworkinfo): returns information about the node's connection to the network. **Updated in Dash Core 0.16.0**
* [GetPeerInfo](core-api-ref-remote-procedure-calls-network#getpeerinfo): returns data about each connected network node. **Updated in Dash Core 0.16.0**
* [ListBanned](core-api-ref-remote-procedure-calls-network#listbanned): lists all banned IPs/Subnets.
* [Ping](core-api-ref-remote-procedure-calls-network#ping): sends a P2P ping message to all connected nodes to measure ping time. Results are provided by the [`getpeerinfo` RPC](core-api-ref-remote-procedure-calls-network#getpeerinfo) pingtime and pingwait fields as decimal seconds. The P2P [`ping` message](core-ref-p2p-network-control-messages#ping) is handled in a queue with all other commands, so it measures processing backlog, not just network ping.
* [SetBan](core-api-ref-remote-procedure-calls-network#setban): attempts add or remove a IP/Subnet from the banned list.
* [SetNetworkActive](core-api-ref-remote-procedure-calls-network#setnetworkactive): disables/enables all P2P network activity.

# [Raw Transaction RPCs](core-api-ref-remote-procedure-calls-raw-transactions)

* [CombineRawTransaction](core-api-ref-remote-procedure-calls-raw-transactions#combinerawtransaction): combine multiple partially signed transactions into one transaction. **_New in Dash Core 0.15.0_**
* [CreateRawTransaction](core-api-ref-remote-procedure-calls-raw-transactions#createrawtransaction): creates an unsigned serialized transaction that spends a previous output to a new output with a P2PKH or P2SH address. The transaction is not stored in the wallet or transmitted to the network. *Updated in Dash Core 0.12.3*
* [DecodeRawTransaction](core-api-ref-remote-procedure-calls-raw-transactions#decoderawtransaction): decodes a serialized transaction hex string into a JSON object describing the transaction. *Updated in Dash Core 0.13.0*
* [DecodeScript](core-api-ref-remote-procedure-calls-raw-transactions#decodescript): decodes a hex-encoded P2SH redeem script.
* [FundRawTransaction](core-api-ref-remote-procedure-calls-raw-transactions#fundrawtransaction): adds inputs to a transaction until it has enough in value to meet its out value. **_Updated in Dash Core 0.15.0_**
* [GetRawTransaction](core-api-ref-remote-procedure-calls-raw-transactions#getrawtransaction): gets a hex-encoded serialized transaction or a JSON object describing the transaction. By default, Dash Core only stores complete transaction data for UTXOs and your own transactions, so the RPC may fail on historic transactions unless you use the non-default `txindex=1` in your Dash Core startup settings. **Updated in Dash Core 0.16.0**
* [SendRawTransaction](core-api-ref-remote-procedure-calls-raw-transactions#sendrawtransaction): validates a transaction and broadcasts it to the peer-to-peer network. **_Updated in Dash Core 0.15.0_**
* [SignRawTransaction](core-api-ref-remote-procedure-calls-raw-transactions#signrawtransaction): signs a transaction in the serialized transaction format using private keys stored in the wallet or provided in the call.

# [Utility RPCs](core-api-ref-remote-procedure-calls-util)

* [CreateMultiSig](core-api-ref-remote-procedure-calls-util#createmultisig): creates a P2SH multi-signature address. **Updated in Dash Core 0.16.0**
* [EstimateFee](core-api-ref-remote-procedure-calls-util#estimatefee): estimates the transaction fee per kilobyte that needs to be paid for a transaction to begin confirmation within a certain number of blocks.
* [EstimateSmartFee](core-api-ref-remote-procedure-calls-util#estimatesmartfee): estimates the transaction fee per kilobyte that needs to be paid for a transaction to begin confirmation within a certain number of blocks and returns the number of blocks for which the estimate is valid. **_Updated in Dash Core 0.15.0_**
* [SignMessageWithPrivKey](core-api-ref-remote-procedure-calls-util#signmessagewithprivkey): signs a message with a given private key.  *New in Dash Core 0.12.3*
* [ValidateAddress](core-api-ref-remote-procedure-calls-util#validateaddress): returns information about the given Dash address. *Updated in Dash Core 0.12.3*
* [VerifyMessage](core-api-ref-remote-procedure-calls-util#verifymessage): verifies a signed message.

# [Wallet RPCs](core-api-ref-remote-procedure-calls-wallet)

**Note:** the wallet RPCs are only available if Dash Core was built with <<glossary:wallet support>>, which is the default.

* [AbandonTransaction](core-api-ref-remote-procedure-calls-wallet#abandontransaction): marks an in-wallet transaction and all its in-wallet descendants as abandoned. This allows their inputs to be respent.
* [AbortRescan](core-api-ref-remote-procedure-calls-wallet#abortrescan): stops current wallet rescan. **_New in Dash Core 0.15.0_**
* [AddMultiSigAddress](core-api-ref-remote-procedure-calls-wallet#addmultisigaddress): adds a P2SH multisig address to the wallet. **Updated in Dash Core 0.16.0**
* [BackupWallet](core-api-ref-remote-procedure-calls-wallet#backupwallet): safely copies `wallet.dat` to the specified file, which can be a directory or a path with filename.
* [DumpHDInfo](core-api-ref-remote-procedure-calls-wallet#dumphdinfo): returns an object containing sensitive private info about this HD wallet New in Dash Core 0.12.2
* [DumpPrivKey](core-api-ref-remote-procedure-calls-wallet#dumpprivkey): returns the wallet-import-format (WIP) private key corresponding to an address. (But does not remove it from the wallet.)
* [DumpWallet](core-api-ref-remote-procedure-calls-wallet#dumpwallet): creates or overwrites a file with all wallet keys in a human-readable format. *Updated in Dash Core 0.13.0*
* [EncryptWallet](core-api-ref-remote-procedure-calls-wallet#encryptwallet): encrypts the wallet with a passphrase.  This is only to enable encryption for the first time. After encryption is enabled, you will need to enter the passphrase to use private keys.
* [GetBalance](core-api-ref-remote-procedure-calls-wallet#getbalance): gets the balance in decimal dash across all accounts or for a particular account. *Updated in Dash Core 0.13.0*
* [GetNewAddress](core-api-ref-remote-procedure-calls-wallet#getnewaddress): returns a new Dash address for receiving payments. If an account is specified, payments received with the address will be credited to that account.
* [GetRawChangeAddress](core-api-ref-remote-procedure-calls-wallet#getrawchangeaddress): returns a new Dash address for receiving change. This is for use with raw transactions, not normal use.
* [GetReceivedByAddress](core-api-ref-remote-procedure-calls-wallet#getreceivedbyaddress): returns the total amount received by the specified address in transactions with the specified number of confirmations. It does not count coinbase transactions. *Updated in Dash Core 0.13.0*
* [GetTransaction](core-api-ref-remote-procedure-calls-wallet#gettransaction): gets detailed information about an in-wallet transaction. _Updated in Dash Core 0.14.0_
* [GetUnconfirmedBalance](core-api-ref-remote-procedure-calls-wallet#getunconfirmedbalance): returns the wallet's total unconfirmed balance.
* [GetWalletInfo](core-api-ref-remote-procedure-calls-wallet#getwalletinfo): provides information about the wallet.  *Updated in Dash Core 0.12.3*
* [ImportAddress](core-api-ref-remote-procedure-calls-wallet#importaddress): adds an address or pubkey script to the wallet without the associated private key, allowing you to watch for transactions affecting that address or pubkey script without being able to spend any of its outputs.
* [ImportElectrumWallet](core-api-ref-remote-procedure-calls-wallet#importelectrumwallet): imports keys from an Electrum wallet export file (.csv or .json) New in Dash Core 0.12.1
* [ImportMulti](core-api-ref-remote-procedure-calls-wallet#importmulti): imports addresses or scripts (with private keys, public keys, or P2SH redeem scripts) and optionally performs the minimum necessary rescan for all imports. *New in Dash Core 0.12.3*
* [ImportPrivKey](core-api-ref-remote-procedure-calls-wallet#importprivkey): adds a private key to your wallet. The key should be formatted in the wallet import format created by the [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#dumpprivkey).
* [ImportPrunedFunds](core-api-ref-remote-procedure-calls-wallet#importprunedfunds): imports funds without the need of a rescan. Meant for use with pruned wallets. *New in Dash Core 0.12.3*
* [ImportPubKey](core-api-ref-remote-procedure-calls-wallet#importpubkey): imports a public key (in hex) that can be watched as if it were in your wallet but cannot be used to spend
* [ImportWallet](core-api-ref-remote-procedure-calls-wallet#importwallet): imports private keys from a file in wallet dump file format (see the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#dumpwallet)). These keys will be added to the keys currently in the wallet.  This call may need to rescan all or parts of the block chain for transactions affecting the newly-added keys, which may take several minutes.
* [KeePass](core-api-ref-remote-procedure-calls-wallet#keepass): provides commands for configuring and managing KeePass authentication. New in Darkcoin Core 0.11.0
* [KeyPoolRefill](core-api-ref-remote-procedure-calls-wallet#keypoolrefill): fills the cache of unused pre-generated keys (the keypool).
* [ListAddressBalances](core-api-ref-remote-procedure-calls-wallet#listaddressbalances): lists addresses of this wallet and their balances *New in Dash Core 0.12.3*
* [ListAddressGroupings](core-api-ref-remote-procedure-calls-wallet#listaddressgroupings): lists groups of addresses that may have had their common ownership made public by common use as inputs in the same transaction or from being used as change from a previous transaction.
* [ListLockUnspent](core-api-ref-remote-procedure-calls-wallet#listlockunspent): returns a list of temporarily unspendable (locked) outputs.
* [ListReceivedByAddress](core-api-ref-remote-procedure-calls-wallet#listreceivedbyaddress): lists the total number of dash received by each address. *Updated in Dash Core 0.13.0*
* [ListSinceBlock](core-api-ref-remote-procedure-calls-wallet#listsinceblock): gets all transactions affecting the wallet which have occurred since a particular block, plus the header hash of a block at a particular depth. **_Updated in Dash Core 0.15.0_**
* [ListTransactions](core-api-ref-remote-procedure-calls-wallet#listtransactions): returns the most recent transactions that affect the wallet. _Updated in Dash Core 0.14.0_
* [ListUnspent](core-api-ref-remote-procedure-calls-wallet#listunspent): returns an array of unspent transaction outputs belonging to this wallet. **_Updated in Dash Core 0.15.0_**
* [ListWallets](core-api-ref-remote-procedure-calls-wallet#listwallets): returns a list of currently loaded wallets. **_New in Dash Core 0.15.0_**
* [LockUnspent](core-api-ref-remote-procedure-calls-wallet#lockunspent): temporarily locks or unlocks specified transaction outputs. A locked transaction output will not be chosen by automatic coin selection when spending dash. Locks are stored in memory only, so nodes start with zero locked outputs and the locked output list is always cleared when a node stops or fails.
* [RemovePrunedFunds](core-api-ref-remote-procedure-calls-wallet#removeprunedfunds): deletes the specified transaction from the wallet. Meant for use with pruned wallets and as a companion to importprunedfunds. *New in Dash Core 0.12.3*
* [RescanBlockChain](core-api-ref-remote-procedure-calls-wallet#rescanblockchain): rescans the local blockchain for wallet related transactions. **New in Dash Core 0.16.0**
* [SendMany](core-api-ref-remote-procedure-calls-wallet#sendmany): creates and broadcasts a transaction which sends outputs to multiple addresses. **_Updated in Dash Core 0.15.0_**
* [SendToAddress](core-api-ref-remote-procedure-calls-wallet#sendtoaddress): spends an amount to a given address. **_Updated in Dash Core 0.15.0_**
* [SetPrivateSendAmount](core-api-ref-remote-procedure-calls-wallet#setprivatesendamount): sets the amount of DASH to be mixed with PrivateSend *New in Dash Core 0.13.0*
* [SetPrivateSendRounds](core-api-ref-remote-procedure-calls-wallet#setprivatesendrounds): sets the number of PrivateSend mixing rounds to use *New in Dash Core 0.13.0*
* [SetTxFee](core-api-ref-remote-procedure-calls-wallet#settxfee): sets the transaction fee per kilobyte paid by transactions created by this wallet.
* [SignMessage](core-api-ref-remote-procedure-calls-wallet#signmessage): signs a message with the private key of an address.
* [WalletLock](core-api-ref-remote-procedure-calls-wallet#walletlock): removes the wallet encryption key from memory, locking the wallet. After calling this method, you will need to call `walletpassphrase` again before being able to call any methods which require the wallet to be unlocked.
* [WalletPassphrase](core-api-ref-remote-procedure-calls-wallet#walletpassphrase): stores the wallet decryption key in memory for the indicated number of seconds. Issuing the `walletpassphrase` command while the wallet is already unlocked will set a new unlock time that overrides the old one.
* [WalletPassphraseChange](core-api-ref-remote-procedure-calls-wallet#walletpassphrasechange): changes the wallet passphrase from 'old passphrase' to 'new passphrase'.

# [Wallet RPCs (Deprecated)](core-api-ref-remote-procedure-calls-wallet-deprecated)

**Note:** the wallet RPCs are only available if Dash Core was built with <<glossary:wallet support>>, which is the default.

* [GetAccount](core-api-ref-remote-procedure-calls-wallet-deprecated#getaccount): returns the name of the account associated with the given address. **_Deprecated_**
* [GetAccountAddress](core-api-ref-remote-procedure-calls-wallet-deprecated#getaccountaddress): returns the current Dash address for receiving payments to this account. If the account doesn't exist, it creates both the account and a new address for receiving payment.  Once a payment has been received to an address, future calls to this RPC for the same account will return a different address. **_Deprecated_**
* [GetAddressesByAccount](core-api-ref-remote-procedure-calls-wallet-deprecated#getaddressesbyaccount): returns a list of every address assigned to a particular account. **_Deprecated_**
* [GetReceivedByAccount](core-api-ref-remote-procedure-calls-wallet-deprecated#getreceivedbyaccount): returns the total amount received by addresses in a particular account from transactions with the specified number of confirmations.  It does not count coinbase transactions. *Updated in Dash Core 0.13.0* **_Deprecated_**
* [ListAccounts](core-api-ref-remote-procedure-calls-wallet-deprecated#listaccounts): lists accounts and their balances. *Updated in Dash Core 0.13.0* **_Deprecated_**
* [ListReceivedByAccount](core-api-ref-remote-procedure-calls-wallet-deprecated#listreceivedbyaccount): lists the total number of dash received by each account. *Updated in Dash Core 0.13.0* **_Deprecated_**
* [Move](core-api-ref-remote-procedure-calls-wallet-deprecated#move): moves a specified amount from one account in your wallet to another using an off-block-chain transaction. **_Deprecated_**
* [SendFrom](core-api-ref-remote-procedure-calls-wallet-deprecated#sendfrom): spends an amount from a local account to a dash address. *Updated in Dash Core 0.13.0* **_Deprecated_**
* [SetAccount](core-api-ref-remote-procedure-calls-wallet-deprecated#setaccount): puts the specified address in the given account. **_Deprecated_**

# [Removed RPCs](core-api-ref-remote-procedure-calls-removed)

* [EstimatePriority](core-api-ref-remote-procedure-calls-removed#estimatepriority): was removed in Dash Core 0.14.0.
* [EstimateSmartPriority](core-api-ref-remote-procedure-calls-removed#estimatesmartpriority): was removed in Dash Core 0.14.0.
* [GetInfo](core-api-ref-remote-procedure-calls-removed#getinfo): **was removed in Dash Core 0.16.0.**
* [MasternodeBroadcast](core-api-ref-remote-procedure-calls-removed#masternodebroadcast): was removed in Dash Core 0.14.0.
* [SentinelPing](core-api-ref-remote-procedure-calls-removed#sentinelping): was removed in Dash Core 0.14.0.