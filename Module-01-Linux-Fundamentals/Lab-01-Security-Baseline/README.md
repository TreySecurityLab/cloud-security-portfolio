# Security Baseline

## Project Overview

This lab established a known-good security baseline for an Ubuntu Server and compared the server's local state with the services visible remotely from Kali Linux.

## Lab Environment

| System | Identity | IP Address | Role |
|---|---|---|---|
| Kali Linux | `treyc@kali-attacker` | `192.168.50.1` | Remote scanner |
| Ubuntu Server | `testlab@ubuntu-server` | `192.168.50.129` | Target server |
| Windows 11 | `testlab@Win11-VM` | `192.168.50.128` | Future Windows target |

**VMware network:** `Host-only`

## Objective

- Confirm system identity and addressing.
- Review listening TCP and UDP sockets.
- Inspect expected services.
- Review the host firewall.
- Compare local listening ports with remotely reachable services.
- Review authentication activity.
- Preserve evidence for later comparison.

## Hands-on Lab

1. Verified the Ubuntu hostname and current user.
2. Identified the Ubuntu Server IP address.
3. Reviewed listening TCP and UDP sockets.
4. Checked Apache and SSH service status.
5. Reviewed UFW firewall status.
6. Tested connectivity from Kali.
7. Performed default, version-detection, and targeted Nmap scans.
8. Reviewed authentication activity.
9. Compared the local and remote views of the server.

## Commands Used

The complete command reference is available in [`commands.md`](commands.md).

## Key Findings

- SSH and Apache were expected services.
- A service can be active without being enabled at boot.
- A locally listening port may not be remotely reachable.
- A known-good baseline provides a comparison point for future investigations.

## Why This Matters in Cloud Security

The same baseline process applies to cloud-hosted Linux virtual machines:

- Identify expected services.
- Confirm allowed network exposure.
- Review firewall and security-group rules.
- Monitor authentication activity.
- Detect unexpected changes.
- Document evidence for investigations.

## Skills Demonstrated

- Linux system identification
- Network-interface review
- Listening-port analysis
- Service management
- Firewall inspection
- Nmap scanning
- Authentication-log review
- Evidence collection

## Technologies Used

- Kali Linux
- Ubuntu Server
- Windows 11
- VMware Workstation Pro
- OpenSSH
- Apache HTTP Server
- UFW
- Nmap
- systemd

## Evidence Collected

Evidence guidance is available in [`evidence/README.md`](evidence/README.md).

## Screenshots

The screenshot checklist is available in [`screenshots/README.md`](screenshots/README.md).

## Lessons Learned

- Local and remote service views answer different security questions.
- Firewall policy affects whether a listening service is reachable.
- Service state and boot enablement are separate properties.
- Baselines support later change detection and incident investigation.
