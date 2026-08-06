# Linux Processes Evidence

Store only sanitized process-investigation artifacts in this directory.

## Recommended Evidence Files

- `current-shell-process.txt` — Current Bash PID, PPID, owner, state, priority, command, and arguments.
- `process-snapshot.txt` — System-wide point-in-time process listing.
- `process-state-summary.txt` — Counts of primary process states.
- `process-tree.txt` — Parent-child process hierarchy.
- `suspicious-process-summary.txt` — Focused metadata for the controlled custom-named process.
- `suspicious-process-parent.txt` — Parent process metadata.
- `suspicious-process-status.txt` — `/proc/<PID>/status` evidence.
- `suspicious-process-cmdline.txt` — Readable null-separated command-line evidence.
- `suspicious-process-links.txt` — Resolved executable and working directory.
- `network-process-listener.txt` — TCP port `8085` and owning process.
- `http-server-process.txt` — HTTP-process metadata.
- `http-server-file-descriptors.txt` — Open descriptors for the HTTP process.
- `top-snapshot.txt` — Noninteractive resource and task snapshot.
- `process-priority.txt` — Nice value and kernel-priority evidence.
- `ssh-service-process.txt` — SSH systemd properties and main PID.
- `suspicious-process-executable-hash.txt` — SHA-256 hash of the process executable.
- `listening-tcp-processes.txt` — Listening TCP sockets and process associations.
- `process-investigation-findings.txt` — Main written investigation assessment.
- `challenge-process-snapshot.txt` — System-wide snapshot during the challenge.
- `challenge-target-processes.txt` — Focused challenge process records.
- `challenge-parent-process.txt` — Challenge parent-process record.
- `challenge-suspect-status.txt` — Challenge `/proc` status.
- `challenge-suspect-cmdline.txt` — Challenge command-line evidence.
- `challenge-suspect-links.txt` — Challenge executable and working directory.
- `challenge-network-listener.txt` — TCP port `9095` attribution.
- `challenge-http-process.txt` — Challenge listener-process record.
- `challenge-assessment.txt` — Facts, suspicious indicators, false positives, unknowns, and final assessment.
- `evidence-checksums.sha256` — Final SHA-256 checksum list.

## Evidence Review

Before committing evidence:

1. Open every text file.
2. Review complete command lines.
3. Review usernames and home-directory paths.
4. Review internal addresses and listening ports.
5. Remove credentials, tokens, cookies, and session values.
6. Remove unrelated processes.
7. Confirm that assessment language distinguishes facts from inference.
8. Confirm that the final checksum verification succeeded.

## How the Evidence Supports the Investigation

The collected artifacts establish:

- Which processes were running
- Who owned them
- Their states and priorities
- Their parent-child relationships
- Their full arguments
- Their actual executable paths
- Their working directories
- Their open descriptors
- Their service relationships
- Their listening network sockets
- Their executable hashes
- Whether the final evidence changed after collection

## GitHub Safety

Evidence is safe to upload only after manual sanitization.

Do not upload:

- Process arguments containing passwords or tokens
- Environment variables containing secrets
- Public home IP addresses
- Sensitive internal addressing
- Proprietary process or service names
- Production usernames
- Sensitive paths
- Unsanitized socket listings
- Untrusted executables, scripts, or binaries
