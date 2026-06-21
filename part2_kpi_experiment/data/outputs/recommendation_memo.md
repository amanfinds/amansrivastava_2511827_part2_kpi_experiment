# Recommendation Memo: Onboarding & Activation Campaign

**To:** Leadership
**From:** Business Analyst
**Re:** Should the new onboarding and activation campaign be launched to all users?

---

## Executive Summary

The new onboarding and activation campaign (Treatment) produced a **statistically significant, large lift in paid conversion rate** — from 3.19% in Control to 7.04% in Treatment, a relative lift of +120.9% (p = 0.0011). However, this lift comes with real costs: the support ticket rate nearly doubled (14.8% → 24.8%), and average revenue per converted user fell sharply (from ~$1,630 to ~$770), suggesting Treatment is converting a higher volume of lower-value customers. Conversion improved across every region, device type, and plan type, with the strongest gains among Free-plan users.

**Recommendation: Launch the campaign, but with conditions** — proceed to a full launch while simultaneously investing in support capacity and monitoring revenue quality, rather than launching unconditionally or continuing to test indefinitely. See "Final Recommendation" below for the full reasoning and an alternative phased option.

## Business Problem Statement

**Decision to be made:** Whether to launch the new onboarding and activation campaign (Treatment) to all users, reject it, launch it only for selected segments, or continue testing.

**Who this impacts:** All new users going through onboarding, the support team (who will absorb any change in ticket volume), and the revenue/finance team (who care about not just conversion count but revenue quality).

**Metric that should improve:** Paid conversion rate (the North Star metric — see below), without materially harming guardrail metrics.

**Risks to monitor:** Refund rate, support ticket volume, revenue per converted user, and time-to-convert. A conversion lift that is purely the result of attracting lower-intent or lower-quality users would not be a genuine win.

**Evidence required before recommending:** A statistically valid comparison of Control vs Treatment on the North Star metric, plus a full guardrail review, plus a segment-level check to rule out the lift being concentrated in (or absent from) specific user groups.

## North Star Metric

**Paid conversion rate** (`converted_to_paid`) — the percentage of signed-up users who become paying customers.

This is selected as the North Star metric because:
- It is the metric most directly tied to revenue growth and the stated purpose of the campaign ("improve user conversion and early engagement").
- Upstream funnel metrics (landing page visits, trial starts, onboarding completion) are necessary but not sufficient — a user can complete every onboarding step and still never pay. Paid conversion is the metric that actually reflects business value delivered.
- Optimizing this metric blindly is risky: it's possible to inflate conversion by lowering signup friction or quality bars, attracting users who convert but churn, refund, or generate high support cost. This is exactly why guardrail metrics are tracked alongside it (see KPI tree and Guardrail Analysis below).

Supporting metrics like landing page visit rate, trial start rate, and onboarding completion rate are useful diagnostic/leading indicators that explain *why* conversion moved, but none of them alone proves the business outcome leadership cares about.

## KPI Tree Explanation

See `outputs/kpi_tree.png` / `screenshots/kpi_tree_preview.png`.

The North Star (Paid Conversion Rate) breaks down into three primary drivers:
1. **Activation Quality** (onboarding completion rate) → sub-drivers: trial start rate, time-to-first-value
2. **Trial Engagement** (trial start rate & engagement score) → sub-drivers: engagement score, support friction
3. **Acquisition Funnel Strength** (landing page visit rate) → sub-drivers: landing page visit rate, traffic source quality

Four guardrail metrics sit alongside the tree to make sure conversion isn't optimized at the expense of the business: refund rate, support ticket rate, average revenue per converted user, and post-conversion engagement score.

## Experiment Result Summary

| Metric | Control | Treatment | Difference |
|---|---|---|---|
| Users | 690 | 710 | +20 |
| Landing page visit rate | 63.62% | 72.39% | +8.77 pp |
| Trial start rate | 25.07% | 29.01% | +3.94 pp |
| Onboarding completion rate | 15.65% | 21.13% | +5.47 pp |
| **Paid conversion rate** | **3.19%** | **7.04%** | **+3.85 pp (+120.9%)** |
| ARPU (all users) | $51.97 | $54.25 | +$2.28 |
| Revenue per converted user | $1,630.10 | $770.41 | -$859.69 |
| Refund rate | 0.00% | 0.42% | +0.42 pp |
| Support ticket rate | 14.78% | 24.79% | +10.01 pp |
| Avg engagement score | 57.03 | 62.94 | +5.91 |
| Avg days to convert | 8.86 | 6.40 | -2.46 days |

Full detail in `outputs/experiment_summary.xlsx` and `screenshots/summary_metrics.png`.

Every step of the funnel improved in Treatment, and converted users in Treatment also converted faster (6.4 days vs 8.9 days on average).

## Hypothesis Test Interpretation

A two-proportion z-test on paid conversion rate produced **Z = 3.264, p = 0.0011** (two-tailed, α = 0.05), with a 95% confidence interval on the difference of [1.56 pp, 6.15 pp]. We reject the null hypothesis: the lift in conversion is statistically significant and very unlikely to be due to random chance. Full notes in `analysis/hypothesis_test_notes.md`.

