
# Wazuh SIEM Deployment & Security Monitoring Lab

### Endpoint Monitoring | Threat Detection | File Integrity Monitoring | Security Event Analysis

**Author:** Chioma Iroka  
**Course:** MCSC 6001 – Advanced Network Security  
**Institution:** Humber college institute of technology and advanced learning 
**Project Type:** Individual Cybersecurity Laboratory  
**Environment:** Isolated Virtual Network

---

## 1. Project Overview

This project demonstrates the deployment and evaluation of a centralized Security Information and Event Management (SIEM) environment using Wazuh.

The lab focused on collecting Windows endpoint security telemetry, monitoring process activity, detecting file modifications, integrating file reputation intelligence, and investigating security events.

The objective was not only to configure security tools but also to validate whether security activity could be observed and investigated through the SIEM.

This project demonstrates practical skills relevant to Security Operations Centers (SOC), cybersecurity risk assessment, security control validation, and Governance, Risk and Compliance (GRC).

---

## 2. Project Objectives

- Deploy and configure the Wazuh SIEM platform.
- Register and monitor a Windows 10 endpoint.
- Integrate Sysmon for enhanced endpoint visibility.
- Investigate process creation and network connection events.
- Configure File Integrity Monitoring (FIM).
- Test detection using a controlled EICAR antivirus test file.
- Integrate VirusTotal for file hash reputation enrichment.
- Evaluate OPNsense firewall log forwarding.
- Identify monitoring gaps and recommend improvements.

---

## 3. Lab Architecture

The project used four virtual machines.

| Component | Function |
|---|---|
| Ubuntu Linux | Wazuh manager, indexer, and dashboard |
| Windows 10 | Monitored endpoint with Wazuh agent and Sysmon |
| Kali Linux | Controlled test-file delivery |
| OPNsense | Firewall and supplementary network log source |

### Monitoring Workflow

Windows 10 Endpoint
→ Wazuh Agent
→ Wazuh Manager
→ Security Alerts
→ Wazuh Dashboard

File changes were additionally processed through File Integrity Monitoring and VirusTotal reputation enrichment.

OPNsense remote syslog forwarding was configured and tested separately, but successful end-to-end delivery was not confirmed.

---

## 4. Tools and Technologies

- Wazuh SIEM 4.14.9
- Ubuntu Linux
- Windows 10
- Sysmon 15.22
- Kali Linux
- OPNsense Firewall
- VirusTotal Integration
- PowerShell
- File Integrity Monitoring
- TCP/IP and Syslog

---

## 5. Implementation and Testing

### Phase 1: Wazuh Deployment

Deployed the Wazuh platform on Ubuntu Linux, including the manager, indexer, and dashboard.

**Verified results:**
- Wazuh dashboard was accessible.
- Windows 10 endpoint successfully enrolled.
- Windows agent displayed active status.
- Wazuh manager service was verified as active.

### Phase 2: Sysmon Integration

Installed and configured Sysmon on Windows 10 to improve visibility into endpoint activity.

**Verified results:**
- Sysmon Event ID 1 recorded process creation.
- Wazuh Discover returned 176 matching process-creation alerts.
- Sysmon Event ID 3 recorded network connections locally on Windows.

**Limitation:** The targeted Wazuh search for Event ID 3 did not return matching alerts. Centralized network-event visibility was therefore not fully verified.

### Phase 3: File Integrity Monitoring

Configured Wazuh FIM to monitor file activity in the Windows Downloads directory.

**Verified results:**
- File creation events were detected.
- File modification events were detected.
- File paths and associated hashes were available for investigation.

**Limitation:** A conclusive file deletion alert was not verified.

### Phase 4: Controlled EICAR Test

Used the standardized EICAR antivirus test file to simulate a suspicious file transfer without introducing real malware.

The test involved hosting the file from Kali Linux and attempting to transfer it to the monitored Windows endpoint.

**Observed results:**
- Chrome initially blocked the test-file download.
- PowerShell was used to complete the controlled file transfer.
- Wazuh FIM generated file-change evidence.
- File hash information was available for enrichment.

The EICAR file is a benign test artifact, not actual malware.

### Phase 5: VirusTotal Integration

Configured VirusTotal enrichment to provide additional reputation context for monitored file hashes.

