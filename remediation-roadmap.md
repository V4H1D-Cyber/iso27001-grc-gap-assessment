# Remediation Roadmap — Meridian FinTech Ltd (Fictional) ISO 27001 Readiness

Prioritized against the risk register (`risk-register.csv`), sequenced so that the highest-severity, fastest-to-fix items land first, and slower structural work (audits, formal certification prep) comes after the operational basics are in place. Timeline is in days from assessment sign-off, matching the "Target Date" column in the risk register.

## Phase 1 — 0-30 days (Critical & quick-win items)

| Action | Addresses | Owner |
|---|---|---|
| Enforce MFA org-wide via SSO admin policy | R-08 (8.5, 5.17) | IT Admin |
| Replace standing admin/root access with time-boxed, role-based elevation | R-01 (8.2, 5.18) | CTO |
| Enforce mandatory second-reviewer approval on production deploy branch | R-12 (5.3, 8.32) | Engineering Lead |

## Phase 2 — 30-60 days (High-severity structural gaps)

| Action | Addresses | Owner |
|---|---|---|
| Build automated offboarding/de-provisioning checklist across all SaaS tools | R-04 (6.5, 5.18) | IT/People Ops |
| Introduce data masking for non-production environments | R-02 (8.11, 8.33) | Engineering Lead |
| Stand up baseline log monitoring / lightweight SIEM with core alerting rules | R-03 (8.16, 5.24) | CTO |
| Introduce SAST + a DAST scanning gate in CI/CD | R-05 (8.25, 8.28, 8.29) | Engineering Lead |
| Roll out security-awareness training and an incident-reporting channel | R-09 (6.3, 6.8) | People Ops |

## Phase 3 — 60-90 days (Compliance & resilience)

| Action | Addresses | Owner |
|---|---|---|
| Commission a DPDP Act-specific compliance gap assessment | R-06 (5.31, 5.34) | Compliance/Legal |
| Document and test a formal disaster-recovery runbook | R-07 (5.30, 8.14) | CTO |
| Introduce a vendor security due-diligence questionnaire | R-10 (5.19, 5.20) | Procurement/CTO |
| Extend logging coverage to authentication and admin actions | R-11 (8.15, 5.28) | Engineering Lead |

## Phase 4 — Beyond 90 days (Formal ISMS maturity)

Once the operational gaps above are closed, the remaining Annex A work is largely about *formalizing* what by then exists in practice rather than fixing hard gaps:

- Draft and get board sign-off on a versioned Information Security Policy (5.1) and supporting policy set (see `policies/`).
- Define a formal data classification scheme (5.12/5.13) and apply it retroactively to existing data stores.
- Commission an independent internal or third-party review of the ISMS (5.35) as a readiness check before considering formal ISO 27001 certification.
- Establish a recurring (e.g. quarterly) access-recertification cycle (5.18) and vendor security reassessment cadence (5.22).

## How to read this roadmap

This is deliberately sequenced by **risk reduction per unit of effort**, not by Annex A control numbering — a real gap assessment should never be worked top-to-bottom by control ID, since that ignores which gaps are actually dangerous versus merely undocumented. Phase 1 items are chosen because they're both high-risk *and* achievable with configuration changes rather than new tooling or budget; Phase 4 items are the ones that only make sense once the underlying practice already exists to formalize.
