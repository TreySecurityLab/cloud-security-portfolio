# Sudo, Privilege Delegation, and Least Privilege

## Project Summary

This project documents a controlled Linux sudo and privilege-delegation investigation on an Ubuntu Server. The work focused on effective sudo authorization, dedicated `/etc/sudoers.d/` policy fragments, unrestricted root delegation, least-privilege remediation, positive and negative authorization testing, and SHA-256 evidence integrity.

The scenario used a controlled `lab10-operator` identity that was deliberately granted excessive `NOPASSWD: ALL` authorization even though its documented role required only permission to view SSH service status. The sudoers policy was then remediated to permit only `/usr/bin/systemctl status ssh`, preserving the approved administrative task while denying unrelated root-level commands.

## Environment

| System | Hostname | Username | Role |
|---|---|---|---|
| Ubuntu Server | `ubuntu-server` | `testlab` | Linux sudo and privilege-delegation investigation target |

## Investigation Scenario

The investigation was designed to answer five practical questions:

1. What sudo privileges are currently available to the controlled operator identity?
2. Does unrestricted `NOPASSWD: ALL` authorization exceed the operator's documented business requirement?
3. What security impact results when an identity can execute unrelated commands as root?
4. Can the sudoers policy be reduced to one explicitly approved command without removing required access?
5. Do positive and negative authorization tests confirm the final least-privilege state?

## Investigation Workflow

1. Reviewed the current administrative identity and documented the intended `lab10-operator` role and approved SSH service-status command.
2. Created the controlled noninteractive operator identity and verified its passwd record.
3. Created a dedicated `/etc/sudoers.d/lab10-operator` policy fragment and validated its syntax with `visudo`.
4. Deliberately configured unrestricted passwordless sudo authorization and preserved the resulting effective privileges as evidence.
5. Demonstrated the security impact by validating that the controlled operator could execute an unrelated root-level identity command.
6. Replaced the excessive rule with a narrowly scoped authorization for `/usr/bin/systemctl status ssh` only.
7. Revalidated the sudoers fragment and captured the remediated effective authorization state.
8. Performed a positive test confirming the approved SSH status command remained authorized.
9. Performed a negative test confirming the unrelated `/usr/bin/id` root command was denied.
10. Documented the final privilege assessment and verified the retained evidence with the SHA-256 checksum manifest.

## Key Findings

- `sudo -l` provided the effective sudo authorization view needed to compare actual privilege with the documented requirement.
- `NOPASSWD: ALL` granted the controlled operator unrestricted passwordless root-command execution and exceeded the intended role.
- A dedicated file under `/etc/sudoers.d/` provided a focused policy location that could be validated independently with `visudo -cf`.
- The excessive rule allowed an unrelated root-level `/usr/bin/id` command, demonstrating that broad sudo delegation creates a direct privilege-escalation path.
- Restricting the policy to `/usr/bin/systemctl status ssh` preserved the required administrative task without retaining unrestricted root access.
- Positive testing confirmed the approved SSH service-status command remained available after remediation.
- Negative testing confirmed the unrelated root-level identity command was denied after remediation.
- Effective authorization testing provided stronger validation than inspecting sudoers text alone.
- The final evidence checksum verification confirmed that the retained investigation artifacts had not changed after collection.

## Selected Commands

The concise command reference is available in [`commands.md`](commands.md). It contains the commands that best demonstrate sudo authorization review, controlled identity creation, sudoers validation, excessive-privilege testing, least-privilege remediation, positive and negative authorization testing, and evidence verification.

## Skills Demonstrated

Linux sudo authorization analysis, privilege-delegation review, `sudo -l` interpretation, controlled account creation, `/etc/sudoers.d/` policy management, `visudo` syntax validation, `NOPASSWD` risk analysis, root-impact validation, least-privilege remediation, command-specific sudo delegation, positive and negative authorization testing, privilege assessment, evidence documentation, and SHA-256 integrity verification.

## Security Relevance

Cloud IAM determines which identities can reach a Linux workload, while local sudo policy determines which administrative actions those identities can perform after reaching the operating system. Reliable privilege reviews must compare effective authorization with documented job requirements, validate sudoers syntax, test both permitted and denied behavior, and remove broad delegation that creates unnecessary root-level capability. Least-privilege sudo rules reduce the blast radius of compromised accounts, misconfiguration, and unauthorized administrative activity.
