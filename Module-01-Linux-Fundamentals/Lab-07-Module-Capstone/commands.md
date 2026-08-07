# Module Capstone — Selected Commands

This concise reference contains the commands that best represent the Lab 07 investigation. PID-dependent commands must be run while the controlled process is still active. Every terminal command is displayed on one line.

## Establish and Verify File Integrity

Applies owner-only read/write permissions to the controlled application configuration:

```bash
chmod 600 ~/cloud-security-labs/lab-07-module-capstone/scenario/config/application.conf
```

Displays permissions, ownership, and the configuration filename:

```bash
stat -c '%A %a %U %G %n' ~/cloud-security-labs/lab-07-module-capstone/scenario/config/application.conf
```

Creates the trusted SHA-256 baseline:

```bash
cd ~/cloud-security-labs/lab-07-module-capstone && sha256sum scenario/config/application.conf > evidence/application-baseline.sha256
```

Verifies the trusted baseline:

```bash
cd ~/cloud-security-labs/lab-07-module-capstone && sha256sum -c evidence/application-baseline.sha256
```

Preserves the failed verification after the controlled configuration change:

```bash
cd ~/cloud-security-labs/lab-07-module-capstone && sha256sum -c evidence/application-baseline.sha256 2>&1 | tee evidence/application-integrity-failure.txt
```

## Investigate the Configuration

Inventories configuration files with permissions, ownership, size, modification time, and path:

```bash
find ~/cloud-security-labs/lab-07-module-capstone/scenario -type f -name '*.conf' -printf '%M %u %g %s %TY-%Tm-%Td %TH:%TM %p\n' | tee ~/cloud-security-labs/lab-07-module-capstone/evidence/config-file-inventory.txt
```

Searches the controlled configuration for security-relevant settings:

```bash
grep -RniE 'debug|password|secret|token|logging|disabled|enabled' ~/cloud-security-labs/lab-07-module-capstone/scenario/config | tee ~/cloud-security-labs/lab-07-module-capstone/evidence/config-content-review.txt
```

## Discover and Attribute the Controlled Process

Creates the controlled custom-labeled process and records its PID:

```bash
cd ~/cloud-security-labs/lab-07-module-capstone && bash -c 'exec -a cloud-update-agent sleep 900' > scenario/logs/cloud-update-agent.log 2>&1 & echo $! > scenario/pids/cloud-update-agent.pid
```

Locates the process by its displayed full command line:

```bash
pgrep -af 'cloud-update-agent'
```

Captures PID, PPID, owner, state, timing, executable name, and full arguments:

```bash
cd ~/cloud-security-labs/lab-07-module-capstone && TARGET_PID="$(cat scenario/pids/cloud-update-agent.pid)"; ps -p "$TARGET_PID" -o pid,ppid,user,stat,lstart,etime,comm,args | tee evidence/process-metadata.txt
```

Resolves the executable associated with the controlled process:

```bash
cd ~/cloud-security-labs/lab-07-module-capstone && TARGET_PID="$(cat scenario/pids/cloud-update-agent.pid)"; readlink -f "/proc/$TARGET_PID/exe" | tee evidence/process-executable.txt
```

Hashes the resolved executable only when path resolution succeeds:

```bash
cd ~/cloud-security-labs/lab-07-module-capstone && TARGET_PID="$(cat scenario/pids/cloud-update-agent.pid)"; EXE_PATH="$(readlink -f "/proc/$TARGET_PID/exe")" && sha256sum "$EXE_PATH" | tee evidence/process-executable.sha256
```

## Correlate the SSH systemd Service

Displays the SSH unit identity, runtime state, enablement state, main PID, and unit-file path:

```bash
systemctl show ssh -p Id -p LoadState -p ActiveState -p SubState -p UnitFileState -p MainPID -p FragmentPath
```

Preserves the structured SSH service evidence:

```bash
systemctl show ssh -p Id -p LoadState -p ActiveState -p SubState -p UnitFileState -p MainPID -p FragmentPath | tee ~/cloud-security-labs/lab-07-module-capstone/evidence/ssh-service-analysis.txt
```

## Restore and Verify the Trusted State

Removes only the controlled `debug=true` line:

```bash
sed -i '/^debug=true$/d' ~/cloud-security-labs/lab-07-module-capstone/scenario/config/application.conf
```

Revalidates the restored configuration against the original baseline:

```bash
cd ~/cloud-security-labs/lab-07-module-capstone && sha256sum -c evidence/application-baseline.sha256 | tee evidence/application-integrity-restored.txt
```

Requests graceful termination of only the controlled process:

```bash
cd ~/cloud-security-labs/lab-07-module-capstone && TARGET_PID="$(cat scenario/pids/cloud-update-agent.pid)"; kill -TERM "$TARGET_PID"
```

## Hash and Verify Evidence

Creates the final evidence checksum manifest without hashing the manifest itself:

```bash
cd ~/cloud-security-labs/lab-07-module-capstone/evidence && find . -maxdepth 1 -type f ! -name 'evidence-checksums.sha256' -printf '%P\0' | sort -z | xargs -0 -r sha256sum > evidence-checksums.sha256
```

Verifies the final evidence checksum manifest:

```bash
cd ~/cloud-security-labs/lab-07-module-capstone/evidence && sha256sum -c evidence-checksums.sha256
```
