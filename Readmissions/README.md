# Cost vs. Outcomes: Hospital Value Analysis (Tableau Data Blend)

**Tools:** Tableau · Data Blending · Calculated Fields · Cross-source KPIs
**Skills demonstrated:** Blending two independent sources at different grains, harmonizing mismatched categories, distinguishing structural gaps from findings, value-based analysis

---

## The Problem

Healthcare organizations increasingly measure not just *what care costs*, but whether that spending buys *better outcomes* — the core idea behind value-based care. Answering that requires joining two very different worlds:

- **A cost source** — medical claims with paid amounts by provider specialty (from a healthcare claims database).
- **An outcomes source** — hospital readmission records indicating whether patients were readmitted within 30 days (public Kaggle dataset).

The challenge: these two sources live at **different grains** and use **different vocabularies**. One is about individual claims; the other about patient episodes. They cannot be joined row-by-row without inflating the numbers — but the business question can only be answered by relating them.

> The question: **Do the specialties that cost the most actually deliver better outcomes — or are we paying more for the same result?**

---

## Research Questions

1. How do you combine two sources at different grains without corrupting either — join, blend, or relationship?
2. The two sources name specialties differently. Which ones genuinely match, and which have no counterpart?
3. Which specialties cost the most per claim?
4. Is higher spending associated with lower readmission — the relationship value-based care assumes?
5. How should specialties that exist in only one source be handled — as errors or as findings?

---

## Approach

- **Used a data blend, not a join** — because the sources measure different things at different granularity. A blend relates them at the aggregate level through a shared dimension (specialty), so each source keeps its own grain. A row-level join would have multiplied records and inflated cost.
- **Harmonized specialty names** across sources with a calculated field, mapping differently-labeled equivalents (e.g., `Family/GeneralPractice` → `Family Med`) and documenting the more ambiguous mappings as reversible assumptions.
- **Built a Readmission Rate calculated field** — converting a categorical Yes/No field into a true rate by averaging 1s and 0s, excluding nulls rather than assuming them.
- **Kept unmatched specialties visible as findings**, not errors — a specialty present in only one source signals a real coverage gap, not a mistake.
- **Visualized the relationship as a scatter plot** — cost on one axis, readmission on the other — so the value mismatch becomes visible at a glance.

---

## Dashboard

![Dashboard overview](images/dashboard-overview.png)

*(scatter of cost vs. readmission rate by specialty — the flat line is the finding)*

---

## 5 Key Insights

**1. Higher cost did not buy better outcomes.**
The most expensive specialty cost roughly **twice as much per case** ($5,548 vs. $2,665) as another — but their readmission rates were nearly identical (~45%). Doubling the spend bought no measurable improvement in outcome. That mismatch is exactly the inefficiency value-based care exists to surface.

**2. The scatter plot revealed a flat cost-outcome line.**
When specialties were plotted with cost on one axis and readmission on the other, the points formed a nearly **horizontal line** — cost varied widely while readmission stayed flat. In an efficient system you'd expect a downward slope (more spend, fewer readmissions). The flat line is the visual proof that spend and outcome were disconnected here.

**3. Family Medicine was the most efficient specialty.**
It delivered a comparable readmission rate at the lowest cost — making it the natural efficiency benchmark, and the reference point against which the expensive specialties look questionable.

**4. Only one specialty matched cleanly across sources — and that's a data-governance finding.**
Of nine specialties in the cost source and seven in the outcomes source, only Cardiology matched by name outright; two more required harmonization. The rest had no counterpart. Rather than forcing matches, the unmatched specialties were reported as coverage gaps — the same discipline that keeps a blended analysis honest.

**5. A blend, not a join, was the only correct way to combine them.**
Because clicks-per-claim and patient-episodes sit at different grains, a row-level join would have multiplied rows and produced inflated, meaningless cost figures. Relating them at the aggregate level through a shared specialty dimension preserved each source's integrity — the single most important modeling decision in the project.

---

## Files in This Folder

| File | Description |
|------|-------------|
| `images/` | Dashboard screenshots |
| `claims_cost.csv` | Cost source — claims with paid amounts by specialty |
| `hospital_readmissions.csv` | Outcomes source — readmission records (Kaggle) |

---

## What This Project Demonstrates

This is a **data-reliability and judgment** project. It shows the ability to combine two messy, mismatched sources without corrupting either, to distinguish a real coverage gap from a data error, and to turn a modeling decision (blend vs. join) into a trustworthy business answer — the core of keeping a data product correct for the people who make decisions with it.
