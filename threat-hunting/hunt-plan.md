# Threat Hunting Plan

## Operation Nightfall

This threat-hunting plan defines defensive hypotheses based on gaps and unanswered questions identified during the simulated **Operation Nightfall** investigation at **BrightWave Logistics Ltd.**

All hunts are intended for **synthetic or authorized security data only**.

---

## Hunt Hypotheses

| Hunt ID | Hypothesis                                                                                         | Data Source                                      | Search Window | What Would Increase Confidence?                                                      |
| ------- | -------------------------------------------------------------------------------------------------- | ------------------------------------------------ | ------------- | ------------------------------------------------------------------------------------ |
| H-01    | Other users may have received the same targeted message.                                           | Email gateway / Microsoft 365                    | 7 days        | Same sender/domain, similar subject, link, attachment, or message characteristics    |
| H-02    | Other user or privileged accounts may have experienced unusual authentication.                     | Identity provider / VPN logs                     | 7 days        | Similar unusual location, device, time, sign-in risk, or privileged-account activity |
| H-03    | Other workstations may have experienced suspicious Office-to-script process activity.              | EDR / endpoint process telemetry                 | 7 days        | Similar Office-to-scripting process relationships on additional endpoints            |
| H-04    | Additional endpoints may contain newly created scheduled execution artifacts.                      | EDR / Windows Task Scheduler logs                | 7 days        | Similar task creation time, creator account, or associated activity                  |
| H-05    | The finance account may have accessed additional internal systems beyond the observed file server. | Identity logs / Windows logs / network telemetry | E-03 to E-15  | Additional unusual server connections or authentication activity                     |
| H-06    | Other systems may have communicated with the same rare external domain or related infrastructure.  | DNS / firewall / proxy / network flow logs       | 14 days       | Multiple internal hosts contacting the same unusual destination                      |
| H-07    | Additional sensitive files may have been staged, archived, or transferred.                         | File-server audit logs / EDR / network telemetry | E-09 to E-15  | Additional bulk file access, staging, archive creation, or unusual outbound transfer |

---

# H-01 — Repeated Targeted Email

### Hypothesis

Other employees may have received messages from the same external domain or messages with similar targeting characteristics.

### Data Sources

* Email gateway
* Microsoft 365 mail logs
* Email security logs

### Search Window

7 days surrounding E-02.

### Search For

* Same sender domain
* Similar sender addresses
* Similar subjects
* Similar timestamps
* Similar recipients
* Similar message characteristics
* Attachment or link indicators

### What Would Increase Confidence?

Confidence increases if multiple employees received messages from the same sender/domain with similar characteristics.

### Possible False Positives

* Legitimate vendor email
* Marketing messages
* Security testing
* Normal business correspondence

### Defensive Outcome

Identify additional potentially affected users and determine whether their accounts or endpoints require investigation.

---

# H-02 — Unusual Authentication Across Other Accounts

### Hypothesis

The unusual authentication observed in E-03 and E-07 may not have been limited to the identified accounts.

### Data Sources

* Identity provider
* Microsoft 365 sign-in logs
* VPN logs
* MFA logs

### Search Window

7 days surrounding the incident.

### Search For

* Unusual locations
* New devices
* Unusual authentication times
* Elevated sign-in risk
* Unusual VPN activity
* Privileged account authentication

### What Would Increase Confidence?

Confidence increases if multiple accounts show similar unusual authentication patterns during the incident period.

### Possible False Positives

* Remote workers
* Business travel
* New devices
* VPN use
* Approved administrator activity

### Defensive Outcome

Identify accounts requiring additional investigation or account-protection measures.

---

# H-03 — Additional Office-to-Script Activity

### Hypothesis

The Office-related scripting activity observed in E-04 may have occurred on additional workstations.

### Data Sources

* EDR
* Windows process telemetry
* Endpoint security logs

### Search Window

7 days surrounding E-04.

### Search For

```text
Office application
        ↓
Scripting / interpreter process
```

Review:

* Hostname
* Username
* Parent process
* Child process
* Timestamp

### What Would Increase Confidence?

Confidence increases if multiple workstations show similar unusual process relationships.

### Possible False Positives

* Approved automation
* Enterprise applications
* Administrative scripts
* Document-management software

### Defensive Outcome

Identify additional endpoints that require investigation.

---

# H-04 — Additional Scheduled Execution Artifacts

### Hypothesis

