
# Incident Report — SMB Brute Force Attack

**Date:** April 4, 2026
**Performed by:** Munnaza Jamil
**Severity:** HIGH
**Status:** Resolved 

---

## 1. Incident Summary
A brute force attack targeting SMB authentication was detected on Windows host (192.168.56.1). The attacker at 192.168.56.101 made
repeated failed login attempts against the Administrator account using automated tooling. Wazuh SIEM generated a Level 10 alert
within seconds of the attack beginning.

---

## 2. Timeline
| Time (UTC) | Event |
|------------|-------|
| 19:53:08 | First failed logon detected (Event ID 4625) |
| 19:53:10 | Wazuh Rule 60122 triggered (Level 5) |
| 19:53:11 | Wazuh Rule 60204 triggered (Level 10) — brute force confirmed |
| 19:53:11 | Wazuh Rule 60115 triggered (Level 9) — account locked out |

---

## 3. Attack Details
- **Attack Type:** SMB Brute Force
- **MITRE Technique:** T1110 — Brute Force
- **Source IP:** 192.168.56.101 (Kali Linux)
- **Target IP:** 192.168.56.1 (Windows 11)
- **Target Account:** Administrator
- **Port:** 445 (SMB)
- **Tool Used:** Hydra with rockyou.txt wordlist

---

## 4. Evidence
- Wazuh Rule 60204 (Level 10): Multiple Windows logon failures
- Wazuh Rule 60115 (Level 9): Account locked out
- Windows Event ID 4625: Failed logon (NTLM, Logon Type 3)
- Source IP 192.168.56.101 consistent across all events
- Status code 0xC000006D confirms credential failure

---

## 5. Analyst Assessment
This is a confirmed brute force attack. The pattern of hundreds of failed logons from a single IP in rapid succession is consistent
with automated password spraying tools. The target (Administrator) is a high-value account. Account lockout mitigated further attempts
automatically.

**Verdict: TRUE POSITIVE**

---

## 6. Recommendations
1. Enforce account lockout after 5 failed attempts
2. Disable the built-in Administrator account
3. Block SMB (445) at the network perimeter if not required
4. Implement MFA for all remote authentication
5. Create Wazuh active response rule to auto-block offending IPs

---

## 7. Lessons Learned
Wazuh successfully detected and correlated the attack in real time.
The automatic MITRE ATT&CK mapping (T1110) demonstrates the value of SIEM correlation rules over manual log review.
