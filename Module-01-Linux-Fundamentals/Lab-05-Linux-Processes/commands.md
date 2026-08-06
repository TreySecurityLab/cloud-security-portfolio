# Linux Processes Commands

This file documents the principal commands used during Lab 05. Every command remains on one line and includes its investigation purpose, command elements, expected result, and security relevance.

## Create the Workspace

Run:

```bash
mkdir -p /home/testlab/cloud-security-labs/lab-05-linux-processes/{evidence,scenario/{logs,pids,webroot},challenge/{logs,pids,webroot}}
```

Command breakdown:

- `mkdir` creates directories.
- `-p` creates missing parent directories and does not fail when directories already exist.
- `/home/testlab/cloud-security-labs/lab-05-linux-processes/` is the lab root.
- `{...}` performs Bash brace expansion.
- `evidence` stores collected evidence.
- `scenario/{logs,pids,webroot}` creates the main scenario subdirectories.
- `challenge/{logs,pids,webroot}` creates the challenge subdirectories.

Expected result: No output is normal.

Security relevance: Organized evidence and scenario paths reduce accidental mixing of live data, training files, and collected artifacts.

Run:

```bash
cd /home/testlab/cloud-security-labs/lab-05-linux-processes
```

Command breakdown:

- `cd` changes the current directory.
- `/home/testlab/cloud-security-labs/lab-05-linux-processes` is the destination.

Expected result: No output is normal.

## Identify the Current Shell

Run:

```bash
printf "Shell PID: %s\nParent PID: %s\n" "$$" "$PPID"
```

Command breakdown:

- `printf` displays formatted text.
- `"Shell PID: %s\nParent PID: %s\n"` is the output format.
- `%s` is a string placeholder.
- `\n` creates a new line.
- `"$$"` expands to the current shell PID.
- `"$PPID"` expands to the shell's parent PID.

Expected result: Two numeric process identifiers.

Security relevance: PID and PPID relationships help reconstruct how a process started.

Run:

```bash
ps -p "$$" -o pid,ppid,user,stat,ni,comm,args
```

Command breakdown:

- `ps` displays process information.
- `-p` selects a process by PID.
- `"$$"` supplies the current shell PID.
- `-o` selects custom fields.
- `pid` displays the PID.
- `ppid` displays the parent PID.
- `user` displays the owner.
- `stat` displays the process state.
- `ni` displays the nice value.
- `comm` displays the short command name.
- `args` displays the full command line.

Expected result: One row describing the current Bash process.

Run:

```bash
ps -p "$$" -o pid,ppid,user,stat,ni,comm,args > evidence/current-shell-process.txt
```

Command breakdown:

- `ps -p "$$"` selects the current shell.
- `-o pid,ppid,user,stat,ni,comm,args` defines the evidence fields.
- `>` redirects and overwrites the destination.
- `evidence/current-shell-process.txt` is the evidence file.

Expected result: No terminal output is normal.

## Capture Process Views

Run:

```bash
ps -ef
```

Command breakdown:

- `ps` displays process information.
- `-e` selects every process.
- `-f` uses full-format output.

Expected result: A system-wide process listing emphasizing UID, PID, PPID, start information, and commands.

Run:

```bash
ps aux
```

Command breakdown:

- `ps` displays process information.
- `a` includes processes for other users and terminal-associated processes.
- `u` uses a user-oriented format with CPU and memory fields.
- `x` includes processes without controlling terminals.

Expected result: A system-wide process view emphasizing ownership and resource use.

Run:

```bash
ps -eo pid,ppid,user,stat,ni,comm,args --sort=pid > evidence/process-snapshot.txt
```

Command breakdown:

- `ps` displays process information.
- `-e` selects every process.
- `-o` defines custom fields.
- `pid,ppid,user,stat,ni,comm,args` records process identity and context.
- `--sort=pid` sorts numerically by PID.
- `>` redirects and overwrites the destination.
- `evidence/process-snapshot.txt` is the evidence file.

