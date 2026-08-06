# Find and Grep Commands

## Create the Workspace

Run:

```bash
mkdir -p /home/testlab/cloud-security-labs/lab-04-find-and-grep/evidence
```

Command breakdown:

- `mkdir` creates directories.
- `-p` creates missing parent directories and does not fail when a directory already exists.
- `/home/testlab/cloud-security-labs/lab-04-find-and-grep/evidence` is the complete destination path.

Run:

```bash
cd /home/testlab/cloud-security-labs/lab-04-find-and-grep
```

Command breakdown:

- `cd` changes the current working directory.
- `/home/testlab/cloud-security-labs/lab-04-find-and-grep` is the destination directory.

Run:

```bash
mkdir -p search-data/configs search-data/logs search-data/scripts search-data/archive search-data/uploads
```

Command breakdown:

- `mkdir` creates directories.
- `-p` creates missing parent directories and avoids errors for existing directories.
- `search-data/configs` stores configuration files.
- `search-data/logs` stores log files.
- `search-data/scripts` stores shell scripts.
- `search-data/archive` stores older files.
- `search-data/uploads` stores test uploads.

## Create Controlled Files

Run:

```bash
printf "application_name=academy-app\ndebug=false\nremote_access=false\nlog_level=INFO\n" > search-data/configs/application.conf
```

Command breakdown:

- `printf` writes formatted text.
- `"application_name=academy-app\ndebug=false\nremote_access=false\nlog_level=INFO\n"` is the configuration content.
- `\n` creates a new line.
- `>` redirects output and overwrites the destination.
- `search-data/configs/application.conf` is the destination file.

Run:

```bash
printf "application_name=academy-app\ndebug=true\nremote_access=true\nadmin_password=TrainingOnly123\n" > search-data/configs/application-backup.conf
```

Command breakdown:

- `printf` writes formatted text.
- `"application_name=academy-app\ndebug=true\nremote_access=true\nadmin_password=TrainingOnly123\n"` is controlled lab content.
- `\n` creates a new line.
- `>` redirects output and overwrites the destination.
- `search-data/configs/application-backup.conf` is the destination file.

Run:

```bash
printf "SERVICE_NAME=REPORTING\nENABLED=true\n" > search-data/configs/Reporting.CONF
```

Command breakdown:

- `printf` writes formatted text.
- `"SERVICE_NAME=REPORTING\nENABLED=true\n"` is the configuration content.
- `\n` creates a new line.
- `>` redirects output and overwrites the destination.
- `search-data/configs/Reporting.CONF` uses an uppercase extension for case-sensitivity testing.

Run:

```bash
printf "2026-08-04 18:00:00 Accepted password for testlab from 192.168.50.1\n2026-08-04 18:05:00 Failed password for invalid user admin from 192.168.50.1\n2026-08-04 18:06:00 Failed password for invalid user root from 192.168.50.1\n" > search-data/logs/auth.log
```

Command breakdown:

- `printf` writes formatted log entries.
- `\n` separates the entries.
- `>` redirects output and overwrites the destination.
- `search-data/logs/auth.log` is the controlled authentication log.

Run:

```bash
printf "2026-08-04 18:00:00 INFO Application started\n2026-08-04 18:03:00 WARN Configuration backup detected\n2026-08-04 18:07:00 ERROR Remote update failed\n2026-08-04 18:08:00 INFO Health check passed\n" > search-data/logs/application.log
```

Command breakdown:

- `printf` writes formatted log entries.
- `\n` separates the entries.
- `>` redirects output and overwrites the destination.
- `search-data/logs/application.log` is the destination log file.

Run:

```bash
printf "DEBUG Legacy diagnostic mode enabled\nDEBUG Temporary connection test completed\n" > search-data/archive/old-debug.log
```

Command breakdown:

- `printf` writes formatted debug entries.
- `\n` separates the entries.
- `>` redirects output and overwrites the destination.
- `search-data/archive/old-debug.log` is the archived log file.

Run:

```bash
for i in {1..200}; do printf "INFO Training event %03d completed\n" "$i"; done > search-data/logs/activity.log
```

Command breakdown:

- `for` begins a loop.
- `i` is the loop variable.
- `in` supplies values to the loop.
- `{1..200}` expands to the integers from 1 through 200.
- `;` separates shell statements on the same line.
- `do` begins the loop body.
- `printf` writes a formatted line.
- `"INFO Training event %03d completed\n"` is the output format.
- `%03d` prints the number using three digits with leading zeros.
- `\n` creates a new line.
- `"$i"` supplies the current loop value.
- `;` separates the loop body from its closing keyword.
- `done` ends the loop.
- `>` redirects all loop output and overwrites the destination.
- `search-data/logs/activity.log` is the destination file.

Run:

