```mermaid
flowchart TB
 subgraph Client["Client / Application"]
        A["Redis Smart Client"]
  end
 subgraph MasterNodes["Master Nodes"]
        M0["Master 7000<br>Slots 0–5460"]
        M1["Master 7001<br>Slots 5461–10922"]
        M2["Master 7002<br>Slots 10923–16383"]
  end
 subgraph ReplicaNodes["Replica Nodes"]
        R0["Replica 7003<br>Replicates 7000"]
        R1["Replica 7004<br>Replicates 7001"]
        R2["Replica 7005<br>Replicates 7002"]
  end
 subgraph Cluster["Redis Cluster (Sharding + HA)"]
        MasterNodes
        ReplicaNodes
  end
    A -- Any request --> M0 & M1 & M2
    R0 -- Replicates --> M0
    R1 -- Replicates --> M1
    R2 -- Replicates --> M2
    M0 -. Failover: if 7000 fails .-> R0
    M1 -. Failover: if 7001 fails .-> R1
    M2 -. Failover: if 7002 fails .-> R2
