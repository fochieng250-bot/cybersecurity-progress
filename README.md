# Cybersecurity Learning Progress

A public portfolio of cybersecurity learning, starting with network fundamentals. This repository summarizes hands-on work in an isolated lab without publishing personal details, network identifiers, raw scan output, or exploit instructions.

## Progress

| Lab | Topic | Status |
| --- | --- | --- |
| 01 | Network fundamentals and introductory service assessment | Completed |

## Lab 01: Network Fundamentals

I practiced the fundamentals in a private, host-only virtual machine lab using Linux, VirtualBox, and standard networking tools. The work covered:

- Inspecting network interfaces, routes, and neighbor tables.
- Understanding the roles of IP addressing, subnet masks, MAC addresses, and address resolution.
- Comparing local loopback communication with connectivity between virtual machines.
- Using packet loss, latency, and TTL as observations, while recognizing their limitations.
- Learning how service identification and public vulnerability references can inform a defensive review.
- Identifying outdated software as a maintenance risk and considering remediation such as patching, disabling unnecessary services, and limiting access.

The lab was restricted to virtual machines I controlled on an isolated virtual network. No public, school, employer, or third-party systems were assessed.

See the [Lab 01 notes](labs/01-network-fundamentals.md) for the sanitized learning record.

## Safe publication choices

This repository intentionally contains no personal name, age, location, account identifiers, IP or MAC addresses, hostnames, raw command output, screenshots, credentials, exploit payloads, or target-specific exploitation steps. The source notes contained sensitive details, so only a high-level summary is published. Do not add unredacted terminal captures or lab exports.

## Next learning goals

- Strengthen subnetting and routing fundamentals with synthetic examples.
- Practice defensive service inventory and explain how to remediate unnecessary or outdated services.
- Keep future lab notes focused on authorized scope, lessons learned, and mitigations.

## Responsible use

All practical work should remain within systems you own or have explicit permission to assess. Tool output and version matches are leads, not proof of exploitability; validate findings safely and prioritize remediation.
