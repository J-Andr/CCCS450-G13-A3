# Assignment 3 — Threat-Informed ATT&CK Lab

This workspace now follows the revised Assignment 3 brief. The original workspace contained empty placeholders, so no lab execution, host versions, screenshots, or authentic telemetry were present to compare or preserve.

## Current status

- Research draft: Scattered Spider, using its MITRE ATT&CK group page and the CISA joint advisory.
- Mapping draft: four distinct, source-supported techniques spanning multiple tactics.
- Lab completed: disposable Ubuntu Desktop VM in VMware Workstation, with a benign Linux process-discovery test (T1057), native Linux Audit (`auditd`) telemetry, and the temporary rule removed after collection.
- Evidence archive draft: [13-assignment3-evidence.zip](13-assignment3-evidence.zip). It contains the supplied raw audit export, terminal screenshots, and evidence README. The raw final event (serial 519) matches the supplemental test screenshot.
- **Still outstanding for the evidence package:** a screenshot showing final event 519 in the terminal, and the actual VMware snapshot name/time (not supplied). The included pilot-event screenshot is identified as pilot evidence, not the final event.
- The report is intentionally absent at the user's request.

The group ID is confirmed as **13**. The chosen guest will be the group's non-production VM. Keep its network disconnected during the test and take a rollback snapshot before testing.

## Files

- [Threat profile](research/threat-profile.md)
- [ATT&CK mapping research](research/attack-mapping.md)
- [Source register](references/sources.md)
- [Lab setup and safety](lab/setup.md)
- [Benign test procedure](lab/test-procedure.md)

## Before submission

1. Confirm group ID, lab track, authorization, isolation, snapshot/rollback, and tool availability.
2. Record exact OS and tool versions; verify the audit source using the pilot in the test procedure before the final test.
3. Perform the one approved, harmless test. Capture the visible command/output and export the corresponding unaltered raw audit records.
4. Add the final-event screenshot and snapshot name/time to the evidence package. Write the report separately when requested. Redact secrets/personal data only in submission copies; retain originals securely.
5. Independently check the source passages, mappings, and every reported event field. Disclose permitted AI assistance and what you verified.
