CCCS450 Group 13 — Assignment 3 Lab Evidence

Status
------
This archive contains the supplied, unmodified raw audit export and original
terminal screenshots. The screenshots show the final T1057 process-discovery
run, the matching raw event 519, and an earlier pilot audit event. No report
is included.

Lab setup
---------
- Guest: Ubuntu Desktop 26.04.1 LTS, x86_64
- Kernel: Linux 7.0.0-43-generic
- Hostname: labvul-VMware-Virtual-Platform
- User: labvul (UID/AUID 1000)
- Time zone: America/Toronto (EST, UTC-05:00)
- Collection: Linux Audit (auditd), active; audit enabled=1, lost=0,
  backlog=0 at the recorded pre-test check
- Tool versions recorded: auditd/auditctl/ausearch 4.1.2, procps-ng 4.0.4,
  Bash 5.3.9
- The user confirmed the VMware network adapter was disconnected for the test
  and a pre-test rollback snapshot was taken.
- Rollback snapshot: `A3-clean-before-test`, created 2026-11-09 at
  12:38:02 PM (VMware Snapshot Manager local display; Windows host time zone
  Eastern Time (US & Canada), UTC-05:00, confirmed by the user).

Collection settings and exact commands
--------------------------------------
Temporary rule:
  sudo auditctl -a always,exit -F arch=b64 -S execve \
    -F exe=/usr/bin/ps -k cccs450_a3_t1057

Harmless pilot:
  ps -p 1 -o pid,comm

Final test (T1057 Process Discovery; lab-only):
  ps -eo pid,ppid,uid,comm --sort=pid

Marker/time/identity line shown in final screenshot:
  CCCS450-13-A3 | 2026-11-09T13:37:04-05:00 |
  user=labvul | host=labvul-VMware-Virtual-Platform

Raw collection command:
  sudo ausearch --input-logs -k cccs450_a3_t1057 --raw

Event locator and interpretation
--------------------------------
The raw export is auditd-cccs450-a3-t1057.raw. Search for:
  msg=audit(1794249424.166:519)

Serial 519 is the final command event at 2026-11-09 13:37:04.166 EST
(UTC-05:00), or 2026-11-09 18:37:04.166 UTC. Its SYSCALL record reports
successful execve, pid=9026, ppid=4341, auid=1000, uid=1000, comm="ps",
exe="/usr/bin/ps", key="cccs450_a3_t1057". The EXECVE record contains:
  ps -eo pid,ppid,uid,comm --sort=pid

Pilot event serial 379 at 2026-11-09 12:41:37.228 EST records
  ps -p 1 -o pid,comm
and is distinct from the final test. Earlier final run serial 384 is also
present in the export; serial 519 matches the later screenshot.

The host event supports that the process was executed with these arguments
under the recorded identity. It does not contain the terminal marker or the
process-list output and does not establish malicious intent.
Screenshot `03-final-event-serial-519.png` shows the raw event selected by
serial and its SYSCALL/EXECVE/PATH/PROCTITLE records.

Time-zone notes
---------------
Audit event timestamps use Unix epoch seconds. Converted in the guest's
America/Toronto time zone (EST, UTC-05:00) on 2026-11-09:
  1794246097.228 -> 2026-11-09 12:41:37.228 EST (pilot, serial 379)
  1794246178.433 -> 2026-11-09 12:42:58.433 EST (earlier final, serial 384)
  1794249424.166 -> 2026-11-09 13:37:04.166 EST (final, serial 519)

Redactions and integrity
------------------------
No edits or redactions have been made to the supplied raw audit export or
screenshots. The visible account is the fictitious lab account. Keep the
originals unchanged; if redaction becomes necessary, make a separately
identified submission copy and document the redaction.

Rollback and cleanup
--------------------
The VMware Snapshot Manager screenshot identifies snapshot
`A3-clean-before-test`, created 2026-11-09 at 12:38:02 PM in the confirmed
Windows host time zone Eastern Time (US & Canada), UTC-05:00. The virtual
network was disconnected for the test. After exporting the evidence, the
temporary rule was removed and `sudo auditctl -l` returned `No rules`.
No audit records were cleared. Do not restore the snapshot until the evidence
archive has been safely retained.

Contents
--------
- auditd-cccs450-a3-t1057.raw: supplied raw audit export
- screenshots/: original screenshots, unaltered
  - 01-final-test-marker-command-output.png: final run and visible output
  - 02-pilot-event-serial-379.png: pilot event (not the final event)
  - 03-final-event-serial-519.png: terminal view of matching final raw event
  - 04-vmware-rollback-snapshot.png: VMware Snapshot Manager details
