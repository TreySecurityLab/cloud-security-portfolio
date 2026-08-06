# Linux Process Investigation Report

## Scope

A controlled process investigation was performed on the Ubuntu Server training system. The review focused on process identity, ancestry, ownership, state, priority, executable validation, local network listeners, systemd relationships, and evidence integrity.

## Controlled Activity

The primary scenario used:

- A `sleep` process with a displayed argument of `system-update-agent`
- A Python HTTP server bound to `127.0.0.1:8085`
- A lower-priority `sleep` process used for nice-value analysis
- A separate challenge using `cloud-sync-helper` and a listener on TCP port `9095`

All activity was created specifically for the lab. No discovered or untrusted content was executed.

## Findings

1. The displayed `system-update-agent` argument did not identify the actual executable. `/proc/<PID>/exe` resolved to the system `sleep` binary.
2. Process metadata and `/proc` data provided stronger attribution than a displayed process name alone.
3. The controlled process entered a stopped state after `SIGSTOP` and resumed after `SIGCONT`.
4. Socket inspection attributed the listener on TCP port `8085` to the controlled Python HTTP-server process.
5. Nice-value inspection demonstrated how scheduling preference can be lowered without treating priority as an indicator of maliciousness.
6. systemd properties connected the SSH service to its main process ID and unit definition.
7. The independent challenge produced the same core conclusion: suspicious naming is an indicator to investigate, not proof of compromise.

## Assessment

The observed activity was consistent with the controlled lab scenario. The investigation demonstrated a repeatable method for distinguishing process labels from executable identity and for correlating processes with ancestry, services, and listening sockets.

In a production investigation, the same findings would require additional validation against change records, workload purpose, package provenance, authentication history, endpoint telemetry, and authorized administrator activity before containment.

## Evidence Handling

Only sanitized text evidence and selected screenshots should be published. Raw process arguments, broad socket inventories, sensitive paths, credentials, tokens, cookies, production usernames, and unrelated processes must be removed before upload.
