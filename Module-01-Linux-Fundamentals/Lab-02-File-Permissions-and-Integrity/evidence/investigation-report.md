# File Permissions and Integrity Investigation Report

## Scope

A controlled configuration file was examined to demonstrate how Linux permissions and SHA-256 integrity verification address different security requirements. The workflow covered access restriction, metadata review, baseline creation, controlled modification, detection, restoration, and final verification.

## Controlled Activity

The file `application.conf` contained a training-only database password. Mode `600` was applied, a SHA-256 baseline was created, `remote_access=true` was appended as the controlled unauthorized change, and the line was removed to restore the trusted state.

## Findings

1. The file's symbolic permissions and ownership could be reviewed independently from its contents.
2. Mode `600` allowed owner read and write access while denying group and other access.
3. The initial checksum verification confirmed the trusted baseline.
4. The controlled modification caused checksum verification to fail.
5. Removing the unauthorized line restored the original checksum result.
6. Integrity verification detected content change but did not attribute the change to a user or process.
7. Permissions and hashing provided complementary controls rather than interchangeable controls.

## Assessment

The observed changes were consistent with the controlled lab scenario. In production, a failed integrity check would require correlation with audit logs, process activity, administrator actions, deployment history, file ownership, and approved change records before containment or restoration.

## Evidence Handling

Only training data and sanitized artifacts should be published. Real passwords, API keys, access tokens, private keys, production secrets, personal information, and unrelated file metadata must not be committed.
