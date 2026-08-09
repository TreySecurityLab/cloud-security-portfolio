# Users and Groups Investigation Report

## Scope

A controlled Linux identity and access-control investigation was performed on the Ubuntu Server training system. The review focused on UID and GID interpretation, account discovery, login-shell analysis, supplemental-group authorization, least-privilege validation, targeted remediation, and final evidence-integrity verification.

## Controlled Activity

The primary scenario used:

- A dedicated `cloud-audit` authorization group
- An interactive `lab08-backup` account with `/bin/bash`
- A noninteractive `lab08-service` identity with `/usr/sbin/nologin`
- A deliberate excessive `cloud-audit` membership assigned to `lab08-service` for detection and remediation

All identities and authorization changes were created specifically for the lab. No unrelated users, groups, password hashes, or discovered content were modified or executed.

## Findings

1. The interactive backup operator required `cloud-audit` membership and retained that authorization after remediation.
2. The service identity used `/usr/sbin/nologin`, consistent with the documented noninteractive role.
3. Adding `lab08-service` to `cloud-audit` created a measurable least-privilege violation because the membership conflicted with the documented requirements.
4. `getent group cloud-audit` preserved the excessive membership before remediation and the corrected membership afterward.
5. `gpasswd -d lab08-service cloud-audit` removed only the unauthorized supplemental membership without deleting the identity or group.
6. Final `id` output confirmed that `lab08-backup` retained the required authorization while `lab08-service` no longer inherited `cloud-audit` access.
7. Password-account state was reviewed safely with `passwd -S`; no shadow hashes were exported.
8. The final evidence checksum manifest established an integrity baseline for the sanitized portfolio evidence.

## Assessment

The observed excessive group membership was consistent with the controlled Lab 08 scenario and was successfully remediated. The final state matched the intended role-based access model: the interactive backup operator retained the required audit authorization, while the noninteractive service identity did not.

In a production investigation, similar findings would require validation against approved access requests, identity-governance records, privileged-group baselines, configuration-management history, authentication telemetry, administrator activity, and the workload's documented service-account requirements before containment or account removal.

## Evidence Handling

Only sanitized, focused evidence and the required screenshots should be published. Broad local account inventories may reveal unrelated usernames or service identities and are intentionally excluded from the default portfolio import. Password hashes, credentials, tokens, secrets, public addresses, sensitive internal identifiers, and unrelated account data must not be committed.
