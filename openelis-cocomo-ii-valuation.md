# COCOMO II Valuation of OpenELIS Global 2

*Post-Architecture model, measured from the `DIGI-UW/OpenELIS-Global-2` repository on 21 April 2026 (`develop` branch, latest commit).*

---

## Headline

| Metric | Value |
| --- | --- |
| Adjusted size | **~736 KSLOC** |
| Effort | **~4,514 person-months (~376 person-years)** |
| Nominal schedule | **~53 months (~4.4 years)** |
| Average parallel staff | **~86 FTE** |
| Estimated value @ $15,000/PM (US) | **~$67.7 million** |
| Estimated value @ $7,500/PM (global blended) | **~$33.9 million** |

---

## What Was Included

The `DIGI-UW/OpenELIS-Global-2` repository is a **monorepo** — unlike OpenMRS, which spreads across dozens of separate repos, OpenELIS ships backend, frontend, database schema, FHIR layer, analyzer tooling, and deployment configuration all in one place. This means a single clone captures essentially the full codebase. Submodules were included with shallow checkout (`--recurse-submodules --shallow-submodules`):

- **Backend platform** — `src/` (Java/Spring, ~338K code lines): the full service layer, REST API, domain model, and Spring Security configuration
- **Frontend** — `frontend/` (React/JSX, ~149K code lines): the new React UI replacing the JSP legacy interface
- **Legacy UI** — `ROOT/WebContent/` (JSP, ~40K code lines): the original Struts/JSP interface, still shipped
- **Database schema** — `db/` (SQL/PLpgSQL, ~119K code lines): Liquibase-managed PostgreSQL migrations, stored procedures, reference data
- **FHIR layer** — `fhir/` directory and associated Java code: FHIR R4 integration
- **Submodules included**: `plugins/`, `tools/Liquibase-Outdated`, `tools/Password-Migrator`, `tools/analyzer-mock-server`, `tools/openelis-analyzer-bridge`
- **Excluded** from count: `dataexport/` submodule (separate Git repo tracked separately), `node_modules`, `dist`, `build`, `target`, `.git`

---

## From cloc Output to COCOMO SLOC

`cloc` 1.90 was run against the shallow clone with exclusions for generated/binary/config directories. The raw output yielded **958,636** total non-blank, non-comment lines across 6,126 unique files. COCOMO expects logical source lines; partial-weight treatment is applied by language:

**Full weight (1.0)** — where the real engineering effort lives:
- Java, JavaScript, JSX, TypeScript, Python

**Partial weight** — real effort but less dense than procedural code:
- SQL / PLpgSQL: **0.50** — schema design and stored procedure logic is engineering, but verbose
- JSP: **0.50** — templating with embedded logic, but mixed declarative content
- CSS / SCSS / LESS: **0.40** — UI styling effort is real but lighter per line
- XML: **0.25** — Spring config, Liquibase changesets, and Maven POMs dominate (129K lines), similar to OpenMRS treatment
- Properties: **0.20** — i18n/config bundles
- Maven / XSLT: **0.25**
- HTML: **0.50**

**Zero weight** — configuration, infrastructure, documentation: YAML/YML, JSON, Dockerfiles, shell scripts, Markdown, TOML (excluded via `--exclude-ext` or weighted zero)

### cloc Results by Language

| Language | Files | Code Lines | Weight | Adjusted SLOC |
| --- | ---: | ---: | ---: | ---: |
| Java | 3,402 | 338,082 | 1.00 | 338,082 |
| JSX | 488 | 149,464 | 1.00 | 149,464 |
| XML | 866 | 129,287 | 0.25 | 32,322 |
| SQL / PLpgSQL | 323 | 119,301 | 0.50 | 59,651 |
| JavaScript | 313 | 88,875 | 1.00 | 88,875 |
| JSP | 167 | 40,209 | 0.50 | 20,105 |
| CSS | 110 | 31,855 | 0.40 | 12,742 |
| Properties | 29 | 16,309 | 0.20 | 3,262 |
| Python | 158 | 13,507 | 1.00 | 13,507 |
| TypeScript | 122 | 12,092 | 1.00 | 12,092 |
| SCSS | 39 | 7,619 | 0.40 | 3,048 |
| LESS | 18 | 4,718 | 0.40 | 1,887 |
| Maven POM | 45 | 4,134 | 0.25 | 1,034 |
| XSLT | 5 | 897 | 0.25 | 224 |
| HTML | 3 | 210 | 0.50 | 105 |
| Other (Perl, PowerShell, misc) | — | 2,077 | 0.00 | 0 |
| **Total** | **6,126** | **958,636** | — | **736,398** |

**Full-weight code total: 602,020 lines**
**Partial-weight total (weighted): 134,378 lines**
**Adjusted SLOC: ~736 KSLOC**

---

## COCOMO II Post-Architecture Formula

$$PM = A \cdot (Size)^E \cdot \prod_{i=1}^{17} EM_i$$

where

