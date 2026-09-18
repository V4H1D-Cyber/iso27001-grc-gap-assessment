# Access Control Policy

**Document owner:** Chief Technology Officer
**Applies to:** All systems storing, processing, or transmitting company or customer data.
**Related controls:** ISO/IEC 27001:2022 Annex A 5.15 (Access control), 5.16 (Identity management), 5.17 (Authentication information), 5.18 (Access rights), 8.2 (Privileged access rights), 8.5 (Secure authentication)

## 1. Purpose

This policy establishes the principles governing who can access company systems and data, and under what conditions, to ensure access is granted on a least-privilege, need-to-know basis.

## 2. Principles

- **Least privilege:** users are granted the minimum level of access required to perform their job function, and no more.
- **Need-to-know:** access to customer data is restricted to roles that require it for legitimate business purposes.
- **Segregation of duties:** no single individual should be able to both introduce and approve a change to production systems without independent review (see 5.3).
- **Time-bound elevation:** administrative/privileged access is granted for a defined period tied to a specific task, not held permanently by default.

## 3. Account Provisioning

- All new accounts are provisioned through the company's SSO provider (Google Workspace) wherever the target system supports it.
- Standalone (non-SSO) credentials are permitted only where a system genuinely does not support SSO, and must be stored in the company password manager, never in plaintext or shared documents.
- Every new account request must specify the business justification and be approved by the requester's manager before provisioning.

## 4. Authentication Requirements

- Multi-factor authentication (MFA) is mandatory on all accounts that support it, with no exceptions for convenience.
- Passwords must meet the company's minimum complexity standard and must never be reused across personal and company accounts.
- Service accounts and API keys must be rotated at least every 90 days, or immediately upon suspected compromise or when an employee with knowledge of the credential departs.

## 5. Privileged Access

- Standing administrator/root access to production infrastructure is prohibited by default.
- Privileged access must be requested per-task, time-boxed (target: expiring automatically within 8 hours), and logged.
- All privileged sessions to production systems must be logged in a manner that supports later review.

## 6. Access Review

- Access rights for all systems handling customer data must be reviewed at least quarterly by the relevant system owner.
- Any account with no legitimate business justification identified during review must be revoked within 5 business days.

## 7. De-provisioning

- Upon termination or role change, access must be revoked across all systems on the employee's last working day, following the offboarding checklist.
- IT must confirm and document completion of de-provisioning within 24 hours of an employee's departure.

## 8. Exceptions

Any exception to this policy must be documented, time-limited, and approved in writing by the CTO.

## 9. Review

This policy will be reviewed annually or following any access-related security incident.

*Version 1.0 — [Date]. Approved by: [Name/Title].*
