# ISO/IEC 27001:2022 ↔ NIST CSF 2.0 Crosswalk

Most real organizations don't pick one framework and stop — a company might certify against ISO/IEC 27001 for customer/vendor assurance while using NIST CSF 2.0 internally to talk to the board about risk, since CSF's six functions (Govern, Identify, Protect, Detect, Respond, Recover) are a more intuitive way to communicate posture to a non-technical audience than 93 Annex A control numbers. Being able to translate a finding from one framework's language into the other's is ordinary day-to-day GRC work, so this crosswalk maps the assessment in this repo onto NIST CSF 2.0 as a second lens on the same evidence.

NIST CSF 2.0 categories referenced below are taken directly from the official framework core (NIST CSWP 29, Feb 2024).

## 1. Annex A theme → CSF function (high-level orientation)

| ISO/IEC 27001:2022 Annex A theme | Controls | Where it mostly lands in CSF 2.0 |
|---|---|---|
| Organizational (5.1–5.37) | Policy, roles, supplier management, incident management, compliance | Spans **GV** (Govern) almost entirely for policy/roles/compliance controls, plus **RS** (Respond) for the incident-management controls (5.24–5.28) and **GV.SC** for the supplier controls (5.19–5.22) |
| People (6.1–6.8) | Screening, training, termination, incident reporting | **PR.AT** (Awareness and Training) and **GV.RR** (Roles, Responsibilities, and Authorities) |
| Physical (7.1–7.14) | Physical/environmental security | **PR.IR** (Technology Infrastructure Resilience) — mostly Not Applicable here since Meridian FinTech is remote-first with no owned premises |
| Technological (8.1–8.34) | Access control, logging, crypto, secure development, backups | Spans **PR.AA**, **PR.DS**, **PR.PS**, **DE.CM**, **DE.AE**, and **RC.RP** depending on the specific control |

This is a general orientation, not a 1:1 mapping — ISO and CSF slice the same problem space differently, so a single Annex A control sometimes touches two or three CSF categories at once. The table below is more precise because it maps at the level of an actual assessed risk rather than a framework category label.

## 2. Risk register → NIST CSF 2.0 category (detailed mapping)

| Risk ID | Risk | ISO 27001 Controls | Primary CSF 2.0 Category | Secondary | Rationale |
|---|---|---|---|---|---|
| R-01 | Standing admin/root privileges, no time-boxed elevation | 8.2, 5.18 | **PR.AA** – Identity Management, Authentication, and Access Control | GV.RM | This is a textbook least-privilege gap — CSF files it under access control, not detection or governance, even though the *cause* (no periodic access review) is a risk-management process failure |
| R-02 | Unmasked production PII used in staging | 8.11, 8.33 | **PR.DS** – Data Security | — | Data-in-non-production-environments is explicitly a data-protection control, not an access-control one, since the people who can see it are already authorized engineers |
| R-03 | No centralized log monitoring / SIEM | 8.16, 5.24 | **DE.CM** – Continuous Monitoring | RS.MA | The absence itself is a detection gap; it also weakens incident management (RS.MA) downstream since nothing triggers a response |
| R-04 | Inconsistent offboarding de-provisioning | 6.5, 5.18 | **PR.AA** | GV.RR | Same access-control category as R-01; the ownership ambiguity (whose job is de-provisioning) is a GV.RR gap |
| R-05 | No SAST/DAST in CI/CD | 8.25, 8.28, 8.29 | **PR.PS** – Platform Security | — | CSF 2.0 groups secure development and vulnerability management for software/platforms under Platform Security |
| R-06 | No DPDP Act compliance mapping | 5.31, 5.34 | **GV.OC** – Organizational Context | ID.RA | Understanding which legal/regulatory obligations apply is squarely an organizational-context function; the follow-on risk assessment of what happens if you don't comply is ID.RA |
| R-07 | Untested disaster-recovery runbook | 5.30, 8.14 | **RC.RP** – Incident Recovery Plan Execution | — | Backups existing (8.13, Implemented) is necessary but not sufficient — CSF's Recover function is specifically about the *tested plan*, not just the backup artifact |
| R-08 | MFA optional rather than enforced | 5.17, 8.5 | **PR.AA** | — | Authentication enforcement is the clearest possible PR.AA example |
| R-09 | No security awareness training | 6.3, 6.8 | **PR.AT** – Awareness and Training | — | Direct match |
| R-10 | No vendor security due-diligence process | 5.19, 5.20 | **GV.SC** – Cybersecurity Supply Chain Risk Management | — | CSF 2.0 elevated supply chain risk to its own Govern category specifically because of gaps like this one |
| R-11 | Inconsistent auth/admin-action logging | 8.15, 5.28 | **DE.AE** – Adverse Event Analysis | DE.CM | Logging exists partially but can't reliably support reconstructing what happened after the fact — that's an analysis capability gap, not just a monitoring one |
| R-12 | No enforced segregation of duties on deploys | 5.3, 8.32 | **GV.RR** – Roles, Responsibilities, and Authorities | PR.PS | The root cause is that "who can deploy unreviewed" was never formally defined — a roles/authorities gap that happens to manifest as a platform-security risk |

## 3. What this shows

Every risk in this assessment lands somewhere in CSF's six functions, and — notably — **none of the 12 highest-priority risks land in Recover (RC.CO) or most of Identify (ID.AM, ID.IM)**, which tracks with the honest picture in the main gap assessment: Meridian FinTech's problems are concentrated in access control, detection, and governance maturity, not in fundamentally not knowing what it owns. That kind of pattern — where the *shape* of an organization's gaps clusters in a predictable place — is exactly the kind of observation a GRC analyst or auditor is expected to surface, not just a control-by-control checklist.
