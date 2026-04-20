---
title: "Why does the 2022 operating result differ between reports?"
type: question
status: open
created: 2026-04-19
updated: 2026-04-19
tags: [financials, 2022, restatement, operating-result]
---

# Why does the 2022 operating result differ between reports?

## Question

The 2022 operating result is reported as **754 MioEUR** in the Annual Report 2022 but appears as **769 MioEUR** in later reports (AR 2023 and AR 2025 comparative tables). What accounts for the 15 MioEUR difference?

## Context

- [Annual Report 2022](../sources/ar-2022.md) states: Operating Result 754 MioEUR (margin 11.8%)
- [Profitability Trends](../financials/profitability-trends.md) uses 769 MioEUR (margin 12.0%), sourced from AR 2025 comparative tables
- The [extraction notes](../_extraction_notes.md) flag this explicitly: "op. result EUR 754m (originally reported as 769 restated)"
- The wiki currently uses the restated 769 figure in all time-series tables ([Overview](../overview.md), [Profitability Trends](../financials/profitability-trends.md))

## Current Thinking

The most likely explanation is a **Purchase Price Allocation (PPA) adjustment**. When companies make acquisitions, IFRS requires allocation of the purchase price to identifiable assets, which can retroactively affect operating results in comparative periods. Rheinmetall made several acquisitions around 2022 (Expal, Loc Performance), and PPA adjustments are a standard reason for restated operating results in subsequent annual reports.

The 2023 AR introduced "EBIT before PPA" as a new metric, which supports the hypothesis that PPA effects were becoming material enough to track separately.

## Resolution

**Not yet resolved.** To confirm:

1. Check AR 2023 notes for 2022 comparative restatements (IFRS requires disclosure of prior-period adjustments)
2. Look for PPA amortization schedules in AR 2023 or AR 2025
3. Check if the 15 MioEUR delta aligns with known acquisition PPA charges
