# Part 2: KPI Framework, Business Experiment Analysis & Decision Recommendation

## Business Context

A subscription based digital product company launched a new onboarding and activation campaign aimed at improving user conversion and early engagement. Users were randomly split into two groups:

- **Control:** Existing onboarding experience
- **Treatment:** New campaign experience

**Business problem statement:**
- **Decision to be made:** Whether to launch the new campaign to all users, reject it, launch it only for selected segments, or continue testing.
- **Who it impacts:** New users going through onboarding, the support team, and the revenue/finance function.
- **Metric that should improve:** Paid conversion rate (North Star), without materially harming guardrail metrics.
- **Risks to monitor:** Refund rate, support ticket volume, revenue quality (revenue per converted user), and time-to-convert.
- **Evidence required:** A statistically valid Control vs Treatment comparison on the North Star metric, a guardrail review, and a segment-level check.


## Dataset Description

File: `data/campaign_experiment_data.xlsx`
- **Sheet `experiment_data`:** 1,408 raw rows (1,400 after removing duplicates), one row per user, with fields covering experiment group, signup metadata (region, device, traffic source, plan type), funnel events (landing page visit, trial start, onboarding completion, paid conversion), and outcome/guardrail fields (revenue, support tickets, refunds, days to convert, engagement score).
- **Sheet `business_context`:** Field definitions and the business framing provided with the dataset.

## North Star Metric Selected

**Paid conversion rate** — the percentage of signed-up users who become paying customers. Selected because it is the metric most directly tied to revenue growth and the stated campaign objective, while upstream funnel metrics (landing page visits, trial starts, onboarding completion) are useful but insufficient proxies on their own. Full reasoning, including the risk of optimizing this metric blindly, is in `outputs/recommendation_memo.md`.

## KPI Tree Summary

See `outputs/kpi_tree.png`. The North Star metric breaks down into three primary drivers — **Activation Quality**, **Trial Engagement**, and **Acquisition Funnel Strength** — each with two sub-drivers, plus four guardrail metrics tracked alongside the tree: refund rate, support ticket rate, revenue per converted user, and post-conversion engagement score.

## Data Cleaning and Preparation

Performed in `analysis/experiment_analysis.xlsx`. Checks performed and how each was handled:

| Check | Finding | Action Taken |
|---|---|---|
| Duplicate user IDs | 8 fully duplicated rows (identical user_id and all field values) | Removed, keeping first occurrence. Final analysis uses 1,400 unique users (690 Control, 710 Treatment). |
| Missing values — `device_type` | 18 blank | Left as missing; excluded only from device-level segment cuts, not from overall metrics. |
| Missing values — `traffic_source` | 24 blank | Left as missing; excluded only from traffic-source segment cuts. |
| Missing values — `engagement_score` | 14 blank | Left as missing; `AVERAGE`/`AVERAGEIFS` formulas ignore blanks automatically. |
| Missing values — `days_to_convert` | 1,336 blank | Expected and correct — only converted users (72) have a conversion date; non-converters have no value by definition. |
| Invalid binary values | None found | All 0/1 fields (`visited_landing_page`, `started_trial`, `completed_onboarding`, `converted_to_paid`, `refund_requested`) contain only valid 0/1 values. |
| Revenue logic consistency | Consistent | Confirmed `revenue_30d > 0` only when `converted_to_paid = 1`, with no mismatches. |
| Revenue outliers | 4 of 72 converted users above the IQR upper bound (e.g., one Basic-plan user with $8,610.72 in 30-day revenue) | Flagged in `analysis/experiment_analysis.xlsx` (sheet `Revenue_Outliers`) and retained in the main analysis, since the converter sample is too small to safely discard real records; called out as a caveat in the recommendation memo. |
| Group balance | 690 Control vs 710 Treatment after dedup | Reasonably balanced; no random-assignment skew. |
| Segment distribution across groups | Region, device type, traffic source, and plan type are all proportionally similar between Control and Treatment | Confirms random assignment was not confounded by segment; detail in sheet `Segment_Distribution`. |

## Experiment Analysis Approach

1. Cleaned and validated the raw dataset (see above).
2. Built a Control vs Treatment summary across 11 required metrics, plus a segment-level cut (region, device type, plan type) on three of them — paid conversion rate, onboarding completion rate, and average engagement score (`outputs/experiment_summary.xlsx`).
3. Framed a formal hypothesis test on the North Star metric (`analysis/hypothesis_test_notes.md`).
4. Ran a two-proportion z-test comparing Control and Treatment conversion rates (`analysis/experiment_analysis.xlsx`, sheet `Hypothesis_Test`).
5. Evaluated guardrail metrics to check whether the conversion lift came with hidden costs.
6. Synthesized everything into a decision recommendation (`outputs/recommendation_memo.md`).

## Hypothesis Test Summary

- **H0:** No difference in paid conversion rate between Control and Treatment.
- **H1:** Paid conversion rate differs between Control and Treatment.
- **Test:** Two-proportion z-test, two-tailed, α = 0.05.
- **Result:** Control 3.19% (22/690) vs Treatment 7.04% (50/710); Z = 3.264, **p = 0.0011**; 95% CI on the difference [1.56 pp, 6.15 pp].
- **Conclusion:** Reject H0 — the lift in paid conversion rate is statistically significant.

Full notes: `analysis/hypothesis_test_notes.md`. Evidence: `screenshots/hypothesis_test_output.png`.

## Guardrail Metrics Considered

1. **Support ticket rate:** Rose from 14.78% to 24.79% (statistically significant, p < 0.0001) — the clearest risk signal, requiring support-capacity planning before launch.
2. **Revenue per converted user:** Fell from ~$1,630 to ~$770 — Treatment converts more users but at lower average value, a quality trade-off worth monitoring.
3. **Refund rate:** Rose slightly (0.00% → 0.42%), not yet statistically significant (p = 0.087) given the small converter sample.

A fourth metric, **engagement score**, actually improved in Treatment, and **days to convert** dropped — both reassuring signs that the conversion lift isn't simply low-quality or disengaged users. Full discussion in `outputs/recommendation_memo.md`.

## Final Recommendation

**Launch the campaign, conditioned on** scaling support capacity ahead of rollout and actively monitoring revenue-per-converted-user and refund rate post-launch. An alternative, more conservative path is to launch first only for the segments with the strongest lift (Free plan, Mobile) while continuing to test on Basic-plan users, where the campaign showed little benefit. Full reasoning in `outputs/recommendation_memo.md`.

## Assumptions and Limitations

- The converter sample is small (72 users across both groups combined), which limits statistical power on guardrail metrics like refund rate.
- Revenue and refund outcomes are observed only within a 30-day window; longer-term retention and lifetime value are not available in this dataset.
- A small number of revenue outliers among converted users affect the revenue-per-converted-user comparison; these were investigated and retained (see `analysis/experiment_analysis.xlsx`, sheet `Revenue_Outliers`) given the data is plausible (e.g., upsells/bundled purchases) rather than clear errors.
- Segment-level findings are observational subgroup comparisons within the experiment and should be treated as directional rather than independently statistically confirmed.



```
