# Executive Incident Summary

## Incident Overview

BrightWave Logistics Ltd. appears to have experienced a coordinated intrusion that began with targeted social engineering and progressed from a finance user's workstation into internal resources. The investigation indicates that an external domain resembling the company's name sent a targeted message to a finance employee (E-02). Shortly afterward, the finance user successfully authenticated and the sign-in was flagged as unusual (E-03). An Office-related process then launched a scripting engine (E-04), followed by the creation of a scheduled execution entry configured to run when the user signed in (E-05). These events are consistent with an attacker establishing execution and persistence, although the exact initial-access mechanism remains unconfirmed.

The potentially affected assets include the finance user's workstation, the privileged account observed on that workstation, and the Windows file server. Evidence also indicates possible access to finance and customer export data. A suspicious privileged-account authentication (E-07), attempted access to stored credential material (E-08), and broad discovery activity involving users, groups, systems, shared folders, and domain resources (E-09) suggest that the activity progressed beyond the initial workstation. The finance account subsequently connected to the file server using an unusual remote administration protocol (E-10).

Information potentially at risk includes finance records, customer export files, shared documents, and credentials or authentication material. E-11 and E-12 show that finance and customer exports were collected, staged, and compressed. E-13 shows periodic encrypted connections to a rare external domain, while E-14 records a large encrypted outbound transfer. These events support the possibility of data exfiltration, but the evidence does not establish exactly which files were transferred.

The likely business impact includes loss of access to shared documents and disruption of normal operations. E-15 reports that many documents could no longer be opened and filenames had changed, indicating a significant impact to business availability.

### Urgent Containment Priorities

1. Preserve relevant identity, email, endpoint, file-server, firewall, and proxy evidence.
2. Isolate the affected workstation and protect the file server.
3. Revoke suspicious sessions and secure affected user and privileged accounts.
4. Block confirmed suspicious external infrastructure.
5. Reset or rotate credentials according to the confirmed scope.
6. Validate backups before beginning controlled recovery.
7. Increase monitoring for additional affected systems or accounts.

## Key Evidence

The main conclusions are supported by **E-02 through E-15**, particularly the unusual authentication (E-03), scripting activity (E-04), persistence artifact (E-05), credential-access alert (E-08), discovery activity (E-09), file-server access (E-10), data staging and compression (E-11/E-12), outbound communications and transfer (E-13/E-14), and business disruption (E-15).

