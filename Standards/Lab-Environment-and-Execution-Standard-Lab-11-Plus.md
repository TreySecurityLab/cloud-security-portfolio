# Lab Environment & Execution Standard — Lab 11+

## Purpose

This standard defines where and how all new Cloud Security Lab Exercises run beginning with Lab 11 — SSH Hardening. It is intentionally separate from the locked Lab Documentation Standard so environment changes cannot inadvertently alter the approved README structure.

## Authority and Scope

The authoritative Lab 01–77 curriculum remains unchanged. This standard must never renumber, rename, replace, insert, delete, or reorder canonical labs, and it must never replace the primary skill objective of a lab.

The governing hierarchy is:

1. Authoritative Curriculum
2. Lab Documentation Standard
3. Lab Environment & Execution Standard — Lab 11+
4. Individual Lab

The Lab Documentation Standard controls how labs are documented. This standard controls execution location, local paths, infrastructure roles, network placement, and home-lab integration beginning with Lab 11.

## Effective Point

This standard applies to every newly generated lab beginning with **Lab 11 — SSH Hardening**.

Labs completed before Lab 11 retain their historical execution paths and evidence as part of the completed record unless the user explicitly authorizes a migration or correction.

## Authoritative Local Paths

Beginning with Lab 11, use these locations:

| Purpose | Authoritative path |
| --- | --- |
| Kali portfolio repository | `~/cyber-portfolio` |
| Ubuntu lab root | `~/cyber-labs` |
| Ubuntu per-lab directory | `/home/testlab/cyber-labs/lab-XX-<lab-name>` |
| Ubuntu per-lab evidence | `/home/testlab/cyber-labs/lab-XX-<lab-name>/evidence` |

`XX` is the canonical two-digit lab number and `<lab-name>` is the canonical lab-name slug used for that lab.

Do not introduce `~/cloud-security-portfolio`, `~/cloud-security-labs`, `/home/testlab/cloud-security-labs`, or other superseded course paths into Lab 11+ instructions, scripts, evidence retrieval, or publication workflows.

## Core System Roles

### Kali Redteam Host

- Bare-metal Kali Linux system.
- Primary local portfolio/repository workstation.
- Portfolio repository path: `~/cyber-portfolio`.
- Offensive-security and remote validation system when appropriate to the canonical lab objective.
- Network role: VLAN 60 — REDTEAM when attached to the final home-lab network.

### Ubuntu Server

- Primary Linux security investigation target used by the course.
- General-purpose Linux server for administration, SSH, services, hardening, logging, automation, and security exercises.
- Lab root: `~/cyber-labs` for user `testlab`.
- Per-lab evidence is stored beneath the corresponding lab directory's `evidence` folder.
- Intended network placement: VLAN 50 — SERVERS on the Enterprise Virtualization Host after the VMware-to-Proxmox migration and network placement are completed and verified.

### Management Host

- Dedicated administration system.
- Network role: VLAN 10 — MANAGEMENT.
- Used to administer OPNsense, SW-Lab-01, Proxmox, servers, and other infrastructure when management-plane activity is relevant to a lab.
- Must not be repurposed as an attack target merely to add infrastructure to an exercise.

### Enterprise Virtualization Host

- Proxmox VE platform for enterprise workloads.
- Proxmox management belongs on VLAN 10 — MANAGEMENT.
- Planned/authoritative workload placement:
  - Windows 11 Workstation — VLAN 20 USERS
  - Web Server — VLAN 30 DMZ
  - DC01 — VLAN 50 SERVERS
  - Ubuntu Server — VLAN 50 SERVERS
  - File Server — VLAN 50 SERVERS

### Security Virtualization Host

- Proxmox VE platform for security monitoring and investigation workloads.
- Proxmox management belongs on VLAN 10 — MANAGEMENT.
- Security tooling belongs on VLAN 40 — SECOPS.
- Baseline security workloads include Wazuh SIEM/XDR, Suricata + Zeek, and Velociraptor/DFIR when deployed and verified.

