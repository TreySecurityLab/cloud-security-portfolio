## Project Summary

This project documents a controlled Linux PAM authentication investigation on an Ubuntu Server. The work focused on PAM service configuration, authentication and account-management stacks, module control behavior, effective authentication testing, deliberate authentication bypass analysis, remediation, and SHA-256 evidence integrity.

The scenario used a controlled `lab10-pam-user` identity and an isolated `/etc/pam.d/lab10-auth` service so authentication behavior could be tested without modifying SSH, sudo, login, or Ubuntu's shared production authentication stack. A deliberately unsafe `auth sufficient pam_permit.so` rule was shown to bypass later `pam_unix.so` password validation, then removed and replaced with required Unix authentication and account checks that were verified with both successful and failed credential tests.

## Environment

| System | Role |
| --- | --- |
| Ubuntu Server | Linux PAM authentication investigation target |

## Investigation Scenario

The investigation was designed to answer five practical questions:

1. How is Ubuntu's existing PAM authentication and account-management policy structured?
2. Can an early `sufficient` rule using `pam_permit.so` bypass later password validation in an isolated PAM service?
3. Does effective authentication testing confirm the impact of the deliberately unsafe module ordering?
4. Can the PAM service be remediated so valid credentials succeed while invalid credentials are denied?
5. Do account-management testing, cleanup verification, and SHA-256 checks confirm the final controlled state?

## Investigation Workflow

1. Created a dedicated Lab 10 evidence workspace and recorded the Ubuntu system context.
2. Inspected active rules in `/etc/pam.d/common-auth` and `/etc/pam.d/common-account` without modifying the shared production PAM stack.
3. Verified `pamtester` availability and created the controlled `lab10-pam-user` identity with a lab-only password.
4. Created the isolated `/etc/pam.d/lab10-auth` service with a deliberately unsafe `auth sufficient pam_permit.so` rule before `pam_unix.so`.
5. Used `pamtester` to demonstrate that the vulnerable stack returned authentication success without normal password validation.
6. Removed the bypass and replaced the authentication and account rules with `required pam_unix.so`.
7. Performed a positive authentication test confirming the valid lab password succeeded.
8. Performed a negative authentication test confirming an incorrect password was rejected.
9. Tested the PAM account-management phase separately and recorded the final security assessment.
10. Removed the experimental PAM service and controlled user, then verified all retained evidence with the SHA-256 checksum manifest.

## Key Findings

- PAM policy must be evaluated as control flow rather than as a simple list of module names.
- An early `auth sufficient pam_permit.so` rule allowed the isolated service to return success before `pam_unix.so` performed password validation.
- The presence of `pam_unix.so` in a PAM stack did not by itself prove that password authentication was effectively enforced.
- Replacing the bypass with `auth required pam_unix.so` made local password validation mandatory for the isolated authentication request.
- The remediated policy accepted the correct lab-only password and rejected an incorrect password.
- The separate `account required pam_unix.so` test demonstrated that PAM account management is distinct from credential authentication.
- Keeping the experiment in `/etc/pam.d/lab10-auth` avoided changing SSH, sudo, login, or Ubuntu's shared authentication policy.
- Cleanup verification confirmed the experimental PAM service and controlled account were removed after testing.
- Final checksum verification confirmed that every evidence artifact listed in `evidence-checksums.sha256` remained unchanged after collection.

## Selected Commands

The concise command reference is available in [`commands.md`](commands.md). It contains the commands that best demonstrate PAM policy inspection, isolated service creation, controlled identity setup, vulnerable authentication testing, remediation, positive and negative `pamtester` validation, account-management testing, cleanup verification, and SHA-256 evidence validation.

## Skills Demonstrated

Linux PAM architecture analysis, `/etc/pam.d/` policy inspection, PAM management-group interpretation, control-flag analysis, `pam_permit.so` risk evaluation, `pam_unix.so` authentication, isolated PAM service design, `pamtester` validation, authentication-bypass analysis, positive and negative credential testing, account-management testing, controlled remediation, cleanup verification, evidence documentation, and SHA-256 integrity verification.

## Security Relevance

Cloud IAM and network controls determine who can reach a Linux workload, while PAM can determine whether an identity is actually authenticated and whether the account is permitted to continue. Secure host authentication therefore depends on effective PAM control flow, not merely on the presence of strong modules somewhere in the configuration. Security engineers must understand module order, control behavior, authentication versus account management, safe testing boundaries, and positive and negative validation so that a weak rule cannot silently bypass otherwise correct password controls on a cloud server.
