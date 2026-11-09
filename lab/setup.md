# Lab setup, authorization, and safety

## Selected track and current status

Linux host process auditing with native Linux Audit (`auditd`), testing **T1057 — Process Discovery**. The selected guest is Ubuntu Desktop 26.04.1 LTS in VMware Workstation 26.0.1. The host WSL environment is not the lab target. The Ubuntu ISO was downloaded from the official release site and its SHA-256 verified against Ubuntu's published `SHA256SUMS`:

- ISO: <https://releases.ubuntu.com/26.04/ubuntu-26.04.1-desktop-amd64.iso>
- SHA-256: `601e30fbf5d97759367c632e2c33630665039b7e2158fd068403da3ccf1bda1f`
- Local ISO path: `C:\Users\Joshua Andre\Downloads\CCCS450-G13-A3-Lab\ubuntu-26.04.1-desktop-amd64.iso`

**Status:** Ubuntu is installed and auditd is active. The user confirmed the VMware network adapter was disconnected for the test. VMware Snapshot Manager shows the pre-test rollback snapshot `A3-clean-before-test`, created **2026-11-09 12:38:02 PM** (local display; America/Toronto, EST, UTC-05:00). The pilot and final benign test were run. The supplemental screenshot and fresh raw export corroborate the final 13:37 run (event serial 519). The user removed the temporary audit rule and verified `sudo auditctl -l` returned `No rules`.

### Verified guest details

Captured in the Ubuntu guest terminal on 2026-11-09:

- OS: Ubuntu 26.04.1 LTS (Resolute Raccoon)
- Kernel/architecture: Linux 7.0.0-43-generic, x86_64
- Hostname: `labvul-VMware-Virtual-Platform`
- User: `labvul` (UID 1000; member of `sudo`)
- Time zone: `America/Toronto` (EST, UTC-05:00); clock synchronized
- Audit service: `active`
- Audit status: enabled (`1`), lost events `0`, backlog `0`
- `ausearch`: version 4.1.2
- `ps`: procps-ng 4.0.4, resolved path `/usr/bin/ps`
- Bash: 5.3.9
- `auditctl`: version 4.1.2, from `sudo auditctl -v`; the command also printed a parameter-processing error, so record the full output rather than treating it as a clean version check.

### Pilot capture verification

The user supplied genuine guest output for the pilot `ps -p 1 -o pid,comm`, which printed PID 1 as `systemd`. The matching raw audit event is `msg=audit(1794246097.228:379)` (serial **379**), corresponding to **2026-11-09 12:41:37.228 EST (UTC-05:00)** / **17:41:37.228 UTC**:

- `type=SYSCALL`: `syscall=59` (`execve` on this x86_64 guest), `success=yes`, `pid=4428`, `ppid=4341`, `auid=1000`, `uid=1000`, `comm="ps"`, `exe="/usr/bin/ps"`, key `cccs450_a3_t1057`.
- `type=EXECVE`: `argc=5`, arguments `ps -p 1 -o pid,comm`.
- `type=PATH`: executable path `/usr/bin/ps`.
- `type=PROCTITLE`: command arguments are represented in hex.

This verifies the configured audit source captured the harmless pilot with executable, arguments, timestamp/serial, and user identity. The pilot is not the final test; preserve its raw output and identify the final event separately.

### Final test event (user-provided raw export)

The provided raw file `auditd-cccs450-a3-t1057.raw` includes a separate final command event, serial **384**, at `msg=audit(1794246178.433:384)` = **2026-11-09 12:42:58.433 EST (UTC-05:00)** / **17:42:58.433 UTC**:

- `type=SYSCALL`: `syscall=59` (`execve` on this x86_64 guest), `success=yes`, `pid=4452`, `ppid=4341`, `auid=1000`, `uid=1000`, `comm="ps"`, `exe="/usr/bin/ps"`, key `cccs450_a3_t1057`.
- `type=EXECVE`: `argc=4`, arguments `ps -eo pid,ppid,uid,comm --sort=pid`.
- `type=PATH`: executable path `/usr/bin/ps`.
- `type=PROCTITLE`: command arguments in hex.

