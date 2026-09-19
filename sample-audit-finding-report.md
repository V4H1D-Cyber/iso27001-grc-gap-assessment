# IT Security Audit Report — Meridian FinTech Ltd (Fictional)

**Engagement:** ISO/IEC 27001:2022 Readiness Assessment — Detailed Findings
**Scope:** Information Security Management System (ISMS) covering cloud infrastructure (AWS), core application, and supporting SaaS tooling
**Report type:** Practice engagement — self-directed portfolio exercise, not a real client audit
**Prepared by:** Vahid Bhasha Shaik
**Date:** September 2026

This report presents the three highest-severity findings from the full 93-control gap assessment in this repository (`iso27001-gap-assessment.csv`), written in the format an IT auditor or GRC analyst would actually deliver to a client or internal stakeholder — not as a raw control checklist, but as findings with evidence, root cause, business impact, and a specific recommendation. The full risk register (`risk-register.csv`, 12 risks) and remediation roadmap cover the complete scope; this document demonstrates the audit-reporting layer on top of that underlying work.

## Executive summary

The assessment identified 39 of 93 Annex A controls as Not Implemented and 37 as Partial, concentrated most heavily in access governance, security monitoring, and secure development practice. None of the findings below reflect malicious intent or gross negligence — they reflect the common and predictable pattern of an early-stage, engineering-led company that has grown faster than its security program, where controls exist informally (a password manager, PR reviews, cloud backups) but haven't been formalized, enforced, or tested. The three findings in this report were selected because they carry the highest combined likelihood and impact (Risk Score 16–20 on the register's 25-point scale) and because remediating them would meaningfully reduce the organization's overall risk exposure ahead of any of the lower-severity, longer-tail items.

---

## Finding AF-01: Standing privileged access with no time-boxed elevation or periodic review

| Field | Detail |
|---|---|
| Risk rating | **Critical** (Risk Score: 20) |
| Related risk register entry | R-01 |
| ISO/IEC 27001:2022 controls | 8.2 (Privileged access rights), 5.18 (Access rights) |
| NIST CSF 2.0 category | PR.AA — Identity Management, Authentication, and Access Control |

**Condition.** Multiple engineers hold standing, permanent administrator/root-level access to production AWS infrastructure. This access is not time-limited, is not subject to a defined approval workflow for elevation, and has never been formally reviewed or recertified since it was originally granted.

**Criteria.** ISO/IEC 27001:2022 Annex A 8.2 requires that the allocation and use of privileged access rights be restricted and managed, and 5.18 requires that access rights be provisioned, reviewed, modified, and removed in accordance with the organization's access control policy — including periodic review.

**Cause.** No access-governance process was ever formally defined as the engineering team grew; access was granted ad hoc based on immediate operational need (e.g., a new engineer needing to debug a production issue) and was never revisited once the immediate need passed.

**Effect.** Any one of the engineers holding standing admin access represents a single point of compromise for the entire production environment — a phished credential, a leaked API key, or a malicious insider action would grant an attacker or bad actor unrestricted access with no additional barrier. This also makes it materially harder to answer a basic incident-response question ("who could have done this?") during any future investigation, since the pool of people who *could* have taken a given production action is effectively "everyone with standing access," not a specific, auditable set.

**Recommendation.** Replace standing access with time-boxed, role-based elevation (for example, AWS IAM Identity Center session-based administrative roles that expire automatically), and introduce a quarterly access-recertification cycle where each privileged grant is explicitly re-approved by a named owner or automatically revoked. This is scoped as a 30-day action in the remediation roadmap because it is achievable through configuration changes to existing AWS tooling, without new budget or vendor procurement.

**Management response.** *Not applicable — practice engagement; no management response was collected.*

---

## Finding AF-02: Customer PII used unmasked in the staging environment

| Field | Detail |
|---|---|
| Risk rating | **Critical** (Risk Score: 20) |
| Related risk register entry | R-02 |
| ISO/IEC 27001:2022 controls | 8.11 (Data masking), 8.33 (Test information) |
| NIST CSF 2.0 category | PR.DS — Data Security |

