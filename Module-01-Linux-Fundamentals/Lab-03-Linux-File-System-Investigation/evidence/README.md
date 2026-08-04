# Linux File System Investigation Evidence

Add sanitized command output and investigation artifacts to this directory.

## Recommended Evidence

- `directory-listing.txt`
- `recent-files.txt`
- `hidden-files.txt`
- `executable-files.txt`
- `suspicious-content.txt`
- `suspicious-script-metadata.txt`
- `file-hashes.txt`
- `investigation-findings.txt`
- `tmp-recent-files.txt`
- `evidence-checksums.sha256`

## Evidence Handling

- Preserve original command output when practical.
- Record enough context to explain how each artifact was collected.
- Separate confirmed facts from indicators and assumptions.
- Hash evidence after collection.
- Recheck stored hashes before relying on the evidence.

## Sanitization Requirements

Do not commit passwords, API keys, access tokens, private keys, personal information, public home IP addresses, unsanitized authentication logs, sensitive company information, production secrets, or unreviewed packet captures.
