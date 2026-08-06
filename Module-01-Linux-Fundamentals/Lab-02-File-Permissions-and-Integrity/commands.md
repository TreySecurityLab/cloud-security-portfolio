# File Permissions and Integrity — Selected Commands

This concise reference contains the commands that best represent the Lab 02 workflow. Every terminal command is displayed on one line.

## Create and Inspect the Controlled File

Creates the training configuration file:

```bash
printf '%s\n' 'database_password=TrainingOnly123' > application.conf
```

Displays the file's symbolic permissions and ownership:

```bash
ls -l application.conf
```

Restricts the file to owner read and write access:

```bash
chmod 600 application.conf
```

Displays detailed file metadata:

```bash
stat application.conf
```

## Establish and Test Integrity

Creates the SHA-256 integrity baseline:

```bash
sha256sum application.conf > application.conf.sha256
```

Verifies the initial trusted state:

```bash
sha256sum -c application.conf.sha256
```

Appends the controlled unauthorized setting:

```bash
printf '%s\n' 'remote_access=true' >> application.conf
```

Runs the integrity check that should report `FAILED` after modification:

```bash
sha256sum -c application.conf.sha256
```

Removes the controlled unauthorized line:

```bash
sed -i '/remote_access=true/d' application.conf
```

Verifies that the restored file matches the original baseline:

```bash
sha256sum -c application.conf.sha256
```

## Verify Evidence

Verifies the final evidence checksum manifest:

```bash
cd evidence && sha256sum -c evidence-checksums.sha256
```
