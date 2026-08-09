# Users and Groups — Selected Commands

This concise reference contains the commands that best represent the Lab 08 identity and access-control investigation. Every terminal command is displayed on one line.

## Inspect Existing Identity Information

Displays the current user's UID, primary GID, and supplemental groups:

```bash
id
```

Retrieves the `testlab` account through the configured passwd database:

```bash
getent passwd testlab
```

Inventories accounts in the normal non-system UID range while excluding UID `65534`:

```bash
getent passwd | awk -F: '$3 >= 1000 && $3 < 65534 {printf "%s UID=%s GID=%s HOME=%s SHELL=%s\n",$1,$3,$4,$6,$7}' | tee ~/cloud-security-labs/lab-08-users-and-groups/evidence/human-account-inventory.txt
```

Inventories identities whose configured shell ends in `nologin` or `false`:

```bash
getent passwd | awk -F: '$7 ~ /(nologin|false)$/ {printf "%s UID=%s SHELL=%s\n",$1,$3,$7}' | tee ~/cloud-security-labs/lab-08-users-and-groups/evidence/noninteractive-account-inventory.txt
```

## Create the Controlled Authorization Model

Creates the dedicated audit group:

```bash
sudo groupadd cloud-audit
```

Creates the interactive backup operator with a home directory and Bash shell:

```bash
sudo useradd -m -s /bin/bash lab08-backup
```

Reviews password-account status without exposing password hashes:

```bash
sudo passwd -S lab08-backup
```

Appends the required `cloud-audit` supplemental membership:

```bash
sudo usermod -aG cloud-audit lab08-backup
```

Verifies the interactive account's effective identity and group memberships:

```bash
id lab08-backup
```

Creates the noninteractive service identity without a home directory:

```bash
sudo useradd -M -s /usr/sbin/nologin lab08-service
```

Verifies the service identity's passwd record and noninteractive shell:

```bash
getent passwd lab08-service
```

## Detect and Remediate Excessive Authorization

Deliberately adds the controlled service identity to the audit group for the investigation scenario:

```bash
sudo usermod -aG cloud-audit lab08-service
```

Preserves the excessive group membership before remediation:

```bash
getent group cloud-audit | tee ~/cloud-security-labs/lab-08-users-and-groups/evidence/cloud-audit-membership-before-remediation.txt
```

Removes only the unauthorized service-account membership:

```bash
sudo gpasswd -d lab08-service cloud-audit
```

Preserves the corrected group membership after remediation:

```bash
getent group cloud-audit | tee ~/cloud-security-labs/lab-08-users-and-groups/evidence/cloud-audit-membership-after-remediation.txt
```

Captures the two controlled passwd records:

```bash
getent passwd lab08-backup lab08-service | tee ~/cloud-security-labs/lab-08-users-and-groups/evidence/controlled-account-records.txt
```

Captures focused final group information for both controlled identities:

```bash
{ id lab08-backup; id lab08-service; } | tee ~/cloud-security-labs/lab-08-users-and-groups/evidence/controlled-account-groups.txt
```

## Hash and Verify Evidence

Creates the final evidence checksum manifest without hashing the manifest itself:

```bash
cd ~/cloud-security-labs/lab-08-users-and-groups/evidence && find . -maxdepth 1 -type f ! -name 'evidence-checksums.sha256' -printf '%P\0' | sort -z | xargs -0 -r sha256sum > evidence-checksums.sha256
```

Verifies every file recorded in the final evidence manifest:

```bash
cd ~/cloud-security-labs/lab-08-users-and-groups/evidence && sha256sum -c evidence-checksums.sha256
```
