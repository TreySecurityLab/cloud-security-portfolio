# Lab Documentation Standard — LOCKED

## Purpose

This standard controls the portfolio-facing documentation structure and presentation for every Cloud Security Lab Exercise. It does not control where a lab runs, which systems execute it, network placement, local filesystem paths, or infrastructure readiness.

Do not change, reinterpret, optimize, add, remove, rename, or reorder the required README sections unless the user explicitly authorizes a documentation-format change. Only lab-specific technical content may change.

## Scope Boundary

This standard controls:

- README section names and order;
- the `System` and `Role` format used in the Environment section;
- portfolio-facing summaries, scenarios, workflows, findings, commands, skills, and security relevance;
- screenshot selection and presentation;
- checksum presentation and evidence-integrity statements; and
- publication-ready documentation quality.

The separate **Lab Environment & Execution Standard — Lab 11+** controls execution systems, infrastructure roles, IP/VLAN placement, local paths, evidence collection locations, operational prerequisites, and retrieval source paths. An execution-standard change does not authorize a README-format change, and a documentation-standard change does not authorize an infrastructure or path change.

## Required README Section Order

1. Project Summary
2. Environment
3. Investigation Scenario
4. Investigation Workflow
5. Key Findings
6. Selected Commands
7. Skills Demonstrated
8. Security Relevance

## Project Summary

Summarize the lab-specific investigation, its primary security objective, the controlled scenario, the validation performed, and the final security outcome. Do not reuse another lab's scenario or technical details.

## Environment

Use only `System` and `Role`. Do not publish hostname, username, IP address, secrets, or other unnecessary identifiers in this table.

| System | Role |
| --- | --- |
| Lab-specific system type | Lab-specific role in the investigation |

## Investigation Scenario

Describe the controlled security problem and the practical questions the investigation must answer. Keep the scenario aligned with the canonical lab objective.

## Investigation Workflow

Provide a concise numbered account of the work actually performed. Preserve the order of investigation, validation, remediation, positive testing, negative testing, evidence collection, and integrity verification when those activities apply.

Do not present planned, skipped, or unverified work as completed.

## Key Findings

State the lab-specific technical conclusions supported by retained evidence. Distinguish observed behavior, security impact, remediation results, and verification outcomes.

## Selected Commands

Link to [`commands.md`](commands.md) and briefly describe the command categories it contains. Keep `commands.md` concise and focused on commands that materially demonstrate the investigation.

## Skills Demonstrated

List the lab-specific technical and analytical skills demonstrated by the completed work. Do not inflate the list with tools or capabilities that were not used and verified.

## Security Relevance

Explain how the lab's verified findings relate to practical defensive security, cloud security, system administration, identity, networking, monitoring, incident response, or risk reduction as appropriate to the canonical objective.

## Evidence Presentation Rules

- Include only evidence that directly supports the canonical objective, a meaningful defense-in-depth control, remediation, or verification.
- Captions must state what the evidence demonstrates without exposing sensitive data.
- Do not claim that a control is operational unless execution evidence verifies it.
- Present checksum verification accurately; a checksum confirms retained-file integrity, not the truth of an unsupported operational claim.
- Do not publish passwords, private keys, tokens, domain secrets, public or ISP-assigned WAN information, full MAC addresses, SNMP credentials or community strings, serial numbers, or other sensitive identifiers.

## Change Control

This standard is independently locked from the Lab Environment & Execution Standard — Lab 11+.

Changes to README headings, section order, Environment formatting, evidence presentation, screenshot presentation, checksum presentation, or portfolio-facing writing belong here only. Changes to systems, paths, VLANs, host roles, switch ports, virtualization placement, firewall behavior, evidence collection locations, or retrieval source paths belong only in the execution standard.