```bash
printf '#!/usr/bin/env bash\nrsync -a /srv/application/ /srv/backups/application/\necho "Maintenance complete"\n' > search-data/scripts/maintenance.sh
```

Command breakdown:

- `printf` writes formatted script content.
- `#!/usr/bin/env bash` identifies Bash as the interpreter.
- `\n` creates a new line.
- `rsync` copies and synchronizes files.
- `-a` enables archive mode.
- `/srv/application/` is the source path.
- `/srv/backups/application/` is the destination path.
- `echo` writes a completion message.
- `>` redirects output and overwrites the destination.
- `search-data/scripts/maintenance.sh` is the script file being created.

Run:

```bash
printf '#!/usr/bin/env bash\ncurl http://192.168.50.1/health-check -o /tmp/health-check.txt\n' > search-data/scripts/diagnostic.sh
```

Command breakdown:

- `printf` writes formatted script content.
- `#!/usr/bin/env bash` identifies Bash as the interpreter.
- `\n` creates a new line.
- `curl` transfers data using a URL.
- `http://192.168.50.1/health-check` is the controlled lab URL.
- `-o` specifies an output file.
- `/tmp/health-check.txt` is the output destination.
- `>` redirects output and overwrites the destination.
- `search-data/scripts/diagnostic.sh` is the script file being created.

Run:

```bash
printf '#!/usr/bin/env bash\nnc -nv 192.168.50.1 8080\n' > search-data/scripts/connectivity-test.sh
```

Command breakdown:

- `printf` writes formatted script content.
- `#!/usr/bin/env bash` identifies Bash as the interpreter.
- `\n` creates a new line.
- `nc` is the Netcat command.
- `-n` prevents name resolution.
- `-v` enables verbose output.
- `192.168.50.1` is the controlled destination address.
- `8080` is the destination port.
- `>` redirects output and overwrites the destination.
- `search-data/scripts/connectivity-test.sh` is the script file being created.

Run:

```bash
chmod 750 search-data/scripts/maintenance.sh search-data/scripts/diagnostic.sh
```

Command breakdown:

- `chmod` changes file permissions.
- `750` gives the owner read, write, and execute; the group read and execute; and others no permissions.
- `search-data/scripts/maintenance.sh` is the first target.
- `search-data/scripts/diagnostic.sh` is the second target.

Run:

```bash
printf "review_required=true\n" > search-data/.review-note
```

Command breakdown:

- `printf` writes formatted text.
- `"review_required=true\n"` is the file content.
- `\n` creates a new line.
- `>` redirects output and overwrites the destination.
- `search-data/.review-note` is hidden because its name begins with a period.

Run:

```bash
touch search-data/uploads/empty-upload.tmp
```

Command breakdown:

- `touch` creates an empty file or updates an existing file's timestamps.
- `search-data/uploads/empty-upload.tmp` is the target file.

Run:

```bash
printf "Quarterly training report\n" > search-data/uploads/report.txt
```

Command breakdown:

- `printf` writes formatted text.
- `"Quarterly training report\n"` is the file content.
- `\n` creates a new line.
- `>` redirects output and overwrites the destination.
- `search-data/uploads/report.txt` is the destination file.

Run:

```bash
printf '#!/usr/bin/env bash\necho "This is script content inside a text file"\n' > search-data/uploads/readme.txt
```

Command breakdown:

- `printf` writes formatted text.
- `#!/usr/bin/env bash` is a Bash shebang stored in a file with a `.txt` extension.
- `\n` creates a new line.
- `echo` writes text when the stored script content is run.
- `>` redirects output and overwrites the destination.
- `search-data/uploads/readme.txt` is the destination file.

Run:

```bash
touch -d "10 days ago" search-data/archive/old-debug.log
```

Command breakdown:

- `touch` updates file timestamps.
- `-d` uses the supplied date description.
- `"10 days ago"` sets the modification time ten days earlier.
- `search-data/archive/old-debug.log` is the target file.

Run:

```bash
touch -d "90 minutes ago" search-data/configs/application-backup.conf
```

Command breakdown:

- `touch` updates file timestamps.
- `-d` uses the supplied time description.
- `"90 minutes ago"` sets the modification time ninety minutes earlier.
- `search-data/configs/application-backup.conf` is the target file.

## Search with Find

Run:

```bash
find search-data -type f
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type f` selects regular files.

Run:

```bash
find search-data -type d
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type d` selects directories.

Run:

```bash
find search-data -type l
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type l` selects symbolic links.

Run:

```bash
find search-data -type f -name "*.conf"
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type f` selects regular files.
- `-name` performs a case-sensitive filename match.
- `"*.conf"` matches filenames ending in lowercase `.conf`.

Run:

