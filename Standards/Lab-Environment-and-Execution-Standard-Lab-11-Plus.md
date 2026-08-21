# Lab Environment & Execution Standard — Lab 11+

## Purpose

This standard defines where and how all new Cloud Security Lab Exercises run beginning with **Lab 11 — SSH Hardening**. It is intentionally separate from the locked Lab Documentation Standard so environment changes cannot alter the approved README structure.

## Authority and Scope

The authoritative Lab 01–77 curriculum remains unchanged. This standard must never renumber, rename, replace, insert, delete, or reorder canonical labs, and it must never replace a lab's primary skill objective.

The governing hierarchy is:

1. Authoritative Curriculum
2. Lab Documentation Standard
3. Lab Environment & Execution Standard — Lab 11+
4. Individual Lab

This standard controls:

- execution systems and infrastructure roles;
- local filesystem paths;
- IP and VLAN placement;
- operational prerequisites and verified-state decisions;
- evidence collection locations and retrieval source paths; and
- home-lab integration during execution.

The Lab Documentation Standard exclusively controls README structure, Environment-table formatting, portfolio-facing prose, screenshot presentation, checksum presentation, and publication formatting. This standard may identify what evidence must be collected and where it resides, but it does not define how that evidence is presented in the README.

## Effective Point

This standard applies to every newly generated lab beginning with Lab 11. Labs completed before Lab 11 retain their historical execution paths and evidence unless the user explicitly authorizes a migration or correction.

## Authoritative Local Paths

| Purpose | Authoritative path |
| --- | --- |
| REDTEAM-01 portfolio repository | `~/cyber-portfolio` |
| LINUX-01 lab root | `~/cyber-labs` |
| LINUX-01 per-lab directory | `/home/testlab/cyber-labs/lab-XX-<lab-name>` |
| LINUX-01 per-lab evidence | `/home/testlab/cyber-labs/lab-XX-<lab-name>/evidence` |

`XX` is the canonical two-digit lab number and `<lab-name>` is the canonical lab-name slug.

Do not introduce `~/cloud-security-portfolio`, `~/cloud-security-labs`, `/home/testlab/cloud-security-labs`, or other superseded course paths into Lab 11+ instructions, scripts, evidence retrieval, or publication workflows.

## Core System Roles

### REDTEAM-01

- Bare-metal Kali Linux system at authoritative address `10.10.60.10` when attached to VLAN 60 REDTEAM.
- Primary local portfolio and repository workstation at `~/cyber-portfolio`.
- Offensive-security and remote-validation system when appropriate to the canonical objective.

### LINUX-01

- Primary Linux security investigation target.
- General-purpose Ubuntu server for administration, SSH, services, hardening, logging, automation, and security exercises.
- Lab root: `~/cyber-labs` for user `testlab`.
- Authoritative planned address: `10.10.50.40` on VLAN 50 SERVERS after Proxmox migration, placement, and validation are complete.
- Until that migration and placement are verified, instructions must use the current verified connection state rather than assume VLAN 50 operation.

### MGMT-01

- Primary dedicated administration system at `10.10.10.5` on VLAN 10 MANAGEMENT.
- Used to administer OPNsense, SW-Lab-01, Proxmox, servers, and infrastructure when management-plane activity is relevant.
- Connected to SW-Lab-01 port 8 as the verified VLAN 10 management access endpoint.
- Must not be repurposed as an attack target merely to add infrastructure to an exercise.

### MGMT-BACKUP

- Secondary management endpoint at `10.10.10.6` when locally attached to VLAN 10 MANAGEMENT.
- Provides a backup administration path; it is not assumed to be continuously connected.

### ENTHOST-01