The scheduled execution artifact observed in E-05 may have been created on other workstations.

### Data Sources

* Windows Task Scheduler logs
* EDR
* Endpoint-management telemetry

### Search Window

7 days surrounding E-05.

### Search For

* Newly created scheduled tasks
* Recently modified tasks
* Unusual task creators
* Tasks created outside maintenance windows
* Tasks associated with unusual endpoint activity

### What Would Increase Confidence?

Confidence increases if similar scheduled execution artifacts are found on multiple endpoints.

### Possible False Positives

* Software updates
* IT maintenance
* Endpoint-management software
* Legitimate application installation

### Defensive Outcome

Identify additional endpoints for investigation.

---

# H-05 — Additional Internal Access by the Finance Account

### Hypothesis

The finance account involved in E-03 and E-10 may have accessed additional internal systems.

### Data Sources

* Identity logs
* Windows authentication logs
* File-server logs
* Network telemetry
* VPN logs

### Search Window

From E-03 through E-15.

### Search For

* Server authentication
* Remote connections
* File-server access
* Source devices
* Destination systems
* Unusual access times

### What Would Increase Confidence?

Confidence increases if the account accessed:

* Previously unused servers
* Systems outside the user's normal role
* Multiple systems within a short period
* Sensitive resources without an approved business reason

### Possible False Positives

* Legitimate finance activity
* IT assistance
* Approved administration
* Normal business workflows

### Defensive Outcome

Determine the scope of potentially unauthorized account activity.

---

# H-06 — Additional Systems Contacting the External Domain

### Hypothesis

The rare external domain observed in E-13 may have been contacted by additional internal systems.

### Data Sources

* DNS logs
* Firewall logs
* Proxy logs
* Network flow telemetry
* EDR network telemetry

### Search Window

14 days surrounding E-13.

### Search For

* Same external domain
* Related destination IPs
* Internal source hosts
* Connection frequency
* Connection timestamps
* Data volume

### What Would Increase Confidence?

Confidence increases if several internal systems contact the same unusual external destination.

### Possible False Positives

* Cloud applications
* SaaS services
* Software updates
* Legitimate vendor infrastructure

### Defensive Outcome

Determine whether the external communication was isolated or affected multiple systems.

---

# H-07 — Additional Data Staging or Transfer

### Hypothesis

The data collection and staging observed in E-11 and E-12 may have affected additional sensitive information.

### Data Sources

* File-server audit logs
* EDR
* File-system telemetry
* Network flow logs
* Firewall/proxy logs

### Search Window

From E-09 through E-15.

### Search For

* Unusual bulk file access
* Large file-copy operations
* Temporary staging activity
* Archive creation
* Access to finance/customer data
* Large outbound transfers

### What Would Increase Confidence?

Confidence increases when the same account or workstation shows a sequence such as:

```text
Bulk File Access
       ↓
Data Staging
       ↓
Archive Creation
       ↓
External Communication
```

### Possible False Positives

* Backup operations
* Data migration
* Reporting
* Approved exports
* Cloud synchronization

### Defensive Outcome

Help determine the scope of potentially affected information and support incident-response decisions.

---

# Hunt Priorities

| Priority | Hunt | Reason                                                          |
| -------- | ---- | --------------------------------------------------------------- |
| High     | H-02 | Identify additional potentially affected accounts               |
| High     | H-05 | Determine the scope of internal account activity                |
| High     | H-06 | Determine whether external communication affected other systems |
| High     | H-07 | Determine the potential scope of data activity                  |
| Medium   | H-01 | Identify additional targeted email recipients                   |
| Medium   | H-03 | Identify additional suspicious endpoint execution               |
| Medium   | H-04 | Identify additional persistence artifacts                       |

---

# Hunt Workflow

```text
Define Hypothesis
       ↓
Identify Data Sources
       ↓
Set Search Window
       ↓
Search Authorized Telemetry
       ↓
Compare With Normal Baseline
       ↓
Investigate Anomalies
       ↓
Correlate With Existing Evidence
       ↓
Document Findings
       ↓
Improve Detection Coverage
```

---

# Expected Outcome

The threat-hunting process should help identify:

* Additional affected users
* Additional affected endpoints
* Additional internal access
* Additional data collection
* Additional external communication
* Gaps in existing security monitoring

Hunt findings should be used to improve the **coverage matrix**, **detection use cases**, and **incident-response plan**.

