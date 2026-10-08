A. Access Control Model Application
A1. Chosen Access Control Model and Its Application
Role-Based Access Control (RBAC) is the access control model applied to FinSecure Corp.’s user role matrix. RBAC ties system permissions to a defined job role rather than to an individual user; a person is assigned to one or more roles, and each role carries a fixed, pre-approved set of permissions needed to perform that job function (National Institute of Standards and Technology [NIST], 2020). RBAC is the appropriate model for FinSecure for two reasons drawn directly from the provided materials: the user role matrix is already organized around a “Role” column, and Section 5 of the Access Control Policy explicitly references RBAC as the organization’s intended access model for core systems. Applying RBAC formally, rather than leaving it as an option “as determined by system owners,” gives FinSecure a consistent, auditable structure for granting and reviewing access.
Four RBAC principles govern how the model should be applied to the organization's access control structure:
•	Least privilege: each role should be granted only the minimum access required to perform its defined duties, and no more (NIST, 2020).
•	Role standardization: every account assigned to the same role should carry the same permission set; individual exceptions undermine the integrity of the model.
•	Separation of duties: roles with conflicting responsibilities (e.g., initiating and approving the same financial transaction) should not be combined in a single account.
•	Lifecycle management: access is a function of active role assignment, when an employee, contractor, or vendor separates from a role, all inherited permissions must be revoked without delay.
The misalignments identified in A2 below represent violations of one or more of these four principles.

A2. Identified Misalignments
Misalignment 1: Excessive privilege for a junior role (J. Lopez, Junior System Admin)
J. Lopez, a Junior System Admin hired in September 2023, holds “Domain admin” privilege, access equal to or exceeding that of T. Miller, the senior IT Administrator. This conflicts directly with RBAC's least-privilege principle, which requires that permission levels scale with the defined responsibilities of the role rather than exceed them. The system logs show that a PRIV_ESCALATION event on 2025-06-29 granted Domain Admin to j.lopez through a “manual_change.” The event indicates that the privilege was granted outside the role-based access structure described in the matrix.
Misalignment 2: Inconsistent permissions within an identical role (Customer Support Reps)
Two employees share the exact role title “Customer support rep”, R. Davis and J. Hall, yet their access differs: J. Hall additionally has access to the payroll system, which R. Davis does not. Under RBAC, permissions attach to the role, not the individual, so two accounts under an identical role should carry an identical permission set. The discrepancy indicates access was granted ad hoc rather than pulled from a role baseline, and it needlessly exposes sensitive payroll data to a role with no defined business need for it.
Misalignment 3: Access not revoked after termination (P. Ellis, HR Assistant)
P. Ellis, an HR Assistant, has a listed end date of 2025-05-20 (terminated), yet the account status remains “Active” with continued access to the HR portal and payroll system. RBAC's lifecycle principle requires that role assignment, and every permission inherited from it, be revoked the moment a user separates from the role. The system logs confirm the real-world consequence of this misalignment: a successful login for p.ellis is recorded more than three months after the listed termination date.
Misalignment 4: Excessive, unscoped privilege for a vendor role (vendor_finapp)
The vendor account “vendor_finapp” holds “Local admin on app server” for the finance application. As a non-employee, external role, RBAC calls for the minimum scoped permissions necessary for the specific support task, not administrative rights to the underlying server. Because the matrix does not identify a named individual responsible for this vendor account, the administrative privilege creates an accountability concern. If multiple vendor personnel use the account, administrative actions may not be traceable to a specific person.

