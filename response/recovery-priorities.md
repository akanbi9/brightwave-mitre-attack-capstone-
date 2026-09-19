# Recovery Priorities

## Operation Nightfall

Recovery should begin after the incident has been contained and affected systems have been investigated.

### 1. Confirm Containment

* Confirm affected endpoints are isolated.
* Secure affected accounts.
* Remove unauthorized persistence.
* Confirm malicious infrastructure is blocked.

### 2. Validate Clean Environment

* Verify endpoint security is active.
* Check security configurations.
* Review privileged accounts.
* Confirm no significant suspicious activity remains.

### 3. Validate Backups

* Identify the latest known-good backup.
* Verify backup integrity.
* Confirm the backup predates the suspected compromise.
* Avoid restoring from potentially affected backups.

### 4. Restore Critical Systems

Restore systems in a controlled order based on business priority.

```text
Identity / Authentication
          ↓
Critical File Services
          ↓
Customer Systems
          ↓
Finance Systems
          ↓
Supporting Services
```

### 5. Restore Business Data

* Restore affected files from validated backups.
* Verify file names and directory structure.
* Confirm users can access required data.
* Preserve relevant evidence before overwriting affected data.

### 6. Validate Recovered Systems

Check:

* Authentication
* MFA
* Endpoint security
* File permissions
* Network connectivity
* Security monitoring

### 7. Return Systems to Normal Operation

Use a gradual process:

```text
Restore → Test → Monitor → Validate → Return to Service
```

If suspicious activity returns, stop recovery and return the affected system to containment.

### 8. Increase Post-Recovery Monitoring

Monitor:

* Privileged accounts
* Authentication activity
* Endpoint processes
* Scheduled tasks
* File-server activity
* DNS requests
* Rare external domains
* Large outbound transfers

### 9. Lessons Learned

* Document the incident.
* Identify detection gaps.
* Review security controls.
* Improve backup and recovery procedures.
* Update incident-response procedures.
* Track control improvements.

## Recovery Completion Criteria

Recovery is substantially complete when:

* Critical systems are operational.
* Required business data is restored.
* Security controls are functioning.
* Affected credentials are secured.
* Unauthorized persistence is removed.
* Monitoring is active.
* No significant recurrence is observed.
* Remaining risks and improvements are documented.


