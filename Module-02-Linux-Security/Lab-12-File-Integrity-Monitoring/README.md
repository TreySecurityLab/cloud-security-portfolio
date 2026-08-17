# AIDE File Integrity Monitoring and Change Detection

## Project Summary

This project documents a controlled Linux file-integrity monitoring investigation on an Ubuntu Server. The work focused on AIDE installation and validation, scoped integrity-policy construction, trusted baseline creation, file-addition, removal, and modification detection, AIDE exit-status interpretation, remediation, post-remediation verification, and SHA-256 evidence integrity.

The scenario used a controlled monitored directory containing trusted application, startup, and logging configuration files. After the AIDE baseline was initialized, `application.conf` was deliberately modified, `startup.conf` was removed, and an unauthorized `persistence.conf` file was introduced. AIDE detected all three integrity conditions against the original trusted database, after which the monitored files were restored and the unauthorized file removed without rebaselining over the changes.

## Environment

| System | Role |
| --- | --- |
| Ubuntu Server | Linux file-integrity monitoring and change-detection investigation target |

## Investigation Scenario

The investigation was designed to answer five practical questions:

1. Can AIDE establish a trusted integrity baseline for a controlled set of Linux configuration files?
2. Can the monitoring policy detect a modified trusted file, a removed trusted file, and a newly introduced file?
3. What does the AIDE exit status reveal when added, removed, and changed conditions occur simultaneously?
4. Can the monitored filesystem be remediated back to the original trusted state without accepting unauthorized changes into a new baseline?
5. Does a final AIDE check and SHA-256 evidence verification confirm both filesystem remediation and investigation-evidence integrity?

## Investigation Workflow

1. Created the controlled Lab 12 workspace with separate monitored, AIDE, evidence, and scenario directories.
2. Installed AIDE from the configured Ubuntu repositories and recorded the installed version and supported features.
3. Created trusted application, startup, and logging configuration files and documented their initial filesystem metadata.
4. Built a scoped AIDE configuration that monitored permissions, file type, ownership, size, and SHA-256 content hashes.
5. Validated the AIDE configuration before initializing the trusted integrity database.
6. Initialized the AIDE database, promoted the generated database to the trusted baseline, and confirmed that the initial integrity check reported no differences.
7. Deliberately modified `application.conf`, removed `startup.conf`, and introduced `persistence.conf` to simulate three distinct integrity events.
8. Ran AIDE against the original baseline and captured the added, removed, and changed findings together with the resulting exit status of `7`.
9. Restored the trusted file contents, recreated the removed trusted file, removed the unauthorized file, and retained the original AIDE database rather than rebaselining over the simulated changes.
10. Performed a final AIDE integrity check, documented the investigation assessment, and verified the retained evidence with the SHA-256 checksum manifest.

## Key Findings

- AIDE provided a repeatable integrity baseline against which the current filesystem state could be compared.
- The custom monitoring rule combined file permissions, file type, user ownership, group ownership, size, and SHA-256 hashing to detect both metadata and content changes relevant to the investigation.
- Modifying `application.conf` caused AIDE to identify a changed trusted file rather than treating it as a new filesystem object.
- Removing `startup.conf` demonstrated that file-integrity monitoring can identify the disappearance of a previously trusted configuration file.
- Introducing `persistence.conf` demonstrated detection of a new file that was not present when the trusted baseline was established.
- The simultaneous added, removed, and changed conditions produced AIDE exit status `7`, reflecting the combined detection state.
- Restoring the trusted files and removing the unauthorized file before rechecking preserved the original baseline as the source of truth instead of legitimizing the simulated changes.
- The post-remediation AIDE check confirmed that the monitored directory again matched the original trusted integrity database.
- The final evidence checksum verification confirmed that the retained investigation artifacts had not changed after collection.

## Selected Commands

The concise command reference is available in [`commands.md`](https://github.com/TreySecurityLab/cyber-portfolio/blob/main/Module-02-Linux-Security/Lab-12-File-Integrity-Monitoring/commands.md). It contains the commands that best demonstrate AIDE installation and validation, integrity-policy construction, baseline initialization, clean-state verification, controlled filesystem changes, change detection, exit-status capture, remediation validation, and evidence verification.

## Skills Demonstrated

Linux file-integrity monitoring, AIDE installation and configuration, trusted baseline creation, scoped monitoring-policy construction, file metadata analysis, SHA-256 content-integrity monitoring, added-file detection, removed-file detection, changed-file detection, AIDE exit-status interpretation, remediation against an original trusted baseline, post-remediation validation, security assessment documentation, evidence collection, and SHA-256 integrity verification.

## Security Relevance

Cloud access controls and Linux permissions reduce the likelihood of unauthorized filesystem modification, but they do not prove that protected files have remained unchanged. File-integrity monitoring provides a host-level detection layer by comparing current filesystem state with a known-good reference and identifying unexpected additions, removals, metadata changes, or content changes. Reliable integrity monitoring requires a deliberately established baseline, carefully scoped monitoring rules, investigation of detected differences before any baseline update, and verification that remediation returns the workload to an approved state. These practices help detect unauthorized configuration changes, malicious persistence, security-control removal, compromised deployments, and other forms of Linux workload tampering.