Expected result: No terminal output is normal.

Security relevance: A point-in-time snapshot preserves process context for later comparison.

## Analyze States and Ownership

Run:

```bash
ps -eo stat= | cut -c1 | sort | uniq -c | sort -nr
```

Command breakdown:

- `ps` displays process information.
- `-e` selects all processes.
- `-o stat=` displays state values without a header.
- `|` sends output to the next command.
- `cut` extracts characters.
- `-c1` selects the first state character.
- `sort` groups identical state values.
- `uniq` removes adjacent duplicates.
- `-c` counts each state.
- `sort -nr` sorts counts numerically in reverse order.

Expected result: Counts for states such as `S`, `I`, and `R`.

Run:

```bash
ps -eo stat= | cut -c1 | sort | uniq -c | sort -nr > evidence/process-state-summary.txt
```

Command breakdown:

- `ps -eo stat=` generates headerless state values.
- `| cut -c1` extracts primary states.
- `| sort` groups identical states.
- `| uniq -c` counts them.
- `| sort -nr` orders counts from largest to smallest.
- `>` redirects and overwrites the destination.
- `evidence/process-state-summary.txt` is the evidence file.

Expected result: No terminal output is normal.

Run:

```bash
ps -u "$USER" -o pid,ppid,stat,ni,comm,args --sort=pid
```

Command breakdown:

- `ps` displays process information.
- `-u` selects processes by effective user.
- `"$USER"` expands to the current username.
- `-o` defines the fields.
- `pid,ppid,stat,ni,comm,args` records identity, state, priority, and commands.
- `--sort=pid` sorts by PID.

Expected result: Processes owned by the current user.

Run:

```bash
ps -U root -o pid,ppid,stat,ni,comm,args --sort=pid | head -n 20
```

Command breakdown:

- `ps` displays process information.
- `-U` selects processes by real user.
- `root` selects root-owned processes.
- `-o pid,ppid,stat,ni,comm,args` defines the fields.
- `--sort=pid` sorts by PID.
- `|` sends output to `head`.
- `head` displays the beginning of the input.
- `-n 20` limits output to 20 lines.

Expected result: A sample of privileged processes.

## Capture the Process Tree

Run:

```bash
ps -eo pid,ppid,user,stat,comm,args --forest > evidence/process-tree.txt
```

Command breakdown:

- `ps` displays process information.
- `-e` selects every process.
- `-o pid,ppid,user,stat,comm,args` defines the fields.
- `--forest` draws parent-child branches.
- `>` redirects and overwrites the destination.
- `evidence/process-tree.txt` is the evidence file.

Expected result: No terminal output is normal.

Security relevance: Unexpected ancestry may reveal abnormal execution chains.

## Manage Foreground and Background Jobs

Run:

```bash
sleep 300 &
```

Command breakdown:

- `sleep` pauses for a duration.
- `300` requests 300 seconds.
- `&` runs the command as a background job.

Expected result: Bash displays a shell job number and PID.

Run:

```bash
jobs -l
```

Command breakdown:

- `jobs` displays jobs managed by the current shell.
- `-l` includes PIDs.

Expected result: The controlled sleep job appears as running or stopped.

Run:

```bash
fg %1
```

Command breakdown:

- `fg` moves a shell job into the foreground.
- `%1` selects job number 1.

Expected result: The terminal waits for the foreground job until it is suspended or finishes.

Run:

```bash
bg %1
```

Command breakdown:

- `bg` resumes a stopped job in the background.
- `%1` selects job number 1.

Expected result: Bash reports that the job resumed.

Run:

```bash
kill %1
```

Command breakdown:

- `kill` sends a signal.
- `%1` identifies shell job number 1.
- With no explicit signal, `SIGTERM` is sent.

Expected result: No output is normal.

## Create the Controlled Main Scenario

Run:

```bash
command -v python3
```

Command breakdown:

- `command` invokes shell command resolution.
- `-v` displays how a command is resolved.
- `python3` is the command being checked.

