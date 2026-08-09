# File Ownership and Permissions Investigation Report

## Scope

A controlled Linux filesystem authorization investigation was performed on the Ubuntu Server training system. The review focused on ownership, group authorization, owner/group/other permission classes, symbolic and octal modes, parent-directory traversal, effective-access testing, least-privilege remediation, and evidence integrity.

## Controlled Activity

The primary scenario used:

- A controlled `cloud-audit-report.txt` resource
- Owner identity `testlab`
- Authorization group `cloud-audit`
- Authorized group member `lab08-backup`
- Unauthorized service identity `lab08-service`
- A deliberate `644` file mode used to demonstrate excessive file-level read permission
- A remediated `640` state aligned to the documented least-privilege requirement

All permission and ownership changes applied only to controlled lab resources or to a temporary, explicitly recorded home-directory traversal change used for the cross-user access test. The original home-directory mode was restored after testing.

## Findings

1. Mode `644` granted read permission to the `other` class at the target file.
2. The initial cross-user test demonstrated that effective filesystem access also depends on execute/traverse permission on every parent directory in the path.
3. The original `/home/testlab` mode blocked `lab08-service` before the target file mode was evaluated, which explained the initial permission-denied result despite the file being `644`.
4. After controlled path traversal was made possible, `lab08-service` could read the deliberately excessive `644` resource as expected.
5. Changing ownership to `testlab:cloud-audit` and mode to `640` matched the documented authorization requirement.
6. `lab08-backup` retained required read access through `cloud-audit` membership after remediation.
7. `lab08-service` was denied after remediation because it no longer qualified for an effective read permission class.
8. Directory permission exercises reinforced that execute permission on a directory controls traversal/search capability.

## Assessment

The controlled excessive permission state was successfully identified, demonstrated through effective-access testing, and remediated without removing required group access. The investigation also demonstrated why a target file's mode cannot be interpreted in isolation from its parent-directory permissions.

In a production investigation, similar findings would require correlation with data classification, application ownership, expected service identities, ACLs, mount options, centralized identity sources, change-management records, and workload purpose before changing access controls.

## Evidence Handling

Only focused, sanitized evidence and required screenshots should be published. The portfolio creates a new checksum manifest after selected Ubuntu evidence is staged and imported. The source Ubuntu `evidence-checksums.sha256` manifest is not copied because its hashes describe the original source evidence set rather than the transformed portfolio evidence files.
