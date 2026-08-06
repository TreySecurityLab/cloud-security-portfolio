# Linux File System Investigation Report

## Scope

A controlled Linux file-system investigation was performed on the Ubuntu Server. The review focused on file discovery, hidden and recent files, executable permissions, metadata, content type, suspicious command patterns, safe script inspection, file hashing, and evidence integrity.

## Controlled Activity

The `suspicious-app` directory contained normal application files, an executable maintenance script, a hidden cache containing a training token, and a non-executable update script containing a controlled `curl` command to `192.168.50.1`. The update script was inspected as text and was not executed.

## Findings

1. `.app-cache` was identified as a hidden regular file containing controlled training data.
2. `maintenance.sh` was executable with mode `750` and contained expected maintenance content.
3. `update-check.sh` contained a remote download instruction referencing the controlled Kali address.
4. The update script lacked execute permission but still contained instructions that could be interpreted by another program or user.
5. Broad keyword searches produced matches requiring context and false-positive analysis.
6. Metadata, file type, content, permissions, timestamps, and purpose provided stronger conclusions when correlated.
7. SHA-256 hashes were generated for controlled files and published evidence.
8. An empty recent-file search under `/tmp` remained a valid investigative finding when no matching files existed.
9. No suspicious or discovered file was executed.

## Assessment

The observed files were consistent with the controlled lab scenario. In production, suspicious scripts or hidden files would require correlation with process execution, authentication history, audit logs, package provenance, network telemetry, approved deployment activity, and file-origin information before classification or containment.

## Evidence Handling

Only sanitized text artifacts and selected screenshots should be published. Training tokens, passwords, API keys, private keys, personal information, public home addresses, unreviewed logs, and unrelated system data must be removed before upload.
