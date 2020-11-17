# Minimal Platform Devnet

Requires following components:
  - Core
  - Insight
  - DAPI 

Limitations:
Given the lack of Drive, such a devnet would only allow interacting with Layer 1 (blockchain-related) DAPI endpoints

Caveats:
 - To use with the `dapi-client` library, it is necessary to configure Core as a masternode (since `dapi-client` uses the masternode list to determine what nodes it will communicate with, at least one masternode must be present on the devnet).

# Full Platform Devnet

Requires all components:
 - Core
 - Insight
 - DAPI
 - Drive

Limitations:
Theoretically none.

Caveats:
 - It's unclear if it will be possible to actually run a fully functional devnet with only a few nodes. State transitions need to be signed by a quorum of some kind and this could be an issue unless there is a "devnet mode" that allows signing by a single node or a smaller quorum.