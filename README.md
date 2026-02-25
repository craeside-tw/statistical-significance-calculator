# A/B Significance Calculator

A self-contained, single-page tool for evaluating statistical significance in A/B (and A/B/n) conversion rate experiments. Built for quick analysis of funnel data exported from FullStory, or for manual data entry.

**[Live Demo →](https://craeside-tw.github.io/ab-significance-calculator/)**

## Features

### Data Input
- **CSV Import** — Drag-and-drop or browse for a FullStory funnel export CSV. Parses multi-step funnels with per-segment user counts, drop-off percentages, and median times automatically.
- **Manual Entry** — Enter visitor and conversion counts directly for up to 4 variants. Useful for quick one-off comparisons without a CSV export.

### Analysis
- **Two-proportion z-test** — Compares conversion rates between every pair of variants using a pooled standard error estimate. Computes a z-statistic and p-value for each pair.
- **Bidirectional comparisons** — Each variant shows its lift relative to every other variant (A vs B *and* B vs A), so you can see the relationship from each variant's perspective.
- **Multi-step funnel support** — For CSV imports with multiple funnel steps, each step gets its own breakdown card with conversion rates computed against the entry step's user counts.

### Visual Output
- **Complete Funnel overview** — A summary banner showing full-funnel conversion rates for each variant with comparison cards displaying lift, p-value, and significance badges.
- **Per-step breakdown** — Collapsible section with a card per funnel step, each containing a data table and variant comparison columns.
- **Imported data preview** — Collapsible table showing the raw imported data with a rename panel for giving variants shorter display names.
- **Time to significance** — Estimates how many more days of data collection are needed for each comparison to reach significance, based on the date range of your data.

### Controls

| Setting | Options | Description |
|---|---|---|
| **Confidence level** | 90%, 95%, 99% | Sets the alpha threshold (α = 1 − confidence). A result is "significant" when p < α. |
| **Power** | 80%, 90% | Used for sample size estimation. Higher power requires more samples to detect a given effect. |
| **Two-tailed** | On/Off | Two-tailed tests detect differences in either direction. One-tailed tests only detect improvements (higher conversion rate). |
| **Bonferroni correction** | On/Off | Adjusts α for multiple comparisons by dividing it by the number of pairwise tests. Reduces false positives when comparing 3+ variants. |

## How the Math Works

### Two-Proportion Z-Test

For each pair of variants, the calculator runs a two-proportion z-test:

1. **Pooled proportion**: p̂ = (x₁ + x₂) / (n₁ + n₂)
2. **Standard error**: SE = √[ p̂(1 − p̂)(1/n₁ + 1/n₂) ]
3. **Z-statistic**: z = (p₁ − p₂) / SE
4. **P-value**:
   - Two-tailed: p = 2 × (1 − Φ(|z|))
   - One-tailed: p = 1 − Φ(z)

Where Φ is the standard normal CDF, p₁ = x₁/n₁ and p₂ = x₂/n₂ are the observed conversion rates, and x is conversions out of n total users.

### Significance Badges

- **Significant** — p < α (adjusted for Bonferroni if enabled)
- **Trending** — p < 2α (approaching but not yet significant)
- **Not significant** — p ≥ 2α

### Lift Calculation

Lift is the relative change in conversion rate from one variant's perspective:

**lift = (rate_A − rate_B) / rate_B**

Displayed as a percentage with directional arrows (↗ positive, ↘ negative).

### Required Sample Size

For the time-to-significance estimation, the calculator computes the minimum per-group sample size needed:

**n = (z_α + z_β)² × [ p₁(1−p₁) + p₂(1−p₂) ] / (p₁ − p₂)²**

Where z_α is the critical value for the chosen confidence level and z_β is the critical value for the chosen power. The remaining days estimate assumes constant daily traffic based on your date range.

### Bonferroni Correction

When enabled, the significance threshold α is divided by the number of unique pairwise comparisons: **α_adj = α / C(k,2)** where k is the number of variants. This controls the family-wise error rate when making multiple comparisons simultaneously.

## Tech Stack

Zero dependencies. Single HTML file with inline CSS and vanilla JavaScript. No build step, no framework, no external libraries. The normal distribution CDF and inverse CDF are implemented from scratch using rational approximations.

## Usage

1. Open the page (or the [live demo](https://craeside-tw.github.io/ab-significance-calculator/))
2. Import a FullStory funnel CSV or enter data manually
3. Adjust confidence, power, tailed-ness, and Bonferroni settings as needed
4. Review the Complete Funnel overview and Per-Step Breakdown
5. Optionally rename variants for cleaner display names
6. Enter your test date range to see time-to-significance estimates