Expected result: A path such as `/usr/bin/python3`.

Run:

```bash
printf "Cloud Security Lab Exercises process investigation\n" > scenario/webroot/index.html
```

Command breakdown:

- `printf` writes formatted text.
- `"Cloud Security Lab Exercises process investigation\n"` is the content.
- `\n` creates a final newline.
- `>` redirects and overwrites the destination.
- `scenario/webroot/index.html` is the controlled page.

Expected result: No output is normal.

Run:

```bash
sleep 900 > scenario/logs/benign-sleep.log 2>&1 & echo $! > scenario/pids/benign-sleep.pid
```

Command breakdown:

- `sleep` pauses for a duration.
- `900` requests 900 seconds.
- `>` redirects standard output.
- `scenario/logs/benign-sleep.log` is the log file.
- `2>&1` redirects standard error to standard output.
- `&` starts the process in the background.
- `echo` displays a value.
- `$!` expands to the newest background PID.
- `>` redirects and overwrites the PID file.
- `scenario/pids/benign-sleep.pid` stores the PID.

Expected result: The background process starts and its PID is stored.

Run:

```bash
bash -c 'exec -a system-update-agent sleep 900' > scenario/logs/system-update-agent.log 2>&1 & echo $! > scenario/pids/system-update-agent.pid
```

Command breakdown:

- `bash` starts Bash.
- `-c` executes the following command string.
- `'exec -a system-update-agent sleep 900'` is the command string.
- `exec` replaces Bash with the target executable.
- `-a` supplies a custom first argument.
- `system-update-agent` is the custom argument.
- `sleep` is the actual executable.
- `900` requests 900 seconds.
- `>` redirects standard output.
- `scenario/logs/system-update-agent.log` is the log.
- `2>&1` redirects standard error to the same log.
- `&` runs the process in the background.
- `echo $!` outputs the new PID.
- `>` redirects and overwrites the PID file.
- `scenario/pids/system-update-agent.pid` stores the PID.

Expected result: A controlled process whose argument may differ from its executable.

Run:

```bash
python3 -m http.server 8085 --bind 127.0.0.1 --directory scenario/webroot > scenario/logs/http-server.log 2>&1 & echo $! > scenario/pids/http-server.pid
```

Command breakdown:

- `python3` starts Python 3.
- `-m` runs a module.
- `http.server` is the HTTP-server module.
- `8085` is the listening TCP port.
- `--bind` specifies a listening address.
- `127.0.0.1` restricts the listener to loopback.
- `--directory` selects served content.
- `scenario/webroot` is the served directory.
- `>` redirects standard output.
- `scenario/logs/http-server.log` is the log.
- `2>&1` redirects standard error to the same log.
- `&` starts the process in the background.
- `echo $!` outputs the new PID.
- `>` redirects and overwrites the PID file.
- `scenario/pids/http-server.pid` stores the PID.

Expected result: A controlled Python process listens locally on port 8085.

## Discover and Inspect Processes

Run:

```bash
pgrep -a sleep
```

Command breakdown:

- `pgrep` searches running processes by name.
- `-a` displays each PID and full command line.
- `sleep` is the process name.

Expected result: Controlled and possibly unrelated sleep processes.

Run:

```bash
pgrep -af 'system-update-agent|http.server'
```

Command breakdown:

- `pgrep` searches running processes.
- `-a` displays full command lines.
- `-f` matches complete command lines.
- `'system-update-agent|http.server'` matches either pattern.
- `|` inside the quoted expression means pattern OR, not a shell pipe.

Expected result: The controlled custom-named process and HTTP server.

Run:

```bash
SUSPECT_PID="$(cat scenario/pids/system-update-agent.pid)"; HTTP_PID="$(cat scenario/pids/http-server.pid)"; printf "SUSPECT_PID=%s\nHTTP_PID=%s\n" "$SUSPECT_PID" "$HTTP_PID"
```

Command breakdown:

