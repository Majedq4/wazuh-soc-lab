# Phishing Triage Practice

Been working through phishing header analysis on its own, separate from
the lab itself since it doesn't need any of the VM setup, just a
checklist and some OSINT tools. Did an easy case first to get the
process down, then built a harder one specifically to see if I'd still
trust the process when nothing looks obviously wrong.

## Case 1 - the easy one

Fake "your password expires in 2 hours" email, IT department
impersonation. Old news at this point but it's what the checklist is
built around. SPF, DKIM, and DMARC all failed outright, Reply-To pointed
to a totally different domain than From (bigger tell than the From
mismatch itself, no real IT desk routes replies through some unrelated
domain), and the link text didn't match where it pointed. Good for
learning the checklist, not really what a well-run phishing attempt
looks like now. Closed as TP without much thought.

## Case 2 - the one that actually made me think

Vendor contract email, claiming to be from a supply company Nortex
Energy already works with. Checklist came back clean this time, SPF/
DKIM/DMARC all pass, From/Return-Path/Reply-To match, link text matches
where it actually goes. Nothing in the headers to flag.

Domain was the actual problem: meridian-supplyco.com, IP 45.132.192.18,
close to but not the vendor's real domain. Clean auth just means the
mail came from that domain's own servers, not that the domain is
legit. WHOIS is what catches this normally, a domain registered a few
days old is a big red flag even with valid SPF/DKIM sitting on top of
it.

Couldn't call TP or FP off headers alone here, so I didn't force it.
Real fix is outside the inbox anyway, confirm the vendor relationship
internally then call a number already on file, not one from the email.

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
