# ELK-Based-Threat-Detection-and-Log-Correlation
Design and implementation of a log-driven threat detection system using Elasticsearch, Logstash, and Kibana with custom correlation rules for identifying multi-stage attack behavior.


# SIEM Implementation using ELK Stack (Winlogbeat + Kali Attacks)

## Overview

This project demonstrates the implementation of a Security Information and Event Management (SIEM) system using the ELK Stack to detect real-world cyber attacks.

Logs from a Windows 10 machine were collected using Winlogbeat and analyzed in Kibana, while attacks were simulated from a Kali Linux machine.

---

## Objectives

* Collect and visualize Windows logs using ELK Stack
* Simulate real-world attacks from Kali Linux
* Develop correlation rules to detect attacks
* Build dashboards for security monitoring

---

## Architecture

```
Kali Linux (Attacker)
        ↓
Windows 10 (Winlogbeat)
        ↓
Elasticsearch + Logstash (Kali)
        ↓
Kibana Dashboard
```

---

## Tech Stack

* Elasticsearch
* Logstash
* Kibana
* Winlogbeat
* Kali Linux
* Windows 10

---

## Setup

### 1. ELK Stack (Kali)

* Install Elasticsearch, Logstash, Kibana
* Start services:

```
bin/elasticsearch
bin/kibana
```

---

### 2. Winlogbeat (Windows)

* Install Winlogbeat
* Configure:

```
output.elasticsearch:
  hosts: ["http://<ELK_IP>:9200"]
```

---

### 3. Enable Logging

* Security Logs (4624, 4625, etc.)
* PowerShell Script Block Logging (Event 4104)

---

## Attack Simulation

### Credential Stuffing

Tool: Hydra

```
hydra -l Administrator -P /usr/share/wordlists/rockyou.txt rdp://<TARGET_IP>
```

Logs Generated:

* 4625 → Failed login
* 4624 → Successful login
* 4672 → Admin privileges

---

### PowerShell Exploitation

```
powershell -EncodedCommand <base64_string>
```

Logs Generated:

* 4104 → Script execution logs

---

### DNS Tunneling (Simulation)

```
nslookup randomstring.test.com
```

Logs Generated:

* DNS query logs

---

## Detection Rules (KQL)

### Credential Stuffing

```
event.code: 4625
```

---

### PowerShell Attack

```
event.code: 4104 AND (
  message: "*EncodedCommand*" OR
  message: "*DownloadString*" OR
  message: "*Invoke-Expression*"
)
```

---

### DNS Tunneling

```
dns.question.name: "*.*.*.*"
```

---

## Correlation Logic

Example attack chain:

```
4625 → 4624 → 4672 → 5379
```

Indicates:

* Brute force → Success → Admin access → Credential extraction

---

## Dashboard

Kibana dashboards were created to visualize:

* Failed login attempts
* Successful logins
* PowerShell activity
* DNS queries

---

## Key Learnings

* Importance of log correlation in SIEM
* Detecting brute-force and post-exploitation activity
* PowerShell logging for attack detection
* Building dashboards for security monitoring

---

## Challenges

* Enabling PowerShell logging
* Handling noisy logs
* DNS logging setup
* Rule tuning

---

## Future Improvements

* Integrate Elastic SIEM module
* Add alerting system
* Use machine learning detection
* Add threat intelligence

---

## References

* Elastic Documentation
* Microsoft Event ID Documentation
* MITRE ATT&CK Framework

---

## Author

Sreekanth A

Cybersecurity Student

---