- Proxmox VE enterprise virtualization host at `10.10.10.3` on VLAN 10 MANAGEMENT.
- Physically connected to SW-Lab-01 port 7.
- Final VLAN-aware guest trunk configuration and verification remain pending; labs must not depend on guest-VLAN placement before verification.
- Planned workloads:
  - WIN11-01 — VLAN 20 USERS, `10.10.20.10`
  - WEB-01 — VLAN 30 DMZ, `10.10.30.10`
  - DC-01 — VLAN 50 SERVERS, `10.10.50.10`
  - DC-02 — VLAN 50 SERVERS, `10.10.50.20`; planned/rotational, approximately 3–4 GB RAM, excluded from the normal 22 GB always-on guest allocation
  - FILE-01 — VLAN 50 SERVERS, `10.10.50.30`
  - LINUX-01 — VLAN 50 SERVERS, `10.10.50.40`

### SECHOST-01

- Proxmox VE security virtualization host at `10.10.10.4` on VLAN 10 MANAGEMENT.
- Security tooling belongs on VLAN 40 SECOPS after deployment and verification.
- Planned baseline workloads include Wazuh SIEM/XDR, Suricata + Zeek, and Velociraptor/DFIR.

## Network Architecture

| VLAN | Name | Network | Primary purpose |
| ---: | --- | --- | --- |
| 10 | MANAGEMENT | `10.10.10.0/24` | Administrative control plane |
| 20 | USERS | `10.10.20.0/24` | Simulated enterprise endpoints |
| 30 | DMZ | `10.10.30.0/24` | Isolated web/application services |
| 40 | SECOPS | `10.10.40.0/24` | Monitoring, detection, and investigation |
| 50 | SERVERS | `10.10.50.0/24` | Enterprise server infrastructure |
| 60 | REDTEAM | `10.10.60.0/24` | Offensive-security activity |

`opnsense-fw` at `10.10.10.1` is directly connected to the ISP and is the Layer-3 gateway and policy-enforcement boundary. `SW-Lab-01` at `10.10.10.2` is the Aruba J9774A Layer-2 switching platform.

Current authoritative Aruba port state:

- Port 1 — verified OPNsense 802.1Q trunk carrying VLANs 10, 20, 30, 40, 50, and 60.
- Port 6 — physical MGMT-BACKUP connection when locally attached; management use verified by the user.
- Port 7 — physical ENTHOST-01 connection; final VLAN-aware guest trunk configuration remains pending.
- Port 8 — verified MGMT-01 VLAN 10 access connection.
- Ports 2–5 and 9 onward — TBD until explicitly assigned; do not infer permanent roles.

## Standing Integration Rule

Every newly generated Lab 11+ exercise must evaluate which verified parts of the home lab support the canonical objective. Incorporate infrastructure only when it improves realism, validation, evidence collection, troubleshooting, or portfolio value.

Infrastructure must support the canonical lesson rather than replace it. For example, Lab 11 must primarily teach SSH hardening even when OPNsense policy or VLAN segmentation demonstrates defense in depth. Higher-level SIEM, NDR, EDR, or automated detection tooling must not replace foundational course skills.

## Verified-State Rule

Do not present planned infrastructure as completed infrastructure.

- A migration, VLAN placement, switch-port role, sensor path, agent, or monitoring integration must be completed and verified before a lab depends on it.
- Planned components may be identified as future opportunities, but commands, IP assumptions, and evidence retrieval must use the current verified state.
- LINUX-01 export, rename, and data-hash verification are complete; Proxmox import and VLAN 50 placement remain pending.

## Evidence Collection and Retrieval

- Collect LINUX-01 evidence under `/home/testlab/cyber-labs/lab-XX-<lab-name>/evidence`.
- Target `~/cyber-portfolio` on REDTEAM-01 for portfolio update and publication workflows.
- Identify the canonical lab directory explicitly during retrieval.
- Fail closed when expected directories, files, or checksums are missing or ambiguous.
- Retain infrastructure evidence only when it directly supports the canonical objective or a meaningful defense-in-depth control.
- Defer README placement, captions, screenshot presentation, checksum presentation, and publication formatting to the Lab Documentation Standard.

## Change Control

This standard is independently locked from the Lab Documentation Standard.

Changes to paths, IPs, VLAN placement, host roles, Proxmox placement, OPNsense behavior, switch-port state, operational prerequisites, evidence collection locations, or retrieval source paths belong here only. They do not authorize changes to README structure or documentation presentation.
