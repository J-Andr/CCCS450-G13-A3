# Benign lab test — T1057 Process Discovery

This is one harmless, local process-listing test. It is **lab-only** and is not an additional Scattered Spider research mapping.

## Expected behavior

The `ps` utility will read the local process table and print process ID, parent process ID, user ID, and command name. The host audit source should record execution of the `ps` binary, its execution identity, timestamp, and command-line metadata if supported. The audit event is not expected to contain the printed process list.

## Preconditions

Complete [lab setup](setup.md), record exact VM/tool versions, verify the `ps` path is `/usr/bin/ps`, confirm auditd is active, and verify authorization/isolation and rollback. Group ID 13 is confirmed.

## Add a temporary, narrowly scoped rule

```sh
sudo auditctl -a always,exit -F arch=b64 -S execve -F exe=/usr/bin/ps -k cccs450_a3_t1057
sudo auditctl -l
```

Confirm the listed rule matches exactly. If it is rejected or absent, stop and resolve the configuration before running either the pilot or final test.

## Pilot: prove capture before the final test

Run this harmless pilot:

```sh
ps -p 1 -o pid,comm
sudo ausearch --input-logs -k cccs450_a3_t1057 --raw
```

The pilot was verified: raw event serial `379` at `2026-11-09 12:41:37.228 EST` records `execve` of `/usr/bin/ps` with arguments `ps -p 1 -o pid,comm` under `labvul` (UID/AUID 1000). Preserve the original output. If repeating the pilot later, confirm its new event similarly. Do not treat the pilot as the final test.

## Final run and visible evidence

Start a terminal recording or screenshot capture that shows the command and its output. Display the required marker beside the terminal timestamp, and include user and host in the captured terminal:

```sh
printf 'CCCS450-13-A3 | %s | user=%s | host=%s\n' \
  "$(date '+%Y-%m-%dT%H:%M:%S%:z')" "$(id -un)" "$(hostname)"
```

Keep that printed line visible in the screenshot, then run the exact final command:

```sh
ps -eo pid,ppid,uid,comm --sort=pid
```

Keep the visible output in the screenshot/terminal evidence. Do not include real usernames or unrelated personal data in a submission image.

The first final run was captured in raw audit event serial `384` at `2026-11-09 12:42:58.433 EST`. A supplemental screenshot sequence shows a repeat run with the marker/time/identity line visibly printed, followed by the exact `ps` command and process output, at `2026-11-09T13:37:04-05:00`. The fresh raw export matches this run to event serial `519` at `2026-11-09 13:37:04.166 EST`, with `EXECVE` arguments `ps -eo pid,ppid,uid,comm --sort=pid`, `exe=/usr/bin/ps`, and `uid/auid=1000` (`labvul`). Use serial `519` as the final evidence row; distinguish it from pilot serial `379` and prior final serial `384`.

## Export and identify the raw event

Export the raw audit records for the unique key without editing them:

```sh
sudo ausearch --input-logs -k cccs450_a3_t1057 --raw > auditd-cccs450-a3-t1057.raw
```

Keep this original file securely. Identify the final-test event using its `msg=audit(timestamp:serial)` value and correlate it with the displayed local time, user, host, and exact `ps` command. The export can include the pilot and final invocation; identify which serial corresponds to the final command. Record any time-zone conversion used, rather than changing the raw event.

Look for available records/fields such as `SYSCALL` (`success`, `syscall`, `pid`, `ppid`, `uid`, `auid`, `euid`, `comm`, `exe`, `key`), `EXECVE`/`PROCTITLE` (arguments), and the shared audit timestamp/serial. Availability varies by OS/audit configuration; report only fields that are actually present. `ppid` identifies a numeric parent PID, not necessarily its executable or process lineage. If the event does not include expected command-line information, state that it was unavailable; do not infer it from the terminal alone.

## Cleanup

After confirming the raw export is readable and preserved, remove the temporary rule:

```sh
sudo auditctl -d always,exit -F arch=b64 -S execve -F exe=/usr/bin/ps -k cccs450_a3_t1057
sudo auditctl -l
```

Verify the rule is gone. Do not delete the raw evidence or clear audit logs. Restore the VM snapshot if required by the course or your rollback plan.

**Cleanup verification:** Completed. The user removed key `cccs450_a3_t1057`; `sudo auditctl -l` returned `No rules`.