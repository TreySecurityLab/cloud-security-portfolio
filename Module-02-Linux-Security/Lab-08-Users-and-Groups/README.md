# Users and Groups

## Project Summary

This project documents a controlled Linux identity and access-control investigation on an Ubuntu Server. The work focused on UID and GID interpretation, local account discovery, service-account shell analysis, supplemental group authorization, least-privilege review, targeted remediation, evidence documentation, and SHA-256 integrity verification.

The scenario created a controlled interactive backup operator, a noninteractive service identity, and a dedicated `cloud-audit` authorization group. A deliberate excessive group membership was introduced for the service identity, detected through identity and group inspection, and removed without deleting unrelated accounts or groups.

## Environment

| System | Hostname | Username | Role |
|---|---|---|---|
| Ubuntu Server | `ubuntu-server` | `testlab` | Linux identity and access-control target |

## Investigation Scenario

The investigation was designed to answer five practical questions:

1. How does Linux represent local users, UIDs, primary GIDs, supplemental groups, home directories, and login shells?
2. Which accounts appear human-oriented and which use noninteractive shells?
3. Does the interactive backup operator have the required `cloud-audit` authorization?
4. Does the service identity have an appropriately restricted shell and only the access its role requires?
5. Can an excessive supplemental-group assignment be detected, remediated, and verified without exposing sensitive password data?

## Investigation Workflow

1. Documented the intended identity and access requirements before changing the system.
2. Reviewed the current user's UID, primary GID, supplemental groups, and passwd database record.
3. Inventoried normal user-range accounts and identities using noninteractive shells.
4. Created the controlled `cloud-audit` group and `lab08-backup` interactive account.
5. Verified the backup operator's password-account status without exposing `/etc/shadow` contents.
6. Added the backup operator to `cloud-audit` using append-only supplemental-group modification.
7. Created `lab08-service` without a home directory and assigned `/usr/sbin/nologin`.
8. Deliberately added the service identity to `cloud-audit` to model excessive authorization.
9. Preserved the excessive membership as evidence and compared it with the documented requirements.
10. Removed only the unauthorized membership, verified the corrected state, documented the assessment, and created the final evidence checksum manifest.

## Key Findings

- Linux authorization depends on numeric UIDs and GIDs even though administrators normally work with readable account and group names.
- `getent` provided identity information through the configured Name Service Switch rather than assuming all identities must be read directly from local text files.
- The controlled `lab08-backup` account used `/bin/bash` and retained the required `cloud-audit` supplemental membership.
- The controlled `lab08-service` identity used `/usr/sbin/nologin`, matching its intended noninteractive role.
- The deliberate `cloud-audit` membership assigned to `lab08-service` exceeded the documented requirement and demonstrated privilege accumulation.
- `gpasswd -d` removed only the unauthorized supplemental membership while preserving both controlled identities and the authorization group.
- Final `id` and `getent group` results confirmed the intended least-privilege state.
- Password status was reviewed with `passwd -S`; password hashes were never exported or published.

## Selected Commands

The concise command reference is available in [`commands.md`](commands.md). It contains the commands that best demonstrate identity discovery, account and group creation, shell validation, supplemental-group authorization, excessive-access detection, targeted remediation, and evidence verification.

## Skills Demonstrated

Linux identity investigation, UID and GID interpretation, Name Service Switch-aware lookups, passwd-record analysis, service-account shell review, local user and group administration, supplemental-group management, least-privilege validation, excessive-access detection, targeted group remediation, password-status review, evidence documentation, sanitization awareness, and SHA-256 evidence-integrity verification.

## Security Relevance

Cloud IAM controls who can reach a workload, while Linux identities and groups determine what that access can do after reaching the operating system. Effective Linux access reviews therefore require correlation across account purpose, UID and GID mappings, supplemental groups, login shells, password state, and documented authorization requirements. Excessive group membership is a common privilege-escalation and persistence risk because it can silently expand an identity's effective permissions.
