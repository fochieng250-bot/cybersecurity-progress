# Lab 01: Network Fundamentals and Service Assessment

**Status:** Completed
**Date recorded in source notes:** 18 October 2026
**Environment:** Kali Linux and Metasploitable 2 virtual machines in an isolated, host-only lab
**Tools used:** VirtualBox, Nmap 7.99, SearchSploit, `ip`, `arp`, `ping`, and `ifconfig`

> **Privacy and safety:** This is a sanitized account of the supplied lab notes. Original addresses, MAC addresses, usernames, hostnames, personal details, screenshots, and raw logs are not reproduced. The addresses below are reserved documentation examples, not the addresses used in the lab. MAC-like values are fabricated. Exploit code, exploit triggers, and commands for automated exploitation are intentionally omitted.

## Objective

Learn how hosts communicate on a local network, inspect connectivity, identify exposed services in an authorized practice environment, and understand how vulnerability references can guide defensive review.

The original lab questions included:

1. How do IP addresses, subnets, and MAC addresses relate to communication between neighboring hosts?
2. Why can loopback communication be faster than communication between virtual machines?
3. What can a TTL value suggest about an operating system, and what are the limitations of that clue?
4. How can a service identify its software version, and how can that version be checked against public vulnerability references?
5. What does it mean when a deliberately vulnerable training service is reported as exploitable?

## 1. Lab setup and scope

The work was performed against virtual machines controlled by the learner, using a host-only virtual network for the exercises. Host-only networking is intended to limit communication to the host and attached virtual machines, but the virtual-machine network configuration should always be checked rather than assumed safe. NAT was used separately for system updates, then the lab was returned to its isolated mode.

The following table uses fabricated labels and documentation-only addresses to explain the topology. These are not the lab's original identifiers and must not be treated as live targets.

| Role | Sanitized example | Purpose |
| --- | --- | --- |
| Analyst VM | `LAB-CLIENT-A`, `192.0.2.21/24`, fabricated MAC `02:00:00:00:00:21` | Linux tools and observations |
| Training VM | `LAB-TARGET-B`, `192.0.2.22/24`, fabricated MAC `02:00:00:00:00:22` | Deliberately vulnerable practice system |
| Host-only subnet | `192.0.2.0/24` | Documentation-only illustration of a local subnet |
| NAT example | `198.51.100.15/24`, gateway `198.51.100.2` | Documentation-only illustration of the separate update path |

`192.0.2.0/24` and `198.51.100.0/24` are reserved for documentation. The examples are substituted values, not captured lab addresses. The example MAC-like identifiers are invented, locally administered placeholders; a MAC address is not a permanent identity proof and does not reliably prove which virtualization product is in use.

## 2. Interfaces, addressing, and routes

The interface inspection exercise used `ip a` and `ifconfig`. The important observations were:

- The analyst VM had a host-only interface with an IPv4 address and a `/24` mask.
- A `/24` subnet contains 256 total addresses; in the familiar private IPv4 setting, 254 are conventionally assignable to hosts.
- The loopback interface was present independently of the virtual network interface.
- A second interface was available for the separate NAT/update configuration.
- Interface packet counters showed that traffic had passed through the virtual interface during the lab.

For this report, the analyst and target are represented by the documentation addresses `192.0.2.21/24` and `192.0.2.22/24`. A `/24` mask makes them members of the same illustrated subnet, so local-link communication does not require a router. These addresses replace, rather than reveal, the actual environment values.

The route-table exercise used `ip r`. In the NAT/update configuration, the recorded route showed a default gateway and a connected network. In host-only mode, the lab subnet appeared as a directly connected route. A default route is used for destinations without a more specific route; a directly connected subnet is reached on its local interface.

The notes compared a host-only interface and a NAT interface to explain that they serve different purposes: isolated lab communication versus an outbound update path. NAT does not make arbitrary lab testing safe, and a host-only configuration should be verified before testing.

## 3. Neighbor discovery

The exercises used `arp -a` and `ip neigh` to inspect neighbor information. The NAT-mode observation included a gateway and another nearby entry; the host-only exercise described the analogous relationship between the training machines.

In general, ARP resolves an IPv4 address to a link-layer address on a local network. Neighbor-table states such as `REACHABLE`, `DELAY`, or `STALE` describe the freshness or validation state of an entry; they do not independently establish that a host is trustworthy.

