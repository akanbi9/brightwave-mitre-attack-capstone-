
# ATT&CK Tactic-by-Tactic Analysis

## ATT&CK Version and Method

**ATT&CK Enterprise Version:** v19.2
**Date Consulted:** 19 September 2026
**Primary Reference:** MITRE ATT&CK Enterprise

This analysis uses the 14-tactic structure specified by the course. The live ATT&CK framework may use updated tactic or technique terminology, so differences between the course taxonomy and the current ATT&CK matrix are noted where relevant.

The analysis distinguishes between:

* **Fact:** Directly stated or observed in the supplied evidence.
* **Inference:** A reasonable interpretation of the available evidence.
* **Assumption:** An unverified possibility that requires additional evidence.

---

# 1. Reconnaissance

### Evidence or scenario clues

E-01 provides direct evidence of publicly available employee names, job titles, and email patterns on company and social-media pages.

This information could help an attacker identify employees, understand organizational roles, and select individuals for targeted activity.

E-01 is therefore the main evidence supporting reconnaissance.

### Defensive actions

The organization can reduce unnecessary exposure by:

* Reviewing publicly available employee information.
* Limiting unnecessary exposure of internal organizational details.
* Monitoring public websites for exposed employee information.
* Reviewing email-address patterns that are publicly visible.
* Providing security-awareness training to employees who may be targeted.
* Monitoring for impersonation or look-alike domains.

### What defenders can and cannot prevent

Defenders can reduce unnecessary information exposure and monitor for suspicious use of publicly available information.

However, an organization cannot realistically remove all publicly available information. Employee names, job roles, company information, and professional profiles may legitimately need to remain public.

The defensive objective is therefore to minimize unnecessary exposure rather than attempt to eliminate all reconnaissance opportunities.

---

# 2. Resource Development

### Evidence suggesting attacker-prepared resources

E-02 identifies a new external domain similar to the company name.

This could indicate preparation of infrastructure for impersonation or targeting. However, the evidence does not prove that the domain was created or controlled by the attacker.

Therefore, this should remain a **low-confidence inference**.

### Defensive monitoring

Defenders can monitor:

* Newly registered domains resembling the company name.
* Typosquatting and look-alike domains.
* Suspicious sender domains.
* Email authentication failures.
* Domain reputation information.
* Certificate and DNS changes associated with suspicious domains.
* Brand-impersonation intelligence.

Email security controls can also compare external domains against known organizational domains.

### Unknown facts and useful intelligence

The following remain unknown:

* Who registered the domain.
* When it was registered.
* Who controls it.
* Whether it was used exclusively against BrightWave.
* Whether other employees received messages from it.

Useful external intelligence would include domain-registration information, DNS history, certificate information, reputation data, and reports of similar activity involving the domain.

---

# 3. Initial Access

### Most likely entry path

E-02 and E-03 provide the strongest evidence for the suspected initial-access path.

E-02 shows a targeted message from an external domain similar to the company name. E-03 shows that the finance user authenticated successfully after opening the message and that the authentication was considered unusual.

This is consistent with a phishing-based entry path.

The exact phishing sub-technique cannot be determined because the scenario does not provide the message's attachment, link, or other delivery details.

### Preventive and detective controls

Useful controls include:

* Strong phishing-resistant MFA where appropriate.
* Email filtering.
* Sender and domain reputation checking.
* Email authentication controls.
* Safe link and attachment analysis.
* User security awareness.
* Unusual-sign-in detection.
* Conditional access.
* Risk-based authentication.

### Evidence required for confirmation

Investigators should obtain:

* Complete email headers.
* Sender and recipient information.
* Message body.
* Attachment information.
* URLs contained in the message.
* Email gateway verdicts.
* Source IP information.
* Authentication method.
* Identity-provider logs.
* Device information.
* Sign-in location and session details.

---

# 4. Execution

### ATT&CK mapping

E-04 shows a scripting engine launching from an Office-related process.

