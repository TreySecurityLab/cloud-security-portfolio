# Linux File System Investigation

## Project Overview

This lab documents a controlled Linux file-system investigation performed on an Ubuntu Server. The investigation focused on discovering hidden files, reviewing permissions and metadata, identifying recently modified and executable files, searching file contents for suspicious behavior, collecting SHA-256 hashes, and preserving investigation evidence.

## Lab Environment

| System | Hostname | Username | IP Address | Role |
|---|---|---|---|---|
| Ubuntu Server | `ubuntu-server` | `testlab` | `192.168.50.129` | Investigation target |

**Virtualization platform:** VMware Workstation Pro  
**VMware network:** Host-only

## Objective

- Navigate and inspect important Linux directories.
- Identify hidden, executable, and recently modified files.
- Review file ownership, permissions, timestamps, and file types.
- Search recursively for suspicious commands and configuration values.
- Inspect suspicious scripts without executing them.
- Generate SHA-256 hashes for file-integrity tracking.
- Preserve evidence and verify its integrity.

## Investigation Scenario

A controlled application directory named `suspicious-app` was created with configuration, log, and script files. The scenario included:

- A normal application configuration file.
- A normal application log.
- An executable maintenance script.
- A hidden cache file containing a training token.
- A non-executable update script containing a remote download command.

The suspicious script referenced `192.168.50.1`, the controlled Kali address used in the lab environment. The script was reviewed as text and was not executed.

## Hands-on Lab

1. Reviewed the Linux root file system and important directories.
2. Created a dedicated investigation workspace.
3. Built a controlled suspicious-application scenario.
4. Identified hidden files with `find`.
5. Located recently modified files with `-mmin` and `-mtime`.
6. Identified executable files.
7. Reviewed file metadata with `stat`.
8. Identified file content types with `file`.
9. Searched recursively for suspicious keywords with `grep`.
10. Reviewed the suspicious script without executing it.
11. Generated SHA-256 hashes for all scenario files.
12. Saved directory listings, metadata, search results, and findings.
13. Created and verified evidence checksums.
14. Investigated recent files under `/tmp`.
15. Confirmed that an empty search result can be a valid investigative finding.

## Commands Used

The complete command reference and command breakdown are documented in [`commands.md`](commands.md).

## Key Findings

1. `.app-cache` was identified as a hidden file containing a temporary training token.
2. `maintenance.sh` was executable with permission mode `750`.
3. `update-check.sh` contained a `curl` command that referenced `192.168.50.1`.
4. `update-check.sh` was not executable, but it still contained instructions that could retrieve a remote payload.
5. A broad keyword search returned both scripts, demonstrating that detection patterns require analyst validation.
6. SHA-256 hashes were collected for all files in the controlled scenario.
7. Evidence files were hashed and successfully verified.
8. The initial `/tmp` search returned no files modified within the previous hour. This was a valid investigative result.
9. No suspicious files were executed during the investigation.

## Security Interpretation

The presence of `curl`, `wget`, `nc`, `ncat`, Bash, or Python in a file does not automatically prove malicious activity. Analysts must review the full command, file purpose, ownership, permissions, timestamps, surrounding context, and expected system behavior.

A file does not need execute permission to contain malicious or unauthorized instructions. It may later be executed by another interpreter, copied elsewhere, sourced by another script, or used as a staging artifact.

Cryptographic hashes can prove that a file changed, but they do not identify who changed it or why. Hashing should be combined with authentication logs, audit data, process telemetry, and network evidence.

## Why This Matters in Cloud Security

These techniques apply directly to:

- Azure Linux virtual machines
- AWS EC2 instances
- Google Compute Engine instances
- Containers
- Kubernetes worker nodes
- Web servers
- Jump hosts
- Security appliances

Cloud security alerts often require operating-system validation to determine whether a file, process, configuration, or network action is expected.

## Skills Demonstrated

- Linux file-system investigation
- Hidden-file discovery
- Permission analysis
- File metadata analysis
- Modification-time analysis
- Recursive file searching
- Threat hunting with regular expressions
- False-positive validation
- Safe suspicious-script inspection
- SHA-256 hashing
- Evidence collection
- Evidence-integrity verification
- Investigation documentation

## Technologies Used

- Ubuntu Server
- VMware Workstation Pro
- Bash
- GNU coreutils
- GNU findutils
- GNU grep
- SHA-256

## Evidence Collected

Evidence guidance is available in [`evidence/README.md`](evidence/README.md).

## Screenshots

The complete screenshot checklist is available in [`screenshots/README.md`](screenshots/README.md).

## Lessons Learned

- Absolute paths are independent of the current working directory.
- Relative output paths depend on the current working directory.
- `find -exec` requires a space between `{}` and `\;`.
- A search that returns no output may still have completed successfully.
- Broad detection patterns can produce false positives.
- Hidden or non-executable files can still require investigation.
- Suspicious content should be reviewed without execution.
- Findings should separate confirmed facts, suspicious indicators, and assumptions.
