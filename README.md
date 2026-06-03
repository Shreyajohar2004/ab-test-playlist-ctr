# A/B Test Analysis: Personalised Playlist Thumbnail CTR

**End-to-end experimentation analysis in Python — designed to mirror real-world A/B testing at consumer tech companies.**

---

## Business Context

A music streaming platform wants to know: does showing **personalised playlist thumbnails** (generated from a user's listening history) increase click-through rates compared to **generic editorial thumbnails**?

This project runs a full end-to-end product experiment — from pre-experiment power analysis through to a final ship/no-ship recommendation — the same workflow used by data science teams at companies like Spotify, Netflix, and Revolut.

---

## Dataset

**Source:** [Kaggle — A/B Testing Dataset](https://www.kaggle.com/datasets/zhangluyuan/ab-testing)

Real-world e-commerce A/B test with ~294,000 user records testing a new webpage design. Reframed here in a music streaming context to demonstrate applied experimentation skills.

| Column | Description |
|---|---|
| `user_id` | Unique user identifier |
| `timestamp` | Visit timestamp |
| `group` | `control` or `treatment` |
| `landing_page` | `old_page` or `new_page` |
| `converted` | 1 = clicked/converted, 0 = did not |

After cleaning (removing group/page mismatches and duplicate user IDs): **290,584 records**, 145,274 control / 145,310 treatment.

---

## What This Project Covers

| Stage | What I did |
|---|---|
| **Experiment design** | Defined hypothesis, primary metric (CTR), guardrail metric, significance level (α = 0.05), and minimum detectable effect (2pp) |
| **Power analysis** | Calculated required sample size: 3,501 per group using Cohen's h effect size. Actual dataset: 145,000+ per group — well-powered. |
| **Data cleaning** | Removed mismatched group/page assignments (3,894 rows) and duplicate user IDs |
| **Sanity check — SRM** | Chi-square test confirmed 50/50 split (χ² = 0.0045, p = 0.9468). No assignment issues. |
| **Hypothesis testing** | Two-proportion z-test (one-tailed): treatment vs control CTR |
| **Confidence intervals** | Wilson method 95% CIs on both variants |
| **Segment analysis** | Broke results by new / returning / power-user cohorts |
| **Visualisation** | 3-panel chart: CTR with error bars, sampling distribution, segment lift |
| **Decision recommendation** | Honest no-ship recommendation with analysis of why the treatment failed |

---

## Key Results

| Metric | Control | Treatment | Lift | Significant? |
|---|---|---|---|---|
| Click-through rate | 12.04% | 11.88% | −0.16 pp (−1.3% relative) | **No** (p = 0.9051) |
| SRM check | 145,274 users | 145,310 users | χ² = 0.0045, p = 0.947 | ✅ Pass |

**Segment analysis:** No segment showed a statistically significant lift. The negative trend was consistent across new, returning, and power users.

---

## Decision Recommendation

> **Recommendation: Do NOT ship the personalised thumbnail feature.**

The treatment showed a small *negative* effect on CTR (−0.16 percentage points), and the result is far from statistical significance (p = 0.9051). There is no evidence the personalised thumbnails improve user engagement — if anything, the data suggests a marginal decrease.

### Next steps before any future test

1. **Qualitative research:** Understand *why* users preferred the original thumbnails. Are generic editorial thumbnails more visually polished? Do users not recognise their own listening history in thumbnail form?

2. **Redesign the treatment:** If personalisation is still a strategic priority, test a different implementation — e.g. artist face recognition, genre-based colour coding, or hybrid editorial + personal signals.

3. **Re-run with a new MDE:** If the business only cares about detecting larger effects (e.g. 5pp+), recalculate sample size and re-run with a longer test window.

4. **Check for interaction effects:** Did the experiment run during an unusual period (holiday season, major artist release) that might have confounded results?

---

## Why Sanity Checks Matter

Most beginner A/B testing tutorials skip straight to the p-value. Real experimentation teams run **pre-analysis sanity checks** before interpreting any results:

- **Sample Ratio Mismatch (SRM):** If users weren't assigned 50/50 as intended, the entire experiment is invalid. This notebook checks for SRM using a chi-square test — result: clean.
- **Segment balance:** Are control and treatment groups similar in composition? Checked via cross-tabulation.

These checks are standard practice at companies like Spotify (documented in their [SAFE framework](https://engineering.atspotify.com/2023/09/the-safe-data-driven-framework-for-product-decisions/)) and Microsoft's ExP platform.

---

## Tech Stack

```
Python 3.10+
├── numpy
├── pandas
├── scipy
├── statsmodels
└── matplotlib
```

---

## How to Run

```bash
# Clone the repo
git clone https://github.com/Shreyajohar2004/ab-test-playlist-ctr.git
cd ab-test-playlist-ctr

# Install dependencies
pip install numpy pandas scipy statsmodels matplotlib

# Download dataset from Kaggle
# https://www.kaggle.com/datasets/zhangluyuan/ab-testing
# Save as ab_data.csv in this folder

# Launch notebook
jupyter notebook ab_testing.ipynb
```

---

## Project Structure

```
ab-test-playlist-ctr/
├── ab_testing.ipynb        # Full analysis notebook
├── ab_test_results.png     # Output visualisation (auto-generated on run)
└── README.md               # This file
```

---

## Key Learning: Null Results Are Valuable

A common misconception is that a "failed" A/B test is a bad portfolio piece. The opposite is true. **Shipping something that doesn't work is expensive.** A null result that is correctly identified and clearly communicated — with honest analysis and actionable next steps — demonstrates exactly the rigour that data science teams at Spotify, Revolut, and similar companies are looking for.

---

## About

Built as part of a self-directed portfolio targeting Data Scientist roles in consumer tech.  
**Author:** Shreya Johar · [LinkedIn](https://linkedin.com/in/shreya-johar-6816a424b) · [GitHub](https://github.com/Shreyajohar2004)
