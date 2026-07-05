# SSH Brute Force Detection Using Splunk

## Project Overview

This project demonstrates how to detect SSH brute force attacks using Splunk Enterprise. Ubuntu Server authentication logs are forwarded to Splunk using the Splunk Universal Forwarder. Failed SSH login attempts are analyzed using SPL (Search Processing Language) queries and visualized through a Splunk dashboard.

---

## Objectives

- Collect Ubuntu authentication logs
- Forward logs to Splunk Enterprise
- Attacker--> Kali Linux | Victim--> Ubuntu-server
- Detect failed SSH login attempts
- Identify attacking IP addresses
- Build a Splunk dashboard
- Create SIEM detection queries

---

## Lab Environment

| Component | Purpose |
|----------|----------|
| Windows 11 | Splunk Enterprise |
| Ubuntu Server 24.04 LTS | SSH Server |
| Kali Linux | Attack Machine |
| VirtualBox | Virtualization Platform |
| Splunk Universal Forwarder | Log Forwarding |

## Installation

## Step 1

Install

- VirtualBox
- Ubuntu Server
- Kali Linux
- Splunk Enterprise
- Splunk Universal Forwarder

---

## Step 2

Configure Splunk Receiver

Settings

- Forwarding and Receiving

- Configure Receiving

- Enable Port

```
9997
```

---

## Step 3

Configure Universal Forwarder

Add Splunk Server

```bash
sudo /opt/splunkforwarder/bin/splunk add forward-server 192.168.56.1:9997
```

Monitor Authentication Log

```bash
sudo /opt/splunkforwarder/bin/splunk add monitor /var/log/auth.log
```

Restart Forwarder

```bash
sudo /opt/splunkforwarder/bin/splunk restart
```

---

# SPL Queries

## View All Authentication Logs

```spl
index=* source="/var/log/auth.log"
```

---

## Failed SSH Login Detection

```spl
index=* source="/var/log/auth.log" "Failed password"
```

---

## Invalid User Detection

```spl
index=* source="/var/log/auth.log" "invalid user"
```

---

## Top Attacking IP Addresses

```spl
index=* source="/var/log/auth.log" "Failed password"
| rex "from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
| top src_ip
```

---

## Top Target Usernames

```spl
index=* source="/var/log/auth.log" "Failed password"
| rex "Failed password for (invalid user )?(?<user>\S+)"
| top user
```

---

## Failed Login Count

```spl
index=* source="/var/log/auth.log" "Failed password"
| stats count
```

---

## SSH Brute Force Detection

```spl
index=* source="/var/log/auth.log" "Failed password"
| rex "from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
| stats count by src_ip
| where count >= 5
| sort - count
```

---

# Dashboard Panels

- Failed SSH Logins Over Time
- Top Attacking IP Addresses
- Top Target Usernames
- Total Failed Login Attempts
- Brute Force Detection
- Successful SSH Logins

---

# Skills Demonstrated

- Splunk Enterprise
- Splunk Universal Forwarder
- SPL Query Writing
- Linux Administration
- SSH Log Analysis
- SIEM
- Log Management
- Incident Detection
- Threat Hunting
- Cybersecurity Monitoring

---

# Tools Used

- Splunk Enterprise
- Splunk Universal Forwarder
- Ubuntu Server
- Kali Linux
- VirtualBox
- OpenSSH

---

# Outcome

Successfully detected SSH brute force attacks by forwarding Ubuntu authentication logs into Splunk Enterprise and identifying repeated failed login attempts using SPL queries. The project demonstrates SIEM deployment, log analysis, attack simulation, and dashboard creation in a virtual lab environment.

---

## Author

**Shiham Faisal**

Aspiring SOC Analyst

LinkedIn: https://www.linkedin.com/in/shiham-faisal-b8b7681b4/

GitHub: https://github.com/shihamfsl
