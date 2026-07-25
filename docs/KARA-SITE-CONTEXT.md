# KARA Site — Build Context

> **Scope notice — read this first.**
>
> This document governs the **Kids At Risk Action (KARA) organizational website** published at
> `invisiblechildren.org`. It does **not** govern the code in this repository.
>
> This repository (`whysice/recovery-connect`) is *RecoveryConnect: Maine Reentry Resource
> Navigator* — a single-file `index.html` deployed at `recovery-connect.netlify.app`. It shares
> no content model, navigation, data layer, or provenance system with the KARA site.
>
> The context below is filed here for durability and reference only. Per its own disambiguation
> rule, the two projects must not be merged. Do not apply KARA's IA rules, badge system, state-page
> contract, or verification gates to RecoveryConnect, and do not treat this file as repository-wide
> working instructions.
>
> **Unresolved:** the `Stack` field below is still the literal placeholder `{{stack}}`. It must be
> confirmed before any KARA scaffolding work begins — the document explicitly forbids assuming it.

The body below is reproduced verbatim as received.

---

Persistent context for this repo. Read before making architectural, IA, or content-model decisions.

---

## What this repo is

The organizational website for **Kids At Risk Action (KARA)**, a 501(c)(3) founded by Mike Tikkanen, published at `invisiblechildren.org`. Subject matter is U.S. child protection: child abuse and neglect reporting, CPS screening decisions, foster care, family court, and child maltreatment fatalities.

**Disambiguation:** other documents in this workspace use "KARA" to mean an LLM-powered resource navigator built on Next.js/Supabase. That is a different system that happens to be deployed on the same domain. This repo is the organizational site and its data/archive products. Do not merge the two concepts.

**Stack:** `{{stack}}` — confirm before scaffolding. Do not assume.

---

## The argument the site makes

The site advances **one claim**: the decisions being made in child protection are visible, measurable, and wrong.

- The **federal data** shows the decision points.
- The **archive** shows what those decisions cost.

The framing is forward, not diagnostic: not *why are these the numbers* but *what are we doing to change them*.

Every surface on this site is evidence for that single claim. When a structural decision is ambiguous, resolve it toward whichever option makes the claim clearer.

---

## Rule 1 — Audience is a use, not a type

**Never build "Data" and "Stories" as sibling top-level sections.** This is the primary failure mode for this project and the most likely thing an agent will default to.

Readers do not sort into data people and story people. A program officer needs the citation *and* the case. A CASA volunteer needs the state numbers *and* the human account. Splitting by content type trains readers to pick one branch and never cross it.

The real split is by **job**, and the same reader does both:

- **Citation** — someone needs a number they can defend in a grant, a hearing, or a story.
- **Conviction** — someone needs to know this is systemic, not anecdotal.

What differs by audience is only the direction of travel:

| Audience | Enters at | Needs next |
|---|---|---|
| Foundations, legislators | Data | A face on the number |
| Media | Data | Local case + source URL |
| Survivors, public, social | Story | Proof it isn't just them |
| Academics | Data | Methodology, raw files |
| CASAs, frontline workers | Either | Their state, their county |

**Implementation:** one navigation, argument-led. Both evidence types appear inside every major surface, cross-linked bidirectionally. No branch is a dead end.

---

## Rule 2 — Provenance is a badge, not a wall

Four labels, rendered **inline at the item level** on every piece of content:

| Badge | Meaning | Required fields |
|---|---|---|
| `KARA VERIFIED` | Government/agency data | source, year, unit |
| `NEWSPAPER OF RECORD` | Published journalism | publication, date, URL |
| `KARA REPORTING` | KARA's own writing/analysis | author, date |
| `SUBMITTED` | First-party account | see Do Not Build |

Once provenance travels with the item, content types can be freely interleaved and nothing is confusing. Mixing without badges destroys the credibility argument; mixing with badges *is* the credibility argument — the site demonstrates the evidentiary standard it asks child protection agencies to meet.

