# SMB Brute Force Attack Detection 🔐
**SOC Home Lab Project | Wazuh SIEM | MITRE ATT&CK T1110**

---

## Project Summary
This project simulates and detects an SMB brute force attack in a 
controlled SOC home lab. The attack was executed using Hydra from 
Kali Linux against a Windows 11 target, with detection and analysis 
performed using Wazuh SIEM.

---

## Lab Architecture
+------------------+       SMB Port 445        +------------------+
|   Kali Linux     | ------------------------> |   Windows 11     |
|  192.168.56.101  |     Brute Force Attack    |  192.168.56.1    |
|  (Attacker)      |                           |  (Target/Agent)  |
+------------------+                           +------------------+
|
Wazuh Agent (logs)
|
v
+------------------+
|   Wazuh SIEM     |
|  (Docker/Win11)  |
|  localhost:443   |
+------------------+

---

## Tools Used
| Tool | Purpose |
|------|---------|
| Kali Linux | Attacker machine |
| Hydra | SMB brute force tool |
| Windows 11 Pro | Target machine |
| Wazuh SIEM v4.7 | Detection & alerting |
| Docker Desktop | Wazuh deployment |

---

## Attack Simulation

**Command used on Kali Linux:**
```bash
hydra -l administrator -P /usr/share/wordlists/rockyou.txt \smb://192.168.56.1 -V -t 4
```

**What this does:**
- Targets the Administrator account over SMB (port 445)
- Uses the rockyou.txt wordlist (real-world password list)
- Sends 4 parallel threads of login attempts

---

## Detection — Wazuh Alerts Generated

| Rule ID | Level | Description | MITRE Technique |
|---------|-------|-------------|-----------------|
| 60122 | 5 | Logon failure - Unknown user or bad password | T1078, T1531 |
| 60115 | 9 | User account locked out (multiple login errors) | T1110, T1531 |
| 60204 | 10 | Multiple Windows logon failures | T1110 |

---

## Key Evidence — Windows Event ID 4625

| Field | Value | Meaning |
|-------|-------|---------|
| Event ID | 4625 | Failed logon attempt |
| Source IP | 192.168.56.101 | Attacker (Kali Linux) |
| Target Account | administrator | Account being attacked |
| Logon Type | 3 | Network-based (remote) |
| Auth Package | NTLM | SMB authentication method |
| Status Code | 0xC000006D | Wrong username or password |

---

## Screenshots
Uploaded in Scrennshots folder of repository.
---

## Main Findings:
- **192.168.56.101** generated hundreds of failed login attempts against the Administrator account within seconds
- Wazuh correctly correlated individual failures (Rule 60122) into a high-severity brute force alert (Rule 60204, Level 10)
- Account lockout triggered automatically (Rule 60115, Level 9)
- MITRE ATT&CK T1110 (Brute Force) confirmed by Wazuh automatically
- Attack used NTLM over network logon (Type 3) — classic SMB pattern

---

## Mitigation Recommendations:
| Control | Description |
|---------|-------------|
| Account Lockout Policy | Lock account after 5 failed attempts |
| MFA | Enable multi-factor authentication |
| SMB Restrictions | Block SMB from untrusted network segments |
| Strong Passwords | Enforce minimum 12 character passwords |
| Alert Tuning | Set automated response for Rule 60204 |

---

## Skills Demonstrated
- SIEM deployment and configuration (Wazuh)
- Real-time alert monitoring and triage
- Windows Event Log analysis (Event ID 4625)
- MITRE ATT&CK framework mapping
- Incident documentation and reporting
