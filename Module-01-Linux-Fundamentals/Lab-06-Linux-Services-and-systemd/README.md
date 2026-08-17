# Linux Services and systemd

## Project Summary

This project documents a controlled Linux service and systemd investigation on an Ubuntu Server. The work focused on service inventory, runtime and enablement states, unit-file inspection, service-to-process correlation, dependency analysis, journal review, controlled service construction, persistence analysis, service hardening, and evidence integrity.

The scenario included investigation of the legitimate SSH service, creation of a controlled `lab-heartbeat.service`, and an independent challenge using `cloud-metrics-cache.service` with a local Python HTTP listener on TCP port `9195`. Only reviewed training content was executed; unfamiliar or discovered service commands were inspected without manual execution.

## Environment

| System | Role |
|---|---|
| Ubuntu Server | Service-investigation target |

**Platform:** VMware Workstation Pro using a host-only network

## Investigation Scenario

The investigation was designed to answer five practical questions:

1. Which services were running, enabled, failed, or inactive?
2. Which unit files, startup commands, users, and processes were associated with each service?
3. How did runtime state differ from boot-time enablement and persistence?
4. Which logs, dependencies, filesystem locations, and hardening properties helped validate service behavior?
5. Could an unexpected service and listener be investigated and removed without treating a service name alone as proof of malicious activity?

## Investigation Workflow

1. Confirmed that systemd was PID 1 and captured running, enabled, and failed service inventories.
2. Investigated the SSH service with `systemctl status`, `show`, `cat`, dependency queries, process correlation, and `journalctl`.
3. Created, reviewed, syntax-checked, and permissioned a controlled heartbeat script.
4. Created and validated `lab-heartbeat.service`, installed it under `/etc/systemd/system`, and reloaded systemd.
5. Started the service, correlated it with its main process, resolved `/proc` links, and verified heartbeat output.
6. Restarted the service and compared its original and replacement PIDs.
7. Enabled and disabled the service to examine the difference between runtime state and persistence.
8. Searched common systemd persistence locations and reviewed service hardening with `systemd-analyze security`.
9. Completed an independent `cloud-metrics-cache.service` challenge on TCP port `9195`, documented the assessment, removed the controlled unit, and verified evidence integrity.

## Key Findings

- Runtime state and enablement state are separate; a service can remain active after being disabled.
- `FragmentPath`, `ExecStart`, `MainPID`, `User`, `WorkingDirectory`, and process metadata provide stronger attribution than a service description alone.
- `systemctl cat` exposes the effective unit configuration and any drop-in overrides without executing discovered commands.
- `journalctl -u` isolates service-specific lifecycle and application records for timeline analysis.
- Enabling `lab-heartbeat.service` created a symbolic link under a target `.wants` directory, demonstrating systemd persistence mechanics.
- Restarting the controlled service replaced its main process and changed its PID.
- `systemd-analyze security` showed that service functionality and service hardening must be evaluated separately.
- The challenge listener on `127.0.0.1:9195` was attributable to the controlled `cloud-metrics-cache.service` Python process.
- A newly created, enabled, or listening service is an investigation lead, not automatic proof of malicious activity.

## Selected Commands

The concise command reference is available in [`commands.md`](commands.md). It contains the commands that best demonstrate service inventory, effective-unit inspection, service-to-process correlation, controlled unit validation, persistence analysis, journal review, hardening analysis, challenge attribution, and checksum verification.

## Skills Demonstrated

Linux service triage, systemd unit analysis, runtime-versus-enablement interpretation, effective-unit inspection, `MainPID` and process correlation, dependency analysis, journal investigation, safe controlled-service construction, unit validation, service lifecycle management, enablement-link analysis, persistence-location review, service hardening assessment, network-listener attribution, false-positive analysis, evidence documentation, and SHA-256 integrity verification.

## Security Relevance

Linux services are a common execution and persistence mechanism on cloud workloads. Reliable service investigations require correlation across unit-file locations, effective configuration, startup commands, users, processes, restart behavior, dependencies, logs, network listeners, enablement links, file metadata, security controls, change history, and business authorization before containment decisions are made.
