# Verification matrix

| Area | Suggested/recorded check | Current status | Evidence needed for a stronger claim |
|---|---|---|---|
| VLSM | Validate network, mask, gateway, usable range, broadcast | Address plan calculated from supplied values | Device interface/host IP output when available |
| VLANs | `show vlan brief` | Port membership is documented from supplied excerpts | Saved current VLAN database per switch |
| Trunks | `show interfaces trunk` | Trunk mode is in the supplied configuration excerpt | Live trunk output |
| Router-on-a-Stick | `show ip interface brief`; inspect subinterface encapsulation | Aden `.30` and `.40` plus addresses are shown `up/up`; Ibb subinterfaces are documented in config excerpt | Current output from both branch routers |
| OSPF | `show ip ospf neighbor`; `show ip route ospf` | OSPF configuration excerpts exist | Neighbor state and advertised VLAN routes, especially Aden VLAN 40 |
| MPLS/LDP | `show mpls ldp neighbor`; `show mpls forwarding-table` | Text excerpts are in the report | Capture date/router names and reconcile prefixes with address plan |
| PPP/CHAP | `show interfaces`; `show running-config interface ...` (redact secret) | Report/config excerpt indicates PPP with CHAP on Ibb link | Interface protocol state and successful auth evidence |
| Multilink PPP | `show ip interface brief`; inspect member interfaces | `Multilink1 10.70.10.2` shown `up/up` on Radden | Member links and bundle config |
| Port Security | `show port-security interface ...` | Output in report shows enabled/secure-up | Reconcile `Restrict` output vs `shutdown` config |
| STP | `show spanning-tree` | Source report lists STP but no independent output was retained | Current STP output from both switches |
| End-to-end | Ping source host to remote host across intended VLANs; traceroute as appropriate | Existing ping is from `rabb` to `198.51.100.2` | Endpoint-to-endpoint results across branches/VLANs |
