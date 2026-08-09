# File Ownership and Permissions

## Project Summary

This project documents a controlled Linux discretionary access-control investigation on an Ubuntu Server. The work focused on file ownership, owner/group/other permission classes, symbolic and octal modes, effective-access testing, directory traversal, least-privilege remediation, and SHA-256 evidence integrity.

The scenario used a controlled audit report that was deliberately configured as mode `644`, making the file readable through the `other` permission class. The resource was then aligned to owner `testlab`, group `cloud-audit`, and mode `640` so the authorized backup identity retained read access while the nonauthorized service identity was denied.

## Environment

| System | Hostname | Username | Role |
|---|---|---|---|
| Ubuntu Server | `ubuntu-server` | `testlab` | Linux ownership and permissions investigation target |

## Investigation Scenario

The investigation was designed to answer five practical questions:

1. How do symbolic and octal Linux permissions represent owner, group, and other access?
2. Does a mode such as `644` create effective read access for an identity with no documented requirement?
3. How do file ownership and group membership interact with the permission bits?
4. Can least privilege be restored without removing required access for the authorized `cloud-audit` group?
5. How do parent-directory traversal permissions affect effective access even when the target file itself appears readable?

## Investigation Workflow

1. Verified the controlled Lab 08 identities and documented the intended access model.
2. Created a controlled `cloud-audit-report.txt` resource and inspected its owner, group, symbolic mode, and octal mode.
3. Deliberately set the file to `644` and preserved the excessive state as evidence.
4. Tested effective read access as `lab08-service` rather than relying only on the mode string.
5. Used path-component analysis to identify that the original `/home/testlab` directory mode restricted cross-user traversal before the file permissions could be evaluated.
6. Performed the controlled access test with only the required temporary traversal change and preserved the original home-directory mode for restoration.
7. Changed the report ownership to `testlab:cloud-audit` and remediated the file to `640`.
8. Verified that `lab08-backup` retained required group-based read access while `lab08-service` was denied.
9. Practiced directory permission interpretation using a controlled `audit-archive` directory and symbolic `chmod` changes.
10. Restored the original home-directory mode, documented the final authorization state, and verified portfolio evidence with SHA-256 checksums.

## Key Findings

- File mode `644` grants owner read/write, group read, and other read permissions at the file level.
- File permissions are not sufficient to determine effective access; an identity must also be able to traverse every parent directory in the pathname.
- The original `/home/testlab` mode prevented `lab08-service` from reaching the controlled file even while the file itself was `644`, demonstrating the importance of path traversal during authorization analysis.
- Owner `testlab`, group `cloud-audit`, and mode `640` matched the documented requirement for owner read/write, authorized-group read, and no access for other identities.
- `lab08-backup` retained access through its `cloud-audit` membership after remediation.
- `lab08-service` was denied after remediation because it was neither the owner nor an authorized group member and the `other` class had no permissions.
- Directory execute permission represents traversal/search capability rather than file execution.
- Effective-access testing provided stronger verification than inspecting mode bits alone.

## Selected Commands

The concise command reference is available in [`commands.md`](commands.md). It contains the commands that best demonstrate ownership inspection, octal and symbolic permission analysis, path traversal, cross-user access validation, least-privilege remediation, and evidence verification.

## Skills Demonstrated

Linux discretionary access control, file ownership analysis, symbolic permission interpretation, octal-mode interpretation, `stat` metadata collection, `chown`, numeric and symbolic `chmod`, supplemental-group authorization, parent-directory traversal analysis, controlled cross-user access testing, least-privilege remediation, before-and-after evidence collection, and SHA-256 integrity verification.

## Security Relevance

Cloud IAM determines who can reach a Linux workload, but local filesystem ownership and permissions determine what those identities can access after reaching the operating system. Reliable authorization reviews must evaluate the complete path, ownership, group membership, permission bits, and effective access. A single permissive mode or traversable directory can expose sensitive configuration, logs, backups, credentials, or incident-response evidence to identities that have no business requirement for the data.
