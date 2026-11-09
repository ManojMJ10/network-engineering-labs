# Day 12 — The Life of a Packet (Cisco CCNA)

## Overview
Packet-level investigation using the same two-router, two-LAN topology as Day 11. Used Cisco Packet Tracer **Simulation Mode** to inspect ARP and ICMP and follow an Echo Request across two routed hops.

## Topology
`PC1 (192.168.1.10) → SW1 → R1 → R2 → SW2 → PC2 (192.168.2.10)`

The routers forward packets using the static routes configured in [Day 11](../Day-11-Static-Routing/README.md).

## Procedure
1. Switch Packet Tracer to Simulation Mode.
2. Filter events to **ARP** and **ICMP**.
3. From PC1, run `ping 192.168.2.10`.
4. Step through events with **Capture/Forward**.
5. Compare **Inbound PDU Details** and **Outbound PDU Details** on R1 and R2.
6. Inspect the ICMP Echo Request (Type 8) and, when observed, the Echo Reply (Type 0).

## Observed IP packet fields
| Field | R1 inbound | R1 outbound | R2 outbound |
|---|---|---|---|
| Source IPv4 | 192.168.1.10 | 192.168.1.10 | 192.168.1.10 |
| Destination IPv4 | 192.168.2.10 | 192.168.2.10 | 192.168.2.10 |
| TTL | 128 | 127 | 126 |
| Protocol | ICMP (1) | ICMP (1) | ICMP (1) |
| ICMP message | Echo Request (Type 8) | Echo Request (Type 8) | Echo Request (Type 8) |

## Layer 2 observations
- On R1 inbound, the frame was addressed to R1's LAN MAC, with PC1's MAC as the source.
- On R1 outbound, the router constructed a new Ethernet frame toward R2; its MAC addresses differed from the inbound frame.
- On R2 outbound, the frame used R2's LAN-facing MAC as the source and PC2's MAC (`0060.5C47.579D` in the captured simulation) as the destination.
- Unlike Ethernet MAC addresses, the end-to-end source and destination IPv4 addresses remained unchanged because no NAT was configured.

## Key learning
A switch forwards Ethernet frames within the LAN without decrementing IP TTL. A router selects a next hop based on the destination IPv4 address, decrements TTL, and re-encapsulates the IP packet with a new Layer 2 header for the outgoing link.

## Evidence and files
Add PDU screenshots to a `screenshots/` directory. A Packet Tracer file with the working topology can be included as `packet-analysis.pkt`. The [Day 11 topology](../Day-11-Static-Routing/README.md) provides the required initial configuration.

## Skills demonstrated
Packet Tracer Simulation Mode, Ethernet framing, MAC address changes across routers, IPv4 TTL, ICMP Echo Requests, and interpretation of PDU headers.

## Packet capture screenshots

Once the screenshot package is uploaded into `screenshots/`, the captures will render below:

### R1 inbound — TTL 128
![R1 inbound ICMP](screenshots/01-r1-inbound-icmp.png)

### R1 outbound — TTL 127
![R1 outbound ICMP](screenshots/02-r1-outbound-icmp.png)

### R2 outbound — TTL 126
![R2 outbound ICMP](screenshots/03-r2-outbound-icmp.png)