This authentic raw event supports execution of the recorded process-discovery command under the `labvul` account. The original screenshot showed the 12:42 command and output; the supplemental screenshot sequence shows `CCCS450-13-A3 | 2026-11-09T13:37:04-05:00 | user=labvul | host=labvul-VMware-Virtual-Platform` followed by `ps -eo pid,ppid,uid,comm --sort=pid` and its output. The marker is not expected inside the host event.

The fresh raw export includes final-run event `msg=audit(1794249424.166:519)`, serial **519**, corresponding to **2026-11-09 13:37:04.166 EST (UTC-05:00)** / **18:37:04.166 UTC**:

- `type=SYSCALL`: `syscall=59` (`execve` on this x86_64 guest), `success=yes`, `pid=9026`, `ppid=4341`, `auid=1000`, `uid=1000`, `comm="ps"`, `exe="/usr/bin/ps"`, key `cccs450_a3_t1057`.
- `type=EXECVE`: `argc=4`, arguments `ps -eo pid,ppid,uid,comm --sort=pid`.
- `type=PATH`: executable path `/usr/bin/ps`.
- `type=PROCTITLE`: command arguments in hex.

The screenshot time rounds to 13:37:04; the audit event provides milliseconds. Together, the matching timestamp, account, executable, arguments, and visible output support the evidence chain for the local T1057 process-discovery test. The raw event does not contain the terminal marker or process-list output; those are shown in the screenshot.

Recommended VM settings:

- Name: `CCCS450-G13-A3-Ubuntu`
- Guest OS: Linux, Ubuntu 64-bit
- Memory: 8 GiB; processors: 4 cores
- Virtual disk: 40 GiB, dynamically allocated
- Network: NAT only for OS/package installation; disconnect the virtual network adapter before the pilot/final test
- Disable shared folders, drag-and-drop, and copy/paste integration with the host

Use the group-owned VM only for this non-production lab. Record the VM snapshot name/time after installing updates and auditd, then take/retain a clean rollback point before testing.

## Authorization and rollback gate

Before any test:

- Confirm the VM belongs to your group and is non-production. Do not expose it to public or production systems; disconnect its virtual network adapter for the test.
- Use only fictitious accounts and dummy data. Do not use malware, exploit code, credential-dumping tools, destructive commands, or attack frameworks.
- Record the VM snapshot name/time or another verified rollback plan.
- Confirm that auditd is already installed/configured or install it from the approved OS repository **before** the test. Do not test until the pilot proves the chosen log source records the command.
- Stop if the VM, audit service, or security tooling behaves unexpectedly.

## Record exact versions before testing

Save the unedited output (or transcribe it accurately) in your lab notes:

```sh
cat /etc/os-release
uname -a
auditctl -v
ausearch --version
ps --version
bash --version
```

Also record the VM hostname, current user, local time zone, and whether the VM is a disposable snapshot. Do not substitute versions from the host running this repository for versions from the lab VM.

## Confirm the audit source and rule target

Run on the isolated Linux VM:

```sh
systemctl is-active auditd
sudo auditctl -s
command -v ps
readlink -f "$(command -v ps)"
```

The test procedure below assumes the resolved `ps` executable is `/usr/bin/ps` and the VM is a 64-bit Linux system using auditd. If either assumption is false, stop and adjust the documented rule and exact test command to the verified host; do not claim the rule captures the test until the pilot is found in raw audit records. If audit rules are immutable or auditd is unavailable, use an already-working, instructor-approved host log source instead of disabling protections.

## Temporary audit rule

After confirming the resolved executable and before the pilot, add the temporary rule in [the test procedure](test-procedure.md). It records execution of the benign `ps` program; it does not inspect or change other processes. The rule is temporary and should be removed after the final raw evidence has been exported. Do not clear the audit log.

## Evidence handling

Keep the original raw event export unchanged. Make a separate redacted submission copy only if it contains secrets or personal data; do not invent, alter, or silently omit event fields. The assignment requires genuine host telemetry, not a generated example.