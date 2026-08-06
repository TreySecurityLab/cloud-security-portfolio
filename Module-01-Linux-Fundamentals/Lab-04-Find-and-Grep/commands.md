# Find and Grep — Selected Commands

This concise reference contains the commands that best represent the Lab 04 search and threat-hunting workflow. No discovered or untrusted script should be executed. Every terminal command is displayed on one line.

## Create the Controlled Scenario

Creates the scenario directory structure:

```bash
mkdir -p search-data/configs search-data/logs search-data/scripts search-data/archive search-data/uploads evidence
```

Creates the weak backup configuration used for investigation:

```bash
printf "application_name=academy-app\ndebug=true\nremote_access=true\nadmin_password=TrainingOnly123\n" > search-data/configs/application-backup.conf
```

Creates the controlled download and Netcat references:

```bash
printf '#!/usr/bin/env bash\ncurl http://192.168.50.1/health-check -o /tmp/health-check.txt\n' > search-data/scripts/diagnostic.sh && printf '#!/usr/bin/env bash\nnc -nv 192.168.50.1 8080\n' > search-data/scripts/connectivity-test.sh
```

Creates the hidden review note and empty upload:

```bash
printf '%s\n' 'review_required=true' > search-data/.review-note && touch search-data/uploads/empty-upload.tmp
```

## Search File Metadata with `find`

Finds configuration and shell-script files using grouped OR logic:

```bash
find search-data -type f \( -iname "*.conf" -o -name "*.sh" \)
```

Finds hidden and empty regular files:

```bash
find search-data -type f \( -name ".*" -o -empty \)
```

Finds files older than seven days:

```bash
find search-data -type f -mtime +7
```

Finds executable regular files:

```bash
find search-data -type f -perm /111
```

Prints permissions, ownership, size, timestamps, and paths in a consistent format:

```bash
find search-data -type f -printf "%M %u:%g %s %TY-%Tm-%Td %TH:%TM %p\n" | sort
```

Runs `stat` safely against matched shell scripts:

```bash
find search-data -type f -name "*.sh" -exec stat {} \;
```

## Search File Contents with `grep`

Compares broad substring matching with word-aware Netcat matching:

```bash
grep -Rni "nc" search-data/scripts; grep -Rniw "nc" search-data/scripts
```

Searches literally for the controlled IP address:

```bash
grep -RnF "192.168.50.1" search-data
```

Displays context around application-log errors:

```bash
grep -nC 1 "ERROR" search-data/logs/application.log
```

Searches only log files for failed-authentication text:

```bash
grep -Rni --include="*.log" "failed" search-data
```

Combines `find` and `grep` to identify weak configuration values:

```bash
find search-data -type f -iname "*.conf" -exec grep -HniE "debug=true|remote_access=true|password" {} +
```

Uses boundary-aware expressions to identify network tools and shell execution patterns:

```bash
grep -RniE "(curl|wget)[[:space:]]+https?://|(^|[[:space:]])(nc|ncat)([[:space:]]|$)|bash[[:space:]]+-c" search-data
```

## Verify Evidence

Verifies the final evidence checksum manifest:

```bash
cd evidence && sha256sum -c evidence-checksums.sha256
```
