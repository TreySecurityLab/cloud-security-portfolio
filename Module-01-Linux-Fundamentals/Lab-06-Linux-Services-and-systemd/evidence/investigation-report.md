# Linux Services and systemd Investigation Report

## Scope

A controlled systemd service investigation was performed on the Ubuntu Server training system. The review focused on service inventory, runtime and enablement state, effective unit configuration, process attribution, dependencies, journal records, persistence mechanics, service hardening, local network listeners, and evidence integrity.

## Controlled Activity

The primary scenario used:

- The existing SSH service as a known legitimate service for comparison
- A reviewed Bash heartbeat application managed by `lab-heartbeat.service`
- Enablement and restart operations used to observe persistence and PID changes
- A separate challenge using `cloud-metrics-cache.service` and a local Python HTTP listener on TCP port `9195`

All lab-created content was reviewed before execution. Unfamiliar or discovered service commands were inspected without manual execution.

## Findings

1. systemd was PID 1 and managed the service units investigated during the lab.
2. Running, enabled, inactive, disabled, and failed states represented different service conditions and could not be treated as interchangeable.
3. SSH service properties and process metadata connected the unit definition to its active main process and executable context.
4. `systemctl cat` and `FragmentPath` provided effective configuration and unit-file location evidence without manually executing discovered service commands.
5. The controlled heartbeat service ran as `testlab`, produced timestamped PID and user records, and received a new main PID after restart.
6. Enabling the controlled service created a target `.wants` symbolic link, while disabling it removed startup persistence without necessarily stopping the active process.
7. Common systemd unit directories and recently modified administrator-managed files provided useful persistence-investigation leads but were not proof of compromise by themselves.
8. `systemd-analyze security` separated service functionality from service-hardening exposure.
9. The challenge attributed TCP port `9195` to the controlled Python process started by `cloud-metrics-cache.service` and demonstrated the need to validate authorization before containment.

## Assessment

The observed activity was consistent with the controlled Lab 06 scenario. The investigation demonstrated a repeatable method for correlating service descriptions with unit files, startup commands, users, processes, logs, dependencies, enablement links, network listeners, and hardening properties before classifying activity.

In a production investigation, the same findings would require validation against change records, package provenance, workload purpose, administrator activity, cloud-management telemetry, endpoint detections, authentication history, and business authorization before disabling or removing a service.

## Evidence Handling

Only sanitized text evidence and selected required screenshots should be published. Raw journal entries, broad service inventories, process arguments, internal addresses, credentials, tokens, proprietary service names, sensitive paths, and unrelated system information must be removed before upload.
