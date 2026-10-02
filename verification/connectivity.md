# Connectivity Verification

## Inter-Site PC-to-PC Connectivity

### From HQ PC (10.10.10.21)

```
C:>ping 10.20.30.21
Reply from 10.20.30.21: bytes=32 time=... TTL=...
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
C:>ping 10.30.30.21
Reply from 10.30.30.21: bytes=32 time=... TTL=...
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

**Result:** HQ can reach both Manchester and Luton user PCs.

### From Manchester User PC (10.20.30.21)
```
C:>ping 10.10.10.21
Success (0% loss)
C:>ping 10.30.30.21
Success (0% loss)
```

**Result:** Manchester Users can reach HQ and Luton.

### From Luton User PC (10.30.30.21)

```
C:>ping 10.10.10.21
Success (0% loss)
C:>ping 10.20.30.21
Success (0% loss)
```

**Result:** Full end-to-end connectivity between all three sites is working.

## Guest Network Connectivity

### From Guest PC (10.20.40.21)

| Destination              | Result                          | Notes                     |
|--------------------------|---------------------------------|---------------------------|
| 10.20.40.1 (own gateway) | Success (0% loss)               | Local gateway reachable   |
| 10.20.40.23 (other Guest)| Success (0% loss)               | Same VLAN communication   |
| 10.20.30.21 (Users)      | Destination host unreachable    | Correctly blocked by ACL  |
| 10.10.10.21 (HQ)         | Destination host unreachable    | Correctly blocked by ACL  |

### From Internal PC to Guest

| Source              | Destination         | Result                          | Notes                          |
|---------------------|---------------------|---------------------------------|--------------------------------|
| 10.20.30.21         | 10.20.40.1 (GW)     | Success (0% loss)               | Allowed for troubleshooting    |
| 10.20.30.21         | 10.20.40.21 (PC)    | Destination host unreachable    | Correctly blocked by ACL       |

## Summary

| Test                              | Result |
|-----------------------------------|--------|
| HQ ↔ Manchester Users             | ✅     |
| HQ ↔ Luton Users                  | ✅     |
| Manchester ↔ Luton Users          | ✅     |
| Guest → Own gateway / other Guests| ✅     |
| Guest → Internal networks         | ❌ (blocked) |
| Internal → Guest gateway          | ✅     |
| Internal → Guest PC               | ❌ (blocked) |

End-to-end connectivity and Guest isolation are both working as designed.
