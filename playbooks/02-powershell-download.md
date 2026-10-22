# 02 - PowerShell Download

> Paper playbook, not lab-tested yet. Written from my own study notes.

## Scenario

Endpoint Detection & Response (EDR) one of the endpoints in a company generated an alert. PowerShell downloaded a file from an unknown website and executed it. The user did not report anything.

## Triage questions

- Which username and computer?
- Is it normal hour timeline for working? if not, it is highly risk
- Is the user download a third party tool for his/her tasks? Check by calling him.
- Does this user has high privilege? if yes, it is more risky.
- did EDR block it or did it run?
- Is the file still on disk? 
- Workstation or server?

## Investigation steps


1. Check the alert details (user, hostname, timeline, PowerShell command, IP address, URL)
2. Analyze the PowerShell command (Look for encode commands, suspicious parameters like Invoke-WebRequest, IEX, -EncodedCommand, or downloads from uknown domains)
3. Check the Parent Process (Identify which process started PowerShell (e.g., cmd.exe, wscript.exe, winword.exe(most likely phishing)). This will show the execution chain)
4. Inspect the Downloaded File (Check the file path, type, size, and calculate the file hash. Analyze it in threat intelligence tools or a sandbox if I have time)
5. Check Network Connections (Look for connections to external IP addresses or malicious domains around the same time)
6. Correlate Logs and Escalate (Review other logs like Defender, proxy, DNS, and firewall logs. If malicious, isolate the endpoint and escalate to L2 / Incident Responder)

## Containment

1. Confirm first if it is normal time, and then confirm by user if possible.
2. if it is not confirmed or look suspicious: isolate the host with EDR, kill the process, block the URL, IP, and file hash, and reset the user’s password if credentials may be exposed.
3. Monitor the host and user for further suspicious activity

## Evidence to keep

- The log details
- File hash, the full command line, and process tree screenshot.
- My response timeline
- My contact to the affected user
- Actions taken by me 
- Any suspicious activity after the incident
- Threat intelligence tools results

## Verdict: real or false alarm?

**Likely a false alarm when:**
- Approved IT script or software deployment

**Likely real when:**
- Encoded command
- Suspicious URL and IP
- Malicious file after checking its details using multiple tools

**For this scenario:** With the clear evidence and the threat intelligence tools results about the malicious file and URL, most likely malicious.

## What the documentation looks like in a real environment
In a real SOC, as I said in the first scenario, every alert like this ends up as a ticket with a summary, timeline, findings, actions, evidence, and root cause. I used same template and the sample content, so all names, IPs, and details are fictional. See the [sample ticket (PDF)](examples/INC-2026-0413-powershell-download-ticket.pdf).

## Lab validation

Not tested yet.