**Verified results:**
- VirusTotal integration generated enrichment alerts.
- The observed reputation response reported 0 positive detections out of 60 engines for the queried hash.

This demonstrates successful enrichment, not a confirmed malicious-file detection.

### Phase 6: OPNsense Firewall Log Forwarding

Configured OPNsense to forward remote syslog data toward the Ubuntu monitoring server using UDP port 514.

**Observed results:**
- Remote logging destination was configured.
- The receiving service was listening on UDP/514.
- Packet capture testing did not confirm arriving firewall logs.

**Limitation:** End-to-end OPNsense log ingestion into Wazuh was not demonstrated.

---

## 6. Validation Summary

| Security Control | Validation Result |
|---|---|
| Wazuh deployment | Verified |
| Windows agent enrollment | Verified |
| Sysmon process monitoring | Verified |
| Sysmon network event collection | Partially verified |
| FIM file creation/modification | Verified |
| FIM deletion detection | Not verified |
| VirusTotal enrichment | Verified |
| OPNsense syslog integration | Not verified end-to-end |

---

## 7. Security Findings and Recommendations

### Finding 1: Incomplete Network Event Visibility

Network connection events were observed locally on Windows, but the targeted Wazuh alert search returned no matching results.

**Recommendation:** Validate Sysmon Event ID 3 ingestion, review Wazuh collection settings, and confirm alert rules and search filters.

### Finding 2: Incomplete FIM Validation

File creation and modification detection worked, but deletion detection was not conclusively demonstrated.

**Recommendation:** Perform a controlled deletion test and verify the corresponding event in Wazuh.

### Finding 3: Unverified Firewall Log Delivery

OPNsense forwarding was configured, but packet captures did not demonstrate successful log delivery.

**Recommendation:** Generate controlled firewall events, verify UDP/514 traffic, and correlate received logs with the SIEM.

### Finding 4: Reputation Enrichment Requires Context

VirusTotal enrichment returned a reputation result without positive detections.

**Recommendation:** Correlate file reputation with endpoint telemetry, file activity, and other security indicators rather than relying on a single reputation score.

---

## 8. Skills Demonstrated

- SIEM deployment and administration
- Windows endpoint monitoring
- Sysmon event analysis
- Security log investigation
- File Integrity Monitoring
- Threat intelligence enrichment
- Controlled security testing
- Security control validation
- Detection gap identification
- Technical security documentation
- Risk-based security recommendations

---

## 9. GRC and Risk Management Relevance

This project demonstrates why implementing a security control is not the same as verifying its effectiveness.

For example:

- An active endpoint agent provides evidence of monitoring availability.
- A searchable security alert demonstrates centralized detection.
- A configured logging destination does not guarantee successful log delivery.
- An incomplete validation test creates an assurance gap that requires further investigation.

These distinctions are important in IT risk assessments, security audits, control testing, and compliance evaluations.

The project strengthened my ability to assess security controls based on evidence, document limitations, and recommend improvements.

---

## 10. Lessons Learned

The most important lesson from this project was the need to distinguish configuration success from operational effectiveness.

A security tool may be installed and running without providing complete detection coverage.

Effective security monitoring requires validated telemetry, reliable event collection, meaningful analysis, and continuous improvement.

This experience reinforced the importance of evidence-based cybersecurity assessments and accurate technical reporting.

---

## 11. Evidence and Screenshots

Selected, sanitized screenshots from the original lab will be added to an `evidence/` directory.

Planned evidence includes:

- Wazuh dashboard and active Windows agent
- Sysmon process creation events
- Wazuh Discover event searches
- File Integrity Monitoring alerts
- VirusTotal enrichment results
- OPNsense logging configuration and validation results

Screenshots will be selected to reflect actual observed results without exposing credentials or sensitive information.

---

## 12. References

- [Wazuh Documentation](https://documentation.wazuh.com/)
- [Microsoft Sysmon Documentation](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
- [MITRE ATT&CK Framework](https://attack.mitre.org/)
- [VirusTotal](https://www.virustotal.com/)
- [EICAR Antivirus Test File](https://www.eicar.org/)

---

**Author:** Chioma Iroka

**Career Focus:** Cybersecurity Governance, Risk and Compliance (GRC) | IT Risk Management | Security Operations

*This project was completed in an authorized academic laboratory environment using simulated security testing activities.*
