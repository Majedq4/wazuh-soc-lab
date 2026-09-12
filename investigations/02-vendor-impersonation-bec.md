# Vendor Impersonation / BEC Attempt - Contract Notification

## Scenario Overview
Constructed training scenario (not a live capture), built to practice a
specific pattern: a vendor-impersonation email where every standard
authentication check passes. Used to test whether the investigation
process holds up when the "obvious" checks don't flag anything.

## Method
- Authentication-Results: SPF pass, DKIM pass, DMARC pass — all valid
  for the sending domain
- From / Return-Path / Reply-To: internally consistent, all point to
  the same domain
- Link text vs actual href: matched
- WHOIS check on the sending domain — in a live case, registration age
  here is the strongest single tell for a freshly-registered lookalike;
  days or weeks old is a major red flag even with clean auth

## Why Headers Alone Weren't Enough
Every technical check passed. That only confirms the mail genuinely came
from the claimed domain's own infrastructure — it says nothing about
whether that domain is one the real vendor actually owns. A domain with
one inserted word or a swapped character passes every one of its own
authentication checks while still being entirely fraudulent.

## Verdict
Not resolvable from headers alone. Classified as suspected BEC/vendor
impersonation, pending external verification. Not every case closes to
a clean TP or FP on the first pass — forcing one here would be guessing.

## Action Taken
1. No interaction with the link
2. Compare the sending domain against the vendor's actual domain on
   file in internal records
3. Out-of-band verification: call the vendor's accounts contact using a
   number already on file, never a number or link supplied in the email
   itself
4. If the vendor doesn't recognize the request: report internally,
   block the sending domain, flag other employees who may have received
   the same message

## IOCs
- Sending domain: meridian-supplyco.com
- Sending IP: 45.132.192.18
- Message-ID domain: meridian-supplyco.com

## ATT&CK Mapping
T1566.002 - Phishing: Spearphishing Link (Initial Access)

## Takeaway
Clean authentication rules out direct spoofing, not impersonation. The
verification that actually matters here happens outside the inbox —
known vendor records and an out-of-band phone call — not in the headers.
