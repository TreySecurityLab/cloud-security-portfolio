# Security Baseline Investigation Report

## Scope

A known-good security baseline was established for the Ubuntu Server and compared with network observations collected from Kali Linux. The review focused on system identity, addressing, listening sockets, expected services, host-firewall policy, remote reachability, authentication activity, and evidence integrity.

## Systems Reviewed

- Kali Linux `kali-attacker` at `192.168.50.1` as the remote scanner
- Ubuntu Server `ubuntu-server` at `192.168.50.129` as the baseline target

## Findings

1. The Ubuntu Server hostname, user context, and host-only address matched the expected lab configuration.
2. SSH and Apache were expected services on the target system.
3. Local listening sockets and remotely reachable ports provided related but different views of exposure.
4. UFW policy and network placement affected whether a locally listening service could be reached from Kali Linux.
5. Service activity did not by itself prove that a service was configured to start automatically at boot.
6. Authentication-log review provided evidence of accepted, failed, or invalid-user events requiring analyst interpretation.
7. The collected information established a baseline suitable for later comparison.

## Assessment

The observed configuration was consistent with the controlled lab design. No conclusion about compromise should be based on an open port, running service, or authentication event alone. Production validation would also require approved-service inventories, firewall and security-group policy, change records, vulnerability data, identity telemetry, and business context.

## Evidence Handling

Only sanitized baseline artifacts and selected screenshots should be published. Passwords, private keys, tokens, sensitive usernames, public home addresses, unsanitized authentication logs, and unrelated system data must be removed before upload.
