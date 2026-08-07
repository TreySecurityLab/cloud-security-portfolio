# Linux Services and systemd — Selected Commands

This concise reference contains the commands that best represent the Lab 06 investigation. Commands that depend on a live controlled service must be run while that service is active. Every terminal command is displayed on one line.

## Inventory and Inspect Services

Confirms that systemd is PID 1:

```bash
ps -p 1 -o pid,ppid,user,stat,comm,args
```

Lists running services:

```bash
systemctl list-units --type=service --state=running --no-pager
```

Lists enabled service unit files:

```bash
systemctl list-unit-files --type=service --state=enabled --no-pager
```

Lists failed services:

```bash
systemctl --failed --type=service --no-pager
```

## Investigate the SSH Service

Displays SSH runtime status and recent records:

```bash
systemctl status ssh --no-pager
```

Displays structured SSH service properties:

```bash
systemctl show ssh -p Id -p Description -p LoadState -p ActiveState -p SubState -p UnitFileState -p MainPID -p User -p Group -p ExecStart -p FragmentPath
```

Displays the effective SSH unit configuration without executing it:

```bash
systemctl cat ssh
```

Correlates the SSH service with its live main process:

```bash
SSH_PID="$(systemctl show ssh -p MainPID --value)"; ps -p "$SSH_PID" -o pid,ppid,user,group,stat,lstart,etime,comm,args
```

Reviews SSH dependencies:

```bash
systemctl list-dependencies ssh --no-pager
```

Reviews recent SSH journal entries:

```bash
sudo journalctl -u ssh -n 25 --no-pager
```

## Validate and Inspect the Controlled Service

Checks the controlled heartbeat script without executing it:

```bash
bash -n scenario/app/lab-heartbeat.sh
```

Validates the controlled systemd unit:

```bash
systemd-analyze verify scenario/units/lab-heartbeat.service
```

Displays key properties after the unit is loaded:

```bash
systemctl show lab-heartbeat -p Id -p LoadState -p ActiveState -p SubState -p UnitFileState -p FragmentPath
```

Correlates the controlled service with its main process:

```bash
LAB_SERVICE_PID="$(systemctl show lab-heartbeat -p MainPID --value)"; ps -p "$LAB_SERVICE_PID" -o pid,ppid,user,group,stat,ni,lstart,etime,comm,args
```

Resolves the controlled service executable and working directory:

```bash
LAB_SERVICE_PID="$(systemctl show lab-heartbeat -p MainPID --value)"; printf "Executable: %s\nWorking directory: %s\n" "$(readlink -f "/proc/$LAB_SERVICE_PID/exe")" "$(readlink -f "/proc/$LAB_SERVICE_PID/cwd")"
```

Reviews the controlled heartbeat output:

```bash
tail -n 5 scenario/logs/lab-heartbeat.log
```

## Analyze Persistence and Hardening

Locates the service enablement link:

```bash
find /etc/systemd/system -type l -lname '*lab-heartbeat.service' -ls
```

Inventories common systemd unit and drop-in locations:

```bash
sudo find /etc/systemd/system /run/systemd/system /usr/lib/systemd/system /lib/systemd/system -maxdepth 3 -type f \( -name '*.service' -o -name '*.conf' \) -printf '%TY-%Tm-%Td %TH:%TM %M %u %g %p\n' 2>/dev/null | sort
```

Reviews controlled-service hardening:

```bash
systemd-analyze security lab-heartbeat.service --no-pager
```

## Hash and Verify Evidence

Hashes the retained controlled script and unit-file copy:

```bash
sha256sum scenario/app/lab-heartbeat.sh scenario/units/lab-heartbeat.service > evidence/lab-heartbeat-file-hashes.txt
```

Verifies the final evidence checksum manifest:

```bash
cd evidence && sha256sum -c evidence-checksums.sha256
```
