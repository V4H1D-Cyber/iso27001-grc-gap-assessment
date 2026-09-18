# Information Security Incident Response Policy

**Document owner:** Chief Technology Officer
**Applies to:** All employees, contractors, and systems.
**Related controls:** ISO/IEC 27001:2022 Annex A 5.24 (Incident management planning and preparation), 5.25 (Assessment and decision on events), 5.26 (Response to incidents), 5.27 (Learning from incidents), 5.28 (Collection of evidence), 6.8 (Information security event reporting)

## 1. Purpose

This policy defines how Meridian FinTech Ltd (fictional) detects, reports, triages, responds to, and learns from information security incidents.

## 2. Definitions

- **Security event:** any observed occurrence that may indicate a security policy violation or control failure (e.g. a failed-login alert).
- **Security incident:** a confirmed event that has resulted in, or has a credible likelihood of resulting in, unauthorized access, data loss, or service disruption.

## 3. Severity Classification

| Severity | Definition | Example | Initial Response Target |
|---|---|---|---|
| Critical | Confirmed unauthorized access to customer data or production systems | Successful account compromise with data exfiltration | 15 minutes |
| High | Confirmed malicious activity with limited or unclear scope | Malware detected on an endpoint with production access | 30 minutes |
| Medium | Suspicious activity requiring investigation, not yet confirmed malicious | Unusual login location flagged by SSO | 4 hours |
| Low | Policy violation with no immediate security impact | An expired but unused credential found in a code repository | 1 business day |

## 4. Reporting

Any employee who observes or suspects a security incident must report it immediately via the designated internal channel (security@[company-domain] and the #security-incidents Slack channel). There is no penalty for reporting a false alarm in good faith — under-reporting is the greater risk.

## 5. Response Process

1. **Triage:** the on-call security point of contact confirms scope and severity within the target time above.
2. **Containment:** isolate the affected account, host, or service to stop further damage (see the role-specific playbooks in `../purple-team-exercise/playbooks/` for detailed technical steps on common incident types).
3. **Eradication:** remove the root cause (malware, compromised credential, malicious persistence mechanism).
4. **Recovery:** restore affected systems/accounts to normal operation, with heightened monitoring for a defined period afterward.
5. **Notification:** for any Critical or High incident involving customer data, legal/compliance must be notified within 24 hours to assess regulatory notification obligations (including under India's DPDP Act).

## 6. Evidence Handling

Logs, forensic images, and other evidence relevant to an incident must be preserved in their original form and access-restricted, in case of later legal or regulatory need.

## 7. Post-Incident Review

Every Medium-severity-or-above incident must have a written post-incident review within 5 business days covering: timeline, root cause, what worked, what didn't, and concrete follow-up actions with owners and dates.

## 8. Review

This policy will be reviewed annually and after every Critical or High-severity incident.

*Version 1.0 — [Date]. Approved by: [Name/Title].*
