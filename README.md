# Optimism-Stack-Architecture
Optimism Stack Architecture

```mermaid
%%{init: {'theme': 'dark', 'themeVariables': { 'fontSize': '16px', 'fontFamily': 'arial' }}}%%
graph TB
    subgraph L1[Layer 1]
        Eth[Ethereum]
        Inbox[Batch Inbox]
        Portal[Portal]
        Bridge1[L1 Bridge]
        Msg1[L1 Messenger]
    end

    subgraph Core[OP Stack Core]
        direction TB
        
        subgraph DA[Data Layer]
            EthDA[Eth DA]
            AltDA[Alt DA]
        end
        
        subgraph DL[Derivation]
            Node[op-node]
        end
        
        subgraph EL[Execution]
            Geth[op-geth]
        end
        
        subgraph SL[Sequencing]
            Batch[Batcher]
            Prop[Proposer]
        end

        subgraph ST[Settlement]
            Proof[Fault Proof]
        end
    end

    subgraph L2[Layer 2]
        Chain[L2 Chain]
        Bridge2[L2 Bridge]
        Msg2[L2 Messenger]
    end

    subgraph Users[Users]
        End[End Users]
        Apps[DApps]
        Wallet[Wallets]
    end

    subgraph Nodes[Operators]
        Seq[Sequencer]
        Ver[Verifier]
        Chal[Challenger]
    end

    %% Key Flows
    End --> Wallet --> Apps
    Apps -->|Tx| Geth
    End -->|Bridge| Bridge1
    
    Bridge1 <--> Portal
    Portal <--> Msg1
    Msg1 <--> Msg2
    
    Geth -->|Execute| Chain
    Node -->|Drive| Geth
    Batch -->|Submit| Inbox
    Inbox --> Node
    
    Prop -->|State| Portal
    Portal --> Proof
    Chal -->|Challenge| Proof
    
    EthDA --> Node
    AltDA -.-> Node
    
    Seq -->|Run| Geth
    Ver -->|Verify| Node

    Chain --> Bridge2
    Bridge2 <--> Msg2
```
