
# Safe Detection Engineering

## Operation Nightfall

This document defines defensive detection use cases for the simulated **Operation Nightfall** incident at BrightWave Logistics Ltd.

The analytics are designed for **synthetic or authorized security telemetry only**. They focus on identifying suspicious behavior and supporting defensive investigation.

No payloads, exploit instructions, credential-dumping commands, ransomware code, or defense-disabling procedures are included.

---

## Detection 1 — Unusual Sign-In by a User or Privileged Account

**ATT&CK Technique:** T1078 — Valid Accounts

### Data Source and Required Fields

* Identity provider sign-in logs
* VPN authentication logs
* Cloud authentication logs
* User/account name
* Source IP
* Device name
* Authentication method
* Timestamp
* Geographic location
* Sign-in risk
* Success/failure status

### Detection Logic

```text
IF
    a successful authentication occurs
AND
    the device, location, time, or authentication pattern is unusual for the account
THEN
    generate an alert
```

For privileged accounts, apply a lower threshold because unexpected administrator authentication has greater security impact.

### Why It Matters

Valid credentials can be used to access systems without immediately appearing as a failed-login attack.

In Operation Nightfall:

* E-03 showed an unusual successful authentication by the finance user.
* E-07 showed a privileged account authenticating outside the administrator's normal pattern.

### Expected False Positives

* Administrator performing emergency maintenance
* Employee travelling
* New workstation
* VPN connection
* Approved remote work
* Newly enrolled device

### Tuning Ideas

* Maintain approved administrator accounts and devices.
* Establish normal login locations and times.
* Exclude approved service accounts.
* Increase sensitivity for privileged accounts.
* Correlate authentication with endpoint and network events.

### Severity

**High**

### Response Action

1. Verify whether the authentication was legitimate.
2. Review recent authentication activity.
3. Review the affected device.
4. Revoke suspicious sessions if unauthorized.
5. Escalate if privileged access is involved.

---

# Detection 2 — Office Application Spawning a Scripting Process

**ATT&CK Technique:** T1059 — Command and Scripting Interpreter

### Data Source and Required Fields

* Endpoint Detection and Response (EDR)
* Windows process creation telemetry
* Process name
* Parent process
* Child process
* User
* Device
* Timestamp
* Process path

### Detection Logic

```text
IF
    an Office or document application starts
    a scripting or command interpreter process
AND
    the process relationship is uncommon in the environment
THEN
    generate an alert
```

Example process relationship:

```text
Office application
        ↓
Scripting / command interpreter
```

The actual command content should not be required for the detection.

### Why It Matters

Office applications normally perform document-related activities. An unusual child process can indicate suspicious user-triggered execution.

E-04 recorded a scripting engine launching from an Office-related process.

### Expected False Positives

* Approved macros
* Automation software
* Document-management systems
* Administrative scripts
* Software deployment tools

### Tuning Ideas

* Establish a baseline of normal Office child processes.
* Allow approved applications and automation tools.
* Alert more aggressively for user workstations.
* Correlate with email and identity events.

### Severity

**High**

### Response Action

1. Investigate the process tree.
2. Identify the logged-in user.
3. Review the originating document or email.
4. Check for related endpoint activity.
5. Isolate the endpoint if additional suspicious behavior is confirmed.

---

# Detection 3 — New Scheduled Persistence Artifact

**ATT&CK Technique:** T1053.005 — Scheduled Task/Job: Scheduled Task

### Data Source and Required Fields

* Windows Task Scheduler logs
* EDR
* Windows event logs
* Task name
* Creation time
* Modification time
* Creating account
* Executable or program reference
* Hostname

### Detection Logic

```text
IF
    a new scheduled task is created or modified
AND
    the action is outside approved administrative activity
THEN
    generate an alert
```

Give higher priority to scheduled tasks created shortly after suspicious process execution.

### Why It Matters

Scheduled execution can provide repeated execution when a user signs in or when a scheduled trigger occurs.

E-05 recorded a new scheduled execution entry configured to run when the user signed in.

### Expected False Positives

* Software installation
* Legitimate system maintenance
* Enterprise software updates
* IT administration
* Approved endpoint-management tools

### Tuning Ideas

* Maintain an allowlist of approved software.
* Monitor newly created tasks.
* Track the creating account.
* Correlate task creation with preceding endpoint events.

### Severity

**High**

### Response Action

1. Verify whether the task is authorized.
2. Identify the creating account and process.
3. Preserve relevant logs.
4. Remove or disable the artifact only through approved incident-response procedures.
5. Investigate related endpoint activity.

---

# Detection 4 — Security or Logging Configuration Change

**ATT&CK Technique:** T1685 — Disable or Modify Tools

> **ATT&CK taxonomy note:** The current ATT&CK taxonomy may place related behavior under Defense Impairment. The course's 14-tactic structure uses the Defense Evasion category.

### Data Source and Required Fields

* Endpoint security logs
* Windows event logs
* EDR
* Security configuration logs
* User/account
* Device
* Timestamp
* Configuration changed
* Previous value
* New value
* Process responsible

