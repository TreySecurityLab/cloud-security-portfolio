# File Permissions and Integrity

## Objective

Understand Linux ownership and permissions, create a cryptographic file-integrity baseline, simulate unauthorized modification, detect the change, and restore the trusted file.

## Environment

- Ubuntu target: `testlab@ubuntu-server`
- Ubuntu IP: `192.168.50.129`
- VMware network: `Host-only`

## Lab Summary

1. Created a dedicated Lab 02 workspace.
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

## Key Findings

- File permissions reduce unauthorized access.
- SHA-256 provides a strong file fingerprint.
- A failed checksum proves the file changed.
- A checksum does not identify who changed the file.
- File-integrity monitoring should be combined with logging and auditing.

## Cloud Security Relevance

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
