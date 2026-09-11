# Windows Process Creation (4688) Visibility Gap

Environment: Ubuntu Wazuh manager, Windows 10 endpoint with Sysmon,
Atomic Red Team for technique execution.

## Background

Testing MITRE ATT&CK T1082 (System Information Discovery) against the lab
to see what actually gets caught vs what a default install misses. Ran it
through Atomic Red Team, then checked Wazuh.

## What happened

Sysmon picked up the process creation fine (Event ID 1). No alert fired
though — expected, since a `systeminfo` command looks exactly like normal
admin activity with no context. Made a note of that and went to compare it
against Windows' own 4688 process creation events.

4688 showed nothing. Not "no alert" — nothing at all. That's what I
actually spent the session chasing.

## Digging in

Isolated it by checking Windows-side and Wazuh-side separately at each
step instead of guessing:

1. **Audit Process Creation wasn't enabled.** Windows wasn't generating
   4688 in the first place. Fixed with
   `auditpol /set /subcategory:"Process Creation" /success:enable`.

2. **Legacy audit policy was overriding it anyway.** Even after enabling
   the subcategory, an older audit policy setting silently took
   precedence. Needed `SCENoApplyLegacyAuditPolicy` set to `1` in the
   registry, plus a full reboot — a policy refresh alone doesn't apply it.


3. **No default Wazuh rule matches plain 4688.** This was the actual
   blocker, even after Windows was logging correctly. The event reached
   Wazuh and got decoded, but with nothing to match it against, it never
   became an alert. Wrote a custom rule for it (see `detection-rules/`).

4. **Command line was still empty** once alerts started firing. Turns out
   "Audit Process Creation" only logs that a process ran, not what
   arguments it was launched with — that's a separate policy
   (`ProcessCreationIncludeCmdLine_Enabled`). Set that too.

## Result

`rule.id: 100003` now returns 4688 events with the command line populated.

## Takeaway

Logged and detected aren't the same thing. This box was generating 4688
the whole time it looked "broken" — Wazuh just had nothing to match it
against. Worth checking on any log source, not just this one.
