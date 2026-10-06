# Dynamic ARP Inspection (DAI) Verification

## DAI Status on SW2

```
SW2#show ip arp inspection
Source Mac Validation      : Disabled
Destination Mac Validation : Disabled
IP Address Validation      : Disabled
Vlan     Configuration    Operation   ACL Match          Static ACL

30       Enabled          Active
40       Enabled          Active
Vlan     ACL Logging      DHCP Logging      Probe Logging

30       Deny             Deny              Off
40       Deny             Deny              Off
Vlan      Forwarded        Dropped     DHCP Drops      ACL Drops

30              0              0              0              0
40              0              0              0              0
Vlan   DHCP Permits    ACL Permits  Probe Permits   Source MAC Failures

30              0              0              0                     0
40              0              0              0                     0
Vlan   Dest MAC Failures   IP Validation Failures   Invalid Protocol Data

30                   0                        0                       0
40                   0                        0                       0
```

## DAI per VLAN
```
SW2#show ip arp inspection vlan 30,40
Source Mac Validation      : Disabled
Destination Mac Validation : Disabled
IP Address Validation      : Disabled
Vlan     Configuration    Operation   ACL Match          Static ACL

30       Enabled          Active
40       Enabled          Active
Vlan     ACL Logging      DHCP Logging      Probe Logging

30       Deny             Deny              Off
40       Deny             Deny              Off
text
```

## DHCP Snooping Bindings (required for DAI)

```
SW2#show ip dhcp snooping binding
MacAddress          IpAddress        Lease(sec)  Type           VLAN  Interface

00:30:A3:68:B0:44   10.20.30.21              0  dhcp-snooping   30  FastEthernet0/3
00:01:63:32:8C:90   10.20.30.22              0  dhcp-snooping   30  FastEthernet0/1
00:E0:F9:D6:D0:C4   10.20.40.21              0  dhcp-snooping   40  FastEthernet0/4
00:D0:BC:34:3D:AE   10.20.40.22              0  dhcp-snooping   40  FastEthernet0/2
Total number of bindings: 4
```


## Summary

| Feature                        | Status          |
|--------------------------------|-----------------|
| DAI enabled on VLAN 30         | ✅ Active       |
| DAI enabled on VLAN 40         | ✅ Active       |
| DHCP Snooping bindings present | ✅ 4 bindings   |

**Notes:**
- DAI is active on the Manchester User (VLAN 30) and Guest (VLAN 40) VLANs.
- It relies on the DHCP Snooping binding table to validate ARP packets.













