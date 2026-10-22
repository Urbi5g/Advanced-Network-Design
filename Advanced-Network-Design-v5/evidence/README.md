# Evidence inventory

This folder is reserved for screenshots or text exports captured directly from the lab. The current HTML report includes text outputs for the tests listed below, but the separate raw screenshot/image files were not supplied as standalone assets when this bundle was assembled.

| Evidence | Current basis | Limitation |
|---|---|---|
| MPLS LDP neighbors | Text output included in report | Tie to exact router/capture when possible |
| MPLS forwarding table | Text output included in report | Some prefixes are `/24` summaries/older output; reconcile against current address plan |
| Ping | Router `rabb` to `198.51.100.2` | Not an end-host-to-end-host branch VLAN test |
| Traceroute | Router `rabb` to `198.51.100.2` | Shows path to ISP address in the captured test only |
| Port Security | Text output included in report | Output says `Restrict`, while supplied configuration says `shutdown` |
| Interface brief | Supplied latest output from `Radden` | Masks are not shown by this command |

When adding evidence, include device name, command, date/time, expected outcome, actual outcome, and an unedited screenshot or text export. Redact credentials and sensitive identifiers.
