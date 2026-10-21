# 01 - Impossible Travel Login

> Paper playbook, not lab-tested yet. Written from my own study notes.

## Scenario

An employee logged in successfully from Riyadh, Saudi Arabia at 10:45 AM. Three minutes later, another successful login happened from Brazil. Both logins used the correct password.

## Triage questions

- Did both logins succeed on the same account?
- Is MFA enabled on this account, and was it used on both logins? No MFA means higher risk.
- Are the two IPs different? What type is each one: home/office, VPN, proxy, or mobile carrier?
- Is the device or browser the same on both logins?
- Is the user known to be traveling or using a VPN?

## Investigation steps

Note: VPNs and proxies cause most impossible travel alerts, so rule that out early.

1. Pull both login events from the authentication logs (time, IP, location, device, MFA status).
2. Compare the time gap with the distance. Riyadh to Brazil in 3 minutes is physically impossible.
3. Check both IPs: reputation and type (VPN, hosting, residential, mobile).
4. Check whether MFA was used on both logins. If not, the risk is higher.
5. Check what the account did after each login: mailbox rules, file downloads, password or MFA changes.
6. Check whether the Brazil IP also touched other accounts.

## Containment

1. Confirm with the user by phone, not email.
2. If it is not confirmed or looks suspicious: kill all active sessions, reset the password, enable MFA (or review the registered MFA devices), and block the suspicious IP.
3. Monitor the account for further suspicious activity.

## Evidence to keep

- Both login log entries (time, IP, location, device, MFA status)
- My timeline with timestamps
- IP lookup results for both IPs
- The user's answer and how I contacted them
- Actions taken, with timestamps (sessions killed, password reset, MFA enabled, IP blocked)
- Any suspicious activity found after the logins

## Verdict: real or false alarm?

**Likely a false alarm when:**
- A VPN or proxy explains one of the locations
- Mobile carrier IPs with wrong geolocation
- The user confirms both logins

**Likely real when:**
- No MFA on the account
- The Brazil login came from a new device or browser
- The user denies the second login
- Suspicious activity right after the login

**For this scenario:** real if MFA was not used, the Brazil device is new, and the user denies it.

## What the documentation looks like in a real environment
In a real SOC, every alert like this ends up as a ticket with a summary, timeline, findings, actions, evidence, and root cause. I used Claude to create the template and the sample content, so all names, IPs, and details are fictional. See the [sample ticket (PDF)](examples/INC-2026-0412-impossible-travel-ticket.pdf).

## Lab validation

Not tested yet.