All addresses and hardware identifiers in any illustrative neighbor entry are replaced or omitted. The original hardware identifiers are not included in this portfolio.

## 4. Connectivity checks

The notes used `ping` to compare the training VM with loopback. For reproducibility in the write-up, the remote lab peer is represented by the documentation-only address `192.0.2.22`. The address is reserved for examples; it is not the original target address.

### Analyst VM to the training VM

The recorded three-packet test received three replies with no packet loss. One run reported round-trip times of approximately **5.09 ms**, **1.13 ms**, and **1.46 ms**, with an average of **2.557 ms**. Another run had different measurements, illustrating normal variation.

### Loopback

The local loopback test also received three replies with no packet loss. The recorded round-trip measurements were approximately **0.687 ms**, **0.035 ms**, and **0.037 ms**, with an average of **0.253 ms**.

### Interpretation and self-test

- Loopback communication stays within the local operating system, while communication between VMs traverses a virtual network path. That helps explain the observed latency difference.
- Zero packet loss means all three test packets received replies in that run; it does not prove that every service is available or that a host is healthy.
- TTL values can provide a rough operating-system clue: common initial TTL defaults differ between operating systems. Routing, configuration, and network devices can change the observed value, so TTL is not a definitive OS identification method.
- Latency depends on load and the virtual network path. A single short sample should not be generalized into a universal network-performance claim.

## 5. Service inventory

The lab included a full TCP port and service assessment, followed by a shorter check of three selected services. The original notes recorded a full-assessment duration of about **243.91 seconds**, with **30 open TCP ports** and **65,505 closed TCP ports** reported. The selected-service check took about **7.88 seconds**.

The example service names and versions below are the versions recorded in the original notes. Host identifiers, raw scan output, and commands for the broad scan are not republished.

| Port | Service reported | Version or observation in notes |
| --- | --- | --- |
| 21/tcp | FTP | vsftpd 2.3.4 |
| 22/tcp | SSH | OpenSSH 4.7p1, Debian/Ubuntu build |
| 80/tcp | HTTP | Apache httpd 2.2.8, Ubuntu, DAV/2 |
| 445/tcp | SMB | Samba smbd 3.0.20, Debian |
| 1524/tcp | Bindshell | Listed by the training VM assessment |
| 3632/tcp | distccd | distccd v1, GNU 4.2.4 |
| 6667/tcp | IRC | UnrealIRCd |
| 8180/tcp | HTTP | Apache Tomcat/Coyote JSP engine 1.1, Tomcat 5.5 |
| Other ports | Various | The notes report 22 additional open ports but do not include their complete service list |

The quick check of FTP, SSH, and HTTP returned the same three service/version families shown above. The notes also recorded that service banners and other network observations informed a broad Linux/Unix identification. The original kernel string and hardware address are deliberately not included.

### What this showed

- A version string can help identify software that deserves patch review, but it can be inaccurate or incomplete.
- A service banner is observable without authenticating to that service; this is useful for inventory but may disclose implementation details.
- Thirty open ports on a deliberately vulnerable legacy training VM represented a large attack surface in this exercise. The appropriate defensive lesson is to minimize exposed services and keep required software maintained—not to assume that any particular port count is universally acceptable.
- The listed versions are old software examples. Product-version matching alone does not account for vendor backports or local configuration.

## 6. Vulnerability-reference review

The lab used SearchSploit's local reference data to look up the service versions recorded above. The notes recorded these matches:

| Observed software family | Reference result recorded in notes | Assessment |
| --- | --- | --- |
| vsftpd 2.3.4 | Public backdoor-command-execution references; associated with CVE-2011-2523 | The training VM's automated check also reported this finding. |
| Samba 3.0.20 | A public username-map-script command-execution reference for versions before 3.0.25rc3 | Candidate match based on version; a search result alone is not proof that a particular build is exploitable. |
| Apache Tomcat 5.5 | References covering information disclosure and cross-site scripting in particular releases | Requires exact version, patch, configuration, and advisory review. |

The original workflow was to compare an observed version with local reference results and then determine whether the finding applied. Search results are leads, not authorization to test a system and not proof of exploitability. Distribution maintainers may backport fixes without changing the upstream version string.

