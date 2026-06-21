# Hypothesis Test Notes

## Metric Being Tested
**Paid conversion rate** (`converted_to_paid`) — the proportion of users in each group who became paying customers.

### Reason for Choosing This Metric
Paid conversion rate is the North Star metric for this experiment (see `README.md` and `outputs/recommendation_memo.md` for full justification). The business decision — whether to launch the new onboarding campaign — depends on whether it actually drives more users to become paying customers, not just whether it improves an upstream funnel step like landing page visits or trial starts. Testing the North Star directly keeps the hypothesis test tied to the real business decision rather than a proxy metric.

## Hypotheses

- **Null hypothesis (H0):** There is no difference in paid conversion rate between the Control group and the Treatment group. `p_treatment = p_control`
- **Alternate hypothesis (H1):** The paid conversion rate differs between the Treatment group and the Control group. `p_treatment ≠ p_control`

## Test Type
- **Two-tailed test.** We did not assume in advance that the campaign could only help — a two-tailed test allows us to detect a meaningful change in either direction (improvement or harm), which is the more conservative and defensible choice before launching to all users.
- **Statistical test used:** Two-proportion z-test, comparing the conversion rate of two independent groups.

## Significance Level
- **α = 0.05** (95% confidence), the standard threshold for business experimentation decisions of this kind.

## Test Inputs
| Group | Conversions | Users | Conversion Rate |
|---|---|---|---|
| Control | 22 | 690 | 3.19% |
| Treatment | 50 | 710 | 7.04% |

(Counts are post-cleaning, after removing 8 duplicate user records — see `README.md` for data cleaning details.)

## Test Output
- Pooled proportion: 0.0514
- Standard error: 0.0118
- **Z-statistic: 3.264**
- **P-value (two-tailed): 0.0011**
- 95% confidence interval on the difference: **[1.56 pp, 6.15 pp]**

Full calculation is shown in `analysis/experiment_analysis.xlsx` (sheet: `Hypothesis_Test`) and captured in `screenshots/hypothesis_test_output.png`.

## Decision Rule
Reject H0 if p-value < α (0.05).

## Result and Interpretation
The p-value (0.0011) is well below 0.05, so **we reject the null hypothesis**. The difference in paid conversion rate between Treatment (7.04%) and Control (3.19%) is statistically significant — it is very unlikely to have occurred by chance alone. The 95% confidence interval [1.56 pp, 6.15 pp] does not cross zero, reinforcing that this is a real effect, not noise.

## Connection to the Business Decision
This result tells leadership that the new onboarding and activation campaign **does cause a real lift in paid conversion** — more than doubling it in relative terms (+120.9%). However, statistical significance on the North Star metric alone is **not sufficient to approve a full launch**. As required by the assignment and outlined in `outputs/recommendation_memo.md`, this result must be weighed against guardrail metrics (refund rate, support ticket rate, revenue quality per converted user, and segment-level performance) before a final launch decision is made. A statistically significant lift in conversion that comes packaged with a doubling of support tickets and a drop in average revenue per converted user is not an unambiguous "launch to everyone" signal — it's a "launch with conditions" signal.
