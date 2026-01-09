
```mermaid
flowchart LR
    %% Clients
    Client[Client Applications]

    %% Redis Master / Replicas
    subgraph Redis_Nodes["Redis Servers"]
        R1["Redis 140:6371\n(Master: mymaster)"]
        R2["Redis 140:6372\n(Replica)"]
        R3["Redis 213:6379\n(Replica)"]
    end

    %% Sentinel Nodes
    subgraph Sentinel_140["Sentinels on 192.168.1.140"]
        S1["Sentinel :26271"]
        S2["Sentinel :26272"]
    end

    subgraph Sentinel_213["Sentinel on 192.168.1.213"]
        S3["Sentinel :26273"]
    end

    %% Client Access
    Client -->|Discover Master| S1
    Client -->|Discover Master| S2
    Client -->|Discover Master| S3

    %% Sentinel Monitoring
    S1 -. monitors .-> R1
    S1 -. monitors .-> R2
    S1 -. monitors .-> R3

    S2 -. monitors .-> R1
    S2 -. monitors .-> R2
    S2 -. monitors .-> R3

    S3 -. monitors .-> R1
    S3 -. monitors .-> R2
    S3 -. monitors .-> R3

    %% Sentinel Communication (Quorum)
    S1 <-->|Sentinel gossip| S2
    S1 <-->|Sentinel gossip| S3
    S2 <-->|Sentinel gossip| S3

    %% Replication
    R1 -->|Replication| R2
    R1 -->|Replication| R3