**Do not** relocate provenance to a methodology page. It belongs next to the claim, inline, even where that is visually inconvenient.

---

## Rule 3 — The state page is the join

The state page is the primary unit of the site. One per state, plus DC and Puerto Rico.

Required contract, in this order:

1. **That state's funnel** — referrals, screen-out %, alternative response vs. investigation, victims, fatalities. Rate-normalized.
2. **That state's gaps** — which NCANDS elements the state does not report, stated plainly. The gaps are on-message, not an embarrassment.
3. **That state's archive** — articles for that state, badge-labeled.
4. **Comparison to national** — where this state sits against the federal figures.

This single artifact serves every audience row above simultaneously. It is also the strongest SEO unit and the only page a state legislator would plausibly bookmark, because a legislator has exactly one state.

**The homepage is not this page.** Build the state pages first.

---

## Rule 4 — Do not anonymize the archive

This reverses an earlier approach. Do not build or extend a de-identification pipeline over the newspaper archive.

Newspaper-of-record articles are already public, already attributable, already verifiable — that is what "of record" means, and it is the entire basis for using them as evidence. Stripping names converts a citable primary source into an uncitable paraphrase and adds months of human review for negative value.

**What the archive actually needs is metadata**, which is scriptable:

- `publication`, `publication_date`, `source_url`, `state`, `source_type`

`source_type` is load-bearing. The archive is believed to contain some blog posts mixed in with newspapers. Until every item is typed, the "newspapers of record" label cannot be defended. **Audit before launch.**

The anonymization pipeline points **only** at first-party submissions (see Do Not Build).

---

## Data layer

### Units discipline

Reports ≠ referrals ≠ children ≠ victims. These are four different denominators and conflating them is the most common failure in this domain. **Label the unit on every figure rendered.**

### Normalization

State comparisons use rates, never raw counts: **per 1,000 children** for referrals/victims, **per 100,000** for fatalities. Raw counts put California and Texas at the top of every list purely on population and make the comparison meaningless.

### Data vintage

Every data surface renders a vintage line. Federal data runs roughly a year behind; without a date, readers assume the site is stale rather than the dataset.

> *Federal data: Child Maltreatment 2024 (FFY 2024), U.S. HHS/ACF Children's Bureau, published January 2025 — most recent available. Unreported-maltreatment estimates: NIS-4, data collected 2005–2006.*

### National reference figures — FFY 2024

Source: *Child Maltreatment 2024*, NCANDS / Children's Bureau, HHS/ACF.

| Stage | Value | Rate |
|---|---|---|
| Referrals (national estimate) | 4,365,000 | 59.3 / 1,000 |
| Referrals (actual sum reported) | 4,087,496 | — |
| Children in total referrals | 7,693,000 | — |
| Screened in ("reports") | 2,058,720 (47.1%) | 28.0 / 1,000 |
| Screened out | 2,028,776 (52.9%) | — |
| Children investigated or AR | — | 40.6 / 1,000 |
| Victims (substantiated/indicated) | 532,228 | 7.2 / 1,000 |
| Fatalities | 1,773 | 2.41 / 100,000 |

Supporting figures:

- **FFY 2023 children in referrals: 7,782,000.** This is the "7.8 million" figure used in KARA materials. It is federally published — cite it directly, do not derive it.
- **Screen-in decline: 57.6% (2017) → 47.1% (2024).** Strongest single trend line available.
- **Alternative response ≈ 14%** of children receiving a CPS response, nationally.
- **NIS-4** (data 2005–06, published 2010): CPS investigated **32%** of children with Harm Standard maltreatment, **43%** under the Endangerment Standard. This is the empirical basis for the "invisible" claim.
- **NIS-4:** school staff recognized **52%** of Harm Standard maltreated children — more than hospitals, police, and the general public combined.
- No NIS-5 has been identified. The last U.S. measurement of maltreatment that never reaches CPS is roughly twenty years old. Treat that gap as a finding, not a footnote.

