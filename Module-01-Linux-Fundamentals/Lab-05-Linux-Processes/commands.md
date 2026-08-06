# Linux Processes — Selected Commands

This concise reference contains the commands that best represent the Lab 05 investigation. PID-dependent commands must be run while the controlled process is still active. Every terminal command is displayed on one line.

## Create the Controlled Scenario

Creates the custom-labeled `sleep` process and records its PID:

```bash
bash -c 'exec -a system-update-agent sleep 900' > scenario/logs/system-update-agent.log 2>&1 & echo $! > scenario/pids/system-update-agent.pid
```

Creates the local HTTP listener on TCP port `8085` and records its PID:

```bash
python3 -m http.server 8085 --bind 127.0.0.1 --directory scenario/webroot > scenario/logs/http-server.log 2>&1 & echo $! > scenario/pids/http-server.pid
```

## Discover and Triage Processes

Displays process ancestry, ownership, state, priority, timing, executable name, and arguments:

```bash
ps -eo pid,ppid,user,stat,ni,lstart,etime,comm,args --forest
```

Finds the controlled processes by full command line:

```bash
pgrep -af 'system-update-agent|http.server'
```

Displays focused metadata for the custom-labeled process:

```bash
SUSPECT_PID="$(cat scenario/pids/system-update-agent.pid)"; ps -p "$SUSPECT_PID" -o pid,ppid,user,stat,ni,lstart,etime,comm,args
```

## Validate the Process Through `/proc`

Displays the null-separated arguments in readable form:

```bash
SUSPECT_PID="$(cat scenario/pids/system-update-agent.pid)"; tr '\0' ' ' < "/proc/$SUSPECT_PID/cmdline"; printf '\n'
```

Resolves the actual executable path:

```bash
SUSPECT_PID="$(cat scenario/pids/system-update-agent.pid)"; readlink -f "/proc/$SUSPECT_PID/exe"
```

Resolves the process working directory:

```bash
SUSPECT_PID="$(cat scenario/pids/system-update-agent.pid)"; readlink -f "/proc/$SUSPECT_PID/cwd"
```

Reviews the process's open file descriptors:

```bash
SUSPECT_PID="$(cat scenario/pids/system-update-agent.pid)"; ls -l "/proc/$SUSPECT_PID/fd"
```

## Control and Verify Process State

Suspends the process and verifies the stopped state:

```bash
SUSPECT_PID="$(cat scenario/pids/system-update-agent.pid)"; kill -STOP "$SUSPECT_PID" && ps -p "$SUSPECT_PID" -o pid,stat,comm,args
```

Resumes the process and verifies its state:

```bash
SUSPECT_PID="$(cat scenario/pids/system-update-agent.pid)"; kill -CONT "$SUSPECT_PID" && ps -p "$SUSPECT_PID" -o pid,stat,comm,args
```

Requests graceful termination of the controlled process:

```bash
SUSPECT_PID="$(cat scenario/pids/system-update-agent.pid)"; kill -TERM "$SUSPECT_PID" && wait "$SUSPECT_PID" 2>/dev/null || true
```

## Attribute Network Activity

Associates TCP port `8085` with its listening process:

```bash
sudo ss -ltnp | grep -F ':8085'
```

## Inspect Priority and Services

Creates a lower-priority controlled process and records its PID:

```bash
nice -n 10 sleep 900 > scenario/logs/low-priority-sleep.log 2>&1 & echo $! > scenario/pids/low-priority-sleep.pid
```

Changes the existing nice value and verifies the result:

```bash
NICE_PID="$(cat scenario/pids/low-priority-sleep.pid)"; renice -n 15 -p "$NICE_PID" && ps -p "$NICE_PID" -o pid,ppid,stat,ni,comm,args
```

Correlates the SSH systemd service with its active main PID:

```bash
systemctl show ssh -p Id -p ActiveState -p SubState -p MainPID -p FragmentPath
```

## Hash and Verify Evidence

Hashes the executable associated with the custom-labeled process only when the executable path resolves successfully:

```bash
SUSPECT_PID="$(cat scenario/pids/system-update-agent.pid)"; EXE_PATH="$(readlink -f "/proc/$SUSPECT_PID/exe")" && sha256sum "$EXE_PATH"
```

Verifies the final evidence checksum manifest:

```bash
cd evidence && sha256sum -c evidence-checksums.sha256
```
