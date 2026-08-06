# Find and Grep

## Project Summary

This project documents a controlled Linux search and threat-hunting exercise using `find` and `grep`. The investigation focused on locating files by metadata, combining logical search conditions, searching file contents with exact and regular-expression matching, reducing false positives, correlating file and content results, and preserving evidence integrity.

## Environment

| System | Hostname | Username | IP Address | Role |
|---|---|---|---|---|
| Ubuntu Server | `ubuntu-server` | `testlab` | `192.168.50.129` | Search and investigation target |

**Platform:** VMware Workstation Pro using a host-only network

## Investigation Scenario

A controlled directory named `search-data` contained configurations, logs, scripts, archived content, uploads, a hidden review note, and an empty file. The data included weak settings, failed-authentication events, and controlled references to `192.168.50.1` for search and false-positive analysis.

No discovered or untrusted script was executed.

## Investigation Workflow

1. Built the controlled `search-data` scenario.
2. Located files and directories by type, name, case-insensitive name, path, size, age, owner, and permissions.
3. Combined `find` conditions with AND, OR, and NOT logic.
4. Formatted file metadata with `find -printf`.
5. Used `find -exec` to inspect matched files.
6. Performed recursive, fixed-string, word-aware, contextual, and extended-expression searches with `grep`.
7. Compared broad substring searches with boundary-aware searches to reduce false positives.
8. Combined `find` and `grep` to investigate weak configurations and network-tool references.
9. Collected sanitized findings and verified published artifacts with SHA-256 checksums.

## Key Findings

- `application-backup.conf` contained `debug=true`, `remote_access=true`, and a lab-only training password.
- `diagnostic.sh` contained a controlled `curl` command referencing `192.168.50.1`.
- `connectivity-test.sh` contained an `nc` command referencing TCP port `8080`.
- `maintenance.sh` was executable and contained a legitimate `rsync` command.
- `old-debug.log` was older than seven days.
- `empty-upload.tmp` was empty, and `.review-note` was hidden.
- A substring search for `nc` could match unrelated text such as `maintenance`.
- Word-aware and boundary-aware patterns improved signal quality but still required analyst validation.
- Weak settings and network-tool references were indicators to investigate, not automatic proof of compromise.

## Selected Commands

The concise command reference is available in [`commands.md`](commands.md). It contains the commands that best demonstrate metadata filtering, logical expressions, formatted output, content searching, false-positive reduction, combined `find` and `grep` workflows, and checksum verification.

## Skills Demonstrated

Linux file discovery, metadata filtering, permission and age-based searching, logical `find` expressions, `find -printf`, `find -exec`, recursive content searching, fixed-string matching, extended regular expressions, word-boundary matching, search-scope control, false-positive analysis, evidence collection, and SHA-256 integrity verification.

## Security Relevance

Cloud alerts frequently identify a host, path, account, address, command, or time window. `find` narrows file-system scope using metadata, while `grep` searches relevant contents. Together they support investigations of weak configurations, embedded credentials, suspicious scripts, unauthorized tooling, authentication events, and other host-level indicators across Linux virtual machines, containers, web servers, jump hosts, build agents, and incident-response snapshots.
