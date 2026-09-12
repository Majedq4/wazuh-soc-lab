# Phishing Triage Practice

Been working through phishing header analysis on its own, separate from
the lab itself since it doesn't need any of the VM setup, just a
checklist and some OSINT tools. Did an easy case first to get the
process down, then built a harder one specifically to see if I'd still
trust the process when nothing looks obviously wrong.

## Case 1 - the easy one

Fake "your password expires in 2 hours" email, IT department
impersonation. Old news at this point, but worth noting since it's what
the checklist was built around. This one had basically every red flag
stacked on top of each other: SPF, DKIM, and DMARC all failed outright,
the Reply-To pointed to a completely different domain than the From
address (a bigger tell than the From mismatch itself, since no real IT
desk routes replies through some unrelated domain), and the link text
didn't match where it actually pointed. Good for learning the checklist,
not really representative of what a well-run phishing attempt looks
like today. Didn't need much beyond running down the headers to close
this one as TP.

## Case 2 - the one that actually made me think

Vendor contract notification, claiming to be from a supply company
Nortex Energy already works with. This time the checklist came back
clean across the board — SPF pass, DKIM pass, DMARC pass, From /
Return-Path / Reply-To all matched each other, link text matched its
actual destination. Nothing to point at.

The catch turned out to be the domain itself, not anything in the
headers. The email came from `meridian-supplyco.com` — close to, but not,
the vendor's actual domain. All that clean authentication only proves
the mail came from that domain's own mail servers. It says nothing about
whether that domain belongs to the vendor it claims to be. A WHOIS check
is exactly what catches this in a live case — a domain registered days
or weeks ago is one of the strongest single signals for a freshly
stood-up lookalike, even with perfectly valid SPF/DKIM sitting on top
of it.

Honestly, headers alone weren't enough to close this one either way, and
I'd rather say that outright than force a TP/FP call I can't back up.
The real next step lives outside the inbox — confirm the vendor
relationship against actual internal records, then verify by phone using
a number already on file, never one supplied in the email. That's
specifically what defeats this kind of attack, since any "helpful"
contact info baked into a fraudulent email just loops back to the
attacker anyway.

## IOCs (case 2)
- Sending domain: meridian-supplyco.com
- Sending IP: 45.132.192.18
- Message-ID domain: meridian-supplyco.com

## ATT&CK
T1566.002 - Phishing: Spearphishing Link

## What I took from this
The checklist gets you through most phishing emails fine. The cases that
actually matter are the ones where the checklist comes back clean, and
you have to know that clean auth isn't the same as legitimate — and that
some cases genuinely need more than headers before you can call it
either way.

## Resources
- [MITRE ATT&CK T1566.002](https://attack.mitre.org/techniques/T1566/002/)
- [Cloudflare - What is DMARC, DKIM, and SPF](https://www.cloudflare.com/learning/email-security/dmarc-dkim-spf/)
- [VirusTotal](https://www.virustotal.com/)
- [urlscan.io](https://urlscan.io/)
- [AbuseIPDB](https://www.abuseipdb.com/)
- [ICANN WHOIS Lookup](https://lookup.icann.org/)
- [MXToolbox](https://mxtoolbox.com/)