A3. Recommendations to Resolve the Misalignments
Recommendation 1: Conduct a least-privilege review of all IT and administrative-tier roles and re-scope privileged rights (e.g., Domain Admin group membership) so they map to a defined role tier rather than an individual, out-of-band grant.
Justification: NIST SP 800-53, Revision 5, control AC-6 (Least Privilege) requires organizations to employ the principle of least privilege, allowing only the access necessary to accomplish assigned organizational tasks (NIST, 2020). This directly addresses Misalignment 1.
Recommendation 2: Establish a formal role-permission catalog as the single source of truth for provisioning, so that any account request is validated against the role's documented permission set before access is granted, with exceptions requiring documented business justification.
Justification: ISO/IEC 27001:2022, Annex A controls A.5.15 (Access Control) and A.5.18 (Access Rights), require that access rights be allocated, reviewed, and adjusted based on documented business and security requirements rather than informal, individual requests (International Organization for Standardization [ISO/IEC], 2022). This directly addresses Misalignment 2.
Recommendation 3: Integrate account deprovisioning with HR and contract-management lifecycle events, so that access is automatically suspended on or before an employee's, contractor's, or vendor's listed end date, rather than depending on a manual seven-day notice.
Justification: NIST SP 800-53 control AC-2 (Account Management) calls for automated mechanisms to disable accounts upon termination or a defined trigger event (NIST, 2020), and the (ISC)² SSCP Common Body of Knowledge Access Controls domain likewise identifies prompt deprovisioning as a core control for limiting exposure from separated personnel ((ISC)², n.d.). This directly addresses Misalignment 3 and supports the lifecycle component of Misalignment 4. The vendor's excessive privilege is addressed separately through the scoped application-level access change in A4.
 
A4. Revised User Role Matrix
The matrix below reflects FinSecure Corp.'s user role assignments under a formally applied RBAC model. Cells shaded and bolded in yellow indicate changes made to resolve the four misalignments identified in A2; all other rows were reviewed against their role's baseline and found to be consistent with least privilege and role standardization.
Role	Assigned User	Employment Type	Dept.	Account Status	System Access	Privilege Level
Finance manager	A. Jones	Employee	Finance	Active	Payroll system, budget tracker	Full access
Finance analyst	L. Cheng	Employee	Finance	Active	Payroll system, budget tracker, CRM	Full access
HR coordinator	M. Singh	Employee	HR	Active	HR portal, payroll system	Read and write
Security analyst	K. Patel	Employee	SecOps	Active	SIEM, network logs, firewall console	Read only
IT administrator	T. Miller	Employee	IT	Active	All internal systems	Full admin
Junior system admin	J. Lopez	Employee	IT	Active	Domain controller (read-only queries), IT support/ticketing tools	Standard admin , least privilege (no Domain Admin group membership)
Customer support rep	R. Davis	Employee	Support	Active	CRM, email server	Read only
Customer support rep	J. Hall	Employee	Support	Active	CRM, email server	Read only
External auditor	D. Nguyen	Contractor	Finance	Access revoked (contract ended 2025-06-15)	None	N/A
Marketing manager	C. Wright	Employee	Marketing	Active	Email server, CRM	Full access
HR assistant	P. Ellis	Employee	HR	Terminated , access revoked (2025-05-20)	None	N/A
Business analyst	S. Kumar	Employee	Finance	Active	Budget tracker, CRM	Read only
Network engineer	R. Lee	Employee	IT	Active	Firewall console, VPN gateway	Admin
Compliance officer	T. Nguyen	Employee	Compliance	Active	Audit logs, policy repository	Read only
Contractor , IT audit	M. Brown	Contractor	SecOps	Active	SIEM, network logs	Read only
Payroll specialist	H. Alvarez	Employee	Finance	Active	Payroll system	Read and write
Data engineer	E. Romero	Employee	Data	Active	Data warehouse, SFTP gateway	Write on ETL paths
Service account , Payroll ETL	svc_payroll_etl	Service	IT	Active	SFTP gateway, payroll system API	Automation token
Vendor support , Finance app	vendor_finapp	Vendor	Finance/IT	Active	VPN gateway, finance app server	Scoped application-level access , no local admin
Help desk technician	B. Ortiz	Employee	IT	Active	Ticketing tool, AD users and computers	Help desk admin (password reset)
Read-only reporting (shared)	report_ro	Shared	Finance	Active	Budget tracker, finance reports	Read only
QA analyst	J. Carter	Employee	QA	Active	Staging web app, test DB	DB read and write (staging)
Facilities coordinator	L. Smith	Employee	Facilities	Active	Badge system, work order system	Badge admin, WO write
Intern , Marketing	P. Rivera	Intern	Marketing	Active	Email server, share drive (marketing)	Read and write (marketing)
 