### Detection Logic

```text
IF
    a security or monitoring configuration changes
AND
    the change is not associated with an approved administrative action
THEN
    generate an alert
```

### Why It Matters

Unexpected security configuration changes may reduce visibility or protection on an endpoint.

E-06 recorded a local protection setting being changed and later restored.

### Expected False Positives

* Approved security maintenance
* Endpoint-management software
* Security product updates
* Troubleshooting by IT
* Policy deployment

### Tuning Ideas

* Record approved maintenance windows.
* Monitor changes made outside those windows.
* Prioritize changes made by unusual accounts.
* Correlate changes with process and authentication telemetry.

### Severity

**High**

### Response Action

1. Determine who made the change.
2. Verify whether it was authorized.
3. Review surrounding endpoint activity.
4. Preserve relevant logs.
5. Escalate if the change is associated with suspicious activity.

---

# Detection 5 — Credential-Access Alert

**ATT&CK Technique:** T1555 — Credentials from Password Stores

### Data Source and Required Fields

* EDR
* Endpoint security alerts
* Windows security telemetry
* User
* Hostname
* Process
* Alert type
* Timestamp
* Detection status

### Detection Logic

```text
IF
    endpoint security reports attempted access to
    stored authentication-related material
THEN
    generate an alert
AND
    correlate with nearby authentication and process activity
```

### Why It Matters

Credential-related activity can indicate an attempt to obtain authentication information that could enable further access.

E-08 recorded a security alert indicating attempted access to stored credential material.

The alert does **not** by itself prove that credentials were successfully obtained.

### Expected False Positives

* Password managers
* Approved enterprise applications
* Security software
* Administrative tools
* Browser operations

### Tuning Ideas

* Maintain approved application baselines.
* Correlate with unusual process activity.
* Prioritize activity involving privileged workstations.
* Combine endpoint alerts with identity events.

### Severity

**High**

### Response Action

1. Investigate the alert and responsible process.
2. Determine whether the activity was authorized.
3. Review subsequent authentication activity.
4. Protect potentially affected accounts.
5. Escalate if privileged credentials may be involved.

---

# Detection 6 — Discovery Burst from a User Workstation

**ATT&CK Techniques:**

* T1087 — Account Discovery
* T1069 — Permission Groups Discovery
* T1018 — Remote System Discovery
* T1135 — Network Share Discovery

### Data Source and Required Fields

* EDR
* Windows security logs
* DNS logs
* Network telemetry
* SMB/file-server logs
* Source workstation
* User
* Destination
* Query/event type
* Timestamp
* Event count

### Detection Logic

```text
IF
    a user workstation performs an unusually high volume
    of account, group, system, or network-share discovery
WITHIN
    a short period
THEN
    generate an alert
```

### Why It Matters

A burst of different discovery activities can indicate systematic information gathering about the internal environment.

E-09 recorded queries involving users, groups, nearby systems, shared folders, and domain resources.

### Expected False Positives

* Network administrators
* IT inventory systems
* Monitoring tools
* Asset-management software
* Help-desk troubleshooting

### Tuning Ideas

* Create separate baselines for administrators and normal users.
* Use event volume thresholds.
* Correlate multiple discovery categories.
* Exclude approved inventory systems.

### Severity

**Medium–High**

### Response Action

1. Identify the workstation and user.
2. Determine whether the activity is expected.
3. Review authentication and process activity.
4. Investigate unusual destinations.
5. Escalate when discovery is followed by remote access or data collection.

---

# Detection 7 — Unexpected Remote Access to a Server

**ATT&CK Technique:** T1021 — Remote Services

### Data Source and Required Fields

* Windows authentication logs
* File-server logs
* Network telemetry
* VPN logs
* Source IP
* Source device
* Destination server
* Account
* Protocol/service
* Timestamp

### Detection Logic

```text
IF
    a user account accesses a server remotely
AND
    the user or workstation does not normally perform that activity
THEN
    generate an alert
```

Increase priority when the destination is a sensitive server.

### Why It Matters

Unexpected remote access can indicate unauthorized movement from a compromised workstation to another internal system.

E-10 recorded the finance account connecting to the file server using a remote administration protocol not normally used by that employee.

### Expected False Positives

* IT support
* Emergency administration
* Server maintenance
* Approved remote workers
* Help-desk activity

### Tuning Ideas

* Maintain administrator-to-server access baselines.
* Track normal remote-access relationships.
* Prioritize sensitive servers.
* Correlate with the source workstation's recent activity.

### Severity

**High**

### Response Action

1. Validate the account and source device.
2. Review the server session.
3. Check whether the access was authorized.
4. Protect the account if unauthorized.
5. Investigate the source workstation.

---

# Detection 8 — Large or Unusual Data Staging and Compression

**ATT&CK Techniques:**

* T1074 — Data Staged
* T1560 — Archive Collected Data

### Data Source and Required Fields

