# Linux File System Investigation

## Project Summary

This project documents a controlled Linux file-system investigation on an Ubuntu Server. The work focused on hidden-file discovery, permission and metadata analysis, recent-file and executable-file identification, content searching, safe script review, SHA-256 hashing, evidence preservation, and false-positive validation.

## Environment

| System | Hostname | Username | IP Address | Role |
|---|---|---|---|---|
| Ubuntu Server | `ubuntu-server` | `testlab` | `192.168.50.129` | File-system investigation target |

**Platform:** VMware Workstation Pro using a host-only network

## Investigation Scenario

A controlled application directory named `suspicious-app` contained configuration, log, hidden, and script files. The scenario included a legitimate maintenance script, a hidden cache containing a training token, and a non-executable update script containing a `curl` command that referenced the controlled Kali address `192.168.50.1`.

No discovered or untrusted script was executed.

## Investigation Workflow

1. Reviewed important Linux directories and established a dedicated workspace.
2. Built the controlled `suspicious-app` scenario.
3. Located hidden, recently modified, and executable files.
4. Reviewed permissions, ownership, timestamps, and file types.
5. Searched recursively for remote-access references and potentially suspicious commands.
6. Inspected the update script as text without executing it.
7. Generated SHA-256 hashes for every scenario file.
8. Collected directory listings, metadata, search results, and findings.
9. Investigated recent files under `/tmp` and documented an empty-result condition when applicable.
10. Verified published artifacts with SHA-256 checksums.

## Key Findings

- `.app-cache` was hidden and contained a controlled temporary training token.
- `maintenance.sh` was executable with permission mode `750`.
- `update-check.sh` contained a `curl` command referencing `192.168.50.1`.
- `update-check.sh` was not executable, but it still contained instructions that could later be interpreted or executed by another mechanism.
- Broad keyword searches returned both expected and suspicious-looking files, demonstrating the need for analyst validation.
- File metadata, content, ownership, permissions, timestamps, and purpose were more useful together than any single indicator.
- SHA-256 hashes established integrity records for the controlled files and collected evidence.
- An empty `/tmp` search result remained a valid and documentable investigative outcome.
- No suspicious file was executed during the investigation.

## Selected Commands

The concise command reference is available in [`commands.md`](commands.md). It contains the commands that best demonstrate controlled scenario creation, metadata review, file discovery, content searching, safe inspection, hashing, and evidence verification.

## Skills Demonstrated

Linux file-system investigation, hidden-file discovery, permission analysis, metadata and timestamp analysis, file-type identification, recursive content searching, behavior-focused regular expressions, false-positive validation, safe script inspection, SHA-256 hashing, evidence preservation, checksum verification, and investigation documentation.

## Security Relevance

These techniques help cloud defenders investigate unexpected scripts, hidden files, modified configurations, staged payloads, suspicious download commands, altered startup content, and other host-level findings on Linux virtual machines, containers, web servers, jump hosts, and Kubernetes nodes. File names and keywords are indicators to investigate, not proof of malicious activity.
