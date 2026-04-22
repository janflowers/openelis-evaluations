# COCOMO Valuation Comparison: OpenMRS & OpenELIS vs. Other EMR and LIMS Software

*Prepared April 22, 2026*

---

## Framing the Comparison

This comparison cuts across two different types of numbers that are worth keeping distinct:

- **COCOMO replacement-cost valuations** — "what would it cost a single organization to rebuild this software from scratch?" This is what we computed for OpenMRS, OpenELIS-Global-2, and the Washington Health Summary.
- **Market pricing and implementation costs** — what proprietary vendors charge customers for licensing and deployment. This is what's publicly available for systems like Epic, LabWare, and LabVantage.

These are not the same thing, and conflating them is the most common error in discussions like this. The comparison below is careful about which number is which.

There is almost no published COCOMO II post-architecture literature on EMR or LIMS software at the same level of rigor as the valuations in this series. The main apples-to-apples reference is the USAID/Boston Consulting Group study from 2019.

---

## EMR Comparisons

### The USAID/BCG Global Digital Health Goods Study (2019)

The most direct published comparison. USAID commissioned Boston Consulting Group to apply COCOMO methodology to three global digital health goods:

| System | COCOMO Replacement Cost | Actual Direct Development Spend | In-Kind Total |
| --- | ---: | ---: | ---: |
| OpenMRS | ~$76M | ~$4–5M | ~$8M |
| CommCare | ~$22M | ~$15.6M (FTE-based) | — |
| iHRIS | ~$10.8M | ~$1.8–2.4M | — |

**Validation against our series:** Our April 2026 COCOMO II post-architecture run on OpenMRS came in at **$73.7M** — within 3% of the USAID/BCG figure, despite being run four years later on an expanded codebase. This is reasonable methodological validation. The gap reflects the difference between basic COCOMO (USAID) and post-architecture COCOMO II (this series), with codebase growth roughly offsetting the model differences.

The USAID annual maintenance estimates are also worth noting: iHRIS needs ~$1.25M/year, OpenMRS needs ~$2.8M/year, and CommCare needs ~$4.2M/year to sustain operations — none of which these systems receive in any given year.

### OpenMRS vs. OpenELIS-Global-2 (This Series)

| System | Adj. KSLOC | Effort (PM) | Cost @ $15K/PM | Model |
| --- | ---: | ---: | ---: | --- |
| OpenMRS (full ecosystem, 24 repos) | 757 | 4,913 | ~$73.7M | COCOMO II Post-Arch |
| OpenELIS-Global-2 (monorepo) | 736 | 4,514 | ~$67.7M | COCOMO II Post-Arch |

Both are large, mature, multi-decade platforms in the same complexity class — high reliability requirements, FHIR integration, complex workflows, internationally distributed contributor bases. The 8% difference in cost reflects slightly different team capability and effort multiplier profiles rather than any meaningful difference in scope.

### OpenEMR

OpenEMR is the other major open-source EMR and a natural comparator. Key facts:
- In continuous development since the early 2000s; GitHub commit history exceeds 10,000 commits
- Primarily PHP/JavaScript/SQL codebase
- No formal COCOMO II post-architecture valuation has been published at comparable rigor
- Open Hub's basic COCOMO estimates for individual OpenMRS sub-repos run in the low millions per repo, consistent with OpenEMR being in a similar overall tier to OpenMRS

A full COCOMO II post-architecture measurement of OpenEMR would be a useful addition to this series.

### Proprietary EMR: Epic

Epic is the dominant commercial EMR in the US market. COCOMO replacement cost is not how Epic publishes numbers, but implementation cost data provides a rough floor on the underlying investment:

- Epic's full software licensing for a large hospital system: **$2–10M** for the license alone
- Implementation of Epic at Mass General Brigham was initially projected at $1.2 billion; by 2018 total expenses reached **$1.6 billion**, with the software itself under $100M and the vast majority in lost revenues, support, and implementation labor
- Denmark's 2016 rollout of Epic across 18 hospitals cost **~$420M USD** (2.8 billion DKK)

These implementation figures are *customer-side total cost of ownership*, not development replacement cost. Epic employs ~13,000 people, has been in continuous development since 1979, and covers a dramatically broader scope than OpenMRS or OpenELIS (revenue cycle, scheduling, analytics, population health, and more). A COCOMO replacement cost estimate for Epic's full codebase would likely run into the **hundreds of millions to low billions** — consistent with it being a 45-year enterprise platform at a company with $4B+ in annual revenue.

**The key contrast:** OpenMRS and OpenELIS deliver $68–74M in software replacement value while being freely available. A comparable Epic implementation costs a single health system $10–500M+ just to *deploy* — before accounting for the underlying software development investment.

---

## LIMS Comparisons

### The Open-Source LIMS Landscape

There are essentially no published COCOMO valuations for major open-source or proprietary LIMS software at a rigor comparable to this series. The market is more fragmented than EMR, with dozens of niche systems.

**GNU LIMS (Occhiolino):** Open Hub estimates approximately 1 year of effort using basic COCOMO — an extremely small system compared to OpenELIS-Global-2's 736 KSLOC and ~376 person-years. GNU LIMS is functionally a lab module extension of GNU Health, not a full standalone LIMS platform.

