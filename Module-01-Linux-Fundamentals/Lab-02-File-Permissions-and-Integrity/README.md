# File Permissions and Integrity

## Project Summary

This project examined Linux ownership and permissions, created a SHA-256 integrity baseline for a controlled configuration file, simulated an unauthorized modification, detected the change, restored the trusted content, and verified the restored state.

## Environment

| System | Role |
|---|---|
| Ubuntu Server | File-permissions and integrity target |

**Platform:** VMware Workstation Pro using a host-only network

## Investigation Scenario

A controlled configuration file named `application.conf` contained a training-only database password. The file was restricted to its owner, hashed to establish a trusted baseline, modified by appending `remote_access=true`, and restored after the checksum verification failed.

The investigation was designed to answer four practical questions:

1. Did the file permissions appropriately restrict access?
2. Could a SHA-256 baseline detect a controlled content change?
3. Could the trusted content be restored safely?
4. Would the restored file verify against the original checksum?

## Investigation Workflow

1. Created the controlled configuration file.
2. Reviewed its initial symbolic permissions and metadata.
3. Restricted access to owner read and write with mode `600`.
4. Created a SHA-256 checksum baseline.
5. Verified the initial trusted state.
6. Appended an unauthorized configuration line.
7. Detected the modification through checksum failure.
8. Removed the unauthorized line.
9. Reverified the file against the original checksum.
10. Documented the results and verified published artifacts with SHA-256 checksums.

## Key Findings

- Mode `600` limited the controlled file to owner read and write access.
- Permissions reduced unauthorized access but did not prove that the file remained unchanged.
- SHA-256 produced a reproducible fingerprint of the trusted file content.
- Appending `remote_access=true` caused the original checksum verification to fail.
- Removing the unauthorized line restored the original checksum result.
- A failed checksum proved that content changed but did not identify who changed it or why.
- File-integrity monitoring is strongest when combined with permissions, logging, auditing, and change-management records.

## Selected Commands

The concise command reference is available in [`commands.md`](commands.md). It contains the commands that best demonstrate permission review, mode changes, metadata inspection, checksum creation, controlled tampering, restoration, and integrity verification.

## Skills Demonstrated

Linux file-permission analysis, numeric permission modes, metadata review, controlled file creation, SHA-256 hashing, integrity-baseline creation, tampering detection, trusted-content restoration, verification, evidence documentation, and checksum validation.

## Security Relevance

Cloud-hosted Linux systems depend on trusted configuration files, startup scripts, service definitions, application settings, and secret-bearing files. Unauthorized changes can weaken controls, enable persistence, expose sensitive information, or disable logging. Permissions help control access, while integrity checks help detect content changes.
