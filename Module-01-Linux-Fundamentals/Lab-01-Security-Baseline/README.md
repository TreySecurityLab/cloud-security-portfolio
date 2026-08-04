# Security Baseline

## Objective

Establish a known-good security baseline for `testlab@ubuntu-server` at `192.168.50.129` and compare its local state with the services visible remotely from `treyc@kali-attacker` at `192.168.50.1`.

## Environment

| System | Identity | IP Address | Role |
|---|---|---|---|
| Kali Linux | `treyc@kali-attacker` | `192.168.50.1` | Remote scanner |
| Ubuntu Server | `testlab@ubuntu-server` | `192.168.50.129` | Target server |
| Windows 11 | `testlab@Win11-VM` | `192.168.50.128` | Future Windows target |

**VMware network:** `Host-only`

## Lab Summary

The following activities were completed:

1. Verified the Ubuntu hostname and current user.
2. Identified the Ubuntu Server IP address.
3. Reviewed listening TCP and UDP sockets.
4. Checked Apache and SSH service status.
5. Reviewed UFW firewall status.
6. Tested connectivity from Kali.
7. Performed default, version-detection, and targeted Nmap scans.
8. Reviewed authentication activity.
9. Compared the local and remote views of the server.

## Key Findings

- SSH and Apache were expected services.
- A service can be active without being enabled at boot.
- A locally listening port may not be remotely reachable.
- A known-good baseline provides a comparison point for future investigations.

## Cloud Security Relevance

The same baseline process applies to cloud-hosted Linux virtual machines:

- Identify expected services
- Confirm allowed network exposure
- Review firewall and security-group rules
- Monitor authentication activity
- Detect unexpected changes
- Document evidence for investigations

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
