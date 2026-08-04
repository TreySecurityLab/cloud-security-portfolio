# File Permissions and Integrity

## Project Overview

This lab examined Linux ownership and permissions, created a cryptographic file-integrity baseline, simulated unauthorized modification, detected the change, and restored the trusted file.

## Lab Environment

- Ubuntu target: `testlab@ubuntu-server`
- Ubuntu IP: `192.168.50.129`
- VMware network: `Host-only`

## Objective

- Review symbolic and numeric Linux permissions.
- Restrict access to a sensitive test file.
- Create a SHA-256 integrity baseline.
- Simulate an unauthorized modification.
- Detect and restore the changed file.
- Verify the restored file against the original checksum.

## Hands-on Lab

1. Created a dedicated workspace.
2. Created a sensitive test configuration file.
3. Reviewed symbolic and numeric permissions.
4. Restricted the file to owner read/write access using mode `600`.
5. Reviewed file metadata.
6. Created a SHA-256 checksum baseline.
7. Verified the trusted state.
8. Appended an unauthorized configuration line.
9. Detected the change through checksum failure.
10. Removed the unauthorized line.
11. Verified the original checksum again.

## Commands Used

The complete command reference is available in [`commands.md`](commands.md).

## Key Findings

- File permissions reduce unauthorized access.
- SHA-256 provides a strong file fingerprint.
- A failed checksum proves the file changed.
- A checksum does not identify who changed the file.
- File-integrity monitoring should be combined with logging and auditing.

## Why This Matters in Cloud Security

Cloud-hosted Linux systems rely on configuration files, startup scripts, application secrets, and service definitions. Unauthorized changes can create persistence, weaken security, expose secrets, or disable logging.

## Skills Demonstrated

- File permissions
- Numeric permission modes
- Ownership and metadata review
- SHA-256 hashing
- Integrity verification
- Controlled tampering simulation
- Evidence collection

## Technologies Used

- Ubuntu Server
- VMware Workstation Pro
- Bash
- GNU coreutils
- SHA-256
- sed

## Evidence Collected

Evidence guidance is available in [`evidence/README.md`](evidence/README.md).

## Screenshots

The screenshot checklist is available in [`screenshots/README.md`](screenshots/README.md).

## Lessons Learned

- Permissions and integrity checks solve different security problems.
- A checksum can detect modification but cannot identify the responsible user.
- Restoring trusted content should be followed by another integrity check.
