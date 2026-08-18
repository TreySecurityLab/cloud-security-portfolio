# Lab Documentation Standard

## Purpose

This standard defines the required structure, presentation, evidence handling, and documentation rules for all cybersecurity labs in this repository. It is derived from the approved Lab 10 baseline and is intentionally separate from the environment and execution rules that begin with Lab 11.

## Authority

This standard is locked. Do not alter its structure, required sections, evidence expectations, or formatting rules unless the user explicitly authorizes the change.

The governing hierarchy is:

1. Authoritative Curriculum
2. Lab Documentation Standard
3. Lab Environment & Execution Standard — Lab 11+
4. Individual Lab

An individual lab may add lab-specific content, but it must not silently redefine a higher-level standard.

## Required README Structure

Each lab README must preserve the approved documentation structure and include the following sections where applicable to the lab:

- Project Summary
- Environment
- Investigation Scenario
- Investigation Workflow
- Key Findings
- Selected Commands
- Troubleshooting
- Evidence / validation material

## Environment Section

The Environment section must show only:

- **System**
- **Role**

Do not include hostname, username, or IP address in the README Environment section unless a later explicit change is authorized.

## Commands and Verification

- Provide exact commands used during the lab.
- Keep expected output or verification guidance directly associated with the command that produces it.
- Do not skip verification steps.
- When a command is expected to change system state, include a follow-up verification command when appropriate.
- Preserve commands exactly when they are part of the completed lab record.

## Investigation and Findings

- The Investigation Scenario must explain the security or administration problem being addressed.
- The Investigation Workflow must document the sequence of actions actually performed.
- Key Findings must summarize material technical observations, validation results, and security implications.
- Do not fabricate findings that were not observed or verified during the lab.

## Troubleshooting

- Record meaningful errors, unexpected behavior, and corrective actions.
- Preserve troubleshooting details that materially explain how the lab was completed.
- Do not silently remove a failed attempt when it is useful evidence of the troubleshooting process.

## Evidence

- Evidence must correspond to the actual lab activity.
- Evidence retrieval and publication workflows must fail closed when evidence is missing, ambiguous, or from the wrong lab.
- Do not substitute unrelated screenshots or output for missing evidence.
- Evidence filenames should clearly identify the lab and the configuration, command, or validation being demonstrated.

## Screenshots and Redaction

When screenshots are required:

- Capture the configuration or result that proves the stated task was completed.
- Frame screenshots so the relevant setting, command output, status, or rule is visible.
- Redact credentials, secrets, API keys, private keys, recovery codes, and other sensitive material.
- Redact public addresses or personally identifying details when they are not necessary to demonstrate the lab objective.
- Do not redact lab-specific RFC1918 addressing or other architectural details when those details are intentional portfolio evidence.

## Scope

This document controls **how labs are documented**. It does not control where labs run, which host or VLAN is used, where local lab directories are stored, or how home-lab infrastructure is incorporated. Those requirements are defined separately by `Lab-Environment-and-Execution-Standard-Lab-11-Plus.md`.
