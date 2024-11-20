# Optimism-Stack-Architecture
Optimism Stack Architecture

```mermaid
graph TB
    subgraph "Layer 1 (Ethereum)"
        L1[L1 Blockchain]
        BatchInbox[Batch Inbox]
        Portal[OptimismPortal]
        L1Bridge[L1 Bridge]
        L1CrossDomain[L1 CrossDomain Messenger]
    end

    subgraph "OP Stack Core Components"
        direction TB
        
        subgraph "Data Availability Layer"
            EthDA[Ethereum DA]
            AltDA[Alt DA]
        end
        
        subgraph "Derivation Layer"
            OpNode[op-node<br/>Consensus Client]
        end
        
        subgraph "Execution Layer"
            OpGeth[op-geth<br/>EVM Engine]
        end
        
        subgraph "Settlement Layer"
            FaultProof[Fault Proof System]
            ProofSubmission[Proof Submission]
        end

        subgraph "Sequencing Layer"
            Batcher[op-batcher]
            Proposer[op-proposer]
        end
    end

    subgraph "Layer 2"
        L2[L2 Blockchain]
        L2Bridge[L2 Bridge]
        L2CrossDomain[L2 CrossDomain Messenger]
        Predeploys[System Contracts]
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
    L1CrossDomain <--> L2CrossDomain
    
    EthDA --> OpNode
    AltDA -.-> OpNode
    
    L2 --> Predeploys
    
    Portal --> FaultProof
    FaultProof --> ProofSubmission
```
