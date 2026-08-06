# Find and Grep Evidence

Add sanitized investigation artifacts to this directory.

## Recommended Evidence

- `file-inventory.txt`
- `configuration-files.txt`
- `hidden-and-empty-files.txt`
- `executable-files.txt`
- `older-files.txt`
- `risky-configurations.txt`
- `network-tool-matches.txt`
- `failed-authentication-events.txt`
- `ip-address-matches.txt`
- `investigation-findings.txt`
- `challenge-results.txt`
- `challenge-ip-matches.txt`
- `evidence-checksums.sha256`

## Evidence Handling

- Preserve original command output when practical.
- Record enough context to explain each artifact.
- Separate confirmed facts, suspicious indicators, false positives, and unknowns.
- Hash evidence after collection.
- Recreate the checksum list after changing any evidence file.
- Verify checksums before relying on the evidence.

## Sanitization Requirements

Do not commit passwords, API keys, access tokens, private keys, personal information, public home IP addresses, unsanitized authentication logs, sensitive company information, production secrets, or unreviewed packet captures.
