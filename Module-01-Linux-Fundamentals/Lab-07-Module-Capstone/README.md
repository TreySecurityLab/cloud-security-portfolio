# Module Capstone — Linux Security Investigation

## Project Summary

This project documents a controlled Linux security investigation that combines the core skills from Module 01. The work focused on file permissions, SHA-256 integrity validation, filesystem and content search, process triage, `/proc` executable attribution, systemd service correlation, evidence documentation, and final checksum verification.

The scenario included a deliberately modified application configuration and a controlled process whose displayed argument was `cloud-update-agent`. The investigation demonstrated why file-integrity failures and suspicious process names are indicators that require validation rather than proof of compromise. Only controlled training content was created and examined; no discovered or untrusted content was executed.

## Environment

| System | Hostname | Username | IP Address | Role |
|---|---|---|---|---|
| Ubuntu Server | `ubuntu-server` | `testlab` | `192.168.50.129` | Linux security investigation target |

**Platform:** VMware Workstation Pro using a host-only network

## Investigation Scenario

The investigation was designed to answer five practical questions:

1. Did the application configuration still match its trusted SHA-256 baseline?
2. Which filesystem metadata and configuration settings helped explain the detected change?
3. Did the `cloud-update-agent` process label identify the executable that was actually running?
4. Which systemd properties connected the SSH service to its runtime state, primary PID, and unit file?
5. Could the investigation be documented and integrity-verified without treating individual indicators as proof of compromise?

## Investigation Workflow

1. Created a controlled application configuration and restricted it to mode `600`.
2. Established and successfully verified a trusted SHA-256 baseline.
3. Added a controlled `debug=true` change and preserved the resulting checksum failure.
4. Used `find` and `grep` to review configuration metadata and security-relevant content.
5. Created and located the controlled `cloud-update-agent` process.
6. Captured focused process metadata and resolved `/proc/<PID>/exe` to validate the actual executable.
7. Hashed the resolved executable for additional attribution evidence.
8. Reviewed the SSH service through structured systemd properties.
9. Restored the original application configuration and verified it against the trusted baseline.
10. Terminated only the controlled process, documented the assessment, and verified the final evidence checksum manifest.

## Key Findings

- SHA-256 validation detected the controlled configuration change immediately, but the failed checksum did not identify who made the change or why.
- File permissions and file-integrity checks answer different security questions and are stronger when used together.
- `find` metadata and `grep` content searches provided investigative context without converting keyword matches into automatic conclusions.
- The `cloud-update-agent` label was not the executable identity; `/proc/<PID>/exe` resolved the process to the system `sleep` binary.
- Focused `ps` output connected the controlled process to its PID, PPID, owner, state, timing, executable name, and full arguments.
- Structured systemd properties separated SSH runtime state, enablement state, main PID, and unit-file location.
- Restoring the configuration caused the original SHA-256 baseline to verify successfully again.
- The final assessment concluded that the observed activity matched the controlled training scenario and did not support an actual compromise.

## Selected Commands

The concise command reference is available in [`commands.md`](commands.md). It contains the commands that best demonstrate integrity validation, filesystem and content investigation, process attribution, systemd correlation, controlled remediation, and checksum verification.

## Skills Demonstrated

Linux file permissions, SHA-256 baseline creation and verification, configuration-drift detection, `find` metadata searches, `grep` content analysis, process discovery, focused `ps` analysis, `/proc` executable attribution, executable hashing, systemd service correlation, controlled process termination, evidence documentation, false-positive analysis, and final evidence-integrity verification.

## Security Relevance

Cloud-hosted Linux systems commonly generate alerts for configuration drift, unfamiliar processes, and service-state changes. Reliable investigation requires correlation across file integrity, permissions, metadata, content, process identity, executable paths, service properties, timing, and business context. No single suspicious indicator should be treated as conclusive without supporting evidence.