B. Access Control Policy Evaluation
B1. Policy Gaps or Inconsistencies
Gap 1: Default VPN provisioning (Policy Section 6, Remote and External Access)
The policy states, “VPN access shall be provisioned by default for all users.” Granting remote network access by default, rather than based on documented job need, directly contradicts the least-privilege principle and unnecessarily expands the organization's external attack surface, every account, regardless of role, becomes a potential remote entry point.
Gap 2: Preference for shared accounts (Policy Section 7, Service Accounts and Shared Accounts)
The policy states that “shared accounts shall be provisioned instead of named user accounts to limit license expenses.” This undermines individual accountability and audit integrity: actions taken under a shared account cannot be traced to a specific person, which weakens both incident investigation and access review.
Gap 3: Unrestricted access to logs (Policy Section 9, Logging and Monitoring)
The policy states, “Access to logs shall be unrestricted to aid in troubleshooting.” Audit logs are themselves a sensitive security asset; unrestricted access threatens both their confidentiality and their integrity, since anyone, including a compromised or malicious account, could view, alter, or clear evidence of an incident. The system logs also show a SIEM alert being cleared as a “false positive” by a security analyst, demonstrating the importance of controlled access, accountability, and independent review of security monitoring activities. The log excerpt does not establish that the alert was improperly cleared or that the action resulted from unrestricted log access.

B2. Recommended Policy Changes
For Gap 1: Revise Section 6 to require that VPN access be provisioned only when a documented business or job function requires it, approved through the standard access request process, and segmented so that VPN sessions land only in the network zones the requesting role needs, with multifactor authentication required for all remote sessions (NIST SP 800-53 control AC-17, Remote Access; NIST, 2020).
For Gap 2: Revise Section 7 to prohibit shared accounts for standard human access; require named, individually attributable accounts for every employee, contractor, and vendor user. Reserve shared or service-style accounts strictly for automated system-to-system functions, each with an assigned business owner and credentials stored in a Privileged Access Management (PAM) vault (CIS Controls, Control 5, Account Management; CIS, 2021).
For Gap 3: Revise Section 9 to restrict log and SIEM access to designated security and compliance roles only, log all access to the logging system itself, and require independent, two-person review before any security alert can be closed or classified as a false positive (NIST SP 800-53 control AU-9, Protection of Audit Information; NIST, 2020).

