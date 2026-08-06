# Find and Grep Investigation Report

## Scope

A controlled Linux search and threat-hunting exercise was performed on the Ubuntu Server. The review focused on file metadata, logical search expressions, content matching, false-positive reduction, weak-configuration discovery, network-tool references, authentication events, and evidence integrity.

## Controlled Activity

The `search-data` directory contained configuration files, logs, scripts, archived content, uploads, a hidden review note, and an empty file. Controlled weak settings and references to `192.168.50.1` were included for investigation practice. No discovered or untrusted script was executed.

## Findings

1. `application-backup.conf` contained `debug=true`, `remote_access=true`, and a training-only password.
2. `diagnostic.sh` contained a controlled `curl` command to `192.168.50.1`.
3. `connectivity-test.sh` contained an `nc` command referencing TCP port `8080`.
4. `maintenance.sh` was executable and contained a legitimate `rsync` command.
5. `.review-note` was hidden, `empty-upload.tmp` was empty, and `old-debug.log` was older than seven days.
6. A substring search for `nc` could match unrelated words such as `maintenance`.
7. Word-aware, fixed-string, and boundary-aware searches reduced irrelevant matches but did not remove the need for context.
8. Combining `find` metadata filters with targeted `grep` searches produced a repeatable investigation workflow.
9. Configuration values and network tools were indicators requiring validation rather than automatic proof of malicious activity.

## Assessment

The observed results were consistent with the controlled lab scenario. In production, weak settings, embedded credentials, or network-tool references would require correlation with ownership, timestamps, process execution, authentication history, change records, network telemetry, operational purpose, and approved administration before classification or remediation.

## Evidence Handling

Only sanitized text artifacts and selected screenshots should be published. Training passwords, API keys, access tokens, private keys, personal information, public home addresses, unreviewed authentication logs, sensitive company information, and unrelated system data must be removed before upload.