### Raw data

State-level and case-level files are distributed by NDACAN (Cornell): NCANDS Child File and Agency File, FFY 2024. Prefer the data files over re-keying tables out of the annual report PDF.

---

## Content model

Minimum entities and required fields.

```
DataPoint
  value, unit, denominator, year, source, source_url, geography, badge

StatePage
  state_code, funnel[DataPoint], reporting_gaps[], archive_items[], national_comparison

ArchiveItem
  title, publication, publication_date, source_url, state, source_type, badge, summary

Article            (KARA REPORTING)
  title, author, date, body, related_states[], related_data_points[], badge

Submission         (SUBMITTED — not live)
  see Do Not Build
```

Every renderable entity carries a `badge`. No exceptions.

---

## Verification gates

These claims appear in KARA source material and are **wrong**. They must not ship. If any appears in copy, flag it rather than publishing it.

| Claim in source material | Correction |
|---|---|
| Minnesota screen-out improved from 60–65% to 40–45% | Reversed. MN screens **in** only ~31% — among the lowest nationally. It got worse. |
| Family assessment vs. investigation is ~50/50 in MN | ~66% assessment. Nationally alternative response is ~14%. |
| "97 CASA programs in the country" | ~890 programs in 48 states + DC, 79,000+ volunteers. |
| CASA federal funding is still terminated | Terminated April 2025, later restored. Staffing damage persists. |
| Eric Dean's "face chewed off" | Bite marks on head/face; died of a perforated small intestine. |
| Eric Dean was "never visited" | One of 15 reports was investigated (2011, broken arm), found no maltreatment; 9 screened out. |
| "15 reports in the last two years" | 15 reports over his life; died February 2013, age 4. |
| Massachusetts does not report | MA reports. The four states excluded from national estimates are NJ, NY, ND, PA. |
| Maltreatment fatalities declined ~10% in 2024 | Five states account for >100% of the decline and attribute it to counting changes. Do not publish as a real decrease. |
| CASA grant termination = $12M | KARA materials say $12M; a DOJ list obtained by Reuters put it near $48.9M. Reconcile before publishing either. |

Minnesota's child protection functions have moved from DHS to the **Department of Children, Youth, and Families (DCYF)**. Update source URLs accordingly.

---

## Do not build yet

**First-party intake (`SUBMITTED`).** A "share your story" form for frontline workers and survivors is desired but blocked. Do not ship until these are answered in writing: who reads submissions and how fast; what confidentiality KARA can actually promise (a nonprofit cannot offer privilege); whether mandated-reporter duties attach to staff and how that collides with an anonymity promise; and what "help" is on offer versus implied. A form that collects disclosures the org cannot handle is worse than no form.

**Board roster page.** Desired, but board composition is unsettled. A published roster is much harder to walk back than a draft.

**A second color palette.** See below.

---

## Settled — do not relitigate

- **Two labeled data sets, one site.** Not two sites, not two brands.
- **Launch imperfect, iterate after.** Perfection is not the bar; shipping is.
- **Branding and social consistency are post-launch concerns.**
- **One palette.** Two visual identities read as two organizations, which is the exact problem the architecture is designed to avoid. Tonal difference between a data surface and a narrative surface comes from density, typography, and framing — not hue.
- **De-identification is off the archive** (Rule 4).

---

## Working notes

- Prefer scripted migration and metadata extraction over manual review anywhere the choice exists.
- Archived content on the legacy install may not be reachable by crawler and is at risk of permanent loss if the old site is decommissioned. Treat recovery of archived white papers and video assets as irreversible work — everything else can be fixed after launch.
- `invisiblechildren.org` shares a name with a well-known unrelated nonprofit. Organic search for the brand name will not be winnable on content alone. Do not build strategy that depends on it.
