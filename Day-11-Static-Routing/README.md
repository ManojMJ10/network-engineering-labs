# Day 11 — Static Routing (Cisco CCNA)

## Overview
Hands-on Cisco Packet Tracer lab connecting two office LANs through two Cisco 2911 routers using IPv4 static routes. Based on my practical study of Jeremy's IT Lab Day 11.

## Topology
`PC1 — SW1 — R1 — R2 — SW2 — PC2`

| Device | Interface | IPv4 address / mask |
|---|---|---|
| R1 | G0/0 | 192.168.1.1/24 |
| R1 | G0/1 | 10.0.0.1/30 |
| R2 | G0/1 | 10.0.0.2/30 |
| R2 | G0/0 | 192.168.2.1/24 |
| PC1 | Fa0 | 192.168.1.10/24; gateway 192.168.1.1 |
| PC2 | Fa0 | 192.168.2.10/24; gateway 192.168.2.1 |

## Static routing configuration
**R1** (global configuration mode):
```cisco
ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

**R2** (global configuration mode):
```cisco
ip route 192.168.1.0 255.255.255.0 10.0.0.1
```

## Verification and results
- `show ip interface brief` — routed interfaces were up/up.
- `show ip route` — each router installed the expected `S` route.
- `ping 192.168.2.10` from PC1 — 4/4 replies on final test (0% loss).
- `ping 192.168.1.10` from PC2 — 4/4 replies (0% loss).
- `tracert 192.168.2.10` showed 192.168.1.1 → 10.0.0.2 → 192.168.2.10.

## Troubleshooting exercise
**Issue:** PC1 could not reach PC2 after I removed R1's static route to 192.168.2.0/24.

**Diagnosis:** `show ip route` no longer contained the remote destination network, while `ping 10.0.0.2` from R1 still succeeded (5/5). The inter-router connection was healthy; the forwarding route was missing.

**Fix:** Re-add `ip route 192.168.2.0 255.255.255.0 10.0.0.2` on R1, then repeat end-to-end ping and tracert. Connectivity returned.

## Packet Tracer file
Upload `static-routing.pkt` to this folder to make the completed lab downloadable.

## Skills demonstrated
Cisco IOS CLI, IPv4 addressing, static route configuration, next-hop selection, routing-table verification, ICMP testing, and fault isolation.
