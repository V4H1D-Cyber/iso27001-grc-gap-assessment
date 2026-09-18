# ISO/IEC 27001:2022 Gap Assessment & Risk Register

A self-directed GRC practice engagement: a full ISO/IEC 27001:2022 Annex A gap assessment, risk register, remediation roadmap, and a starter policy set, built the way a junior GRC analyst or IT auditor would actually structure this work for a real client engagement.

**This is explicitly a practice exercise, not a real client engagement.** The assessed organization, "Meridian FinTech Ltd," is fictional, built as a realistic small (~40-person, remote-first) fintech startup so the gaps and risks read like a genuine early-stage company's actual security posture — some real strengths (backups, source-code access control, environment separation), a lot of realistic gaps (no SIEM, no formal incident response plan, unmasked production data in staging), and nothing implausibly perfect or implausibly broken. I built this to demonstrate the GRC/IT-audit workflow end-to-end: control assessment → risk scoring → prioritized remediation → policy drafting.

## Why I built this

My hands-on background is in SOC monitoring and penetration testing (see my [SOC Sentinel lab](https://github.com/V4H1D-Cyber/azure-sentinel-soc-lab)), which is deep on the technical/detection side but doesn't demonstrate the governance, risk, and compliance side of security — control assessment, framework mapping, risk registers, and policy writing. This project fills that gap in my portfolio deliberately, using the same evidence-based, no-shortcuts approach as my other projects: every one of the 93 Annex A controls was individually assessed and justified, not just marked "TODO."

## Methodology

1. Defined the fictional target organization's profile (small fintech, cloud-native on AWS, remote-first, handling customer PII and payment-adjacent data) to ground every control judgment in a specific, plausible context rather than assessing controls in the abstract.
2. Walked all 93 controls across the four ISO/IEC 27001:2022 Annex A themes (Organizational, People, Physical, Technological), assigning a status of Implemented / Partial / Not Implemented / Not Applicable with a one-line evidence justification for each — see `iso27001-gap-assessment.csv`.
3. Converted the highest-impact gaps into a formal risk register (`risk-register.csv`), scoring each risk by likelihood × impact (1-5 scale) and mapping it back to the specific controls it relates to.
4. Built a remediation roadmap (`remediation-roadmap.md`) sequenced by risk-reduction-per-effort rather than by control number, split into 30/60/90-day phases.
5. Drafted three of the foundational policies the assessment identified as missing or informal (`policies/`), written as real, usable documents rather than placeholders.

## Results summary

| Status | Count (of 93 controls) |
|---|---|
| Implemented | 7 |
| Partial | 37 |
| Not Implemented | 39 |
| Not Applicable | 10 |

This distribution is intentional and realistic: most early-stage companies have a handful of controls genuinely nailed (usually the ones a cloud provider or SaaS tool gives them for free, like backups or TLS), a large middle band of "partial" controls that exist informally but aren't documented or consistently enforced, and a meaningful chunk of controls that simply haven't been addressed yet because nobody owns security full-time. That "Partial" band is deliberately the largest category here — it's the realistic shape of a company that cares about security but has never been through a formal assessment, and it's also where a GRC analyst's day-to-day work actually concentrates (turning "partial" into "implemented" is most of the job).

## Contents

| File | Purpose |
|---|---|
| `iso27001-gap-assessment.csv` | All 93 Annex A controls, status, and evidence/notes |
| `risk-register.csv` | 12 risks derived from the highest-impact gaps, scored and mapped to controls |
| `remediation-roadmap.md` | Prioritized 30/60/90-day remediation plan |
| `policies/Acceptable-Use-Policy.md` | Sample AUP (Annex A 5.10) |
| `policies/Access-Control-Policy.md` | Sample access control policy (Annex A 5.15-5.18, 8.2, 8.5) |
| `policies/Incident-Response-Policy.md` | Sample incident response policy (Annex A 5.24-5.28, 6.8), cross-referenced to the SOC playbooks in my detection-engineering lab |

## Author

Built as a personal GRC/IT-audit practice project by **Vahid Bhasha Shaik** (MSc Cyber Security, OSCP).