- `SUSPECT_PID=` assigns a shell variable.
- `"$(cat scenario/pids/system-update-agent.pid)"` reads the suspect PID.
- `;` separates commands.
- `HTTP_PID=` assigns another variable.
- `"$(cat scenario/pids/http-server.pid)"` reads the HTTP PID.
- `printf` displays the values.
- `%s` receives each PID.
- `\n` creates new lines.

Expected result: Two named numeric PID values.

Run:

```bash
ps -p "$SUSPECT_PID" -o pid,ppid,user,stat,ni,lstart,etime,comm,args > evidence/suspicious-process-summary.txt
```

Command breakdown:

- `ps` displays process information.
- `-p "$SUSPECT_PID"` selects the process.
- `-o` defines fields.
- `pid,ppid,user,stat,ni,lstart,etime,comm,args` records identity, timing, state, priority, and command data.
- `>` redirects and overwrites the destination.
- `evidence/suspicious-process-summary.txt` is the evidence file.

Expected result: No terminal output is normal.

Run:

```bash
SUSPECT_PARENT="$(ps -o ppid= -p "$SUSPECT_PID" | tr -d ' ')"; ps -p "$SUSPECT_PARENT" -o pid,ppid,user,stat,comm,args > evidence/suspicious-process-parent.txt
```

Command breakdown:

- `SUSPECT_PARENT=` assigns the parent PID.
- `ps -o ppid= -p "$SUSPECT_PID"` returns the PPID without a header.
- `|` sends the value to `tr`.
- `tr -d ' '` removes padding spaces.
- `;` separates the assignment from the next command.
- `ps -p "$SUSPECT_PARENT"` selects the parent.
- `-o pid,ppid,user,stat,comm,args` defines the evidence fields.
- `>` redirects and overwrites the destination.
- `evidence/suspicious-process-parent.txt` is the evidence file.

Expected result: No terminal output is normal.

## Investigate Through /proc

Run:

```bash
cat "/proc/$SUSPECT_PID/status" > evidence/suspicious-process-status.txt
```

Command breakdown:

- `cat` reads file content.
- `"/proc/$SUSPECT_PID/status"` is the live process status source.
- `>` redirects and overwrites the destination.
- `evidence/suspicious-process-status.txt` is the evidence file.

Expected result: No terminal output is normal.

Run:

```bash
tr '\0' ' ' < "/proc/$SUSPECT_PID/cmdline" > evidence/suspicious-process-cmdline.txt; printf '\n' >> evidence/suspicious-process-cmdline.txt
```

Command breakdown:

- `tr` translates characters.
- `'\0'` represents null separators.
- `' '` is the replacement space.
- `<` redirects the process command line into `tr`.
- `"/proc/$SUSPECT_PID/cmdline"` is the source.
- `>` redirects and overwrites the evidence file.
- `;` separates commands.
- `printf '\n'` creates a final newline.
- `>>` appends to the evidence file.

Expected result: A readable process command line is stored.

Run:

```bash
printf "Executable: %s\nWorking directory: %s\n" "$(readlink -f "/proc/$SUSPECT_PID/exe")" "$(readlink -f "/proc/$SUSPECT_PID/cwd")" > evidence/suspicious-process-links.txt
```

Command breakdown:

- `printf` writes formatted text.
- `"Executable: %s\nWorking directory: %s\n"` is the format.
- `"$(readlink -f "/proc/$SUSPECT_PID/exe")"` resolves the executable path.
- `"$(readlink -f "/proc/$SUSPECT_PID/cwd")"` resolves the working directory.
- `>` redirects and overwrites the destination.
- `evidence/suspicious-process-links.txt` is the evidence file.

Expected result: No terminal output is normal.

Run:

```bash
ls -l "/proc/$SUSPECT_PID/fd"
```

Command breakdown:

- `ls` lists directory entries.
- `-l` displays detailed symbolic-link targets.
- `"/proc/$SUSPECT_PID/fd"` contains open file descriptors.

Expected result: Descriptors such as 0, 1, and 2.

