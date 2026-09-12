# Wazuh SOC Lab

Home SOC lab built with Wazuh, used to practice detection engineering and
incident investigation the way I'd actually do it on the job, not just
following a tutorial.

## Setup

- Ubuntu server running Wazuh manager
- Windows 10 VM as the monitored endpoint, with Sysmon installed
- Atomic Red Team to safely run real MITRE ATT&CK techniques and see what
  the setup actually catches

## What's in here

- `detection-rules/` — custom Wazuh rules I wrote by hand when the default
  ruleset didn't catch something it should have
- `investigations/` — write-ups of what I found, how I found it, and what
  I did about it

## Investigations

- [Windows Process Creation (4688) Visibility Gap](investigations/01-windows-4688-visibility-gap.md) —
  ran a MITRE discovery technique, expected it to show up in Wazuh, it
  didn't. Took some digging to figure out why.
- [Phishing Triage Practice](investigations/02-phishing-triage-practice.md) — checklist walkthrough on an easy case, then a harder one where every technical check actually passes

More write-ups get added as I keep working through this lab.