$$E = B + 0.01 \cdot \sum_{j=1}^{5} SF_j$$

and schedule:

$$TDEV = C \cdot (PM)^{D + 0.2 \cdot 0.01 \cdot \sum SF_j}$$

Calibrated constants (Boehm 2000): **A=2.94, B=0.91, C=3.67, D=0.28**

---

## Scale Factor Ratings

| Factor | Rating | Value | Rationale |
| --- | --- | --- | --- |
| **PREC** — Precedentedness | High | 2.48 | Laboratory IS is a well-understood problem domain. OpenELIS has a ~20-year lineage (originally ITECH/UW); the DIGI-UW team knows the territory well. |
| **FLEX** — Development flexibility | High | 2.03 | OSS project; requirements are community-driven and implementer-tailored across many country contexts. No single requirements authority. |
| **RESL** — Architecture/risk resolution | Nominal | 4.24 | The codebase is in active architectural transition — JSP legacy UI co-exists with a new React frontend, and analyzer integration patterns are evolving. Risk is being resolved but not yet settled. |
| **TEAM** — Team cohesion | Nominal | 3.29 | Internationally distributed contributors across UW, I-TECH, and country implementer teams. Cooperation is good but coordination friction is inherent at this scale. |
| **PMAT** — Process maturity | Low | 6.24 | Strong CI/CD (GitHub Actions, backend + frontend + E2E pipelines), PR reviews, code formatters (Spotless, Prettier), and release cadence — roughly CMM Level 2. No formal process assessment on record. |

**Sum of SF = 18.28 → E = 0.91 + 0.1828 = 1.0928** (slight diseconomy of scale)

---

## Effort Multiplier Ratings

| Multiplier | Rating | Value | Rationale |
| --- | --- | --- | --- |
| RELY (required reliability) | High | 1.10 | Lab results directly affect patient care and clinical decisions. |
| DATA (database size) | High | 1.08 | Large PostgreSQL schema: test catalog, reference ranges, patient records, FHIR resources, 119K SQL lines of migrations and procedures. |
| CPLX (product complexity) | High | 1.17 | Analyzer integration (ASTM/HL7/proprietary protocols), FHIR R4 layer, complex workflow engine (pathology, cytology, QC), dual UI (JSP + React), multi-country deployment. |
| RUSE (reusability) | High | 1.07 | Explicitly designed for reuse across diverse health system contexts; plugin architecture, configurable workflows. |
| DOCU | Nominal | 1.00 | ReadTheDocs site + wiki + inline READMEs are adequate for the project's scale. |
| TIME | Nominal | 1.00 | No tight execution-time constraint in normal operation. |
| STOR | Nominal | 1.00 | No unusual main-storage constraint. |
| PVOL (platform volatility) | High | 1.15 | Spring Boot upgrades, React ecosystem churn, Liquibase migration management, PostgreSQL version dependencies, Docker orchestration changes. |
| ACAP (analyst capability) | Nominal | 1.00 | Mixed community; core analysts have strong lab IS domain knowledge. |
| PCAP (programmer capability) | Nominal | 1.00 | Mixed community capability across a global contributor base. |
| PCON (personnel continuity) | Low | 1.12 | OSS projects have inherently high contributor churn; global health software depends on grant cycles affecting staffing. |
| APEX (applications experience) | High | 0.88 | Core team and frequent contributors have deep lab IS / public health informatics domain experience. |
| PLEX (platform experience) | High | 0.88 | Java/Spring, React, PostgreSQL are well-understood by the contributor base. |
| LTEX (language/tool experience) | High | 0.91 | Commonly used languages and tools across the stack. |
| TOOL (software tools) | High | 0.90 | Modern CI pipelines, Spotless/Prettier formatters, Docker-based dev environment, Playwright + Cypress E2E, GitHub SpecKit for SDD. |
| SITE (multisite development) | High | 0.93 | Globally distributed but strong electronic collaboration (GitHub, Atlassian Jira, documented contribution workflow). |
| SCED (schedule constraint) | Nominal | 1.00 | OSS; ships on community readiness, not compressed schedule. |

**Product of EMs = 1.1299**

---

## Computed Results

$$PM = 2.94 \cdot (736.4)^{1.0928} \cdot 1.1299 \approx 4{,}514 \text{ person-months}$$

$$TDEV = 3.67 \cdot (4514)^{0.28 + 0.003656} \approx 53 \text{ months}$$

| Output | Value |
| --- | --- |
| Effort | **4,514 PM ≈ 376 person-years** |
| Nominal schedule | **~53 months ≈ 4.4 years** |
| Average headcount | **≈ 86 FTE over the schedule** |
| **Cost @ $15,000/PM (US fully-burdened)** | **~$67.7 million** |
| Cost @ $22,000/PM (US senior/2026 market) | ~$99.3 million |
| Cost @ $7,500/PM (global blended) | ~$33.9 million |

---

## Sanity Checks

