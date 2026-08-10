# Sudo, Privilege Delegation, and Least Privilege

## Overview

This lab examined Linux sudo authorization as a privileged-access control boundary. A controlled operator account was intentionally granted excessive root-level command authorization, the effective permissions and security impact were documented, and the configuration was remediated to a narrowly scoped command rule that permits only SSH service-status inspection.

## Skills Demonstrated

- Inspected effective sudo authorization with `sudo -l`.
- Created and validated dedicated `/etc/sudoers.d/` policy fragments with `visudo`.
- Identified an excessive `NOPASSWD: ALL` authorization condition.
- Demonstrated the root-level impact of unrestricted sudo delegation using a controlled account.
- Replaced broad authorization with least-privilege command delegation.
- Performed positive and negative authorization testing after remediation.
- Collected investigation evidence and verified its integrity with SHA-256 checksums.

## Security Findings

The controlled `lab10-operator` identity initially had unrestricted passwordless authorization to execute commands as root. This exceeded the documented requirement to view only the SSH service status and represented a direct privilege-escalation path.

The sudoers policy was remediated so the account could execute only `/usr/bin/systemctl status ssh` as root. Verification confirmed the approved command remained available while an unrelated root-level `/usr/bin/id` request was denied.

## Evidence

The `evidence/` directory contains the authorization state before and after remediation, positive and negative authorization-test results, the final privilege assessment, and the SHA-256 integrity manifest produced on the Ubuntu Server.

## Screenshots

The `screenshots/` directory contains the required terminal proof for the excessive authorization, root-impact demonstration, sudoers validation, least-privilege result, authorization tests, and evidence-integrity verification.
