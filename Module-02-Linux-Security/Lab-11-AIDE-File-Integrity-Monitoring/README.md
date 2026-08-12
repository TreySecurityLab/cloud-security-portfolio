# AIDE File Integrity Monitoring and Change Detection

## Overview

This lab implemented host-based file integrity monitoring with AIDE on an Ubuntu Server. A controlled set of configuration files was baselined, verified as trusted, deliberately changed to produce added, removed, and modified-file findings, and then remediated back to the original trusted state.

## Skills Demonstrated

- Installed and validated AIDE from Ubuntu package repositories.
- Built a scoped AIDE policy for a controlled monitored directory.
- Monitored permissions, file type, ownership, size, and SHA-256 content hashes.
- Initialized and promoted a trusted AIDE database.
- Verified a clean baseline before testing changes.
- Detected added, removed, and changed files and interpreted AIDE exit status `7`.
- Remediated the filesystem without rebaselining over unauthorized changes.
- Verified the restored state against the original trusted database.
- Preserved investigation evidence and verified its integrity with SHA-256 checksums.

## Security Findings

The controlled integrity test produced all three major AIDE change categories: one trusted file was modified, one trusted file was removed, and one unauthorized file was introduced. AIDE detected the combined condition and the captured status reflected added, removed, and changed findings.

The monitored files were restored to their original trusted contents and the unauthorized file was removed. A final AIDE comparison against the original database reported no differences, demonstrating remediation rather than acceptance of the altered state as a new baseline.

## Evidence

The `evidence/` directory contains the AIDE version record, baseline inventory, initialization output, clean baseline check, change-detection report, captured AIDE exit status, post-remediation check, final integrity assessment, and the SHA-256 evidence manifest produced on the Ubuntu Server. The manifest verifies the listed evidence artifacts and intentionally does not hash itself.

## Screenshots

The `screenshots/` directory contains the required terminal proof for AIDE installation and validation, policy validation, clean baseline verification, controlled file changes, integrity-change detection, post-remediation verification, and evidence-integrity checking.