This behavior is consistent with execution following interaction with an Office document or related application. The appropriate ATT&CK mapping is the **Command and Scripting Interpreter** technique at the level supported by the evidence.

The exact interpreter and command cannot be determined because the scenario intentionally omits the command content.

### Evidence to request

The investigation should request:

* Full process tree.
* Parent process.
* Child process.
* Process ID.
* User account.
* Command-line information.
* File path.
* File hash.
* Process creation timestamp.
* Endpoint security alerts.
* Relevant Office application events.

### Safe detection hypothesis

A defensive detection could identify unusual cases where an Office application starts a scripting or command interpreter.

A useful detection should consider:

* Parent process.
* Child process.
* User.
* Endpoint.
* Frequency.
* File location.
* Known administrative software.

The detection should alert more strongly when the behavior is unusual for the user or endpoint.

No malicious commands or payloads are required to test this detection.

---

# 5. Persistence

### Observed persistence-related behavior

E-05 records the creation of a new scheduled execution entry configured to run when the user signs in.

This is consistent with a persistence mechanism that could allow repeated execution across user logons.

The evidence supports a high-confidence mapping to scheduled task/job behavior.

### Endpoint artifacts

Investigators should examine:

* Scheduled-task configuration.
* Task name.
* Creation time.
* Last-run time.
* Associated executable or script.
* Creating account.
* Task action.
* Trigger configuration.
* Endpoint security events.
* Relevant Windows event logs.

### Monitoring and remediation

Defenders should:

* Alert on unexpected scheduled tasks.
* Compare newly created tasks against approved software.
* Monitor tasks created by unusual users.
* Investigate tasks pointing to unusual locations.
* Remove unauthorized persistence after evidence preservation.
* Verify that no related persistence mechanisms remain.

---

# 6. Privilege Escalation

### Possible privilege escalation scenario

E-07 shows that a privileged account authenticated on the same workstation outside the administrator's normal pattern.

This does not prove how higher privileges were obtained.

Two major possibilities should be considered:

1. **Stolen or misused privileged credentials**
2. **Exploitation or abuse of a local privilege mechanism**

The current evidence provides stronger support for investigating possible credential misuse than for claiming a specific exploitation method.

### Evidence required

For possible credential misuse, investigators should review:

* Identity-provider logs.
* Authentication timestamps.
* Source workstation.
* Source IP.
* Authentication method.
* Session information.
* MFA events.
* Privileged-account activity.

For possible exploitation or misconfiguration, investigators should review:

* Endpoint security alerts.
* Process creation events.
* Security logs.
* Privilege-assignment events.
* Software and patch status.
* Local administrator membership.
* Configuration changes.

### Privileged-access controls

Recommended controls include:

* MFA for privileged accounts.
* Separate administrative accounts.
* Least privilege.
* Privileged access management.
* Just-in-time administrative access where appropriate.
* Monitoring of privileged authentication.
* Restrictions on where administrative accounts can authenticate.
* Regular privileged-account reviews.

A suspicious privileged login should be investigated rather than automatically treated as proof of compromise.

---

# 7. Defense Evasion

### Evidence

E-06 records a local security-protection setting being changed and later restored.

This is consistent with possible activity intended to reduce security visibility or weaken endpoint protection.

### ATT&CK consideration

The supplied evidence can be mapped to behavior involving modification or impairment of security tools.

The live ATT&CK framework has evolved its terminology around this area. The course continues to use **Defense Evasion** as one of its 14 tactics, so this document retains the course terminology rather than renaming the required section.

### Security-control changes that should generate alerts

Alerts should be generated for:

* Endpoint protection being disabled.
* Security configuration changes.
* Logging configuration changes.
* Tamper-protection changes.
* Unexpected administrative changes.
* Security services stopping.
* Changes made outside approved maintenance windows.

### Recommended controls

Organizations should use:

* Tamper protection.
* Centralized logging.
* Restricted security-administration privileges.
* Change-management controls.
* Alerts for security configuration changes.
* Monitoring of endpoint security status.