**Condition.** Production customer data, including personally identifiable information, is copied directly into the staging environment for testing purposes without any masking, anonymization, or synthetic-data substitution.

**Criteria.** Annex A 8.11 requires data masking to be used in accordance with the organization's data classification and access-control policies, and 8.33 requires that test information be appropriately selected, protected, and controlled. Beyond the ISO control language, this practice also creates direct exposure under India's Digital Personal Data Protection (DPDP) Act, which does not distinguish between "production" and "test" environments when it comes to an organization's obligations over personal data it holds.

**Cause.** Staging was set up early, before the company held meaningful volumes of customer PII, as a fast way to get realistic test data without building a synthetic-data pipeline. As the customer base grew, the practice was never revisited even though the risk profile changed substantially.

**Effect.** Staging environments are conventionally held to a lower security bar than production — access is broader, monitoring is thinner, and change control is looser, on the reasonable assumption that "it's just test data." Here, that assumption is false: a compromise of staging (through a less-hardened dependency, a misconfigured access rule, or a departing engineer's lingering access) would expose the exact same customer PII as a production breach, while being defended like a much lower-value target.

**Recommendation.** Implement data masking or synthetic-data generation for all non-production environments before the next scheduled data refresh from production to staging, and treat this as a hard gate — no further production-to-staging data copies should occur unmasked, even temporarily, while a masking pipeline is being built.

**Management response.** *Not applicable — practice engagement; no management response was collected.*

---

## Finding AF-03: No centralized security log monitoring or SIEM capability

| Field | Detail |
|---|---|
| Risk rating | **High** (Risk Score: 16) |
| Related risk register entry | R-03 |
| ISO/IEC 27001:2022 controls | 8.16 (Monitoring activities), 5.24 (Information security incident management planning and preparation) |
| NIST CSF 2.0 category | DE.CM — Continuous Monitoring |

**Condition.** Application logs are centralized in AWS CloudWatch, but no one actively and routinely reviews them for security-relevant activity, and there is no SIEM or equivalent alerting layer that would surface a suspicious pattern (repeated failed logins, unusual API call volume, access from an unexpected geography) without a human proactively going looking for it.

**Criteria.** Annex A 8.16 requires networks, systems, and applications to be monitored for anomalous behavior, and 5.24 requires that the organization plan and prepare for managing information security incidents — which is not meaningfully possible without a mechanism to detect that an incident is happening in the first place.

**Cause.** Logging infrastructure (CloudWatch) was adopted for operational/debugging purposes, not security purposes, and no one on the small engineering team has been assigned security-monitoring responsibility as a distinct function from general operations.

**Effect.** The organization currently has no reliable way to detect an active compromise except by accident — a customer report, a billing anomaly, or a public disclosure. Combined with Finding AF-01 (standing privileged access), this means that if credential compromise did occur, an attacker's access could persist for an extended, undefined period before being noticed, materially increasing the likely scope of any breach.

**Recommendation.** Stand up a lightweight monitoring capability — this does not require an enterprise SIEM; a focused deployment of an open-source stack (e.g., the ELK stack) or a lightweight cloud-native option (e.g., Microsoft Sentinel's consumption-based tier, demonstrated hands-on in this author's companion [SOC Sentinel lab](https://github.com/V4H1D-Cyber/azure-sentinel-soc-lab)) ingesting authentication events, administrative actions, and infrastructure-level logs, with a small initial set of high-confidence alerting rules (impossible travel, repeated auth failures, privilege escalation events) is sufficient to close the most dangerous part of this gap within the 60-day window in the roadmap.

**Management response.** *Not applicable — practice engagement; no management response was collected.*

---

## Overall conclusion

None of these three findings require the organization to purchase new enterprise tooling or make a large capital investment — each is addressable through configuration of infrastructure Meridian FinTech already owns (AWS IAM, existing CloudWatch logs, existing CI/CD pipeline). That is a deliberate and realistic feature of this exercise, not a coincidence: most early-stage companies' highest-severity gaps are process and configuration gaps, not missing-technology gaps, and a GRC analyst's recommendations are most useful — and most likely to actually get implemented — when they're scoped that way rather than defaulting to "buy a bigger security product."
