# Lab 01: Network Fundamentals

**Status:** Completed
**Environment:** Two virtual machines on an isolated, host-only virtual network
**Tools studied:** Linux networking utilities, `ping`, Nmap, and SearchSploit

## Objective

Understand how hosts communicate on a local network, how to inspect basic connectivity, and how service inventory relates to defensive vulnerability review.

## Work completed

1. Inspected network interfaces and routing information to distinguish local connectivity from a default route.
2. Reviewed neighbor-table concepts and the relationship between network-layer addresses and link-layer addresses.
3. Compared loopback communication with communication between virtual machines, observing latency and packet loss.
4. Learned that TTL values can provide a rough operating-system clue but are not definitive identification.
5. Practiced service identification in the isolated lab and consulted local vulnerability-reference data to understand how reported software versions can guide patch review.
6. Considered defensive responses to legacy services, including upgrading, disabling services that are not needed, and restricting network access.

## Takeaways

- A subnet mask describes which addresses are local to a network; a router is used to reach other networks.
- Neighbor discovery helps a host deliver traffic on the local link.
- Loopback traffic remains within the local machine, while virtual-machine traffic traverses a virtual network.
- Latency and TTL depend on the path and system configuration; neither should be treated as conclusive evidence on its own.
- A service banner or advisory match is an initial lead. It does not by itself establish that a system is vulnerable.
- Isolating deliberately vulnerable practice systems reduces the risk of affecting unrelated networks.

## Scope and publication

Testing was limited to virtual machines under my control in an isolated lab. This public summary omits all addresses, hardware identifiers, usernames, hostnames, raw outputs, screenshots, exact target details, and exploitation procedures from the working notes. No raw evidence is published.

## Follow-up

Continue with subnetting and routing exercises using synthetic examples, then document defensive service-hardening lessons without publishing target-specific details.