```bash
find search-data -type f -iname "*.conf"
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type f` selects regular files.
- `-iname` performs a case-insensitive filename match.
- `"*.conf"` matches configuration filenames regardless of case.

Run:

```bash
find search-data -type f -path "*/logs/*"
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type f` selects regular files.
- `-path` matches the complete path.
- `"*/logs/*"` matches files beneath a directory named `logs`.

Run:

```bash
find search-data -type f ! -path "*/archive/*"
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type f` selects regular files.
- `!` negates the following condition.
- `-path` matches the complete path.
- `"*/archive/*"` identifies files beneath the archive directory.

Run:

```bash
find search-data -type f -name ".*"
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type f` selects regular files.
- `-name` matches filenames.
- `".*"` matches filenames beginning with a period.

Run:

```bash
find search-data -type f -empty
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type f` selects regular files.
- `-empty` selects empty files.

Run:

```bash
find search-data -type f -size +1k
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type f` selects regular files.
- `-size` filters by file size.
- `+1k` matches files larger than one 1,024-byte block.

Run:

```bash
find search-data -type f -size -100c
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type f` selects regular files.
- `-size` filters by file size.
- `-100c` matches files smaller than 100 bytes.
- `c` measures the size in bytes.

Run:

```bash
find search-data -type f -mmin -60
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type f` selects regular files.
- `-mmin` filters by modification time in minutes.
- `-60` matches files modified less than sixty minutes ago.

Run:

```bash
find search-data -type f -mmin +60
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type f` selects regular files.
- `-mmin` filters by modification time in minutes.
- `+60` matches files modified more than sixty minutes ago.

Run:

```bash
find search-data -type f -mtime +7
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type f` selects regular files.
- `-mtime` filters by modification time in 24-hour periods.
- `+7` matches files modified more than seven days ago.

Run:

```bash
find search-data -type f -perm /111
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type f` selects regular files.
- `-perm` filters by permission bits.
- `/111` matches files with any execute bit set.

Run:

```bash
find search-data -type f \( -iname "*.conf" -o -name "*.sh" \)
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type f` selects regular files.
- `\(` begins a grouped expression.
- `-iname "*.conf"` matches configuration files regardless of case.
- `-o` performs logical OR.
- `-name "*.sh"` matches shell-script filenames.
- `\)` ends the grouped expression.

Run:

```bash
find search-data -type f -printf "%M %u %g %10s %TY-%Tm-%Td %TH:%TM %p\n" | sort
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type f` selects regular files.
- `-printf` controls the displayed information.
- `%M` displays symbolic permissions.
- `%u` displays the owner.
- `%g` displays the group.
- `%10s` displays the size padded to ten spaces.
- `%TY-%Tm-%Td` displays the modification date.
- `%TH:%TM` displays the modification time.
- `%p` displays the path.
- `\n` starts a new output line.
- `|` sends the output into another command.
- `sort` places the results in alphabetical order.

Run:

```bash
find search-data -type f -name "*.sh" -exec stat {} \;
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type f` selects regular files.
- `-name "*.sh"` matches shell-script filenames.
- `-exec` runs a command against each match.
- `stat` displays file metadata.
- `{}` represents the current matched path.
- `\;` ends the `-exec` expression.

## Search with Grep

Run:

```bash
grep -Rni "remote_access" search-data
```

Command breakdown:

- `grep` searches file contents.
- `-R` searches directories recursively.
- `-n` displays matching line numbers.
- `-i` ignores letter case.
- `"remote_access"` is the search pattern.
- `search-data` is the directory being searched.

Run:

```bash
grep -Rni "nc" search-data/scripts
```

Command breakdown:

- `grep` searches file contents.
- `-R` searches directories recursively.
- `-n` displays matching line numbers.
- `-i` ignores letter case.
- `"nc"` is a substring search pattern.
- `search-data/scripts` is the directory being searched.

Run:

```bash
grep -Rniw "nc" search-data/scripts
```

Command breakdown:

- `grep` searches file contents.
- `-R` searches directories recursively.
- `-n` displays matching line numbers.
- `-i` ignores letter case.
- `-w` requires a complete-word match.
- `"nc"` is the search pattern.
- `search-data/scripts` is the directory being searched.

Run:

```bash
grep -RnF "192.168.50.1" search-data
```

Command breakdown:

- `grep` searches file contents.
- `-R` searches directories recursively.
- `-n` displays matching line numbers.
- `-F` treats the pattern as a fixed literal string.
- `"192.168.50.1"` is the controlled address.
- `search-data` is the directory being searched.

Run:

```bash
grep -nC 1 "ERROR" search-data/logs/application.log
```

Command breakdown:

- `grep` searches file contents.
- `-n` displays matching line numbers.
- `-C 1` displays one line before and after each match.
- `"ERROR"` is the search pattern.
- `search-data/logs/application.log` is the file being searched.