C. Operational Practices Evaluation
Weakness 1: Informal, unreviewed change management
Weakness: FinSecure's change management process has no formal requirement to log changes in a ticketing system, permits emergency changes without managerial approval, and includes no post-change review step.
CIA impact: integrity and availability. Undocumented changes to critical security devices can introduce misconfigurations or unauthorized modifications that go unnoticed, and without a review or rollback step, a harmful change can persist or cause an outage. This is not hypothetical: the system logs show a firewall rule change made by t.miller with “ticket_id=None,” meaning a modification to a core security control left no formal record of what changed, why, or who approved it.
Recommendation: Require all changes, including emergency changes, to be logged in a ticketing system with a documented justification, rollback plan, and mandatory post-implementation review, consistent with configuration change control practices described in NIST SP 800-53, control CM-3 (NIST, 2020).
Weakness 2: Minimal, non-recurring security awareness training
Weakness: Employees receive only a short annual awareness email and a five-minute verbal onboarding briefing; no phishing simulations or practical exercises are conducted.
CIA impact: This primarily threatens confidentiality. Employees who are not regularly trained to recognize phishing and social engineering are more likely to have credentials or data compromised. The system logs show a pattern consistent with this risk: user j.hall had five failed login attempts followed by a successful login from an unfamiliar foreign IP address (“geo=Unknown, outside normal range”), and the following day the same user's system generated a DNS query to a domain flagged as known-malicious, a sequence consistent with a phished or compromised account.
Recommendation: Implement recurring, role-based security awareness training with quarterly phishing simulations and tracked completion rates, consistent with NIST SP 800-53, control AT-2, Literacy Training and Awareness (NIST, 2020).
Weakness 3: Untracked assets with no sanitization process
Weakness: Laptops are assigned to employees but not tracked in an inventory database, departing employees are only “expected” to return devices with no formal verification, and there is no documented process for wiping or reimaging returned devices.
CIA impact: This primarily threatens confidentiality. A device that is never verified as returned, or that is reissued without being wiped, can carry sensitive organizational data, financial records, payroll data, customer information, outside the organization's control long after an employee's departure.
Recommendation: Implement a centralized IT asset inventory (CMDB) requiring every device to be tagged, tracked, and verified at offboarding, and mandate a documented sanitization procedure before any device is reissued or disposed of, consistent with NIST SP 800-53 controls CM-8, System Component Inventory, and MP-6, Media Sanitization (NIST, 2020).

D. System Log Analysis
D1 & D2. Identified Threats, Classification, and Mitigation
Threat 1: Unauthorized privilege escalation, Insider threat / privilege misuse
Evidence: On 2025-06-29 at 10:25 UTC, adsvc logged a PRIV_ESCALATION event granting user j.lopez the “Domain Admin” privilege via “manual_change”, outside any visible approval workflow.
Classification: Insider threat / unauthorized privilege escalation.
Mitigation: Deploy a Privileged Access Management (PAM) solution that requires a formal, multi-party approval workflow for any change to privileged groups (such as Domain Admins) and generates an automatic alert on out-of-band manual changes, paired with an immediate least-privilege re-certification of the affected account.
Justification: This mitigation maps to the Select and Monitor steps of the NIST Risk Management Framework: selecting AC-6 (Least Privilege) controls to constrain who can hold Domain Admin rights and continuously monitoring privileged group membership for unauthorized change (NIST, 2018).
Threat 2: Brute-force login followed by anomalous access, Brute force / account takeover
Evidence: User j.hall had a LOGIN_FAILURE event indicating attempt 5 of 5 from IP 185.63.145.20, immediately followed by a LOGIN_SUCCESS from the same IP with “geo=Unknown (outside normal range).” The next day, the same user's system generated a SUSPICIOUS_QUERY to a domain flagged as known-malicious.
Classification: Credential attack / potential account compromise.
Mitigation: Enforce account lockout or authentication throttling after repeated failed attempts, require multifactor authentication for all remote and cloud logins, and configure geo-velocity (“impossible travel”) alerting to flag and hold logins from unfamiliar locations pending verification.
Justification: This aligns with CIS Controls, Control 6, Access Control Management, which calls for centralized authentication controls including MFA and monitoring of authentication events for anomalies (CIS, 2021).
Threat 3: Malware infection, Malware (Remote Access Trojan)
Evidence: On 2025-06-29 at 14:54 UTC, the SIEM logged a MALWARE_ALERT on host 10.1.1.45, associated with user k.patel, identifying the “GhostX RAT” malware family at HIGH severity.
Classification: Malware (Remote Access Trojan).
Mitigation: Immediately isolate the affected host from the network, use endpoint detection and response (EDR) tooling to eradicate the malware, and initiate a formal incident response process, including root-cause analysis to determine the infection vector before the host is returned to service.
Justification: This mitigation follows the containment, eradication, and recovery phases described in NIST SP 800-61, Revision 2, the Computer Security Incident Handling Guide (NIST, 2012).
