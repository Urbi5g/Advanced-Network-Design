# Advanced Enterprise Network — IP Addressing Plan

> **Version:** v3 — updated from the latest details supplied by the project owner on 22 October 2026.  
> **Scope:** IPv4 subnet plan, VLANs, WAN point-to-point links, and ISP loopback/LDP identifiers.  
> **Evidence limitation:** The original simulator project is not currently available, so this plan preserves the supplied data and marks older-report excerpts that must be aligned before publication.

## 1. Branch VLANs and endpoint networks

Role labels use the project owner's clarification and the endpoint labels in the supplied original diagram: Employee = Ibb VLAN 10 / Aden VLAN 30; Manager = Ibb VLAN 20 / Aden VLAN 40. The old Aden switch excerpt used inconsistent VLAN-name text, so actual running names cannot be confirmed without the simulator file.

### Ibb branch

| VLAN | Role | Network / Prefix | Subnet mask | Default gateway | Usable host range | Broadcast | Switch access ports in supplied configuration |
|---:|---|---|---|---|---|---|---|
| 10 | Employee | `192.168.30.0/28` | `255.255.255.240` | `192.168.30.1` | `192.168.30.1–192.168.30.14` | `192.168.30.15` | `e0/1`, `e0/3` |
| 20 | Manager | `192.168.30.64/26` | `255.255.255.192` | `192.168.30.65` | `192.168.30.65–192.168.30.126` | `192.168.30.127` | `e0/2`, `e1/0` |

**VLAN 20 update:** The network, gateway, and mask above use the project owner's latest confirmation. The public HTML report has been aligned with these values. Because the original simulator file is unavailable, this confirms the documentation update—not the live running configuration. The intended subinterface addressing is:

```text
interface FastEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.30.1 255.255.255.240
!
interface FastEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.30.65 255.255.255.192
```

The trunk port shown in the supplied switch configuration is `e0/0`.

### Aden branch

| VLAN | Role | Network / Prefix | Subnet mask | Default gateway | Usable host range | Broadcast | Switch access ports in supplied configuration |
|---:|---|---|---|---|---|---|---|
| 30 | Employee | `192.168.10.0/26` | `255.255.255.192` | `192.168.10.1` | `192.168.10.1–192.168.10.62` | `192.168.10.63` | `e0/1`, `e0/2` |
| 40 | Manager | `192.168.10.64/28` | `255.255.255.240` | `192.168.10.65` | `192.168.10.65–192.168.10.78` | `192.168.10.79` | `e1/0`, `e1/1` |

The latest `show ip interface brief` output from `Radden` reports:

- `FastEthernet0/0.30` — `192.168.10.1` — `up/up`
- `FastEthernet0/0.40` — `192.168.10.65` — `up/up`
- `Multilink1` — `10.70.10.2` — `up/up`

The supplied output does not display subnet masks. Masks in this plan are based on the owner's addressing data and the report/topology, not directly read from `show ip interface brief`.

## 2. WAN point-to-point links

All listed links use `/30` (`255.255.255.252`) and provide two usable IPv4 addresses.

| Link | Network / Prefix | Subnet mask | First endpoint | Second endpoint | Broadcast |
|---|---|---|---|---|---|
| `ISP_Ibb – Main_ISP` | `203.0.113.0/30` | `255.255.255.252` | `203.0.113.1` — `ISP_Ibb` | `203.0.113.2` — `Main_ISP` | `203.0.113.3` |
| `Main_ISP – ISP_Aden` | `198.51.100.0/30` | `255.255.255.252` | `198.51.100.1` — `Main_ISP` | `198.51.100.2` — `ISP_Aden` | `198.51.100.3` |
| `GW_Ibb – ISP_Ibb` | `10.10.10.0/30` | `255.255.255.252` | `10.10.10.1` — `GW_Ibb` | `10.10.10.2` — `ISP_Ibb` | `10.10.10.3` |
| `GW_Aden – ISP_Aden` | `10.20.10.0/30` | `255.255.255.252` | `10.20.10.1` — `GW_Aden / GWADEN` | `10.20.10.2` — `ISP_Aden` | `10.20.10.3` |
| `GW_Ibb – R_Ibb` | `10.60.10.0/30` | `255.255.255.252` | `10.60.10.1` — `GW_Ibb` | `10.60.10.2` — `R_Ibb / rabb` | `10.60.10.3` |
| `GW_Aden – R_Aden` | `10.70.10.0/30` | `255.255.255.252` | `10.70.10.1` — `GW_Aden / GWADEN` | `10.70.10.2` — `R_Aden / Radden` (`Multilink1`) | `10.70.10.3` |

The owner previously referred to the Aden gateway as `GW_Sanaa` in one table; the topology and report use `GWADEN`. The diagram uses `GW_Aden / GWADEN` provisionally to make the naming relationship visible without inventing a different device.

## 3. ISP loopback / LDP identifiers

| Router | Confirmed identifier / address | Prefix length |
|---|---|---|
| `Main_ISP` | `100.100.100.1` | Not supplied |
| `ISP_Ibb` | `100.100.100.2` | Not supplied |
| `ISP_Aden` | `100.100.100.3` | Not supplied |

These values are confirmed by the project owner as correct. Do not add `/32` to the table unless an archived configuration confirms the interface prefix.

## 4. Checks before publishing

1. **Ibb VLAN 20:** The current public documentation uses the owner-confirmed `192.168.30.65 255.255.255.192`. If the simulator is recovered, verify that its running subinterface configuration and OSPF statement match this plan; the live state cannot be confirmed from the available materials.
2. **Aden subinterfaces:** The latest output confirms subinterfaces `.30` and `.40`, unlike the older report excerpt that assigns `192.168.10.1/26` to the physical `FastEthernet0/0`. Revise the report to document the latest observed interface layout.
3. **Aden VLAN 40 routing:** The old OSPF excerpt only shows a network statement for `192.168.10.0/26`; confirm whether `192.168.10.64/28` is also advertised before claiming both VLANs are reachable through OSPF.
4. **Aden role names:** The topology uses VLAN 30 = Employee and VLAN 40 = Manager based on labels in the original drawing and the project owner's clarification. The older switch excerpt contains conflicting name text; do not claim the running VLAN database was independently verified.
5. **Multilink:** The latest output puts `10.70.10.2` on `Multilink1`; an older excerpt shows it on `Serial1/1`. Use the latest observation in the topology/report and do not present the old excerpt as current configuration.
6. **Security evidence:** The Port Security configuration excerpt sets violation mode to `shutdown`, while its evidence output says `Restrict`. Mark those as separate captures or reconcile them if more evidence is found.
7. **Credentials:** Remove actual CHAP passwords, shared secrets, and other credentials from the public GitHub copy.

## 5. Role-to-VLAN mapping

| Branch | VLAN | Role |
|---|---:|---|
| Ibb | 10 | Employee |
| Ibb | 20 | Manager |
| Aden | 30 | Employee |
| Aden | 40 | Manager |

The mapping is used consistently in the topology and README. The historical switch configuration text for Aden used conflicting VLAN names, so the mapping here is a documentation decision based on the supplied diagram and owner clarification, not a new claim about the unavailable running configuration.

## 6. Subnet validation summary

- Ibb VLAN 10 (`192.168.30.0/28`) and VLAN 20 (`192.168.30.64/26`) do not overlap.
- Aden VLAN 30 (`192.168.10.0/26`) and VLAN 40 (`192.168.10.64/28`) do not overlap.
- Each `/30` WAN network contains two usable addresses for its point-to-point link.
- This is a documentation baseline. Operational claims should be supported by saved command output or screenshots from the lab.