## Send Controlled Signals

Run:

```bash
kill -STOP "$SUSPECT_PID"
```

Command breakdown:

- `kill` sends a signal.
- `-STOP` selects `SIGSTOP`.
- `"$SUSPECT_PID"` identifies the controlled process.

Expected result: No output is normal; the process state should begin with `T`.

Run:

```bash
kill -CONT "$SUSPECT_PID"
```

Command breakdown:

- `kill` sends a signal.
- `-CONT` selects `SIGCONT`.
- `"$SUSPECT_PID"` identifies the stopped process.

Expected result: No output is normal; the process resumes.

## Associate a Listener With a Process

Run:

```bash
curl -s http://127.0.0.1:8085
```

Command breakdown:

- `curl` transfers data using a URL.
- `-s` suppresses progress output.
- `http://127.0.0.1:8085` requests the controlled local service.

Expected result: The controlled page content.

Run:

```bash
sudo ss -ltnp | grep -F ':8085' > evidence/network-process-listener.txt
```

Command breakdown:

- `sudo` runs the socket command with administrative privileges.
- `ss` displays sockets.
- `-l` selects listening sockets.
- `-t` selects TCP.
- `-n` displays numerical addresses and ports.
- `-p` displays process information.
- `|` sends output to `grep`.
- `grep -F ':8085'` performs a fixed-string port match.
- `>` redirects and overwrites the destination.
- `evidence/network-process-listener.txt` is the evidence file.

Expected result: No terminal output is normal.

Run:

```bash
ps -p "$HTTP_PID" -o pid,ppid,user,stat,ni,lstart,etime,comm,args > evidence/http-server-process.txt
```

Command breakdown:

- `ps` displays process information.
- `-p "$HTTP_PID"` selects the HTTP process.
- `-o pid,ppid,user,stat,ni,lstart,etime,comm,args` defines evidence fields.
- `>` redirects and overwrites the destination.
- `evidence/http-server-process.txt` is the evidence file.

Expected result: No terminal output is normal.

Run:

```bash
ls -l "/proc/$HTTP_PID/fd" > evidence/http-server-file-descriptors.txt
```

Command breakdown:

- `ls` lists entries.
- `-l` displays detailed link targets.
- `"/proc/$HTTP_PID/fd"` contains the HTTP process descriptors.
- `>` redirects and overwrites the destination.
- `evidence/http-server-file-descriptors.txt` is the evidence file.

Expected result: No terminal output is normal.

## Inspect Resource Use and Priority

Run:

```bash
top -b -n 1 | head -n 30 > evidence/top-snapshot.txt
```

Command breakdown:

- `top` displays resource and process information.
- `-b` enables batch mode.
- `-n 1` performs one refresh.
- `|` sends output to `head`.
- `head -n 30` limits evidence to 30 lines.
- `>` redirects and overwrites the destination.
- `evidence/top-snapshot.txt` is the evidence file.

Expected result: No terminal output is normal.

Run:

```bash
nice -n 10 sleep 900 > scenario/logs/low-priority-sleep.log 2>&1 & echo $! > scenario/pids/low-priority-sleep.pid
```

Command breakdown:

- `nice` starts a command with an adjusted nice value.
- `-n 10` requests nice value 10.
- `sleep 900` is the controlled command.
- `>` redirects standard output.
- `2>&1` redirects standard error to the same log.
- `&` runs the process in the background.
- `echo $!` outputs its PID.
- `>` writes the PID file.

Expected result: A controlled lower-priority process starts.

Run:

```bash
NICE_PID="$(cat scenario/pids/low-priority-sleep.pid)"; renice -n 15 -p "$NICE_PID"
```

Command breakdown:

- `NICE_PID=` assigns the controlled PID.
- `"$(cat scenario/pids/low-priority-sleep.pid)"` reads the PID file.
- `;` separates commands.
- `renice` changes an existing process's nice value.
- `-n 15` requests nice value 15.
- `-p` identifies a PID target.
- `"$NICE_PID"` supplies the PID.

