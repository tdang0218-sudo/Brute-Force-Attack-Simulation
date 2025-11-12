# SOC Playbook: Brute Force Attack

## Purpose: 
- Detect whether a brute-force attack is happening or not and go through the process of incident response. 

## Detection Source
- SIEM alerts (Splunk)
- Query1: index=* host="<name>" EventCode=4625
         | timechart span=1m count
         | fillnull value=0
- Query2: index=* host="<name>" EventCode=4625
         | stats count by Workstation_Name, Account_Name
         | sort =count
- Query3: index=* host="<name>" EventCode=4625
         | stats count by src_ip, _time
         | timechart span=1m count by src_ip
         | fillnull value=0

## Severity/Priority
| Priority | Short name | Criteria (examples) | Immediate actions (within first 15 min) | Who to notify / escalate | ID |
|---------:|------------|----------------------|------------------------------------------|--------------------------|----|
| P1 | Critical | CEO account compromised through brute-force attacks, admin account most likely compromised | Kicked account off all sessions, Password reset, MFA enabled, Account disabled, Isolate affected environments (VPN sessions, endpoints, etc..), collected logs, IP source, and timestamps | SOC Manager, CISO Security Director, CEO, Legal team (if breached) | 8374 |
| P2 | High | Regular employee account compromised via brute-force attack, Suspicious priviledge escalation on non-executive accounts | Kicked account off all sessions, Password reset, MFA enabled, Account disabled, Isolated affected environments (VPN sessions, endpoint, etc..) colleceted logs, IP source, and timestamps | SOC Tier 2, Incident response manager, Affected department manager | 8375 |
| P3 | Low | Multiple failed log in that were spaced out on regular employee account, UserId most likely compromised but password probably still unknown. Minimal or no impact detected| Password reset, review logs, check IPs, document to monitor | SOC Tier 1 | 8376 |

(Ficticious scenario: Three users reported an incident. P1 is the CEO and their account was accessed through brute-force, P2 is a regular user and their account was also accessed through brute force, P3 is a regular user and their account was not accessed, but there were multiple failed attempts (spaced out, not a brute-force attack).


## Initial Triage
- Confirm validity of attacks (check duplicate alerts, false positive).
- Gather account names, workstation names, ips, event codes, logon types, timestamps.
- Determine if any successful login (EventCode=4624) were triggered after brute-force attack.
- Check IPs and see if they're external or internal.
- Assigned incidents based on priority on the table above.

## Containment (immediate)
- Block attacker IPs at firewall.
- Isolate compromised workstation from network.
- Disable/lock accounts.
- Reset password and Enable MFA from compromised accounts.
- Kick compromised accounts out of all possible sessions.

## Investigation
- Splunk: Searched for src_ip, Workstation_Name, and Account_Name across the last 24-72 hours.
- Checked EDR to look for any suspicious activity over the last 24-72 hours, credential dumping, malware execution, or suspicious priviledge execution.
- Used Azure to check for unusual sign-ins (impossible travel, concurrent sessions).

## Eradication
- If malware is found, device will be re-imaged.
- Remove any attacker tools present on device.
- Block attacker IPs via firewall.

## SLA/Response Time
- P1: Acknowledge within 2 minutes, containment initiated within 9 minutes.
- P2: Acknowledge within 7 minutes, containtment initiated within 15 minutes.
- P3: Acknowledge within 10 minutes, containment initiated within 21 minutes.

## Post-Incident/Maintenance
- Will continue to monitor ID 8374 and ID 8375 throughout the next couple days to see if any further actions are required.
- **Deliverables**: timeline, root cause, impacted systems, remediation actions.
