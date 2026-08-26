# Cross-Source Marketing Campaign Analysis (DCM + Google Analytics)

**Tools:** Power BI · Power Query · DAX · Dimensional Modeling
**Skills demonstrated:** Data harmonization, cross-source reconciliation, distinguishing data-quality errors from real-world events, conformed-dimension modeling

---

## The Problem

A pharmaceutical display campaign was measured by two completely different systems, and the business wanted a single, trustworthy view of performance:

- **DCM (ad server)** — what was *delivered*: 4,244 rows of impressions and clicks across 21 sites and 57 placements over 10 months.
- **Google Analytics (site tag)** — what *happened on the site*: 2,403 rows of sessions, users, and pageviews over 7 months.

The two sources *looked* like they should combine — both had vendor names, months, and device fields. But they measured **different events at different moments**. The core challenge was knowing what could honestly be compared and what could not.

The headline tension:

> DCM recorded **295,599 clicks**. Google Analytics recorded **135,765 sessions**. Less than half.

If these two tables were merged or summed, the resulting dashboard would look clean and be completely wrong. The project's real work was resisting that.

---

## Research Questions

1. Can these two sources be combined at all — and if not, how should they be related so the numbers stay honest?
2. Why do clicks (296K) and sessions (136K) differ by more than half? Is that an error or a real-world phenomenon?
3. Which vendors are actually the same entity hiding behind inconsistent names?
4. Where is paid traffic being lost between the ad click and the website, and is that loss fixable or structural?
5. How should missing data (months with no GA coverage, null devices) be handled without inventing information?

---

## Approach

- **Kept two separate fact tables** (DCM and GA) connected through **conformed dimensions** (vendor, date, device) rather than merging them — so each source keeps its own grain and its totals still reconcile against the raw file.
- **Harmonized 32 raw vendor names into 19 real vendors** using a mapping table, resolving ambiguous cases by cross-referencing placements, creatives, and active months — not by name matching alone.
- **Built a cross-source KPI, Click-to-Session Rate**, that neither table could produce on its own.
- **Validated every total against the source** (113,052,983 impressions; 295,599 clicks; 135,765 sessions).
- **Preserved anomalies and gaps as findings**, labeling nulls as `(not set)` and letting un-computable metrics return blank rather than a misleading zero.

---

## 5 Key Insights

**1. The click-to-session gap is the finding, not an error.**
A click and a session are different events, recorded by different systems, at different moments — the ad server counts the exit, GA counts the arrival. The 54% gap represents paid traffic lost between the click and the page (bounces before load, ad blockers, redirect failures, lost UTMs). Summing the two would have been meaningless.

**2. Facebook is the outlier in both directions.**
Facebook drove **58% of all campaign clicks from just 12% of impressions** — a CTR five times the campaign average — but only **26% of those clicks became sessions**, versus a 46% campaign average. Roughly 126,000 paid clicks never reached the site.

**3. The loss is fixable, not structural.**
Aptus landed **89% of its clicks** with a comparable session volume, using a quarter of Facebook's clicks. That contrast proves the problem isn't the media buying — it's the landing experience — and reframes the recommendation from "buy better clicks" to "fix the leak before pouring in more."

**4. Vendor names are the weakest signal for identity.**
32 raw names collapsed to 19 real vendors. Two sources labeled the same property differently (e.g., a source GA called `ParkinsonsNewsToday` matched DCM's `Skin Disease News Today`, confirmed by identical ROS placements and shared creatives in overlapping months). Identity had to be verified by behavior, not text.

**5. Landing efficiency doubled, then reversed — a real trend worth investigating.**
Monthly Click-to-Session rate climbed from **38% in April to 76% in August**, then fell to **64% by October** while impressions stayed high. The decline isn't a volume effect — something changed between August and October worth isolating (a new placement, a creative rotation, or a tagging regression).

---

## Files in This Folder

| File | Description |
|------|-------------|
| `Analytics_Test_Data_Set.xlsx` | Source data — DCM (RFI) and Raw GA sheets |
| `vendor_device_mapping.xlsx` | Harmonization map: 32 raw vendor names → 19 clean; 11 raw devices → 6 clean |
| `findings_report.md` | Full written analysis with assumptions log |

---

## What This Project Demonstrates

This is a **data-quality and reliability** project at its core. It shows the judgment to distinguish a data error from a real-world event, the discipline to validate before delivering, and the modeling skill to relate two messy sources without corrupting either — exactly the work of keeping a data product trustworthy for the people who make decisions with it.
