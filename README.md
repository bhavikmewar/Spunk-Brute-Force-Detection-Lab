# Spunk-Brute-Force-Detection-Lab
Detect repeated failed Windows logins and alert on potential brute-force activity

# Project Overview
The project simulates a real-world brute force attack against a Windows system 
Windows Security Logs are forwarded to a centralized Splunk server
Custom Search Processing Language (SPL) queries and alerts identify failed login attempts and suspicous authentication behaviors
This lab replicates basic SOC monitoring and detection engineering workflows

# Lab Architecture
- Ubuntu VM - Splunk Enterprise (SIEM)
- Windows 10 VM - Target System
- Splunk Universal Forwarder - Log collection agent
- VirtualBox Host-Only + NAT networking

  Data Flow:
  VirtualBox Host-Only -> Windows Security Logs -> Splunk Universal Fowarder -> Splunk Server (Log 9997) -> Detection Searches -> Alerts + Dashboard

# Lab Architecture Diagram
![Splunk Lab Architecture](images/architecture.png)

# Data Sources
*Log Source:* Windows 10 Security Event Logs
*Fowarder:* Splunk Universal Forwarder
*Index:* Windows
*Sourcetype:* WinEventLog:Security

# Key Event IDs Monitored
- *4624* - Successful Logon
- *4625* - Failed Logon
- *4672* - Special privileges assigned to new logon

# Detection
Detection 1 - Multiple Failed Logins

