This section describes the Dash P2P network protocol (but it is not a specification). It does not describe the <<glossary:BIP70 payment protocol>>, the [GetBlockTemplate mining protocol](core-guide-mining-block-prototypes#getblocktemplate-rpc), or any network protocol never implemented in an official version of Dash Core.

All peer-to-peer communication occurs entirely over TCP.

> 🚧 
>
> Note: unless their description says otherwise, all multi-byte integers mentioned in this section are transmitted in little-endian order.

<img src="https://files.readme.io/2f6f207-home-map-1.svg" alt="Worldwide network" style="width:55%;text-align:center;"/>