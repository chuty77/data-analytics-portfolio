**Campaign Analytics Test --- Findings & Recommendations**

*Dermicool --- Skin Disease Media Campaign \| DCM + Google Analytics \|
Jan--Oct 2021*

Paulo Quirós Guerrero · Data & BI Analyst · Tool: Microsoft Power BI

**Executive summary**

Two independent data sources were consolidated into a single Power BI
model: DCM (ad server delivery) and Google Analytics (on-site
behaviour). The two systems measure different events and cannot be added
together --- 295,599 recorded clicks produced only 135,765 sessions.
That gap is not an error; it is the most valuable signal in the dataset.

Harmonising both sources against shared dimensions made three things
visible that neither source could show alone:

-   Facebook delivered 58% of all campaign clicks from only 12% of
    impressions --- a 1.25% CTR against a 0.26% campaign average --- but
    74% of that traffic never reached the site.

-   Aptus lands 89% of its clicks against Facebook\'s 26%, for a
    comparable session volume --- proving the loss is fixable, not
    structural.

-   Landing efficiency doubled between April (38%) and August (76%),
    then declined to 64% by October.

-   On-site engagement is critically weak across every vendor: 1.25
    pages per session, 23.7 seconds average, and 97.7% new users. The
    campaign buys traffic that does not stay.

> *Headline: the media is working. The landing experience is not.
> Optimisation effort should shift from buying more clicks to converting
> the ones already paid for.*

**1. What will be your approach in analyzing this data set?**

**Step 1 --- Audit before modelling**

No transformation was written until both tabs had been profiled: row
counts, column types, granularity, date ranges, null distribution and
--- critically --- whether the two sources were comparable at all. This
audit surfaced every issue listed below before a single visual existed.

**Step 2 --- Establish what each source actually measures**

DCM is an ad server: it counts what was delivered (impressions) and what
was clicked. Google Analytics is a site tag: it counts what arrived
(sessions, users, pageviews). A click and a session are different events
recorded by different systems at different moments. They are related but
not equivalent, and they must never be summed.

**Step 3 --- Harmonise dimensions, not facts**

Rather than merging the two tables row by row --- which would multiply
rows and inflate impressions --- each source was kept as its own fact
table and connected to conformed dimensions shared by both: vendor, date
and device. A single filter now controls both sources simultaneously,
which is what makes cross-source KPIs possible.

**Step 4 --- Define KPIs by source**

Every metric is explicitly owned. CTR belongs to DCM. Pages per session
belongs to GA. Click-to-Session Rate is the only metric that requires
both --- and it is the one that produced the campaign\'s key finding.

**Step 5 --- Validate against the source**

Totals in the dashboard were reconciled against the raw file:
113,052,983 impressions, 295,599 clicks, 135,765 sessions. Any deviation
would have meant a join was duplicating or dropping rows.

> *Design principle applied throughout: a metric that cannot be
> calculated honestly returns blank, never zero. Jan--Mar shows
> impressions and clicks but no Click-to-Session Rate, because GA has no
> data for that period. Zero would have implied failure; blank correctly
> says \"not measured\".*

**2. What insights can you draw from this data set?**

**Insight 1 --- Facebook is the outlier in both directions**

Facebook accounts for 12.2% of impressions but 58.3% of all clicks in
the campaign. Its CTR of 1.25% is roughly five times the 0.26% campaign
average. However, only 44,407 of its 172,350 clicks became sessions ---
a 26% Click-to-Session Rate against a 46% campaign average.

  ----------------------------------------------------------------------------------------------
  **Vendor**      **Impressions**   **Clicks**   **CTR**   **Sessions**   **Click-to-Session**
  --------------- ----------------- ------------ --------- -------------- ----------------------
  Facebook        13,770,222        172,350      1.25%     44,407         26%

  Aptus           25,645,693        43,738       0.17%     38,904         89%

  Campaign total  113,052,983       295,599      0.26%     135,765        46%
  ----------------------------------------------------------------------------------------------

The contrast with Aptus is the sharpest signal in the dataset. Aptus
buys clicks at a seventh of Facebook\'s CTR, yet 89% of them reach the
site against Facebook\'s 26%. Both vendors deliver a comparable session
volume --- 38,904 versus 44,407 --- but Facebook burns roughly 128,000
paid clicks to get there while Aptus loses under 5,000.

Two readings are possible and both should be tested: either Facebook\'s
click quality is genuinely poor (accidental mobile taps, misleading
creative), or a large share of Facebook clicks are lost to redirect and
tag failures before GA fires. The distinction matters --- one is a
creative problem, the other is a tracking problem, and Aptus proves the
26% is not an industry floor.

**Insight 2 --- Landing efficiency improved, then reversed**

Click-to-Session Rate by month: April 38%, May 36%, June 49%, July 60%,
August 76%, September 73%, October 64%. The campaign nearly doubled its
efficiency in four months and then lost twelve points in two.
Impressions remained high through October, so the decline is not a
volume effect. Whatever changed between August and October is worth
isolating --- a new placement, a creative rotation, or a tagging
regression.

