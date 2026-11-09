# Assignment 3 — Threat-Informed ATT&CK Lab

This workspace now follows the revised Assignment 3 brief. The original workspace contained empty placeholders, so no lab execution, host versions, screenshots, or authentic telemetry were present to compare or preserve.

## Current status

- Research draft: Scattered Spider, using its MITRE ATT&CK group page and the CISA joint advisory.
- Mapping draft: four distinct, source-supported techniques spanning multiple tactics.
- Lab completed: disposable Ubuntu Desktop VM in VMware Workstation, with a benign Linux process-discovery test (T1057), native Linux Audit (`auditd`) telemetry, and the temporary rule removed after collection.
- Evidence archive: [13-assignment3-evidence.zip](13-assignment3-evidence.zip). It contains the supplied raw audit export, four screenshots, and evidence README. Final event 519 matches the test screenshot and is shown in its own screenshot.
- The pre-test rollback snapshot is documented as `A3-clean-before-test`, created 2026-11-09 at 12:38:02 PM Eastern Time (US & Canada, UTC-05:00).
- The report is intentionally absent at the user's request.

The group ID is **13**. The lab used an isolated, non-production Ubuntu VM with its network disconnected for the test. The temporary audit rule was removed and verified absent after evidence collection.

## Files

- [Threat profile](research/threat-profile.md)
- [ATT&CK mapping research](research/attack-mapping.md)
- [Source register](references/sources.md)
- [Lab setup and safety](lab/setup.md)
- [Benign test procedure](lab/test-procedure.md)

## AI-use statement and contributions

**AI assistance:** An AI assistant helped organize the assignment materials; draft the threat profile, ATT&CK mapping rationales, and control recommendations; prepare the safe Linux/auditd procedure; and interpret and package the user-provided lab evidence. The AI did not run commands inside the Ubuntu VM or generate, alter, or fabricate the raw logs, screenshots, test output, or VMware snapshot. The group member independently installed and operated the VM, ran the pilot and final benign test, captured and supplied the terminal screenshots and raw audit export, confirmed the snapshot details, and removed the temporary audit rule. The member should independently verify the cited MITRE/CISA passages and mapping rationales against the listed sources before submission.

**Joshua Andre — McGill ID 261275243:** Created and configured the disposable Ubuntu VMware lab, installed and checked auditd, disconnected the VM network, and took the pre-test rollback snapshot. Ran the harmless pilot and T1057 process-discovery test, captured the authentic terminal screenshots and raw audit export, identified event 519 and matched its fields to the command/time/user, then removed the temporary audit rule.

**Ryan Young — McGill ID 260152636:** Will independently verify Joshua's source passages, ATT&CK mappings, and lab evidence. Will contribute to preparing the final assignment report and check that its claims agree with the submitted evidence.

**Daniel Henriques da Silva — McGill ID 261323142:** Will independently verify Joshua's source passages, ATT&CK mappings, and lab evidence. Will contribute to preparing the final assignment report and check that its claims agree with the submitted evidence.

## Before submission

1. Write the separate report PDF when ready; the report is not included in this repository yet. Add member names/IDs and adjust contribution statements for every group member.
2. Verify the source passages and mappings, then include the required mapping/evidence tables, two control recommendations, one remaining gap, and the AI-use statement in the report.
3. Keep the evidence ZIP and raw logs unchanged except for documented redactions in a separate submission copy. One group member submits the PDF and ZIP.
