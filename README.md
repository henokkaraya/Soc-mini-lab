# 🛡️ SOC Mini Lab – SSH Brute Force Detection & Response

## Overview
This project simulates a realistic SSH brute-force attack against a Linux server and demonstrates how such an incident can be detected, investigated, and mitigated using standard SOC and incident response techniques.

The focus is not exploitation, but **log analysis, incident timeline creation, and defensive controls** such as firewall hardening and Fail2Ban.

---

## Objectives
- Establish a baseline for authentication logs
- Simulate an SSH brute-force attack
- Detect malicious activity via log analysis
- Build an incident timeline
- Apply preventive security controls
- Verify that the attack is successfully mitigated

---

## Lab Environment

### Victim VM
- OS: Ubuntu Server  
- Services:
  - SSH running on **non-standard port 2222**
  - UFW firewall
  - Fail2Ban (sshd jail)

### Attacker VM
- OS: Ubuntu Desktop  
- Tools:
  - `nmap`
  - `ssh`

---

## Project Structure
```text
soc-mini-lab/
├─ evidence/
│  ├─ logs/
│  │  ├─ auth-log-baseline.txt
│  │  ├─ auth-log-incident.txt
│  │  └─ ssh-incident-timeline.txt
│  └─ screenshots/
│     ├─ auth-log-baseline.png
│     ├─ auth-log-incident.png
│     ├─ nmap-ssh-open.png
│     ├─ ssh-bruteforce-attempts-attacker.png
│     ├─ fail2ban-installed.png
│     ├─ fail2ban-sshd-status.png
│     ├─ fail2ban-ip-banned.png
│     └─ ufw-enabled-and-ssh-allowed.png
Phase 1 – Baseline Creation
Before any attack activity, a baseline of /var/log/auth.log was collected to understand normal authentication behavior.

Evidence:

auth-log-baseline.txt

auth-log-baseline.png

This baseline contains:

Legitimate user logins

Cron jobs

No failed SSH authentication attempts

Phase 2 – Reconnaissance
From the attacker machine, a full TCP port scan was performed.

Result:

SSH service discovered on port 2222

Port 22 confirmed closed

Evidence:

nmap-ssh-open.png

Phase 3 – Attack Simulation (SSH Brute Force)
Multiple SSH login attempts were made using an invalid username, intentionally causing authentication failures.

Command used:

bash
Kopiera kod
ssh invaliduser@<victim-ip> -p 2222
Result:

Repeated Permission denied responses

Failed login attempts recorded in auth.log

Evidence:

ssh-bruteforce-attempts-attacker.png

auth-log-incident.txt

auth-log-incident.png

Phase 4 – Incident Detection & Timeline
Relevant log entries were extracted from auth.log to isolate the incident.

Indicators observed:

Failed password for invalid user

Repeated attempts from a single IP address

Short time interval between attempts

An incident timeline was created showing:

Attack start

Number of attempts

Source IP

Targeted service

Evidence:

ssh-incident-timeline.txt

Phase 5 – Mitigation & Hardening
Firewall (UFW)
Default deny incoming traffic

Explicitly allow SSH on port 2222

Evidence:

ufw-enabled-and-ssh-allowed.png

Fail2Ban
Fail2Ban was configured to:

Monitor SSH authentication failures

Ban an IP after 3 failed attempts

Apply a ban duration of 600 seconds

Result:

Attacker IP automatically banned

SSH brute-force attempts blocked

Evidence:

fail2ban-installed.png

fail2ban-sshd-status.png

fail2ban-ip-banned.png

Verification
After mitigation:

SSH attempts from the attacker IP failed immediately

Fail2Ban confirmed the active ban

No new failed attempts logged from the banned IP

Key Takeaways
Log analysis is critical for detecting brute-force attacks

Baseline comparison simplifies incident detection

Defense-in-depth (firewall + Fail2Ban) is effective

Non-standard ports reduce noise but are not security by themselves

Skills Demonstrated
Linux system administration

Log analysis & incident response

Network reconnaissance detection

Firewall configuration (UFW)

Intrusion prevention with Fail2Ban

Structured documentation & evidence handling

Disclaimer
This project was conducted in an isolated lab environment for educational purposes only.

Author
Henok Araya
IT Security Specialist (Student)
