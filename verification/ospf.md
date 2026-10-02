# OSPF Verification

## Neighbor Adjacencies (R2)

```
R2#show ip ospf neighbor
Neighbor ID     Pri   State           Dead Time   Address         Interface
3.3.3.3           1   FULL/DR         00:00:37    10.0.23.2       GigabitEthernet0/2
1.1.1.1           1   FULL/BDR        00:00:31    10.0.12.1       GigabitEthernet0/1

```

**Result:**  
- Full adjacency with R1 (1.1.1.1) on the R1–R2 link  
- Full adjacency with R3 (3.3.3.3) on the R2–R3 link  

Both neighbors are in the FULL state → OSPF adjacency is healthy.

## OSPF Routes (R2)

```
R2#show ip route ospf
10.0.0.0/8 is variably subnetted, 17 subnets, 3 masks
O        10.0.13.0 [110/2] via 10.0.12.1, 03:00:22, GigabitEthernet0/1
[110/2] via 10.0.23.2, 03:00:22, GigabitEthernet0/2
O        10.10.10.0 [110/2] via 10.0.12.1, 03:00:22, GigabitEthernet0/1
O        10.10.20.0 [110/2] via 10.0.12.1, 03:00:22, GigabitEthernet0/1
O        10.10.99.0 [110/2] via 10.0.12.1, 03:00:22, GigabitEthernet0/1
O        10.30.30.0 [110/2] via 10.0.23.2, 03:00:22, GigabitEthernet0/2
O        10.30.40.0 [110/2] via 10.0.23.2, 03:00:22, GigabitEthernet0/2
O        10.30.99.0 [110/2] via 10.0.23.2, 03:00:22, GigabitEthernet0/2
```

**Routes learned via OSPF:**
- HQ networks: 10.10.10.0/24, 10.10.20.0/24, 10.10.99.0/24
- Luton (R3) networks: 10.30.30.0/24, 10.30.40.0/24, 10.30.99.0/24
- Inter-router link: 10.0.13.0/30

## OSPF Interfaces (R2)

```
R2#show ip ospf interface brief
Interface    PID   Area   IP Address/Mask           Cost  State Nbrs F/C
Gi0/1        1     0      10.0.12.2/30              1     DR    0/0
Gi0/0.30     1     0      10.20.30.1/24             1     DR    0/0
Gi0/0.40     1     0      10.20.40.1/24             1     DR    0/0
Gi0/0.99     1     0      10.20.99.1/24             1     DR    0/0
Gi0/2        1     0      10.0.23.1/30              1     BDR   0/0
text
```

All relevant interfaces are participating in Area 0.

## Summary

| Check                        | Result     |
|-----------------------------|------------|
| R2 ↔ R1 adjacency           | FULL       |
| R2 ↔ R3 adjacency           | FULL       |
| HQ networks learned         | Yes        |
| Luton networks learned      | Yes        |
| Inter-site routing working  | Yes        |

OSPF is fully operational across all three sites.