The original notes also record a manual Metasploit attempt against the training service that received a connection-refused result during a cooldown. That attempt did not establish successful access; a refusal or failed attempt is not evidence that a system is safe.

## 7. Automated finding and validation outcome

An automated vulnerability check in the isolated practice lab reported the vsftpd 2.3.4 issue associated with **CVE-2011-2523**. The recorded result included a privileged command-identity outcome, indicating root-level execution on the deliberately vulnerable training VM. This confirms the lab's reported finding; it is not presented as a procedure to reproduce.

The check against the SMB service reported a negative result for one specific SMB vulnerability check. That result does not rule out other SMB issues and does not invalidate the separate version-reference match. Each check covers a limited condition.

To keep the public portfolio from becoming a reusable exploitation guide, the automated exploitation command, script details, trigger, payload, exploit code, module path, and raw output are intentionally omitted. The defensive lesson is that a deliberately vulnerable legacy service can have critical impact, and that assessment must remain confined to an authorized isolated lab.

## 8. Key learning summary

| Concept | Lesson from this lab | Defensive relevance |
| --- | --- | --- |
| IPv4 subnet and `/24` | Addresses in the same subnet can communicate over the local link without a router. | Helps reason about network boundaries and routing. |
| Private lab versus public network | The exercise used host-only virtual machines; the report substitutes documentation-only addresses. | Verify isolation and scope before any testing. |
| MAC and neighbor tables | Address resolution maps local IPv4 peers to link-layer addresses; entries can become stale. | Useful for troubleshooting, not proof of identity. |
| Ping and packet loss | Replies demonstrated connectivity at that moment; they did not validate application health. | Use measurements in context and avoid overclaiming. |
| TTL | A TTL can suggest an initial default, but it is not definitive OS identification. | Treat network fingerprints as tentative clues. |
| Latency | Loopback was faster than the VM-to-VM path in the recorded sample. | Path, host load, and virtualization affect timing. |
| Host-only and NAT | The two modes served different purposes in the lab. | Keep vulnerable systems isolated and verify adapter settings. |
| Service identification | Version banners helped build an inventory of old services. | Inventory supports patching and attack-surface reduction. |
| SearchSploit and CVEs | Local reference matches linked versions to published vulnerability records. | Validate exact applicability; do not treat a match as proof. |
| Vulnerability validation | A lab check reported a critical result on the intentionally vulnerable VM. | Prioritize remediation; never run intrusive checks outside authorized scope. |

## 9. Evidence checklist

The source notes listed possible terminal screenshots for interfaces, connectivity tests, service inventory, and vulnerability-reference research. No screenshot files were supplied with the source document, so none are claimed or published here. Do not add screenshots without checking them for real addresses, usernames, hostnames, browser tabs, file paths, or other identifying information.

| Evidence topic from source notes | Sanitized portfolio status |
| --- | --- |
| Interface and address configuration | Described above; original output and addresses omitted |
| VM-to-VM connectivity | Recorded packet-loss and timing summary retained |
| Loopback comparison | Recorded timing summary retained |
| Selected-service inventory | Recorded service/version summary retained |
| Local vulnerability-reference lookup | Findings summarized; exploit paths and code omitted |
| Automated vulnerability validation | Outcome summarized; command and reproduction details omitted |

## 10. Scope and ethics

The supplied notes state that testing was conducted on owned virtual machines in host-only isolation and that no public, school, or third-party networks were scanned. This portfolio preserves that stated scope while omitting personal identity, location, real network identifiers, raw command output, and screenshots.

All practical testing should stay within systems you own or have explicit permission to assess. Keep deliberately vulnerable machines isolated. Avoid bridged networking for vulnerable targets unless the network owner has explicitly approved and secured the setup.

## 11. Next planned study

The source notes proposed a second project using the Damn Vulnerable Web Application (DVWA) in a local Docker environment. This is a future plan, not completed work. The intended learning goals are to study web-application input handling, understand SQL-injection risk in a purpose-built training app, and compare unsafe handling with parameterized queries/prepared statements. No account credentials or exploitation steps from the original notes are reproduced.

## Source-document note

This report is based on `01-Network-Fundamentals-Lab.md`, supplied for this portfolio. It expands the recorded work while replacing personal and network identifiers, removing unsafe reproduction details, and distinguishing completed activities from future plans.