Expected result: Output shows the old and new nice values.

Run:

```bash
ps -p "$NICE_PID" -o pid,ni,pri,stat,comm,args > evidence/process-priority.txt
```

Command breakdown:

- `ps` displays process information.
- `-p "$NICE_PID"` selects the controlled process.
- `-o pid,ni,pri,stat,comm,args` defines priority and identity fields.
- `>` redirects and overwrites the destination.
- `evidence/process-priority.txt` is the evidence file.

Expected result: No terminal output is normal.

## Correlate Services and Processes

Run:

```bash
systemctl list-units --type=service --state=running --no-pager | head -n 25
```

Command breakdown:

- `systemctl` inspects systemd.
- `list-units` lists known units.
- `--type=service` selects services.
- `--state=running` selects running units.
- `--no-pager` prints directly.
- `| head -n 25` limits the display.

Expected result: A sample of running services.

Run:

```bash
systemctl show ssh -p Id -p ActiveState -p SubState -p MainPID -p FragmentPath > evidence/ssh-service-process.txt
```

Command breakdown:

- `systemctl` inspects systemd.
- `show` displays unit properties.
- `ssh` identifies the SSH service.
- Each `-p` selects a property.
- `Id` displays the resolved unit name.
- `ActiveState` displays the high-level state.
- `SubState` displays the detailed state.
- `MainPID` displays the primary process ID.
- `FragmentPath` displays the unit file path.
- `>` redirects and overwrites the destination.
- `evidence/ssh-service-process.txt` is the evidence file.

Expected result: No terminal output is normal.

## Terminate Controlled Processes Safely

Run:

```bash
BENIGN_PID="$(cat scenario/pids/benign-sleep.pid)"; kill -0 "$BENIGN_PID"
```

Command breakdown:

- `BENIGN_PID=` assigns the controlled PID.
- `"$(cat scenario/pids/benign-sleep.pid)"` reads the PID file.
- `;` separates commands.
- `kill` tests signal access.
- `-0` sends no actual signal.
- `"$BENIGN_PID"` supplies the PID.

Expected result: No output means the process exists and is signalable.

Run:

```bash
kill -TERM "$BENIGN_PID"
```

Command breakdown:

- `kill` sends a signal.
- `-TERM` selects graceful termination.
- `"$BENIGN_PID"` identifies the controlled process.

Expected result: No output is normal.

## Complete Evidence and Hashes

Run:

```bash
sha256sum "$(readlink -f "/proc/$SUSPECT_PID/exe")" > evidence/suspicious-process-executable-hash.txt
```

Command breakdown:

- `sha256sum` calculates a SHA-256 hash.
- `"$(readlink -f "/proc/$SUSPECT_PID/exe")"` resolves the live process executable.
- `>` redirects and overwrites the destination.
- `evidence/suspicious-process-executable-hash.txt` is the evidence file.

Expected result: No terminal output is normal.

Run:

```bash
sudo ss -ltnp > evidence/listening-tcp-processes.txt
```

Command breakdown:

- `sudo` runs with administrative privileges.
- `ss` displays sockets.
- `-l` selects listeners.
- `-t` selects TCP.
- `-n` displays numerical values.
- `-p` displays process information.
- `>` redirects and overwrites the destination.
- `evidence/listening-tcp-processes.txt` is the evidence file.

Expected result: No terminal output is normal.

Run:

```bash
nano evidence/process-investigation-findings.txt
```

Command breakdown:

- `nano` opens the Nano editor.
- `evidence/process-investigation-findings.txt` is the assessment file.

Expected result: Nano opens for editing.

Run:

```bash
sha256sum evidence/*.txt > evidence/evidence-checksums.sha256
```

Command breakdown:

- `sha256sum` calculates SHA-256 hashes.
- `evidence/*.txt` selects evidence text files.
- `>` redirects and overwrites the checksum file.
- `evidence/evidence-checksums.sha256` is the checksum list.

Expected result: No terminal output is normal.

