# Detection Data Sources

## Operation Nightfall

This identifies the telemetry and log sources required to support the defensive detections. 

The sources are based on the simulated BrightWave Logistics Ltd. environment and are intended for **authorized or synthetic security monitoring**.

---

## 1. Identity Provider Logs

**Examples:**

* Microsoft 365 / cloud identity logs
* Identity provider authentication logs
* MFA logs
* Account-risk events

**Useful fields:**

* Username
* Account type
* Source IP
* Device name
* Location
* Timestamp
* Authentication method
* Success/failure
* Sign-in risk
* MFA result

**Supports:**

* T1078 — Valid Accounts
* Unusual user authentication
* Privileged account monitoring

**Incident evidence:**

* E-03
* E-07

---

## 2. Endpoint Detection and Response (EDR)

EDR provides visibility into activity occurring on Windows endpoints.

**Useful fields:**

* Hostname
* Username
* Process name
* Parent process
* Child process
* Process path
* Timestamp
* Network connection
* Security alert
* File activity

**Supports:**

* T1059 — Command and Scripting Interpreter
* T1053.005 — Scheduled Task/Job
* T1555 — Credentials from Password Stores
* T1685 — Disable or Modify Tools
* Discovery techniques

**Incident evidence:**

* E-04
* E-05
* E-06
* E-08
* E-09

---

## 3. Windows Security Event Logs

Windows security logs provide information about authentication, account activity, and system events.

**Useful fields:**

* Event ID
* Username
* Computer name
* Source address
* Logon type
* Timestamp
* Authentication result
* Account used

**Supports:**

* Account authentication monitoring
* Privileged account monitoring
* Remote access detection
* Account discovery investigations

**Incident evidence:**

* E-03
* E-07
* E-10

---

## 4. Task Scheduler Logs

Task Scheduler telemetry can identify new or modified scheduled execution artifacts.

**Useful fields:**

* Task name
* Task path
* Creating account
* Creation time
* Modification time
* Trigger
* Hostname
* Associated program

**Supports:**

* T1053.005 — Scheduled Task/Job: Scheduled Task

**Incident evidence:**

* E-05

---

## 5. Endpoint Security Logs

Security-product telemetry records changes and alerts involving endpoint protection.

**Useful fields:**

* Alert ID
* Alert type
* Hostname
* Username
* Process
* Timestamp
* Configuration changed
* Previous state
* New state
* Detection status

**Supports:**

* Security configuration monitoring
* Credential-access detection
* Endpoint investigation

**Incident evidence:**

* E-06
* E-08

---

## 6. DNS Logs

DNS logs show domain lookups made by systems inside the organization.

**Useful fields:**

* Timestamp
* Source hostname
* Source IP
* Requested domain
* Response
* Destination IP
* Query type

**Supports:**

* Rare-domain detection
* Internal discovery investigation
* Suspicious external communication

**Incident evidence:**

* E-09
* E-13

---

## 7. Firewall Logs

Firewall telemetry records connections between internal systems and external destinations.

**Useful fields:**

* Timestamp
* Source IP
* Destination IP
* Destination port
* Protocol
* Bytes sent
* Bytes received
* Action
* Domain where available

**Supports:**

* Periodic outbound connection detection
* Large outbound transfer detection
* Network investigation

**Incident evidence:**

* E-13
* E-14

---

## 8. Proxy Logs

Web proxy logs provide additional visibility into outbound web traffic.

**Useful fields:**

* Username
* Source device
* Destination domain
* URL/category where available
* Timestamp
* HTTP method
* Response status
* Bytes transferred

**Supports:**

* T1071.001 — Web Protocols
* Rare-domain monitoring
* Outbound transfer investigation

**Incident evidence:**

* E-13
* E-14

---

## 9. Network Flow Telemetry

Network-flow data provides metadata about communication without requiring inspection of the full content.

**Useful fields:**

* Source IP
* Destination IP
* Source port
* Destination port
* Protocol
* Start time
* End time
* Bytes sent
* Bytes received
* Connection count

**Supports:**

* Periodic communication detection
* Large outbound transfer detection
* Internal discovery
* Remote-service monitoring

**Incident evidence:**

* E-09
* E-10
* E-13
* E-14

---

## 10. File-Server Audit Logs

File-server telemetry records access to shared business files.

**Useful fields:**

* Username
* Source workstation
* File path
* File name
* Access type
* Timestamp
* File size
* Operation
* Success/failure

**Supports:**

* T1021 — Remote Services
* T1074 — Data Staged
* T1560 — Archive Collected Data
* Impact monitoring

**Incident evidence:**

* E-10
* E-11
* E-12
* E-15

---

## 11. File-System Telemetry

File-system monitoring records changes to files and directories.

**Useful fields:**

* Hostname
* Username
* File path
* File name
* File extension
* File size
* Creation time
* Modification time
* Rename event
* Delete event

**Supports:**

* Data staging detection
* Archive creation detection
* Large-scale file-change detection
* Impact monitoring

**Incident evidence:**

* E-11
* E-12
* E-15

---

## 12. Email Gateway Logs

Email security telemetry provides visibility into suspicious or unusual messages.

**Useful fields:**

* Sender address
* Sender domain
* Recipient
* Timestamp
* Subject
* Message ID
* Domain reputation
* Detection result
* Delivery status

**Supports:**

* Suspicious email investigation
* Phishing-related detection
* Initial-access investigation

**Incident evidence:**

* E-04

---

# Priority Telemetry

For Operation Nightfall, the most important telemetry sources are:

1. **Identity logs** — detect unusual account use.
2. **EDR** — detect suspicious endpoint behavior.
3. **File-server logs** — detect unauthorized access and data collection.
4. **Firewall/proxy logs** — detect unusual external communication.
5. **DNS logs** — identify unusual domain activity.
6. **Email gateway logs** — investigate suspicious messages.

---

# Correlation Strategy

Individual logs provide limited context. Combining multiple sources creates a stronger detection picture.

Example:

```text
Email Gateway
      ↓
Unusual Identity Authentication
      ↓
EDR Process Activity
      ↓
Scheduled Task
      ↓
Credential-Access Alert
      ↓
Internal Discovery
      ↓
File-Server Access
      ↓
Data Staging
      ↓
Archive Creation
      ↓
Firewall / Proxy Connection
      ↓
Large Outbound Transfer
```

This correlation approach helps to investigate the incident as a **sequence of related events** rather than treating each alert independently.

---

# Data Retention and Quality Considerations

Security telemetry should have sufficient retention to support incident investigation.

Important considerations include:

* Consistent timestamps across systems.
* Accurate hostname and username information.
* Centralized log collection where possible.
* Protection against unauthorized log modification.
* Appropriate retention periods.
* Monitoring for missing telemetry.
* Regular validation that important data sources are reporting correctly

# Detection Engineering Goal

The main goal of these data sources is to give security analysts enough information to spot unusual activities, investigate alerts, understand what happened during an incident, and respond properly.

No single data source can detect every type of attack. That is why effective security monitoring requires information from different areas, including endpoints, user accounts, servers, email systems, and network traffic. When these sources are used together, they provide a clearer picture of what is happening across the environment.


