# Optimism-Stack-Architecture
Optimism Stack Architecture

```mermaid
graph TB
    subgraph "Layer 1 (Ethereum)"
        L1[L1 Blockchain]
        BatchInbox[Batch Inbox]
        Portal[OptimismPortal]
        L1Bridge[L1 Bridge Contracts]
        L1CrossDomain[L1 CrossDomain Messenger]
        DisputeGame[Dispute Game Factory]
        SystemConfig[System Config]
    end

    subgraph "OP Stack Core Components"
        direction TB
        
        subgraph "Data Availability Layer"
            EthDA[Ethereum DA]
            AltDA[Alternative DA]
        end
        
        subgraph "Derivation Layer"
            OpNode[op-node<br/>Consensus Client]
        end
        
        subgraph "Execution Layer"
            OpGeth[op-geth<br/>EVM Engine]
        end
        
        subgraph "Sequencing Layer"
            Batcher[op-batcher]
            Proposer[op-proposer]
        end

        subgraph "Settlement Layer"
            FaultProof[Fault Proof System]
            ProofSubmission[Proof Submission]
        end
    end

    subgraph "Layer 2"
        L2[L2 Blockchain]
        L2Bridge[L2 Bridge]
        L2CrossDomain[L2 CrossDomain Messenger]
        Predeploys[System Contracts]
    end

    subgraph "User Interactions"
        Users[End Users]
        Dapps[DApps]
        Wallets[Wallets]
    end

    subgraph "Node Operators"
        Sequencer[Sequencer]
        Verifier[Verifier Nodes]
        Challenger[Challengers]
    end

    %% User Transaction Flow
    Users --> Wallets
    Wallets --> Dapps
    Dapps -->|Submit L2 Tx| OpGeth
    Users -->|Bridge Assets| L1Bridge
    
    %% L1 -> L2 Flow
    L1Bridge <--> Portal
    Portal <--> L1CrossDomain
    L1CrossDomain <--> L2CrossDomain
    
    %% Sequencing Flow
    OpGeth -->|Execute Tx| L2
    OpNode -->|Drive| OpGeth
    Batcher -->|Submit Batches| BatchInbox
    BatchInbox --> OpNode
    
    %% Settlement Flow
    Proposer -->|Submit State| Portal
    Portal --> DisputeGame
    Challenger -->|Challenge| DisputeGame
    DisputeGame --> FaultProof
    
    %% Data Flow
    EthDA --> OpNode
    AltDA -.->|Optional| OpNode
    
    %% Node Operations
    Sequencer -->|Operate| OpGeth
    Verifier -->|Verify| OpNode
    
    %% System Config
    SystemConfig -.->|Configure| OpNode
    SystemConfig -.->|Configure| OpGeth

    %% L2 Components
    L2 --> Predeploys
    L2Bridge <--> L2CrossDomain
```
