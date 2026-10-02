# Troubleshooting Scenarios

This section documents real issues encountered during the lab and how they were resolved.  
Documenting both the problem and the fix is valuable for a portfolio.

---

## Scenario 1 – Guest PC cannot reach its own gateway

**Symptom**

```
C:>ping 10.20.40.1
Reply from 10.20.40.1: Destination host unreachable.
```


**Investigation**
- Checked ACL on R2 (`show access-lists`)
- Found that a broad `deny` statement was placed **before** the more specific `permit` for the Guest subnet

**Root Cause**  
ACL rule order. The statement:

```
deny ip 10.20.40.0 0.0.0.255 10.20.0.0 0.0.255.255
```
was matching traffic to the Guest gateway before the more specific permit could allow it.

**Fix**
```
ip access-list extended GUEST-RESTRICT
permit ip 10.20.40.0 0.0.0.255 10.20.40.0 0.0.0.255   ← moved to the top
deny ip 10.20.40.0 0.0.0.255 10.10.0.0 0.0.255.255
deny ip 10.20.40.0 0.0.0.255 10.20.0.0 0.0.255.255
deny ip 10.20.40.0 0.0.0.255 10.30.0.0 0.0.255.255
permit ip any any
```

**Verification**  
Guest PC could successfully ping 10.20.40.1 after the change.

**Lesson**  
Always place the most specific ACL rules before broader ones.

---

## Scenario 2 – Internal PC cannot ping Guest PC (even though routing is correct)

**Symptom**

```
C:>ping 10.20.40.21
Request timed out.   (or Destination host unreachable)
```

**Investigation**
- Confirmed routing and OSPF were working
- Guest PC firewall was off
- Checked both ACLs on R2

**Root Cause**  
Two factors:
1. `GUEST-RESTRICT` (inbound) blocks the return traffic from the Guest PC
2. `INTERNAL-TO-GUEST` (outbound) intentionally blocks Internal → Guest PC traffic

**Resolution**  
This was left as intentional design (Guest isolation in both directions).  
Internal users can still reach the Guest **gateway** for troubleshooting purposes.

**Lesson**  
When applying ACLs in both directions, always test return traffic.  
What looks like a “connectivity failure” can actually be the security policy working correctly.

---

## Scenario 3 – Sticky MAC addresses disappear after reload

**Symptom**  
After saving the running-config and reloading a switch, the sticky MAC addresses were gone and ports went into violation state.

**Root Cause**  
Sticky MAC addresses are stored in the running-config.  
If `copy running-config startup-config` is not performed, they are lost on reload.

**Fix**
```
copy running-config startup-config
```

**Verification**  
After saving and reloading, sticky MACs remained and no violations occurred.

**Lesson**  
Always save the configuration after sticky MAC learning is complete.

---

## Scenario 4 – ACL counters not increasing

**Symptom**  
Pings were being blocked, but `show access-lists` showed 0 matches on the deny statements.

**Investigation**
- Confirmed the ACL was applied on the correct interface and direction
- Cleared counters and retested

**Root Cause**  
ACL was applied in the wrong direction initially, or traffic was taking a different path.

**Fix**  
Verified and corrected the interface application:

```
interface GigabitEthernet0/0.40
ip access-group GUEST-RESTRICT in
ip access-group INTERNAL-TO-GUEST out
```

**Lesson**  
Always confirm both the ACL content **and** the interface + direction with:


**Lesson**  
Always confirm both the ACL content **and** the interface + direction with:

```
show ip interface  | include access list
```
---

## Quick Troubleshooting Checklist Used in This Lab

1. Can the device ping its own gateway?
2. Is the correct VLAN assigned on the switchport?
3. Is the trunk allowing the required VLANs?
4. Are OSPF neighbors in FULL state?
5. Does `show ip route` contain the destination network?
6. Is an ACL applied on the path? Check direction and order.
7. Are sticky MACs present and has the config been saved?







