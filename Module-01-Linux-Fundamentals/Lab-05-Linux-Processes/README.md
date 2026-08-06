# Linux Processes

## Project Summary

This project documents a controlled Linux process investigation on an Ubuntu Server. The work focused on process discovery, parent-child relationships, ownership, states, scheduling priority, `/proc` validation, network-listener attribution, systemd correlation, signal handling, and evidence integrity.

The scenario included a process whose displayed argument was intentionally changed to `system-update-agent`, a local Python HTTP listener on TCP port `8085`, and an independent challenge using `cloud-sync-helper` with TCP port `9095`. Only controlled training processes were created and examined; no discovered or untrusted content was executed.

## Environment

| System | Hostname | Username | IP Address | Role |
|---|---|---|---|---|
| Ubuntu Server | `ubuntu-server` | `testlab` | `192.168.50.129` | Process-investigation target |

**Platform:** VMware Workstation Pro using a host-only network

## Investigation Scenario

The investigation was designed to answer five practical questions:

1. Which processes were associated with the controlled activity?
2. What were their owners, states, priorities, parents, and full arguments?
3. Did a displayed process label match the executable actually running?
4. Which process owned each local listening port?
5. Could the findings be documented and verified without treating a process name alone as proof of malicious activity?

## Investigation Workflow

1. Captured a process overview and parent-child process tree.
2. Located the controlled processes with `pgrep` and focused `ps` queries.
3. Inspected `/proc/<PID>/status`, `cmdline`, `exe`, `cwd`, and `fd`.
4. Suspended and resumed a controlled process with `SIGSTOP` and `SIGCONT`.
5. Correlated TCP port `8085` with its Python HTTP-server process.
6. Reviewed and adjusted a controlled process's nice value.
7. Correlated the SSH systemd service with its main PID.
8. Completed a separate process-and-listener attribution challenge on TCP port `9095`.
9. Documented the assessment and verified published artifacts with SHA-256 checksums.

## Key Findings

- The `system-update-agent` label appeared in the process arguments, while `/proc/<PID>/exe` resolved to the legitimate `sleep` executable.
- Process arguments can be useful investigative clues, but they can also be changed and must be validated against stronger evidence.
- Parent PID, owner, state, start time, elapsed time, executable path, working directory, and open descriptors provided necessary context.
- `SIGSTOP` moved the controlled process into a stopped state beginning with `T`; `SIGCONT` returned it to a runnable or sleeping state.
- The Python HTTP-server process owned the listener on `127.0.0.1:8085`.
- A larger nice value reduced the controlled process's scheduling preference.
- systemd properties connected the SSH service definition to its active main PID.
- The challenge reinforced that a suspicious-looking name is not sufficient to classify a process as malicious.

## Selected Commands

The concise command reference is available in [`commands.md`](commands.md). It contains the commands that best demonstrate process triage, executable validation, signal handling, socket attribution, service correlation, priority analysis, and checksum verification.

## Skills Demonstrated

Linux process triage, PID and PPID analysis, process-state interpretation, `/proc` investigation, executable-path validation, process-tree reconstruction, signal handling, safe process control, priority analysis, systemd correlation, network-listener attribution, false-positive analysis, evidence documentation, and SHA-256 integrity verification.

## Security Relevance

Linux process analysis helps investigate unexpected interpreters, reverse shells, cryptominers, unauthorized listeners, persistence agents, tunneling utilities, credential-access tools, and processes disguised as legitimate services. Reliable conclusions require correlation across process identity, ancestry, ownership, executable paths, arguments, sockets, service relationships, timing, and business purpose.