---

# 8. Credential Access

### Evidence

E-08 records an endpoint security alert indicating attempted access to stored credential material.

This is consistent with an attempt to obtain authentication-related information.

The evidence does not prove that credentials were successfully obtained.

### Useful logs and EDR data

Investigators should review:

* Endpoint detection alerts.
* Process trees.
* Process hashes.
* User accounts.
* File-access events.
* Security logs.
* Authentication logs.
* Credential-store access events.
* Related activity before and after E-08.

The timing of E-08 should also be compared with E-07 because the suspicious privileged-account authentication occurred shortly before the credential-access alert.

### Defensive recommendations

Recommended controls include:

* Strong MFA.
* Secure credential storage.
* Least privilege.
* Privileged-account separation.
* Credential-access monitoring.
* Password resets where compromise is reasonably suspected.
* Session revocation.
* Credential rotation for affected privileged accounts.
* Regular review of privileged access.

---

# 9. Discovery

### Discovery behavior

E-09 provides strong evidence of broad internal information gathering.

The workstation queries:

* Users.
* Groups.
* Nearby systems.
* Shared folders.
* Domain resources.

This can be mapped to multiple discovery behaviors rather than forcing the entire event into one technique.

### Discovery telemetry

| Activity                | Useful Telemetry                     |
| ----------------------- | ------------------------------------ |
| User/account queries    | Identity logs, endpoint process logs |
| Group queries           | Directory-service logs               |
| System discovery        | Endpoint and network telemetry       |
| Share discovery         | File-server and network logs         |
| Domain resource queries | Directory and DNS logs               |
| Process responsible     | EDR process telemetry                |

### Legitimate activity and false positives

Discovery activity can occur legitimately.

Examples include:

* IT administration.
* Asset inventory.
* Network troubleshooting.
* Security monitoring.
* Software deployment.
* Directory administration.

Context can reduce false positives by considering:

* User role.
* Device role.
* Time of activity.
* Frequency.
* Number of systems queried.
* Process generating the activity.
* Whether the activity matches normal administrative behavior.

A sudden burst of broad discovery from a finance workstation would deserve more investigation than routine discovery from an authorized IT administration system.

---

# 10. Lateral Movement

### Evidence

E-10 shows the finance user account connecting to the file server using a remote administration protocol not normally used by that employee.

This is consistent with possible remote movement from the affected workstation to the file server.

The exact protocol is intentionally not provided, so the most specific sub-technique cannot be selected confidently.

### Required telemetry

Investigators should collect:

* Source IP.
* Destination IP.
* Authentication logs.
* Account used.
* Authentication method.
* Remote-service logs.
* File-server logs.
* Endpoint process information.
* Network connection records.
* Session duration.

### Defensive controls

Recommended controls include:

* Network segmentation.
* Restricting administrative protocols.
* Separate privileged accounts.
* MFA for remote administration.
* Limiting administrative access by workstation.
* Monitoring unusual remote connections.
* Restricting finance users from unnecessary administrative services.

---

# 11. Collection

### Evidence

E-11 shows a large collection of finance and customer export files being copied into a temporary staging folder.

E-12 then shows the staged files being compressed into an archive.

Together, these events are consistent with deliberate collection and preparation of information.

### File-system and data-access evidence

Investigators should examine:

* File names.
* File paths.
* File sizes.
* File timestamps.
* File-access events.
* User accounts.
* Source systems.
* Destination folders.
* Archive creation time.
* Archive size.
* Process responsible for creating the archive.

### Defensive monitoring

Defenders should monitor for:

* Unusual bulk file access.
* Large numbers of file copies.
* Access to sensitive finance data.
* Access to customer exports outside normal roles.
* Creation of archives in temporary directories.
* Unusual file movement before external network activity.

---

# 12. Command and Control

### Evidence

E-13 records periodic encrypted web connections from the affected workstation to a rare external domain every few minutes.

