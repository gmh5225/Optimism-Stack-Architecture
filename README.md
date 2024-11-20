# Optimism-Stack-Architecture
Optimism Stack Architecture

```mermaid
graph TB
    subgraph "Layer 1 (Ethereum)"
        L1[L1 Blockchain]
        BatchInbox[Batch Inbox]
        Portal[OptimismPortal Contract]
        L1Bridge[L1 Bridge Contracts]
        L1Messenger[L1 CrossDomain Messenger]
        DisputeGame[Dispute Game Factory]
    end

    subgraph "Core Components"
        direction TB
        
        subgraph "Execution Layer"
            OpGeth[op-geth]
        end
        
        subgraph "Consensus Layer"
            OpNode[op-node]
        end
        
        subgraph "Sequencing Layer"
            Batcher[op-batcher]
            Proposer[op-proposer]
        end
    end

    subgraph "Layer 2"
        L2[L2 Blockchain]
        L2Bridge[L2 Bridge Contracts]
        L2Messenger[L2 CrossDomain Messenger]
        GasPriceOracle[Gas Price Oracle]
        FeeVault[Fee Vaults]
    end

    %% Connections
    L1 --> BatchInbox
    BatchInbox --> OpNode
    OpNode --> OpGeth
    OpGeth --> L2
    
    Batcher --> BatchInbox
    Proposer --> Portal
    
    L1Bridge <--> Portal
    Portal <--> L2Bridge
    L1Messenger <--> L2Messenger
    
    L2 --> GasPriceOracle
    L2 --> FeeVault
```
