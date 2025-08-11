# Algen L2: EVM compatible execution layer

![Banner](https://pub-4e071c7391f448248e0beb38499ef45f.r2.dev/algen/ALG2.png)

## Overview
ALG Layer2: Execution and Services

Blockchain is designed for transaction systems that don't require a trusted third party, where
storing data on-chain is inherently expensive and inefficient. The maximum Gas limit set for
blockchain blocks restricts the number of transactions that can be processed per block. For
applications such as social media and gaming, which require high peak experiences of 25,000
TPS, this is undoubtedly beyond the current performance capabilities of blockchain. Therefore,
blockchain's processing of information must necessarily include a hierarchy of information. From
a value perspective, information such as user identity, transactions, and assets have higher value
and on-chain security requirements, while more casual information such as comments, likes, and
in-game interactions, a lighter L2 based on data verification may be a more appropriate solution.
Unlike the L2 processing solution mentioned earlier, ALG L2 goes one step further in terms of
off-chain data availability, proposing a new expansion solution. ALG L2 does not need to
package transactions and submit them to L1, but sends transactions to the data availability layer,
achieving large-scale transactions and reducing costs, and providing higher scalability required
by social networks and games, thereby avoiding the limitations of block space or block time
allocation. Its data processing flow is divided into the following steps:

- The client signs the transaction and sends it to the ALG Handler.
- The ALG Handler sends the signed transaction to ALG, and ALG checks the signed transaction
for legality (without being recorded on-chain).
- The ALG Handler sends the checked transaction message to ALG DA.
- ALG DA returns the transaction hash to the client.
- The client can call ALG Proof at any time to check whether the transaction is on ALG DA.

ALG L2 constructs transactions that still require signatures from the wallet (which will be
propagated on the chain). However, there is no need to send and broadcast the real transactions
on the chain, thereby eliminating the requirement for on-chain transaction execution and the
associated gas cost.

ALG can process ALG off-chain transactions, enabling massive scalability and cost reduction.
Transaction signatures and typed data are used to create DA metadata as transactions. Then,
these transactions are transmitted to the DA layer, containing block numbers, block hashes,
signature type data, transaction signatures, and other crucial details during the creation of such
transactions. The data structure can be fully verified by archival nodes. Each user can run as a
real-time node, validating operations without trusting transaction submitters. Additionally, ALG
allows L2 Dapps themselves to act as nodes, encouraging users' participation in validation. More
validators will further enhance ALG's True Network Value (TNV).

<img width="912" alt="image" src="https://github.com/user-attachments/assets/4b1913da-af87-469d-a498-08f9a9190e15" />

## Adding nodes
To add nodes to the rollup, you need to initialize `op-node` and `op-geth`, similar to what you did for the first node.
You should *not* add an `op-batcher` because there should be only one.
<Steps>
### Configure the OS and prerequisites as you did for the first node

### Build the Optimism monorepo and `op-geth` as you did for the first node

### Copy from the first node these files:

    ```bash
    ./genesis.json
    ./rollup.json
    ```

### Create a new `jwt.txt` file as a shared secret:

    ```bash
    cd ~/op-geth
    openssl rand -hex 32 > jwt.txt
    cp jwt.txt ~/op-node
    ```

### Initialize the new op-geth:

    ```bash
    cd ~/op-geth
    ./build/bin/geth init --datadir=./datadir ./genesis.json
    ```

### Turn on peer to peer synchronization to enable L2 nodes to synchronize directly
If you do it this way, you won't have to wait until the transactions are written to L1.
    If you already have peer to peer synchronization, add the new node to the `--p2p.static` list so it can synchronize.

### Start `op-geth` (using the same command line you used on the initial node)
<Callout type="warning">
**Important:** Make sure to configure the `--rollup.sequencerhttp` flag to point to your sequencer node. This HTTP endpoint is crucial because `op-geth` will route `eth_sendRawTransaction` calls to this URL.
</Callout>

### Start `op-node` (using the same command line you used on the initial node)
</Steps>