**Bika LIMS / Senaite (Python/Plone):** A more apt OSS comparator to OpenELIS in terms of clinical lab scope and international deployment, but no published COCOMO estimate exists. Based on its codebase size and age (~20 years of active development), a COCOMO II estimate would likely fall in the **$15–40M** range — substantially smaller than OpenELIS due to a narrower feature scope and less complex architecture.

**C4G BLIS (Basic LIMS):** A small public health LIMS used in sub-Saharan Africa; COCOMO estimate would be in the low single-digit millions.

### Proprietary LIMS: LabWare and LabVantage

The two dominant enterprise LIMS platforms. Both are closed-source; no codebase metrics are public. Published pricing reflects customer-side costs:

| Vendor | Founded | License cost (enterprise) | Implementation cost | Annual maintenance |
| --- | --- | --- | --- | --- |
| LabWare | 1987 | $50K–$500K/yr | $10K–$100K+ | ~18–22% of license |
| LabVantage | 1981 | $50K–$100K+ perpetual | $5K–$50K+ | ~20% of license |
| STARLIMS (Abbott) | — | $550+/user/month | $10K–$60K | varies |

These are *customer acquisition costs*, not replacement cost. Both LabWare and LabVantage have been in development for ~35–40 years with large dedicated engineering teams. Their true COCOMO replacement costs are almost certainly in the **$50–150M range** — comparable to OpenELIS-Global-2 — though neither has published this.

**The global public health gap:** LabWare and LabVantage are designed for pharmaceutical, research, and reference lab contexts in high-income countries, with pricing that makes them inaccessible for public health laboratories in low- and middle-income countries. OpenELIS fills this gap with comparable technical sophistication at zero license cost — the COCOMO valuation quantifies exactly what that represents in replacement-cost terms.

---

## The Leverage Argument

The USAID framing captures the core insight: OpenMRS spent approximately $8M in actual development investment to produce software with a $74M replacement cost — roughly a **9:1 leverage ratio**. The open-source community model, grant funding, and implementer contributions collectively produce enterprise-grade software at a fraction of what a single organization would need to spend to recreate it.

The same argument applies to OpenELIS-Global-2: actual cumulative development investment (grant-funded, distributed across UW/DIGI, I-TECH, country implementers, and community contributors) is likely in the **$5–15M range** over ~20 years, against a COCOMO replacement cost of **~$68M**. That's a 5–14× leverage ratio.

| System | Est. Actual Investment | COCOMO Replacement Cost | Leverage Ratio |
| --- | ---: | ---: | ---: |
| OpenMRS | ~$8M | ~$74M | ~9× |
| OpenELIS-Global-2 | ~$5–15M (est.) | ~$68M | ~5–14× |
| iHRIS | ~$2M | ~$11M | ~5× |

This framing — replacement cost vs. actual investment — is the most compelling argument for sustained public funding of open-source global health digital goods. The question isn't "why fund open source?" but "how much would it cost to stop?"

---

## Summary Comparison Table

| System | Type | COCOMO Replacement Cost | Basis |
| --- | --- | ---: | --- |
| OpenMRS (full ecosystem) | OSS EMR | ~$73.7M | COCOMO II Post-Arch, April 2026 |
| OpenELIS-Global-2 | OSS LIMS | ~$67.7M | COCOMO II Post-Arch, April 2026 |
| OpenMRS (USAID/BCG) | OSS EMR | ~$76M | Basic COCOMO, 2019 |
| CommCare (USAID/BCG) | OSS mHealth | ~$22M | Basic COCOMO, 2019 |
| iHRIS (USAID/BCG) | OSS HRIS | ~$10.8M | Basic COCOMO, 2019 |
| WA Health Summary (4 repos) | OSS Health Data | ~$1.1M | COCOMO II Post-Arch, April 2026 |
| GNU LIMS (Occhiolino) | OSS LIMS | ~1 year effort | Basic COCOMO (Open Hub) |
| Epic Systems | Proprietary EMR | Not published (est. $500M–$2B+) | No public codebase |
| LabWare / LabVantage | Proprietary LIMS | Not published (est. $50–150M) | No public codebase |

---

## Caveats on Cross-System Comparisons

**Model differences matter.** The USAID/BCG study used basic COCOMO (coefficients a=2.4, b=1.05); this series uses COCOMO II post-architecture (A=2.94, B=0.91 + scale factors). Basic COCOMO tends to produce lower estimates on large codebases because it lacks the diseconomy-of-scale exponent. Direct numerical comparisons across studies should account for this.

**Labor rate assumptions dominate.** At a global blended rate of $7.5K/PM rather than $15K/PM, all figures in this table drop by roughly half. The USAID study used $56K/person-year; this series uses $180K/year ($15K/PM). The 3× difference in rate assumption alone explains most headline variation across studies.

**Scope definitions vary.** The OpenMRS figure in this series includes 24 repos; USAID likely counted a subset. OpenELIS-Global-2 is a monorepo that captures most of its functionality in one place, making it unusually complete for a single-repo measurement.

**Proprietary system estimates are counterfactual.** Epic and LabWare estimates are speculative; without access to their codebases no credible COCOMO measurement is possible.

---

*Part of the DIGI/UWCIRG OSS Valuation Series | All measurements: April 22, 2026 | Model: COCOMO II Post-Architecture (Boehm 2000) where noted*
