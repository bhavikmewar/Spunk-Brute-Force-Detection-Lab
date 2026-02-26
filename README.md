# Spunk Brute Force Detection Lab


In this lab, I built a small SIEM environment using Splunk to detect brute force login attempts against a Windows 10 machine.


I created three virtual machines (Ubuntu Linux, Windows 10, and Kali Linux).


The goal was to understand how brute force attacks appear in logs and how a SOC analyst would detect them.

# Lab Architecture
- Ubuntu VM - Splunk Enterprise (SIEM)
- Windows 10 VM - Target System
- Splunk Universal Forwarder - Log collection agent
- VirtualBox networking (Host-Only + NAT) 

  Data Flow:
  VirtualBox Host-Only -> Windows Security Logs -> Splunk Universal Fowarder -> Splunk Server (Port 9997) -> Detection Searches -> Alerts + Dashboard

# Lab Architecture Diagram
![Splunk Lab Architecture](images/architecture.png)

# Data Sources
Log Source: Windows 10 Security Event Logs
Fowarder: Splunk Universal Forwarder
Index: wineventlog
Sourcetype: WinEventLog:Security

# Key Event IDs Monitored
- 4624 - Successful Logon
- 4625 - Failed Logon
- 4672 - Special privileges assigned to new logon

#  Attack Simulation


To simulate brute force behavior, I generated repeated RDP login attempts from the Kali Linux VM using xfreerdp3 command.


These attempts created multple Event ID 4625 logs in Windows.


RDP kali command used:
![Vizualization](images/kalihydracommand.png)

# Detection 1 - Multiple Failed Logins


The first detection identifies repeated failed login attempts.


This helped confirm:
- Logs were being ingested correctly
- The attacker IP was visible
- Failed log counts increased during the attack


# SPL Brute Force Detection
![Brute Force Detection](images/detectionquery.png)

# Behavioral Visualization
To identify burst patterns, I analyzed failed logins over time to see spikes during the attack window

# Visualization
![Vizualization](images/Visualization.png)

--
# Detection 2 - Threshold-Based Brute Force Detection
This detection is more realistic
Instead of just counting failures, I grouped failed logins into 5-minute time brackets and flagged activity when attempts exceeded a threshold from a single source IP.
This better represents how a SOC might detect brute force behavior

# PICTURE
![Vizualization](images/aggregatedetection.png)

# Log Validation
I verified the attack activity in multiple places:

- Windows Event Viewer (Event ID 4625)
- Splunk raw events (WinEventLog:Security)
- Source_Network_Address field matching the Kali attacker IP

# Windows Event Viewer
![Vizualization](images/WindowsEventViewer.png)

# Splunk Raw Events
![Vizualization](images/SplunkRawEvents.png)


# What I learned
- How to configure Splunk Universal Forwarder
- How Windows authentication logs are structured
- How to write SPL queries for detection
- How brute force activity appears in logs
- How time-based aggregation improves detection accuracy
- How to build and segment a small virtual lab environment