The regular communication pattern is consistent with possible communication between the compromised workstation and an external service.

However, the domain has not been confirmed as attacker-controlled.

### Useful telemetry

Investigators should review:

* DNS queries.
* Destination domains.
* Destination IP addresses.
* Proxy logs.
* Firewall logs.
* TLS metadata.
* Connection frequency.
* Connection duration.
* Endpoint process responsible for the connection.
* User and workstation information.

### Detection idea

A defensive detection can identify endpoints that:

* Contact rare external domains.
* Make repeated connections at regular intervals.
* Communicate with destinations not normally associated with the organization.
* Generate the connections from unusual processes.

The detection should use allowlists and organizational baselines to reduce false positives.

---

# 13. Exfiltration

### Evidence

E-14 records a large encrypted outbound transfer to an unfamiliar external service.

This is consistent with possible unauthorized data transfer outside the organization.

The evidence does not establish exactly what data was transferred.

### Potential data exposure

E-11 shows that finance and customer export files were staged.

E-12 shows that those files were compressed.

E-14 then shows a large outbound transfer.

The sequence creates a reasonable concern that sensitive information may have left the organization. However, it would be inappropriate to state that all staged files were definitely exfiltrated without additional evidence.

### Defensive and response actions

Recommended controls include:

* Data-loss prevention.
* Egress monitoring.
* Firewall controls.
* Proxy monitoring.
* Network traffic analysis.
* Monitoring of unusual outbound volume.
* Blocking confirmed malicious destinations.
* Preservation of network evidence.

If sensitive customer information is confirmed to have been exposed, appropriate legal, regulatory, privacy, and business escalation procedures should be followed according to the organization's obligations.

---

# 14. Impact

### Evidence

E-15 reports that many shared documents can no longer be opened and filenames have changed. Business operations are disrupted.

This is the clearest evidence of direct business impact in the scenario.

The behavior is consistent with a destructive or encrypting effect on shared data, but the exact mechanism must be confirmed through forensic analysis.

### Recovery priorities

Immediate priorities should include:

1. Preserve evidence before unnecessary remediation.
2. Protect affected accounts.
3. Isolate affected systems where appropriate.
4. Protect the file server.
5. Identify the scope of affected files.
6. Determine whether clean backups are available.
7. Validate backup integrity.
8. Restore critical services in a controlled manner.
9. Monitor restored systems for recurring suspicious activity.
10. Document lessons learned.

### Resilience controls

The organization should strengthen:

* Tested backups.
* Offline or otherwise protected recovery copies.
* Recovery procedures.
* File-server access controls.
* Endpoint protection.
* Centralized logging.
* Incident-response procedures.
* Regular restoration testing.

---

# Overall Tactic Assessment

The evidence supports activity across several stages of the course's 14-tactic model.

| Tactic               | Evidence   | Assessment |
| -------------------- | ---------- | ---------- |
| Reconnaissance       | E-01       | Supported  |
| Resource Development | E-02       | Suspected  |
| Initial Access       | E-02, E-03 | Suspected  |
| Execution            | E-04       | Supported  |
| Persistence          | E-05       | Supported  |
| Privilege Escalation | E-07       | Suspected  |
| Defense Evasion      | E-06       | Suspected  |
| Credential Access    | E-08       | Supported  |
| Discovery            | E-09       | Supported  |
| Lateral Movement     | E-10       | Suspected  |
| Collection           | E-11, E-12 | Supported  |
| Command and Control  | E-13       | Suspected  |
| Exfiltration         | E-14       | Suspected  |
| Impact               | E-15       | Supported  |

## Analytical Conclusion

The strongest sequence is:

**Targeted activity → suspicious authentication → endpoint execution → persistence → credential-related activity → discovery → unusual file-server access → data staging → archive creation → periodic external communication → large outbound transfer → business disruption.**

The exact attacker identity, initial delivery mechanism, credentials obtained, files actually transferred, and mechanism responsible for the final file changes remain unconfirmed.
