# Optimism-Stack-Architecture
Optimism Stack Architecture

```mermaid
%%{init: {'theme': 'dark', 'themeVariables': { 'fontSize': '16px', 'fontFamily': 'arial' }}}%%
graph TB
    subgraph Users[Users]
        End[End Users]
        Apps[DApps]
        Wallet[Wallets]
    end

    subgraph L2[Layer 2]
        Chain[L2 Chain]
        Bridge2[L2 Bridge]
        Msg2[L2 Messenger]
    end

    subgraph Core[OP Stack Core]
        direction TB
        
        subgraph SL[Sequencing]
            Seq[Sequencer]
            Batch[Batcher]
            Prop[Proposer]
        end
        
        subgraph EL[Execution]
            Geth[op-geth]
        end
        
        subgraph DL[Derivation]
            Node[op-node]
        end

        subgraph ST[Settlement]
            Proof[Fault Proof]
        end
    end

    subgraph L1[Layer 1]
        Eth[Ethereum]
        Inbox[Batch Inbox]
        Portal[Portal]
        Bridge1[L1 Bridge]
        Msg1[L1 Messenger]
    end

    %% User Normal Transaction Flow
    End --> Wallet
    Wallet --> Apps
    Apps --> Seq
    Seq --> Geth
    Geth --> Chain

    %% Batch Processing Flow
    Batch --> Inbox
    Node --> Geth
    Prop --> Portal

    %% Bridge Flow
    End --- Bridge2
    Bridge2 --- Msg2
    Msg2 --- Msg1
    Msg1 --- Bridge1

    %% Verification
    Portal --> Proof
    
    %% L1 Data
    Inbox --> Node
```

## User Flow
### Standard L2 Transaction Path
- ``End Users -> Wallets -> DApps -> Sequencer -> op-geth -> L2 Chain``
- Users interact through Optimism-supported wallets (e.g., MetaMask)
- Interact with DApps on L2
- Transactions are sent to Sequencer for ordering
- op-geth executes transactions and updates L2 state
### Cross-chain Bridge Path
- ``End Users -> L2 Bridge -> L2 Messenger -> L1 Messenger -> L1 Bridge``
- Users can transfer assets between L1 and L2
- Bridge contracts handle asset locking/releasing
- Messenger system ensures reliable cross-chain communication