**Per-LOC cost:** $67.7M / 736K ≈ **$92/LOC**. Basic COCOMO at US rates typically lands $30–40/LOC for simple systems. The ~2.5× multiplier is consistent with the combined effect of E > 1 (diseconomies of scale at 736 KSLOC) and above-nominal effort multipliers for reliability, complexity, and platform volatility — appropriate for a complex public health laboratory system.

**Against OpenMRS (same methodology, same date):** OpenMRS came in at ~$73.7M on 757 KSLOC. OpenELIS-Global-2 at ~$67.7M on 736 KSLOC is proportionally similar and defensible — both are large, complex, internationally-deployed medical OSS platforms in the same complexity class.

**Against project history:** OpenELIS has been in active development for ~20 years. 376 person-years over 20 years implies ~19 FTE/year average across the full community. Given a small paid core team plus implementer-contributed development, this is plausible. COCOMO's 86-FTE figure is the *nominal parallel staffing* to hit a 53-month schedule; OSS projects trade schedule compression for lower parallel staffing while preserving total effort.

**Against the GitHub language breakdown:** GitHub reports Java 57.7%, JavaScript 22.1%, PLpgSQL 15.0% — a heavily backend-weighted system. This is consistent with the cloc findings and confirms the adjusted SLOC figure isn't driven by inflated config or generated files.

---

## Caveats

**The `dataexport` submodule is not included.** It was excluded because it lives in a separate GitHub org (`I-TECH-UW/dataexport`) and tracking it as a submodule at a specific commit would require independent measurement. Including it would modestly increase the headline — probably +5–10%.

**SLOC weighting is judgment-based.** The treatment of 129K XML lines at 0.25 weight is the single largest judgment call (contributing ~32K adjusted SLOC). If XML were given 0.5 weight, total adjusted SLOC would rise ~8% and the cost estimate would increase proportionally. If all non-Java/JS/JSX/TS/Python were excluded entirely, adjusted SLOC would drop ~18%.

**Effort multipliers are calibrated to the project from the outside.** Ratings like CPLX, PCON, and RELY were assigned based on public code review and documentation. An OpenELIS core contributor walking through the 17 EMs would likely push CPLX toward Very High (analyzer integration is extremely complex), PCON lower (grant-dependent staffing), and RELY toward Very High (ISO/SLIPTA accreditation requirements). Each notch up on a high-weight EM multiplies the result.

**COCOMO II is calibrated to commercially-managed projects.** Applying it to an OSS community effort is the standard "what would it cost a single organization to rebuild this" thought experiment — useful as a cost floor for reimplementation, but not the actual historical investment of the global OpenELIS community.

**Labor rate spreads dominate uncertainty.** The gap between $33.9M (global blended) and $99.3M (US senior 2026) is a 3× range from the rate assumption alone — larger than uncertainty in any other input.

**The JSP legacy UI is included.** It adds ~40K raw lines / ~20K adjusted SLOC. One could argue it should be excluded as deprecated code on its way out; doing so would reduce the headline by ~4%.

---

## Data Quality Notes

**No anomalous file was found comparable to the OpenMRS `yarn.js` inflation issue.** The XML count (129K lines / 866 files ≈ 149 LOC/file average) is consistent with Liquibase changeset files and Spring XML configs — not a red flag. The SQL count (119K lines) is dominated by the `db/` migration directory, which is legitimate schema engineering. Python (158 files / 13.5K lines) represents automation scripts, test infrastructure, and tooling — correctly included at full weight.

---

## How to Tighten This Estimate

If a defensible number for a grant proposal, donor report, or valuation exercise is needed:

1. **Measure `dataexport` separately** and add its adjusted SLOC to the total.
2. **Convene an OpenELIS core contributor** to walk through the 17 effort multipliers — especially CPLX, RELY, and PCON. The conversation takes under 20 minutes and would materially reduce uncertainty.
3. **Decide explicitly what question the valuation answers**: "reimplementation cost" vs. "historical community investment" vs. "asset value for grant justification" — each uses different labor rates and framing.
4. **Exclude or separately report the JSP legacy layer** if the intent is to value the current-generation software only.
5. **Present as a range**, not a point estimate. A reasonable defensible range given current inputs is **$35M–$100M** depending on labor rate assumptions, with the $68M US-convention figure as the midpoint.

---

## Overall Confidence Assessment

The size measurement is the strongest part — the repo was actually cloned and measured at a specific commit with defensible weighting. The COCOMO II math is mechanically correct given the inputs. The effort multipliers are informed estimates from code review and documentation, not measurements.

If someone asked "is OpenELIS Global 2 worth roughly $35M–$100M as a software asset under varying labor conventions?" — **yes, with reasonable confidence**. If they asked "is it exactly $67.7M?" — **no**; that precision is false. The headline is the midpoint of a wide band.

---

*Measured: 21 April 2026 | Branch: `develop` | Tool: cloc 1.90 | Model: COCOMO II Post-Architecture (Boehm 2000)*
