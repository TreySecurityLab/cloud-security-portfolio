# File Ownership and Permissions — Selected Commands

This concise reference contains the commands that best represent the Lab 09 ownership and permissions investigation. Every terminal command is displayed on one line.

## Inspect Ownership and Permissions

Displays symbolic mode, octal mode, owner, group, and path for the controlled report:

```bash
stat -c 'Mode=%A Octal=%a Owner=%U Group=%G File=%n' ~/cloud-security-labs/lab-09-file-ownership-and-permissions/scenario/cloud-audit-report.txt
```

Creates the controlled excessive file mode used for the investigation:

```bash
chmod 644 ~/cloud-security-labs/lab-09-file-ownership-and-permissions/scenario/cloud-audit-report.txt
```

Preserves the excessive pre-remediation state:

```bash
stat -c 'Mode=%A Octal=%a Owner=%U Group=%G File=%n' ~/cloud-security-labs/lab-09-file-ownership-and-permissions/scenario/cloud-audit-report.txt | tee ~/cloud-security-labs/lab-09-file-ownership-and-permissions/evidence/permissions-before-remediation.txt
```

## Validate Path Traversal and Effective Access

Displays every pathname component with its ownership and permissions:

```bash
namei -l ~/cloud-security-labs/lab-09-file-ownership-and-permissions/scenario/cloud-audit-report.txt
```

Records the original home-directory mode before the controlled traversal test:

```bash
stat -c '%a' "$HOME" > ~/cloud-security-labs/lab-09-file-ownership-and-permissions/scenario/home-mode-before.txt
```

Temporarily adds only `other` traversal permission for the controlled cross-user test:

```bash
chmod o+x "$HOME"
```

Tests the deliberately excessive file as the controlled service identity:

```bash
sudo -u lab08-service -- cat ~/cloud-security-labs/lab-09-file-ownership-and-permissions/scenario/cloud-audit-report.txt
```

## Remediate Ownership and Permissions

Aligns the controlled report with the intended owner and authorization group:

```bash
sudo chown testlab:cloud-audit ~/cloud-security-labs/lab-09-file-ownership-and-permissions/scenario/cloud-audit-report.txt
```

Applies owner read/write, group read, and no other access:

```bash
chmod 640 ~/cloud-security-labs/lab-09-file-ownership-and-permissions/scenario/cloud-audit-report.txt
```

Verifies that the authorized backup identity retains read access:

```bash
sudo -u lab08-backup -- cat ~/cloud-security-labs/lab-09-file-ownership-and-permissions/scenario/cloud-audit-report.txt
```

Verifies that the unauthorized service identity is denied:

```bash
if sudo -u lab08-service -- cat ~/cloud-security-labs/lab-09-file-ownership-and-permissions/scenario/cloud-audit-report.txt; then echo 'UNEXPECTED: lab08-service can read the protected file'; else echo 'EXPECTED: lab08-service access denied'; fi
```

Restores the exact home-directory mode captured before the controlled traversal test:

```bash
chmod "$(cat ~/cloud-security-labs/lab-09-file-ownership-and-permissions/scenario/home-mode-before.txt)" "$HOME"
```

## Inspect Directory Permissions

Configures the controlled archive directory for owner full access, group read/traverse, and no other access:

```bash
sudo chown testlab:cloud-audit ~/cloud-security-labs/lab-09-file-ownership-and-permissions/scenario/audit-archive && chmod 750 ~/cloud-security-labs/lab-09-file-ownership-and-permissions/scenario/audit-archive
```

Removes group traversal symbolically:

```bash
chmod g-x ~/cloud-security-labs/lab-09-file-ownership-and-permissions/scenario/audit-archive
```

Restores group traversal symbolically:

```bash
chmod g+x ~/cloud-security-labs/lab-09-file-ownership-and-permissions/scenario/audit-archive
```

## Hash and Verify Evidence

Creates the evidence checksum manifest while excluding the manifest itself:

```bash
cd ~/cloud-security-labs/lab-09-file-ownership-and-permissions/evidence && find . -maxdepth 1 -type f ! -name 'evidence-checksums.sha256' -printf '%P\0' | sort -z | xargs -0 -r sha256sum > evidence-checksums.sha256
```

Verifies every evidence artifact recorded inside the manifest:

```bash
cd ~/cloud-security-labs/lab-09-file-ownership-and-permissions/evidence && sha256sum -c evidence-checksums.sha256
```

Confirms separately that the integrity manifest itself exists and is non-empty:

```bash
test -s ~/cloud-security-labs/lab-09-file-ownership-and-permissions/evidence/evidence-checksums.sha256 && echo 'evidence-checksums.sha256: EXISTS AND NON-EMPTY'
```