Run:

```bash
grep -Rni --include="*.log" "failed" search-data
```

Command breakdown:

- `grep` searches file contents.
- `-R` searches directories recursively.
- `-n` displays matching line numbers.
- `-i` ignores letter case.
- `--include="*.log"` limits the search to lowercase `.log` files.
- `"failed"` is the search pattern.
- `search-data` is the directory being searched.

Run:

```bash
grep -Rni --exclude-dir="archive" "debug" search-data
```

Command breakdown:

- `grep` searches file contents.
- `-R` searches directories recursively.
- `-n` displays matching line numbers.
- `-i` ignores letter case.
- `--exclude-dir="archive"` skips directories named `archive`.
- `"debug"` is the search pattern.
- `search-data` is the directory being searched.

Run:

```bash
find search-data -type f -iname "*.conf" -exec grep -HniE "debug=true|remote_access=true|password" {} +
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type f` selects regular files.
- `-iname "*.conf"` matches configuration files regardless of case.
- `-exec` runs a command against the matched files.
- `grep` searches file contents.
- `-H` displays filenames.
- `-n` displays line numbers.
- `-i` ignores letter case.
- `-E` enables extended regular expressions.
- `"debug=true|remote_access=true|password"` matches any listed pattern.
- `{}` represents the matched files.
- `+` groups matched paths into fewer command executions.

## Collect Evidence

Run:

```bash
find search-data -printf "%M %u %g %10s %TY-%Tm-%Td %TH:%TM %p\n" | sort > evidence/file-inventory.txt
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-printf` controls the displayed information.
- `%M` displays symbolic permissions.
- `%u` displays the owner.
- `%g` displays the group.
- `%10s` displays the size padded to ten spaces.
- `%TY-%Tm-%Td` displays the modification date.
- `%TH:%TM` displays the modification time.
- `%p` displays the path.
- `\n` starts a new output line.
- `|` sends the output into another command.
- `sort` sorts the output.
- `>` redirects output and overwrites the destination.
- `evidence/file-inventory.txt` is the evidence file.

Run:

```bash
find search-data -type f -iname "*.conf" -exec grep -HniE "debug=true|remote_access=true|password" {} + > evidence/risky-configurations.txt
```

Command breakdown:

- `find` searches the file system.
- `search-data` is the starting directory.
- `-type f` selects regular files.
- `-iname "*.conf"` matches configuration files regardless of case.
- `-exec` runs a command against the matched files.
- `grep` searches file contents.
- `-H` displays filenames.
- `-n` displays line numbers.
- `-i` ignores letter case.
- `-E` enables extended regular expressions.
- `"debug=true|remote_access=true|password"` matches any listed pattern.
- `{}` represents the matched files.
- `+` groups matched paths into fewer command executions.
- `>` redirects output and overwrites the destination.
- `evidence/risky-configurations.txt` is the evidence file.

Run:

```bash
grep -RniE "(curl|wget)[[:space:]]+https?://|(^|[[:space:]])(nc|ncat)([[:space:]]|$)|bash[[:space:]]+-c" search-data > evidence/network-tool-matches.txt
```

Command breakdown:

- `grep` searches file contents.
- `-R` searches directories recursively.
- `-n` displays line numbers.
- `-i` ignores letter case.
- `-E` enables extended regular expressions.
- `(curl|wget)` matches either download utility.
- `[[:space:]]+` requires one or more whitespace characters.
- `https?://` matches `http://` or `https://`.
- `|` inside the quoted expression separates alternatives.
- `(^|[[:space:]])` requires the Netcat command to begin at the line start or after whitespace.
- `(nc|ncat)` matches either Netcat command name.
- `([[:space:]]|$)` requires whitespace or the line end after the command.
- `bash[[:space:]]+-c` matches Bash followed by whitespace and `-c`.
- `search-data` is the directory being searched.
- `>` redirects output and overwrites the destination.
- `evidence/network-tool-matches.txt` is the evidence file.

Run:

```bash
nano evidence/investigation-findings.txt
```

Command breakdown:

- `nano` opens the Nano text editor.
- `evidence/investigation-findings.txt` is the findings file being created or edited.

Run:

```bash
sha256sum evidence/*.txt > evidence-checksums.sha256
```

Command breakdown:

- `sha256sum` calculates SHA-256 hashes.
- `evidence/*.txt` selects text files in the evidence directory.
- `>` redirects output and overwrites the destination.
- `evidence-checksums.sha256` stores the checksum list.

Run:

```bash
sha256sum -c evidence-checksums.sha256
```

Command breakdown:

- `sha256sum` calculates and verifies SHA-256 hashes.
- `-c` checks files against the stored checksum list.
- `evidence-checksums.sha256` is the checksum file being verified.