## Network Architecture

Authoritative VLANs:

| VLAN | Name | Network | Primary purpose |
| ---: | --- | --- | --- |
| 10 | MANAGEMENT | `10.10.10.0/24` | Administrative control plane |
| 20 | USERS | `10.10.20.0/24` | Simulated enterprise endpoints |
| 30 | DMZ | `10.10.30.0/24` | Isolated web/application services |
| 40 | SECOPS | `10.10.40.0/24` | Monitoring, detection, and investigation |
| 50 | SERVERS | `10.10.50.0/24` | Enterprise server infrastructure |
| 60 | REDTEAM | `10.10.60.0/24` | Offensive-security activity |

OPNsense is the Layer-3 gateway and policy-enforcement boundary between lab VLANs. SW-Lab-01, the Aruba J9774A, provides Layer-2 VLAN transport and segmentation.

Current authoritative Aruba port roles:

- Port 1 — OPNsense trunk; tagged VLANs 10, 20, 30, 40, 50, and 60.
- Port 8 — MANAGEMENT PC access; untagged VLAN 10.
- All other switch ports — TBD until explicitly assigned. Do not infer permanent port roles.

## Standing Integration Rule

Beginning with Lab 11, every newly generated lab must evaluate which verified parts of the upgraded home lab can support the canonical objective. Incorporate infrastructure only when it improves realism, security validation, evidence collection, troubleshooting, or portfolio value.

Relevant integration may include:

- management-plane access from the Management Host or MANAGEMENT segment;
- VLAN and security-zone awareness;
- OPNsense policy as a network-layer control;
- Aruba switching and VLAN configuration as Layer-2 context;
- Proxmox-hosted workloads after migration and network placement are verified;
- host and network evidence correlation when it materially strengthens the investigation;
- positive and negative testing across host and network controls;
- troubleshooting that distinguishes endpoint, switching, routing, firewall, authentication, authorization, and application causes.

## Lab Objective Protection

Infrastructure must support the canonical lesson, not replace it.

For example, Lab 11 must primarily teach SSH hardening even when OPNsense policy or VLAN segmentation is used to demonstrate defense in depth. Higher-level tooling such as SIEM, NDR, EDR, or automated detection platforms must not replace foundational course skills. Introduce those tools only when they naturally align with the curriculum or supplement the primary objective.

## Verified-State Rule

Do not present planned infrastructure as completed infrastructure.

- A migration, VLAN placement, switch-port assignment, security sensor path, or monitoring integration must be completed and verified before a lab depends on it as an operational prerequisite.
- Planned components may be identified as future integration opportunities, but commands, IP assumptions, and evidence retrieval must use the current verified state.
- The authoritative local filesystem paths in this document are approved for Lab 11+ and are not subject to the previous-path preservation warning.

## Evidence and Publication Workflow

Beginning with Lab 11:

- Ubuntu evidence must be collected under `/home/testlab/cyber-labs/lab-XX-<lab-name>/evidence`.
- Portfolio update and publication workflows must target `~/cyber-portfolio` on Kali.
- Evidence retrieval must identify the canonical lab directory explicitly and must fail closed if the expected directory, files, or checksums are missing or ambiguous.
- Existing screenshot, checksum, README, updater, and publication requirements remain governed by the separate Lab Documentation Standard.
- Infrastructure evidence should be retained only when it directly supports the lab objective or demonstrates a meaningful defense-in-depth control.

## Change Control

This standard is independently locked from the Lab Documentation Standard.

A future change to paths, VLAN placement, host roles, Proxmox placement, OPNsense behavior, Aruba port assignments, or evidence retrieval changes this document only. It does not authorize changes to README structure or documentation formatting.

Likewise, changes to README headings, Environment formatting, evidence presentation, or other documentation rules must be made through the Lab Documentation Standard and do not automatically alter this environment standard.
