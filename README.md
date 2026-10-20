# Cybersecurity Learning Progress

A portfolio documenting hands-on cybersecurity learning, beginning with network fundamentals and service assessment in an isolated virtual lab.

## Progress

| Lab | Topic | Status |
| --- | --- | --- |
| 01 | Network fundamentals, service inventory, and vulnerability-reference review | Completed |
| 02 | Local web-application security practice with DVWA | Planned; not yet completed |

## Completed work

### Lab 01 — Network Fundamentals

The lab covered interface and route inspection, local-network addressing, neighbor discovery, connectivity checks, service inventory, and reviewing reported software versions against vulnerability references. It was performed with Linux virtual machines on an isolated, host-only network.

The detailed [Lab 01 report](labs/01-network-fundamentals.md) preserves the recorded observations and findings while replacing host addresses and hardware identifiers with synthetic examples. It also distinguishes verified results from tentative version matches and future plans.

## How to read the examples

- Addresses in the report come from ranges reserved for documentation and examples; they are not the lab's original addresses and are not reachable lab targets.
- MAC-like identifiers are fabricated placeholders.
- Recorded timings, service versions, and assessment outcomes are retained where useful, with identifying context removed.
- The report does not reproduce exploit code, exploit triggers, or commands for automated vulnerability exploitation. Raw terminal captures and screenshots were not supplied and are not included.

## Responsible practice

Practical testing belongs only on systems you own or are explicitly authorized to assess. Keep vulnerable training machines isolated from public, school, employer, and other third-party networks. A service banner or vulnerability-reference match is a lead to verify, not proof by itself; prioritize safe validation and remediation.

## Next learning goals

- Continue subnetting and routing practice with synthetic examples.
- Study defensive service inventory and hardening.
- Set up the planned local DVWA exercise, then document the security lessons and prepared-statement remediation without publishing personal data or sensitive test evidence.
