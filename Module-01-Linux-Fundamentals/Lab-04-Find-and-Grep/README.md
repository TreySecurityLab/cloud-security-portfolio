# Find and Grep

## Project Overview

This lab documents a controlled Linux search and threat-hunting exercise using `find` and `grep`. The investigation focused on locating files by metadata, filtering file-system results with logical expressions, searching file contents with exact and regular-expression matching, reducing false positives, and preserving evidence.

## Lab Environment

| System | Hostname | Username | IP Address | Role |
|---|---|---|---|---|
| Ubuntu Server | `ubuntu-server` | `testlab` | `192.168.50.129` | Investigation target |

**Virtualization platform:** VMware Workstation Pro  
**VMware network:** Host-only

## Objective

- Locate files by type, name, path, size, owner, permissions, and timestamps.
- Combine `find` expressions using AND, OR, and NOT logic.
- Search file contents recursively with `grep`.
- Compare substring, word-aware, fixed-string, and regular-expression searches.
- Include or exclude selected files and directories.
- Combine `find` and `grep` into a repeatable investigation workflow.
- Save and verify investigation evidence.

## Investigation Scenario

A controlled directory named `search-data` was created with configuration files, logs, scripts, archived content, uploads, a hidden file, and an empty file. The data included intentionally weak settings and controlled references to `192.168.50.1` for investigation practice.

No discovered script was executed during the investigation.

## Hands-on Lab

1. Created the Lab 04 workspace and evidence directory.
2. Built the controlled `search-data` scenario.
3. Located regular files, directories, and symbolic links.
4. Searched filenames using case-sensitive and case-insensitive matching.
5. Filtered results by path, size, age, owner, and permissions.
6. Combined conditions with AND, OR, and NOT logic.
7. Formatted output using `find -printf`.
8. Ran commands against matched files with `find -exec`.
9. Performed basic, recursive, fixed-string, and extended-expression searches with `grep`.
10. Compared substring matching with word-aware matching.
11. Displayed context around matches.
12. Included and excluded selected file types and directories.
13. Combined `find` and `grep` to investigate risky settings and network commands.
14. Saved sanitized evidence.
15. Documented findings and verified evidence checksums.

## Commands Used

The complete command reference is available in [`commands.md`](commands.md).

## Key Findings

1. `application-backup.conf` contained `debug=true`.
2. `application-backup.conf` contained `remote_access=true`.
3. `application-backup.conf` contained a lab-only training password.
4. `diagnostic.sh` contained a `curl` command referencing `192.168.50.1`.
5. `connectivity-test.sh` contained an `nc` command referencing port `8080`.
6. `maintenance.sh` was executable and contained a legitimate `rsync` command.
7. `old-debug.log` was older than seven days.
8. `empty-upload.tmp` was empty.
9. `.review-note` was hidden.
10. A substring search for `nc` could match unrelated words such as `maintenance`.

## Security Interpretation

A technically correct search match is not automatically a meaningful security finding. Substring searches may produce false positives, while word-aware and boundary-aware patterns can improve signal quality.

Configuration values such as `debug=true`, `remote_access=true`, or password-like strings require validation in context. Commands such as `curl`, `wget`, `nc`, and `ncat` may represent legitimate administration or suspicious behavior depending on ownership, timing, destination, process history, and operational purpose.

## Why This Matters in Cloud Security

These techniques apply directly to:

- Azure Linux virtual machines
- AWS EC2 instances
- Google Compute Engine instances
- Containers
- Kubernetes nodes
- Web servers
- Jump hosts
- Build agents
- Incident-response snapshots

Cloud alerts often identify a host, account, path, process, address, or time window. `find` narrows the file-system scope, while `grep` searches the relevant contents.

## Skills Demonstrated

- Linux file discovery
- Metadata filtering
- Permission-based searching
- Modification-time analysis
- Logical `find` expressions
- Custom `find -printf` formatting
- `find -exec` usage
- Recursive content searching
- Fixed-string searching
- Extended regular expressions
- Word-boundary matching
- Search-scope control
- False-positive analysis
- Evidence collection
- Evidence-integrity verification

## Technologies Used

- Ubuntu Server
- VMware Workstation Pro
- Bash
- GNU findutils
- GNU grep
- GNU coreutils
- SHA-256

## Evidence Collected

Evidence guidance is available in [`evidence/README.md`](evidence/README.md).

## Screenshots

The screenshot checklist is available in [`screenshots/README.md`](screenshots/README.md).

## Lessons Learned

- `find` searches file-system metadata; `grep` searches file contents.
- `-name` is case-sensitive; `-iname` is not.
- Wildcards passed to `find` should normally be quoted.
- `-mmin -60` means less than 60 minutes ago.
- `-mmin +60` means more than 60 minutes ago.
- Adjacent `find` expressions use AND logic.
- `-o` provides OR logic.
- `!` and `-not` exclude matches.
- Parentheses must be escaped in shell commands.
- `grep` normally matches substrings.
- `grep -w` reduces partial-word false positives.
- `grep -F` performs literal matching.
- `grep -E` enables extended regular expressions.
- Empty results are valid investigation outcomes.
