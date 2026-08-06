# Linux Processes

## Project Overview

This lab documents a controlled Linux process investigation performed on an Ubuntu Server. The investigation examined process IDs, parent-child relationships, ownership, states, process trees, foreground and background jobs, process discovery, `/proc` data, signals, safe termination, process priority, systemd services, listening network ports, and misleading process arguments.

The lab used only controlled training processes created during the exercise. No discovered or untrusted script, binary, command, or file was executed.

## Lab Environment

| System | Hostname | Username | IP Address | Role |
|---|---|---|---|---|
| Ubuntu Server | `ubuntu-server` | `testlab` | `192.168.50.129` | Process-investigation target |

**Virtualization platform:** VMware Workstation Pro  
**VMware network:** Host-only

## Objective

- Explain Linux process concepts and terminology.
- Identify PIDs, PPIDs, owners, states, priorities, commands, and arguments.
- Reconstruct process ancestry using process trees and parent records.
- Manage controlled foreground and background jobs.
- Discover processes with `ps`, `pgrep`, `jobs`, and `top`.
- Inspect live process data through `/proc`.
- Associate processes with listening network sockets.
- Correlate systemd services with their main processes.
- Use signals to suspend, resume, and terminate controlled processes safely.
- Analyze a process whose displayed argument differs from its executable.
- Collect, review, hash, and verify investigation evidence.

## Hands-on Lab

1. Created an organized process-investigation workspace.
2. Identified the current Bash PID and PPID.
3. Compared `ps -ef`, `ps aux`, and custom `ps -o` output.
4. Captured a system-wide process snapshot.
5. Summarized primary process states.
6. Reviewed user-owned and root-owned processes.
7. Captured and analyzed a process tree.
8. Practiced foreground and background job control.
9. Created controlled sleep, custom-named, HTTP-listener, and lower-priority processes.
10. Recorded process IDs in PID files.
11. Discovered processes using short-name and full-command-line searches.
12. Investigated ownership, state, start time, elapsed time, parentage, and arguments.
13. Inspected `/proc/<PID>/status`, `cmdline`, `exe`, `cwd`, and `fd`.
14. Suspended and resumed a controlled process using signals.
15. Correlated TCP port `8085` with its Python process.
16. Captured a noninteractive `top` snapshot.
17. Started and adjusted a controlled lower-priority process.
18. Correlated the SSH systemd service with its main process.
19. Used `kill -0` to verify process existence without sending a terminating signal.
20. Requested graceful termination using `SIGTERM`.
21. Hashed the executable associated with the custom-named process.
22. Captured listening TCP process evidence.
23. Documented main investigation findings.
24. Created and verified an initial checksum baseline.
25. Completed a challenge involving `cloud-sync-helper` and TCP port `9095`.
26. Wrote an assessment separating facts, indicators, false positives, and unknowns.
27. Recreated and verified the final evidence checksum list.
28. Cleaned up only the controlled training processes.

## Commands Used

The complete command reference and command breakdowns are documented in [`commands.md`](commands.md).

Primary tools and interfaces included:

- `ps`
- `pgrep`
- `jobs`
- `fg`
- `bg`
- `kill`
- `wait`
- `top`
- `nice`
- `renice`
- `systemctl`
- `ss`
- `curl`
- `readlink`
- `/proc`
- `sha256sum`

## Key Findings

1. A process has a PID, parent PID, owner, state, priority, executable name, and command line.
2. Process ownership defines the security context under which the process operates.
3. Parent-child relationships provide execution context and can reveal suspicious ancestry.
4. Shell job numbers are not the same as system process IDs.
5. `jobs` shows jobs managed by the current shell, while `ps` provides system process visibility.
6. The controlled `system-update-agent` process displayed a custom argument but used the real `sleep` executable.
7. `/proc/<PID>/exe` provided stronger executable evidence than the displayed argument.
8. `/proc/<PID>/cmdline` showed null-separated arguments and required safe formatting for review.
9. `/proc/<PID>/cwd` identified the process working directory.
10. `/proc/<PID>/fd` exposed the process's open file descriptors.
11. `SIGSTOP` changed the controlled process state to stopped, and `SIGCONT` resumed it.
12. `SIGTERM` successfully requested graceful termination of controlled processes.
13. The controlled Python process owned the listener on `127.0.0.1:8085`.
14. A larger nice value reduced a controlled process's scheduling preference.
15. The SSH service properties connected systemd management information to a main PID.
16. Process arguments, socket listings, and `/proc` data may contain sensitive information.
17. A process name or command-line label alone is not sufficient to classify activity as malicious.
18. Evidence checksums had to be recreated after challenge evidence was added.

## Why This Matters in Cloud Security

Linux processes represent active behavior on cloud workloads. Process analysis can reveal:

- Unexpected interpreters or shells
- Unauthorized network listeners
- Cryptominers
- Reverse shells
- Credential-access tools
- Persistence agents
- Downloaders
- Tunneling utilities
- Data-staging processes
- Processes disguised as legitimate services
- Unexpected privileged execution
- Workloads consuming abnormal CPU or memory

Reliable conclusions require correlation of ownership, ancestry, executable paths, arguments, working directories, open files, sockets, service relationships, hashes, timing, and business purpose.

## Skills Demonstrated

- Linux process triage
- PID and PPID analysis
- Process ownership analysis
- Process-state interpretation
- Process-tree reconstruction
- Foreground and background job control
- Process discovery
- Full-command-line searching
- `/proc` investigation
- Executable-path validation
- Open-file-descriptor review
- Linux signal handling
- Safe process termination
- Nice-value and priority analysis
- systemd service correlation
- Network-listener attribution
- Suspicious-process analysis
- False-positive analysis
- Evidence collection
- Evidence-integrity verification
- Investigation assessment writing

## Technologies Used

- Ubuntu Server
- VMware Workstation Pro
- Bash
- procps-ng
- systemd
- iproute2 `ss`
- Python 3 HTTP server
- GNU coreutils
- SHA-256

## Evidence Collected

Evidence guidance is available in [`evidence/README.md`](evidence/README.md).

Recommended evidence includes:

- Current shell process details
- System process snapshot
- Process-state summary
- Process tree
- Custom-named process summary and parent
- `/proc` status, command line, executable, and working directory
- Network-listener attribution
- HTTP process and file descriptors
- Resource and priority snapshots
- SSH service properties
- Executable hash
- Listening TCP process inventory
- Main investigation findings
- Challenge process and listener evidence
- Challenge assessment
- Final evidence checksums

## Screenshots

The complete screenshot checklist is available in [`screenshots/README.md`](screenshots/README.md).

## Lessons Learned

- A stored program becomes a process when Linux runs it.
- PIDs identify processes; PPIDs identify the processes that created them.
- Ownership determines process permissions and access.
- Process state is context, not proof of malicious activity.
- A process tree can reveal abnormal execution ancestry.
- Shell jobs and system processes are related but distinct concepts.
- Full command lines can be useful and misleading.
- Process arguments can be manipulated.
- `/proc/<PID>/exe` helps validate the actual executable.
- `/proc` data disappears when the process exits.
- `SIGTERM` should normally precede forced termination.
- Unknown production processes should not be stopped without validation and authorization.
- A listening port must be connected to a PID, owner, executable, service, and purpose.
- Nice values influence scheduling preference but do not directly indicate maliciousness.
- A systemd service may manage one main process and additional child processes.
- Evidence containing commands, sockets, paths, or usernames must be sanitized before publication.
- Checksums must be recreated whenever evidence files are added or modified.
