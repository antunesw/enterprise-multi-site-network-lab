# ACL Verification – Guest Isolation

## ACL Configuration on R2

R2#show access-lists
Extended IP access list INTERNAL-TO-GUEST
10 permit ip 10.10.0.0 0.0.255.255 host 10.20.40.1
20 permit ip 10.20.0.0 0.0.255.255 host 10.20.40.1
30 permit ip 10.30.0.0 0.0.255.255 host 10.20.40.1
40 deny ip 10.10.0.0 0.0.255.255 10.20.40.0 0.0.0.255
60 deny ip 10.30.0.0 0.0.255.255 10.20.40.0 0.0.0.255
70 deny ip 10.20.30.0 0.0.0.255 10.20.40.0 0.0.0.255 (12 match(es))
80 permit ip any any
Extended IP access list GUEST-RESTRICT
10 permit ip 10.20.40.0 0.0.0.255 10.20.40.0 0.0.0.255 (4 match(es))
20 deny ip 10.20.40.0 0.0.0.255 10.10.0.0 0.0.255.255 (4 match(es))
30 deny ip 10.20.40.0 0.0.0.255 10.20.0.0 0.0.255.255 (8 match(es))
40 deny ip 10.20.40.0 0.0.0.255 10.30.0.0 0.0.255.255
50 permit ip any any


## Interface Application

R2#show ip interface gigabitEthernet 0/0.40 | include list
Outgoing access list is INTERNAL-TO-GUEST
Inbound access list is GUEST-RESTRICT


## Test Results

### From Guest PC (10.20.40.21)

C:>ping 10.20.40.1
Reply from 10.20.40.1: bytes=32 time<1ms TTL=255
...
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
C:>ping 10.20.40.23
Reply from 10.20.40.23: bytes=32 time<1ms TTL=128
...
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
C:>ping 10.20.30.21
Reply from 10.20.40.1: Destination host unreachable.
...
Packets: Sent = 4, Received = 0, Lost = 4 (100% loss)
C:>ping 10.10.10.21
Reply from 10.20.40.1: Destination host unreachable.
...
Packets: Sent = 4, Received = 0, Lost = 4 (100% loss)


### From Internal PC (10.20.30.21)

C:>ping 10.20.40.1
Reply from 10.20.40.1: bytes=32 time<1ms TTL=255
...
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
C:>ping 10.20.40.21
Reply from 10.20.30.1: Destination host unreachable.
...
Packets: Sent = 4, Received = 0, Lost = 4 (100% loss)


## Summary

| Traffic Flow                    | Result     | ACL Responsible      |
|--------------------------------|------------|----------------------|
| Guest → Own gateway            | Allowed    | GUEST-RESTRICT       |
| Guest → Other Guest            | Allowed    | GUEST-RESTRICT       |
| Guest → Internal networks      | Blocked    | GUEST-RESTRICT       |
| Internal → Guest gateway       | Allowed    | INTERNAL-TO-GUEST    |
| Internal → Guest PC            | Blocked    | INTERNAL-TO-GUEST    |

## Key Learning

ACL rule order is critical. A broader `deny` placed before a more specific `permit` will incorrectly block legitimate traffic (Guest → own gateway). Always place the most specific rules first.