Run:

```bash
sha256sum -c evidence/evidence-checksums.sha256
```

Command breakdown:

- `sha256sum` calculates and compares hashes.
- `-c` checks files against the stored list.
- `evidence/evidence-checksums.sha256` is the checksum list.

Expected result: Every listed file reports `OK`.

## Challenge Commands

Run:

```bash
printf "Challenge web service\n" > challenge/webroot/index.html
```

Command breakdown:

- `printf` writes formatted text.
- `"Challenge web service\n"` is the page content.
- `>` redirects and overwrites the destination.
- `challenge/webroot/index.html` is the challenge page.

Expected result: No output is normal.

Run:

```bash
bash -c 'exec -a cloud-sync-helper sleep 1200' > challenge/logs/cloud-sync-helper.log 2>&1 & echo $! > challenge/pids/cloud-sync-helper.pid
```

Command breakdown:

- `bash -c` executes a command string.
- `exec -a cloud-sync-helper` assigns a custom argument and replaces Bash.
- `sleep 1200` is the actual controlled process.
- `>` and `2>&1` record output and errors.
- `&` starts the process in the background.
- `echo $!` outputs its PID.
- `>` stores the PID in `challenge/pids/cloud-sync-helper.pid`.

Expected result: The controlled process starts silently.

Run:

```bash
python3 -m http.server 9095 --bind 127.0.0.1 --directory challenge/webroot > challenge/logs/challenge-http.log 2>&1 & echo $! > challenge/pids/challenge-http.pid
```

Command breakdown:

- `python3 -m http.server` starts the Python HTTP module.
- `9095` is the challenge port.
- `--bind 127.0.0.1` limits the listener to loopback.
- `--directory challenge/webroot` selects served content.
- `>` and `2>&1` record output and errors.
- `&` starts the process in the background.
- `echo $!` outputs its PID.
- `>` stores the PID.

Expected result: The challenge listener starts.

Run:

```bash
CHALLENGE_SUSPECT_PID="$(cat challenge/pids/cloud-sync-helper.pid)"; CHALLENGE_HTTP_PID="$(cat challenge/pids/challenge-http.pid)"; ps -p "$CHALLENGE_SUSPECT_PID,$CHALLENGE_HTTP_PID" -o pid,ppid,user,stat,ni,lstart,etime,comm,args > evidence/challenge-target-processes.txt
```

Command breakdown:

- `CHALLENGE_SUSPECT_PID=` reads and stores the custom process PID.
- `;` separates commands.
- `CHALLENGE_HTTP_PID=` reads and stores the listener PID.
- `ps` displays process information.
- `-p` selects the two comma-separated PIDs.
- `-o` defines detailed investigation fields.
- `>` redirects and overwrites the destination.
- `evidence/challenge-target-processes.txt` is the evidence file.

Expected result: No terminal output is normal.

Run:

```bash
ps -eo pid,ppid,user,stat,ni,comm,args --sort=pid > evidence/challenge-process-snapshot.txt
```

Command breakdown:

- `ps` displays process information.
- `-e` selects every process.
- `-o` defines custom fields.
- `--sort=pid` sorts by PID.
- `>` redirects and overwrites the destination.
- `evidence/challenge-process-snapshot.txt` is the evidence file.

Expected result: No terminal output is normal.

Run:

```bash
CHALLENGE_PARENT="$(ps -o ppid= -p "$CHALLENGE_SUSPECT_PID" | tr -d ' ')"; ps -p "$CHALLENGE_PARENT" -o pid,ppid,user,stat,comm,args > evidence/challenge-parent-process.txt
```

Command breakdown:

- `CHALLENGE_PARENT=` assigns the parent PID.
- `ps -o ppid= -p "$CHALLENGE_SUSPECT_PID"` obtains the PPID.
- `| tr -d ' '` removes padding.
- `;` separates commands.
- `ps -p "$CHALLENGE_PARENT"` selects the parent.
- `-o pid,ppid,user,stat,comm,args` defines the fields.
- `>` redirects and overwrites the destination.
- `evidence/challenge-parent-process.txt` is the evidence file.

