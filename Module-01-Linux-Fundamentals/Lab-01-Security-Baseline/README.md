# Security Baseline

## Project Summary

This project established a known-good security baseline for an Ubuntu Server and compared the server's local state with the services visible remotely from Kali Linux. The review covered system identity, addressing, listening sockets, service status, host-firewall policy, remote exposure, authentication activity, and evidence integrity.

Only systems used in the completed lab are listed below.

## Environment

| System | Hostname | Username | IP Address | Role |
|---|---|---|---|---|
| Kali Linux | `kali-attacker` | `treyc` | `192.168.50.1` | Remote scanner |
| Ubuntu Server | `ubuntu-server` | `testlab` | `192.168.50.129` | Baseline target |

**Platform:** VMware Workstation Pro using a host-only network

## Investigation Scenario

The baseline was designed to answer five practical questions:

1. Did the systems have the expected identity and addressing?
2. Which TCP and UDP sockets were listening locally on the Ubuntu Server?
3. Were Apache, SSH, and UFW operating as expected?
4. Which services were reachable remotely from Kali Linux?
5. Did recent authentication activity contain accepted, failed, or invalid-user events that required review?

## Investigation Workflow

1. Confirmed the Ubuntu hostname, current user, and IP configuration.
2. Captured the server's listening TCP and UDP sockets with owning-process details.
3. Reviewed Apache and SSH service status.
4. Inspected the UFW firewall configuration.
5. Tested network connectivity from Kali Linux.
6. Performed default, version-detection, and targeted Nmap scans.
7. Reviewed authentication events on the Ubuntu Server.
8. Compared the local listening-socket view with the remotely reachable service view.
9. Documented the baseline and verified published artifacts with SHA-256 checksums.

## Key Findings

- The Ubuntu Server identity and host-only address matched the expected lab design.
- SSH and Apache were expected services on the target.
- Service activity and boot enablement represented separate operational states.
- A locally listening socket was not automatically reachable from another system.
- UFW and network placement influenced remote exposure.
- Local socket inspection and remote scanning answered different security questions and were most useful when compared.
- Authentication events provided a starting point for detecting unexpected access attempts.
- The completed baseline created a reference point for future change detection and incident investigation.

## Selected Commands

The concise command reference is available in [`commands.md`](commands.md). It contains the commands that best demonstrate system identification, socket inspection, service and firewall review, remote scanning, authentication review, and checksum verification.

## Skills Demonstrated

Linux system identification, network-interface review, listening-port analysis, process-to-socket attribution, systemd service inspection, host-firewall review, connectivity testing, Nmap scanning, authentication-log review, local-versus-remote exposure comparison, baseline documentation, and SHA-256 integrity verification.

## Security Relevance

A reliable server baseline helps cloud defenders identify unexpected services, unapproved network exposure, firewall drift, suspicious authentication activity, and changes from a known-good state. The same method applies to Linux virtual machines, jump hosts, web servers, administrative systems, and other cloud workloads.