## Guardrail Analysis

Per the assignment's caution — *"a better conversion rate may still be risky if refunds, support tickets, or low-quality revenue increase"* — three guardrails were evaluated:

1. **Support ticket rate** rose from 14.78% to 24.79% (+10 pp, statistically significant, p < 0.0001). This is the clearest warning sign in the data. The new onboarding flow is driving more users to convert, but a meaningfully larger share of those users are also needing help — a real cost to the support organization that must be planned for before a full rollout.

2. **Revenue per converted user** fell from $1,630 to $770 (excluding extreme outliers, $872 vs $732). Treatment is converting more users, but those users appear to be lower-value on average — plausibly because the campaign is pulling in more Free/Basic-plan and lower-intent signups who convert but spend less. This needs to be watched; conversion *count* going up while conversion *value* goes down is a quality trade-off leadership should be aware of, not just a clean win.

3. **Refund rate** increased slightly (0.00% → 0.42%), which is not statistically significant at current sample sizes (p = 0.087) and likely not yet a real risk, but should continue to be monitored as volume scales — a near-zero base rate makes it hard to be confident with only ~700 users per group.

A fourth, positive-direction guardrail — **engagement score** — actually improved in Treatment (57.0 → 62.9, p < 0.0001) and **days to convert** dropped (8.9 → 6.4 days), suggesting Treatment users who do convert are not less engaged; if anything they are converting faster and with higher engagement. This is reassuring and suggests the conversion lift isn't simply "junk" users churning out immediately.

## Segment-Level Insight

Conversion lift held up across every segment cut, but its size varied:

- **By plan type:** The lift was much larger for **Free** plan users (3.06% → 9.29%, nearly +6 pp) and **Premium** users (2.75% → 6.25%) than for **Basic** plan users (3.62% → 3.88%, barely moved). The campaign appears far more effective at converting Free-tier signups into paying customers than at moving existing Basic-tier users further.
- **By region:** Lift was positive everywhere, strongest in North (3.48% → 8.89%) and weakest in West (3.38% → 5.08%).
- **By device type:** Lift was strongest on Mobile (2.58% → 7.41%) and Tablet (1.82% → 7.14%), and more modest on Desktop (4.50% → 6.57%).

This suggests the campaign's biggest value is in converting Free users and mobile users — exactly the population it was likely designed to better activate.

## Final Recommendation

**Launch the campaign — but with conditions, not unconditionally.**

This is not a clean "yes" or "no": the conversion lift is real and large, but the support ticket increase is also real and large, and revenue per converted user dropped. Given that:
- the North Star result is statistically significant and economically large,
- the guardrail signal most likely to scale into a real problem (support tickets) is operational, not existential, and can be planned for,
- and the lift holds (or is even stronger) in the segments — Free plan, Mobile — that represent a large share of the user base,

the recommendation is to **launch to all users, conditioned on**:
1. Scaling support capacity ahead of launch to absorb the ~10 pp increase in ticket rate, and
2. Instrumenting ongoing monitoring of revenue-per-converted-user and refund rate post-launch, since both moved in a less favorable direction and the refund-rate effect was not yet statistically confirmed at this sample size.

An alternative, more conservative option leadership could choose instead is to **launch only for the Free and Mobile segments** first (where the lift is largest and most clearly worth the support cost), while continuing to test on Basic-plan users, where the campaign showed little benefit.

## Risks and Limitations

- **Small converter sample:** Only 72 users converted across both groups combined (22 Control, 50 Treatment). This is enough to detect the conversion-rate effect, but makes revenue-per-converted-user and refund-rate estimates noisier and less reliable.
- **Revenue outliers:** A small number of very high-revenue converted users (e.g., one Basic-plan user with $8,610.72 in 30-day revenue) pull the Control group's average revenue per converted user upward; the gap between groups shrinks but does not disappear when these are excluded (see `analysis/experiment_analysis.xlsx`, sheet `Revenue_Outliers`).
- **Refund rate not yet significant:** The refund-rate increase (0% → 0.42%) is directionally concerning but not statistically confirmed; it should not be treated as proven risk, only as something to watch.
- **Short observation window:** Revenue and refunds are measured at 30 days; longer-term retention, churn, and lifetime value are not observed in this dataset and could change the calculus.
- **No causal claim beyond conversion:** While the conversion result is causal (from random assignment), the segment-level and guardrail findings are observational subgroup comparisons within the experiment and should be treated as directional, not independently confirmed.

## Next Steps

1. Brief the support team on expected ticket volume increase and ensure adequate staffing before full launch.
2. Stand up a post-launch dashboard tracking conversion rate, refund rate, support ticket rate, and revenue per converted user weekly for the first 60–90 days.
3. Investigate *why* Treatment converts more Free-tier and lower-value users — is the campaign messaging/incentive structure attracting more price-sensitive signups, and is that an acceptable trade-off long-term?
4. Re-run the refund-rate comparison once a larger sample of converted users has accumulated post-launch, since the current test was underpowered to confirm or rule out that risk.
5. Consider a follow-up experiment specifically targeting Basic-plan users, since the current campaign shows little benefit for that segment.
