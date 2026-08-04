# Lab 02 Commands

### Create the workspace

```bash
mkdir -p /home/testlab/cloud-security-labs/lab-02-file-integrity
```

- `mkdir` — Creates directories.
- `-p` — Creates missing parent directories and avoids errors if they already exist.
- `/home/testlab/cloud-security-labs/lab-02-file-integrity` — Destination path.

### Enter the workspace

```bash
cd /home/testlab/cloud-security-labs/lab-02-file-integrity
```

- `cd` — Changes the current directory.

### Create the test file

```bash
echo "database_password=TrainingOnly123" > application.conf
```

- `echo` — Produces text.
- `>` — Redirects output and overwrites the destination.
- `application.conf` — Destination file.

### Review permissions

```bash
ls -l application.conf
```

- `ls` — Lists files.
- `-l` — Uses long format.

### Restrict permissions

```bash
chmod 600 application.conf
```

- `chmod` — Changes permissions.
- `600` — Owner read/write; group and others no access.

### Review metadata

```bash
stat application.conf
```

- `stat` — Displays detailed file metadata.

### Create the hash baseline

```bash
sha256sum application.conf > application.conf.sha256
```

- `sha256sum` — Calculates a SHA-256 hash.
- `>` — Writes the result to a file.
- `application.conf.sha256` — Baseline checksum file.

### Verify the baseline

```bash
sha256sum -c application.conf.sha256
```

- `-c` — Checks files against stored checksums.

### Simulate unauthorized modification

```bash
echo "remote_access=true" >> application.conf
```

- `>>` — Appends output without overwriting existing content.

### Detect the change

```bash
sha256sum -c application.conf.sha256
```

Expected result: `FAILED`

### Restore the file

```bash
sed -i '/remote_access=true/d' application.conf
```

- `sed` — Stream editor.
- `-i` — Edits the file in place.
- `/remote_access=true/d` — Deletes matching lines.

### Verify the restored file

```bash
sha256sum -c application.conf.sha256
```

Expected result: `OK`
