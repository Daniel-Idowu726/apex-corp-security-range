# Apex Corporation: Hybrid Security Homelab

A three-machine case study in security architecture, GRC, threat intelligence and SOC detection.

**Author:** Daniel Idowu, LLB (Hons)  
**Contact:** [LinkedIn](https://www.linkedin.com/in/dan-idowu-901a9098/) | d.idowu720@hotmail.com

## Project overview

I built a simulated corporate network in VirtualBox to explore how security decisions can be designed, documented and tested. The lab combines a Windows Active Directory domain controller, an Ubuntu workstation and a separate Ubuntu server running Splunk Enterprise. I configured central logging, examined the risks behind selected settings and ran controlled tests to check what an analyst could see in the logs.

My legal training shaped the way I documented the scope, reasoning and consequences of technical decisions. The three volumes follow the project from infrastructure and control mapping, through threat intelligence, to hands-on detection and analyst review.

## The three volumes

| Volume | What it covers | Read |
| --- | --- | --- |
| **1 - GRC case study** | The VirtualBox network, Active Directory and DNS, the Splunk server, Windows and Linux log forwarding, configuration reviews, a temporary storage risk exception and a simulated information security policy. | [Read Volume 1](Volume-1-GRC-Case-Study.pdf) |
| **2 - Threat intelligence** | An APT29 advisory briefing, selected adversary techniques, an insider-risk scenario, MITRE ATT&CK mapping, STIX/TAXII research and an IPv6 configuration case study. The London and Thames Valley professional-services firm is the business scenario used for the assessment. | [Read Volume 2](Volume-2-Threat-Intelligence.pdf) |
| **3 - SOC detection tests and analyst runbook** | Controlled Linux authentication and `sudo` tests, a Windows failed-logon test, Splunk searches and alert validation, a SOC dashboard, an analyst review procedure and Splunk retention planning. | [Read Volume 3](Volume-3-SOC-Runbook.pdf) |

**Reading order:** Volume 1 explains the environment; Volume 3 shows how I tested and monitored it. Volume 2 provides the threat intelligence context and can also be read independently.

## Lab architecture

| Component | Role |
| --- | --- |
| **CorpNetwork (10.0.5.0/24)** | Private VirtualBox NAT network connecting the three virtual machines. |
| **CORP-DC-01 (10.0.5.10)** | Windows Server 2022 domain controller and DNS server; forwards selected Windows Event Logs. |
| **Splunk server (10.0.5.20)** | Headless Ubuntu Server running Splunk Enterprise; receives forwarded events on TCP port 9997. |
| **CORP-CL-01 (DHCP)** | Ubuntu Desktop 24.04 LTS workstation; forwards `/var/log/auth.log`. |

I managed the Splunk server from my Windows 11 laptop using SSH and Splunk Web port forwards bound to `127.0.0.1`. The Splunk service ran under its dedicated `splunk` account rather than `root`. The lab was built within the limits of a 16 GB host laptop.

## Tests and results

- Generated failed local `su` attempts on `CORP-CL-01` and found the authentication failures in Splunk.
- Discovered that the everyday `corp-user` account initially had `sudo` access. I removed it from the `sudo` group, repeated the protected-file test and found the denial in Splunk.
- Created a saved Splunk alert for denied `sudo` activity and confirmed that a controlled repeat attempt appeared in Triggered Alerts.
- Generated a failed sign-in on `CORP-DC-01` and found the corresponding Windows Security Event ID 4625 in Splunk.
- Built a dashboard comparing event counts from the Windows and Linux hosts, and wrote a simulated Tier 1 procedure for reviewing the `sudo` alert.

These tests connect the architecture in Volume 1 to the analyst workflow in Volume 3. Volume 2 uses published reporting on APT29 to assess a separate espionage risk scenario and to consider how threat intelligence can inform defensive decisions. The lab tests focus on authentication and access control; the policy, analyst procedure and retention plan are identified as simulated or planning exercises in the relevant volumes.

## Frameworks and references

I mapped selected lab decisions to relevant controls in UK Cyber Essentials, ISO/IEC 27001:2022 and NIST SP 800-53 Rev. 5. Volume 2 includes the sources used for the APT29 assessment and MITRE ATT&CK mappings. The control references explain the reasoning behind the lab choices and support discussion of how similar decisions would be reviewed in an organisation.