* File-server audit logs
* EDR
* Filesystem telemetry
* File access logs
* File creation events
* Username
* Hostname
* File path
* File size
* Number of files
* Archive creation event
* Timestamp

### Detection Logic

```text
IF
    a user or workstation accesses an unusually large
    number or volume of files
AND
    the files are copied to a temporary or staging location
OR
    a large archive is subsequently created
THEN
    generate an alert
```

### Why It Matters

Bulk collection followed by staging or compression can indicate preparation for unauthorized transfer.

E-11 recorded a large collection of finance and customer export files being copied to a temporary staging folder.

E-12 recorded the staged files being compressed into an archive.

### Expected False Positives

* Backup jobs
* Data migration
* Reporting
* Approved exports
* IT maintenance

### Tuning Ideas

* Baseline normal backup activity.
* Monitor sensitive directories more closely.
* Set different thresholds for finance, HR, and customer data.
* Correlate staging with subsequent network transfers.

### Severity

**High**

### Response Action

1. Identify the account and workstation.
2. Determine which files were involved.
3. Preserve file and audit evidence.
4. Investigate subsequent outbound connections.
5. Escalate if sensitive data may have been transferred.

---

# Detection 9 — Periodic Outbound Connections to a Rare Domain

**ATT&CK Technique:** T1071.001 — Application Layer Protocol: Web Protocols

### Data Source and Required Fields

* DNS logs
* Firewall logs
* Proxy logs
* EDR network telemetry
* Domain
* Source host
* Destination IP
* Timestamp
* Connection count
* Connection interval
* Domain age/reputation where available

### Detection Logic

```text
IF
    a workstation repeatedly connects to the same
    rare or newly observed external domain
AT
    relatively regular intervals
THEN
    generate an alert for investigation
```

Do not treat domain rarity alone as proof of malicious activity.

### Why It Matters

Regular outbound communication to a rare external service can be useful for identifying potentially suspicious communication.

E-13 recorded periodic encrypted web connections from the compromised workstation to a rare external domain.

### Expected False Positives

* Cloud applications
* Software update services
* SaaS platforms
* Telemetry services
* Newly introduced business services

### Tuning Ideas

* Maintain an approved-domain list.
* Baseline common SaaS services.
* Combine domain rarity with endpoint risk.
* Correlate periodic connections with process and identity telemetry.

### Severity

**High**

### Response Action

1. Identify the process generating the connection.
2. Validate the destination.
3. Review DNS and proxy history.
4. Investigate the endpoint for related activity.
5. Block or restrict the destination only through approved defensive procedures when malicious activity is confirmed.

---

# Detection 10 — Large Encrypted Outbound Transfer

**ATT&CK Technique:** T1048 — Exfiltration Over Alternative Protocol

### Data Source and Required Fields

* Firewall logs
* Proxy logs
* Network flow telemetry
* DLP logs where available
* Source host
* Destination
* Destination domain/IP
* Bytes sent
* Timestamp
* Protocol
* User
* Session duration

### Detection Logic

```text
IF
    outbound encrypted traffic volume is significantly
    above the workstation's normal baseline
AND
    the destination is unfamiliar or unusual
THEN
    generate an alert
```

Increase priority when the event occurs shortly after large-scale file staging or archive creation.

### Why It Matters

E-14 recorded a large encrypted outbound transfer to an unfamiliar external service.

The available evidence does not establish the exact contents of the transfer, so the detection should identify it as **potential unauthorized data movement**, not automatically confirmed exfiltration.

### Expected False Positives

* Cloud backups
* Large legitimate file uploads
* Software distribution
* Video or media services
* Business cloud-storage applications

### Tuning Ideas

* Establish normal outbound-volume baselines.
* Monitor sensitive workstations more closely.
* Maintain approved cloud-service destinations.
* Correlate outbound volume with file staging and archive events.
* Apply different thresholds to servers and user workstations.

### Severity

**Critical**

### Response Action

1. Identify the source workstation and user.
2. Review recent staging and archive activity.
3. Investigate the destination.
4. Preserve network and endpoint evidence.
5. Escalate according to the organization's incident-response and data-breach procedures.

## Key Detection Gaps

The simulated incident highlights several areas requiring improved monitoring:

1. Limited visibility into unusual authentication behavior.
2. Incomplete endpoint process-chain detection.
3. Insufficient monitoring of new persistence artifacts.
4. Limited correlation between credential-access alerts and identity events.
5. Weak visibility into unusual internal discovery.
6. Limited detection of abnormal remote administration.
7. Insufficient monitoring of bulk data staging.
8. Limited correlation between staging and outbound network transfers.

## Defensive Objective

The goal of this detection program is to improve the organization's ability to:

* Detect suspicious activity earlier.
* Correlate events across identity, endpoint, server, and network telemetry.
* Reduce false positives through environmental baselines.
* Prioritize high-impact alerts.
* Support authorized incident investigation.
* Improve detection coverage against the behaviors observed in Operation Nightfall.

```
```
