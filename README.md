# Dual-Environment Security Log Analysis (Windows & Linux)

## Project Overview
This repository contains a hands-on cybersecurity log analysis project designed to detect, filter, and segregate malicious activity across both **Linux (Syslog/Auth logs)** and **Windows (Security Event Logs)**. 

The project demonstrates CLI-based text processing in a Linux environment alongside structured data filtering and threat count analysis in Microsoft Excel.

---

## File Structure
* **`windows_event_logs.csv`**: Raw dataset containing 1,000 generated Windows Security Events.
* **`filtered_malicious_logs.csv`**: Segregated dataset containing only suspicious and malicious events filtered from the main dataset.
* **`practice1.txt`**: Linux syslog sample used for CLI log auditing and frequency extraction.

---

## Technical Skills & Methodologies

### 1. Linux Security Log Analysis (VMware Environment)
Using the Linux Command Line Interface (CLI), text-based log entries were audited to identify unauthorized login attempts and track frequent attacker IP addresses.
* **Key Commands Used:**
  * `grep`: Filtered specific error strings (e.g., `Failed password`).
  * `awk`: Extracted target data columns like IP addresses (`$NF`).
  * `sort`: Alphabetically ordered data to group identical entries together.
  * `uniq -c`: Counted occurrences to flag brute-force attacker IPs.

### 2. Windows Event Log Analysis & Threat Segregation
A dataset of 1,000 Windows Security Event Logs was processed using Microsoft Excel to identify unauthorized activities based on Event IDs, Status, and Activity Types.
* **Key Excel Functions Used:**
  * Data Filters: Filtered `Activity_Type` to isolate `Suspicious` and `Malicious` entries.
  * `COUNTIF` / Pivot Tables: Aggregated total threat counts per threat category.

---

## Key Findings & Threat Breakdown

* **Total Logs Analyzed:** 1,000 logs
* **Total Malicious/Suspicious Events Identified:** 90 logs

### Threat Summary Table

| Event ID | Threat Category | Event Description | Total Count |
| :--- | :--- | :--- | :--- |
| **4625** | Brute Force Attempt | Failed logon attempts using invalid credentials | **45** |
| **4672** | Privilege Escalation | Special privileges assigned to new/unauthorized sessions | **15** |
| **7045** | Persistence | New service installed on the system (Backdoor mechanism) | **15** |
| **4720** | Unauthorized Account Creation | New local user account created without prior authorization | **15** |
| **Total** | | | **90** |

---

## Incident Analysis & Mitigation Strategies

1. **Brute Force Detection (Event ID 4625):**
   * **Observation:** 45 failed logon attempts were detected, primarily targeting standard accounts like `admin` or `root`.
   * **Mitigation:** Enforce Account Lockout Policies after 5 consecutive failed login attempts and require complex passwords.

2. **Privilege Escalation & Persistence (Event IDs 4672 & 7045):**
   * **Observation:** Unscheduled service installations and elevated privilege assignments were identified on target hosts.
   * **Mitigation:** Restrict Administrative Rights using Least Privilege Principles (PoLP) and implement Endpoint Detection and Response (EDR) agents to flag unauthorized service creation.

3. **Account Creation (Event ID 4720):**
   * **Observation:** New accounts were created during non-business hours without a corresponding Change Management ticket.
   * **Mitigation:** Configure real-time SIEM alerts for any Event ID 4720 triggers to verify user onboarding status immediately.
