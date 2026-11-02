# Two-office NAT/PAT | Project 11
Dublin and Cork PCs share one outside interface address when opening a simulated external website. This Cisco Packet Tracer lab extends an OSPF office network with an ISP router, an outside HTTP server and PAT (NAT overload).

![Network topology](images/topology.png)

## Addressing
| Segment/device | Address |
|---|---|
| Dublin-PC / gateway | 192.168.10.11/24 / 192.168.10.1 |
| Cork-PC / gateway | 192.168.20.11/24 / 192.168.20.1 |
| Dublin–Cork link | 10.0.0.1/30 and 10.0.0.2/30 |
| Dublin G0/2 – ISP G0/0 | 203.0.113.1/30 and 203.0.113.2/30 |
| ISP G0/1 | 198.51.100.1/24 |
| OUTSIDE-WEB / gateway | 198.51.100.20/24 / 198.51.100.1 |

The outside ranges are documentation addresses used for simulation; this lab does not connect to the real internet. Existing Dublin-Web stays at 192.168.10.20/24.

## Configuration
- Dublin G0/0 and G0/1 are NAT inside; G0/2 is NAT outside.
- ACL 1 selects both office subnets for translation; it is a NAT selector, not an interface blocking filter.
- NAT overload uses the G0/2 address, 203.0.113.1.
- Dublin's default route points to ISP 203.0.113.2; Cork's points to Dublin 10.0.0.1.
- OSPF routing, Cork SSH and the earlier Cork ACL 100 remain in the supplied configs. That ACL blocks Cork-PC echo requests only to Dublin-Web, not OUTSIDE-WEB.

## Observed result
Both PCs opened the outside webpage. Both originally used TCP source port 1032. The final translation snapshot shows Cork mapped to 203.0.113.1:1024 and Dublin to 203.0.113.1:1032. The distinct outside ports let PAT distinguish the connections.

## Reproduce and review
Open [the Packet Tracer file](lab/Project-11-Office-NAT.pkt), open http://198.51.100.20 from both PCs, then run `show ip nat translations` on Dublin. Reload pages if entries have expired.

[Three router configs and setup instructions](configs/) · [Tests and troubleshooting](tests/) · [Combined evidence JPG](images/Project-11-Network-Evidence.jpg)

## Evidence scope
The original .pkt and screenshot are included unchanged. The .pkt was not independently executed or inspected internally during packaging. Router configuration transcriptions retain the supplied statements, omitting repeated empty separators. CLI image panels are recreated from supplied text, not original terminal screenshots. Website success was reported by the learner and supported by the TCP/80 translation entries. Preserved Cork password hashes are for the earlier isolated lab credentials, which must not be reused outside the lab.
