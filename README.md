# Cybersecurity Portfolio

This repository documents structured, hands-on Cybersecurity lab exercises focused on Linux security, networking, offensive security, detection, forensics, Azure, Microsoft Defender, Microsoft Sentinel, and incident response.

## Lab Environment

The lab is built as a segmented cybersecurity environment supporting enterprise infrastructure, defensive security monitoring, offensive security testing, virtualization, and network security.

### Physical Infrastructure

| System | Platform | Memory | Role |
|---|---|---:|---|
| Enterprise Virtualization Host | Proxmox VE | 32 GB | Hosts enterprise servers, Windows endpoints, and application workloads |
| Security Virtualization Host | Proxmox VE | 32 GB | Hosts SIEM, network monitoring, and DFIR/security workloads |
| Management Host | Dedicated Laptop | 16 GB | Trusted workstation for infrastructure administration and management |
| Redteam Host | Kali Linux | 16 GB | Dedicated offensive security and penetration-testing workstation |
| SW-Lab-01 | Aruba J9774A | — | Managed Layer 2 switch providing VLAN segmentation and traffic mirroring |
| opnsense-fw | OPNsense | 8 GB | Bare-metal firewall providing routing, VLAN gateways, filtering, and network security |
| Wireless Router | TP-Link Archer AXE75 | — | Provides upstream network and wireless connectivity |

### Enterprise Virtualization Host

| Virtual Machine | RAM | Purpose |
|---|---:|---|
| Windows Server / DC01 | 4 GB | Active Directory Domain Services and DNS |
| Ubuntu Server | 4 GB | Linux administration, security, and monitoring target |
| Web Server | 2 GB | DMZ-hosted web application and security testing target |
| Windows 11 | 8 GB | Domain-joined enterprise workstation and security-testing endpoint |
| File Server | 4 GB | Windows file services, permissions, SMB, and enterprise access-control testing |

### Security Virtualization Host

| Virtual Machine | RAM | Purpose |
|---|---:|---|
| Wazuh SIEM/XDR | 8 GB | Centralized security monitoring, log analysis, detection, and alerting |
| Network Security Monitoring | 8 GB | Suricata IDS and Zeek network telemetry and traffic analysis |
| DFIR / Threat Hunting | 4 GB | Velociraptor-based endpoint investigation, threat hunting, and incident response |

### Network Segmentation

| VLAN | Name | Purpose |
|---:|---|---|
| 10 | MANAGEMENT | Infrastructure administration and trusted management traffic |
| 20 | USERS | User workstations and domain endpoints |
| 30 | DMZ | Internet-facing and security-testing server workloads |
| 40 | SECOPS | Security monitoring and defensive infrastructure |
| 50 | SERVERS | Internal enterprise server infrastructure |
| 60 | REDTEAM | Offensive security and adversary-simulation systems |

## Curriculum

1. [Module 01 — Linux Fundamentals](Module-01-Linux-Fundamentals/)
2. [Module 02 — Linux Security](Module-02-Linux-Security/)
3. [Module 03 — Networking](Module-03-Networking/)
4. [Module 04 — Offensive Security](Module-04-Offensive-Security/)
5. [Module 05 — Detection and Forensics](Module-05-Detection-and-Forensics/)
6. [Module 06 — Cloud Fundamentals](Module-06-Cloud-Fundamentals/)
7. [Module 07 — Azure Security](Module-07-Azure-Security/)
8. [Module 08 — Microsoft Defender](Module-08-Microsoft-Defender/)
9. [Module 09 — Microsoft Sentinel](Module-09-Microsoft-Sentinel/)
10. [Module 10 — Incident Response](Module-10-Incident-Response/)
11. [Module 11 — Final Capstone](Module-11-Final-Capstone/)

## Completed Modules

✅ [Module 01 — Linux Fundamentals](Module-01-Linux-Fundamentals/)