Expected result: No terminal output is normal.

Run:

```bash
cat "/proc/$CHALLENGE_SUSPECT_PID/status" > evidence/challenge-suspect-status.txt
```

Command breakdown:

- `cat` reads the live process status.
- `"/proc/$CHALLENGE_SUSPECT_PID/status"` is the source.
- `>` redirects and overwrites the destination.
- `evidence/challenge-suspect-status.txt` is the evidence file.

Expected result: No terminal output is normal.

Run:

```bash
tr '\0' ' ' < "/proc/$CHALLENGE_SUSPECT_PID/cmdline" > evidence/challenge-suspect-cmdline.txt; printf '\n' >> evidence/challenge-suspect-cmdline.txt
```

Command breakdown:

- `tr` replaces null separators with spaces.
- `<` redirects the live command line into `tr`.
- `>` stores the readable command line.
- `;` separates commands.
- `printf '\n'` appends a final newline.
- `>>` appends to the same file.

Expected result: No terminal output is normal.

Run:

```bash
printf "Executable: %s\nWorking directory: %s\n" "$(readlink -f "/proc/$CHALLENGE_SUSPECT_PID/exe")" "$(readlink -f "/proc/$CHALLENGE_SUSPECT_PID/cwd")" > evidence/challenge-suspect-links.txt
```

Command breakdown:

- `printf` writes formatted evidence.
- `readlink -f` resolves the executable and working-directory links.
- `>` redirects and overwrites the destination.
- `evidence/challenge-suspect-links.txt` is the evidence file.

Expected result: No terminal output is normal.

Run:

```bash
sudo ss -ltnp | grep -F ':9095' > evidence/challenge-network-listener.txt
```

Command breakdown:

- `sudo ss -ltnp` displays listening TCP sockets with process details.
- `|` sends output to `grep`.
- `grep -F ':9095'` selects the challenge port literally.
- `>` redirects and overwrites the destination.
- `evidence/challenge-network-listener.txt` is the evidence file.

Expected result: No terminal output is normal.

Run:

```bash
ps -p "$CHALLENGE_HTTP_PID" -o pid,ppid,user,stat,ni,lstart,etime,comm,args > evidence/challenge-http-process.txt
```

Command breakdown:

- `ps` displays process information.
- `-p "$CHALLENGE_HTTP_PID"` selects the listener process.
- `-o` defines detailed fields.
- `>` redirects and overwrites the destination.
- `evidence/challenge-http-process.txt` is the evidence file.

Expected result: No terminal output is normal.

Run:

```bash
nano evidence/challenge-assessment.txt
```

Command breakdown:

- `nano` opens the Nano editor.
- `evidence/challenge-assessment.txt` is the challenge assessment file.

Expected result: Nano opens for editing.

Run:

```bash
kill -TERM "$CHALLENGE_SUSPECT_PID" "$CHALLENGE_HTTP_PID"
```

Command breakdown:

- `kill` sends signals.
- `-TERM` requests graceful termination.
- `"$CHALLENGE_SUSPECT_PID"` is the controlled custom process.
- `"$CHALLENGE_HTTP_PID"` is the controlled listener.

Expected result: No output is normal.

Run:

```bash
sha256sum evidence/*.txt > evidence/evidence-checksums.sha256
```

Command breakdown:

- `sha256sum` recalculates hashes for the complete evidence set.
- `evidence/*.txt` selects all evidence text files.
- `>` replaces the previous checksum list.
- `evidence/evidence-checksums.sha256` is the final checksum file.

Expected result: No terminal output is normal.

Run:

```bash
sha256sum -c evidence/evidence-checksums.sha256
```

Command breakdown:

- `sha256sum` verifies hashes.
- `-c` checks the stored list.
- `evidence/evidence-checksums.sha256` is the final checksum file.

Expected result: Every evidence file reports `OK`.
