# Module Capstone — Linux Security Investigation Report

## Scope

A controlled Linux security investigation was performed on the Ubuntu Server training system. The review combined file permissions, SHA-256 integrity checks, filesystem and content searches, process triage, `/proc` executable attribution, systemd correlation, controlled remediation, and evidence-integrity verification.

## Controlled Activity

The primary scenario used:

- An `application.conf` file with a trusted SHA-256 baseline
- A deliberate `debug=true` configuration change used to trigger integrity failure
- A `sleep` process whose displayed argument was intentionally set to `cloud-update-agent`
- The existing SSH service as a legitimate systemd service for structured correlation

All activity was created specifically for the lab. No discovered or untrusted content was executed.

## Findings

1. The application configuration initially matched its trusted SHA-256 baseline.
2. Appending `debug=true` caused checksum verification to fail, proving the file changed without establishing attribution or malicious intent.
3. File metadata and configuration-content searches provided additional context for the detected change.
4. The `cloud-update-agent` process label did not identify the actual executable; `/proc/<PID>/exe` resolved to the system `sleep` binary.
5. Process metadata supplied the PID, PPID, owner, state, start time, elapsed time, short executable name, and full arguments needed for stronger attribution.
6. The executable hash provided an additional integrity reference for the resolved process image.
7. Structured systemd properties connected `ssh.service` to its load state, runtime state, enablement state, main PID, and unit-file location.
8. Removing the controlled configuration change restored successful verification against the original SHA-256 baseline.
9. The final evidence checksum manifest provided an integrity baseline for the sanitized portfolio evidence.

## Assessment

The observed activity was consistent with the controlled Lab 07 scenario. The investigation demonstrated a repeatable method for validating configuration drift and suspicious process labels through stronger filesystem, process, executable, and service evidence before making a security classification.

In a production investigation, the same indicators would require additional validation against change-management records, authentication history, package provenance, cloud-management telemetry, endpoint detections, administrator activity, workload purpose, and business authorization before containment.

## Evidence Handling

Only sanitized text evidence and selected required screenshots should be published. Credentials, tokens, secrets, public addresses, sensitive usernames, proprietary service information, internal production paths, unrelated process data, and unredacted screenshots must be removed before upload.