**Insight 3 --- On-site engagement is the real bottleneck**

  -----------------------------------------------------------------------
  **Metric**          **Value**     **Reading**
  ------------------- ------------- -------------------------------------
  Pages per session   1.25          Users see barely more than the
                                    landing page

  Avg. time on site   23.7 seconds  Below any meaningful engagement
                                    threshold

  New users           97.7% of all  Almost no one returns
                      users         
  -----------------------------------------------------------------------

These three numbers describe the same behaviour from three angles:
traffic arrives, glances, and leaves. For a skin-disease medicine --- a
considered, high-involvement decision --- a 23-second visit means the
landing page is not answering the question that made the user click.

**Insight 4 --- Two vendors are not display and must be read
separately**

AdPrime reports 3,328 clicks against zero impressions. Its placement is
\"Patient Caregiver Lead Gen\" with a 1x1 creative --- a tracking pixel,
not a banner. Those clicks are lead events, not ad clicks, and its 0.00%
CTR is undefined rather than poor.

AdTheorent shows a 218% Click-to-Session Rate: 334 sessions against 153
clicks. Its placements are site retargeting (\"MTP.com Site
Retargeting\", \"Dermicool.com Site Retargeting\"), where GA can
attribute sessions that DCM never recorded as clicks.

> *Both are kept visible in the dashboard rather than removed. A number
> that looks impossible is information --- hiding it would remove the
> finding along with the noise.*

**3. What additional information would you need to provide deeper
insights?**

**Critical --- blocks the primary business question**

  -----------------------------------------------------------------------
  **Missing**      **Why it matters**
  ---------------- ------------------------------------------------------
  Media cost /     Without cost, no CPM, CPC or cost-per-session exists.
  spend            Facebook produces 58% of clicks --- but at what price?
                   Every optimisation recommendation in this report is
                   directional until spend is attached.

  Conversion       Sessions are not the objective. A skin-disease
  events           campaign presumably drives prescriptions, sign-ups or
                   doctor-locator use. Without a conversion event the
                   funnel stops at \"arrived\".

  GA data for      22.2M impressions (20% of the campaign) have no
  Jan--Mar 2021    on-site counterpart. Either the tag was deployed in
                   April, or the export was truncated. This must be
                   confirmed before any full-campaign conclusion.
  -----------------------------------------------------------------------

**Important --- would materially sharpen the analysis**

-   **Facebook delivery detail.** GA reports 44,407 sessions from
    Facebook creatives, but DCM contains only 40 Facebook rows. Facebook
    is almost certainly bought outside DCM, meaning the ad server does
    not see most of it. Confirming this changes how the 26% rate should
    be read.

-   **Creative-level mapping.** Only 3 of 18 creative codes exist in
    both sources (1623, 1624, 2333), covering 49% of DCM impressions and
    38% of GA sessions. GA\'s largest creative --- NU1559Branded, 780
    rows across 13 vendors --- has no DCM record at all. A DCM-to-GA
    naming key would unlock creative-level analysis, which is currently
    impossible.

-   **Landing page URLs.** With 1.25 pages per session, the landing
    experience is the constraint. Page-level data would show whether one
    page is responsible.

-   **Placement taxonomy.** DCM uses 57 placement names, GA uses 62, and
    the vocabularies are unrelated (\"Skin Disease Crossix Segment
    Targeting\" vs \"SDPCrossixSegment\"). A shared naming convention
    would make placement-level harmonisation automatic instead of
    manual.

**Data governance issues found**

  -----------------------------------------------------------------------
  **Issue**           **Detail**
  ------------------- ---------------------------------------------------
  Vendor naming is    32 raw names describe 19 vendors. \"Health Union\"
  uncontrolled        and \"Health Union, LLC\" are the same partner;
                      \"goodrx.com\" and \"GoodRx \" (with a trailing
                      space) are the same partner. No naming standard is
                      enforced at either source.

  Device tracking     576 DCM rows and 180 GA rows have no platform or
  gaps                device value.

  Anonymisation       GA reports a source named \"ParkinsonsNewsToday\"
  leakage             and DCM a placement named \"MTP.com Site
                      Retargeting\", inside a campaign described as
                      skin-disease. Cross-referenced against DCM\'s
                      \"Skin Disease News Today ROS\" placement, these
                      appear to be the same property under its original
                      name. Flagged for confirmation, not assumed to be
                      an error.
  -----------------------------------------------------------------------

**4. What would you recommend a marketer could do based on this data?**

**Immediate --- this week**

Diagnose the Facebook gap before touching budget. 126,000 paid clicks
did not become sessions. Test the click path end to end on mobile and
desktop, verify redirect latency, and confirm the GA tag fires on the
landing page. If the loss is technical, this is the single
highest-return fix available --- it recovers traffic already paid for.
Only if the path is clean should creative be blamed.

Investigate the August-to-October decline. Efficiency fell from 76% to
64% while impressions stayed high. Identify what changed: placements
added, creative rotated, or tracking altered.

**Short term --- this month**

Fix the landing experience. Every vendor, without exception, delivers
roughly 1.25 pages per session and under 30 seconds. That consistency is
diagnostic: when every source performs the same way, the problem is not
the sources --- it is the destination. No amount of media optimisation
compensates for a page that loses users in 24 seconds.

Separate lead-gen from display in all reporting. AdPrime\'s 1x1 pixel
tactic should not sit in the same CTR benchmark as a 300x250 banner.

Enforce a vendor naming standard across DCM and GA. The mapping table
built for this analysis works, but it is a patch on a governance gap
that will keep reappearing every month.

**Strategic --- next planning cycle**

Rebalance on click efficiency, not click volume. Facebook and Aptus
deliver almost the same number of sessions --- 44,407 and 38,904 --- but
Facebook spends 172,350 clicks to do it against Aptus\'s 43,738. If
clicks are priced anywhere near each other, Facebook is paying roughly
four times as much for the same arrival. Cost data would confirm the
magnitude, but the direction is already unambiguous.

Define the real objective. Sessions are a proxy, not a goal. Until a
conversion event exists, no vendor can be judged on business outcome ---
only on traffic.

> *Recommendation hierarchy: fix the leak before increasing the flow.
> The campaign already generates strong click volume at a competitive
> CTR. The value is being lost between the click and the page, and again
> between the page and the second page.*

**5. What format would you use to report findings?**

**For this delivery**

An interactive Power BI dashboard plus this written summary. The
dashboard answers \"what happened\" and lets the reader interrogate it
by date, vendor and device. The document answers \"so what\" --- the
part a dashboard cannot carry on its own.

**Recommended operating cadence**

  ------------------------------------------------------------------------
  **Audience**    **Format**         **Cadence**   **Content**
  --------------- ------------------ ------------- -----------------------
  Media /         Interactive Power  Live,         Full detail with all
  campaign team   BI dashboard       refreshed     filters. Self-service.
                                     weekly        

  Brand / client  One-page PDF       Monthly       3 KPIs, trend, top and
  lead            export                           bottom vendors, one
                                                   recommendation.

  Executive /     3-slide deck       Quarterly     Headline, decision
  stakeholder                                      required, budget
                                                   implication.
  ------------------------------------------------------------------------

The principle is that format follows decision, not audience seniority. A
media buyer optimising placements needs filters and detail. A brand lead
approving budget needs one number and one recommendation. Sending either
the other\'s report wastes both.

**What the dashboard must always carry**

-   An explicit data-coverage note. This dashboard states that
    Click-to-Session covers April--October only. A reader who does not
    know that will draw a wrong conclusion from a correct number.

-   Anomalies visible, not hidden. AdPrime\'s zero impressions and
    AdTheorent\'s 218% remain in view with explanation. Silent exclusion
    is how findings get lost.

-   Source attribution per metric. Each KPI declares whether it comes
    from DCM, from GA, or from the join. When numbers are questioned ---
    and they always are --- the answer must be immediate.

**Appendix --- Assumptions log**

*The brief invited assumptions. These were made explicitly and are
reversible if the client confirms otherwise.*

  ------------------------------------------------------------------------------
  **\#**   **Assumption**            **Basis**
  -------- ------------------------- -------------------------------------------
  1        \"ParkinsonsNewsToday\"   Both run ROS placements with identical
           (GA) and \"Skin Disease   creatives across identical months. DCM\'s
           News Today\" (DCM) are    placement is literally \"Skin Disease News
           the same property.        Today ROS\"; GA\'s ad content is
                                     \"ROS.\*\".

  2        \"MiQ\" and \"MediaIQ\"   DCM lists one vendor. GA splits it into two
           (GA) are the same vendor  tagging conventions, with MiQ carrying only
           as \"Media IQ\" (DCM).    117 of 9,241 combined sessions ---
                                     consistent with a residual legacy tag.

  3        Vendor legal suffixes are \"Health Union, LLC\" = \"Health Union\";
           cosmetic.                 \"Remedy Health Media, LLC\" = \"Remedy
                                     Health\"; \"goodrx.com\" = \"GoodRx\". 32
                                     raw names resolve to 19 vendors.

  4        Null device values are a  756 rows across both sources. Labelled
           tracking gap, not a       \"(not set)\" and kept visible rather than
           device type.              dropped.

  5        Acuity and PulsePoint are Both serve Dermicool and SD_1624 creatives
           live vendors absent from  in GA. They ran; DCM simply does not report
           the DCM export.           them.

  6        GA tracking began in      Cleanest explanation for 22.2M DCM
           April 2021.               impressions with zero GA rows in Jan--Mar.
                                     Requires client confirmation.

  7        Monthly grain is the      Both sources report by month. No daily data
           finest available.         exists, so date is anchored to the first of
                                     each month.
  ------------------------------------------------------------------------------

*Paulo Quirós Guerrero · paulo-77@outlook.es · Campaign Analytics Test ·
2026*
