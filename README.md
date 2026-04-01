SMB Brute Force Attack Detection (SOC Lab Project)🔐 

Overview:
This project demonstrates the simulation and detection of an SMB brute-force attack in a controlled SOC home lab environment.

Lab Setup:
- Attacker: Kali Linux
- Target: Windows 11
- Tools: Hydra, Nmap, Event Viewer

Attack Simulation:
A brute-force attack was performed using Hydra:
hydra -l administrator -P /usr/share/wordlists/rockyou.txt smb://192.168.56.1

Detection:
- Event Viewer → Security Logs
- Event ID: 4625 (Failed Login)
- Source IP: 192.168.56.101

Analysis:
Multiple failed login attempts from the same IP indicate a brute-force attack targeting SMB authentication.

Mitigation:
- Strong password policies
- Account lockout mechanism
- Multi-Factor Authentication (MFA)
- Restrict SMB access

Screenshots:📸 
(See screenshots folder)

Skills Demonstrated:
- SOC Analysis
- Log Monitoring
- Attack Simulation
- Incident Detection.
