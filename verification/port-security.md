# Port Security Verification


## SW1

```
SW1#show port-security
Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
(Count)        (Count)          (Count)
Fa0/2              1              1                0         Restrict
```

## SW2

```
SW2#show port-security
Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
(Count)        (Count)          (Count)
Fa0/1              1              1                0         Restrict
Fa0/2              1              1                0         Restrict
Fa0/3              1              1                0         Restrict
Fa0/4              1              1                0         Restrict
```


## SW3
```
SW3#show port-security
Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
(Count)        (Count)          (Count)
Fa0/1              1              1                0         Restrict
Fa0/2              1              1                0         Restrict
```


## SW4
```
SW4#show port-security
Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
(Count)        (Count)          (Count)
Fa0/1              1              1                0         Restrict
Fa0/2              1              1                0         Restrict
```

## Summary

| Switch | Ports Secured | Max MACs | Current MACs | Violations | Action   | Status |
|--------|---------------|----------|--------------|------------|----------|--------|
| SW1    | Fa0/2         | 1        | 1            | 0          | Restrict | ✅     |
| SW2    | Fa0/1–Fa0/4   | 1        | 1            | 0          | Restrict | ✅     |
| SW3    | Fa0/1–Fa0/2   | 1        | 1            | 0          | Restrict | ✅     |
| SW4    | Fa0/1–Fa0/2   | 1        | 1            | 0          | Restrict | ✅     |

All access ports facing end devices are secured with sticky MAC learning.  
Maximum of 1 MAC address allowed per port.  
Zero security violations.  
Security action set to **Restrict**.
