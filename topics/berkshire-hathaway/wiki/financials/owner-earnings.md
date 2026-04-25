---
title: "Owner Earnings"
type: financial
sources:
  [
    buffett-letter-1983,
    buffett-letter-1986,
    buffett-letter-1994,
    buffett-letter-2000,
    buffett-letter-2002,
    buffett-letter-2010,
    buffett-letter-2015,
    buffett-letter-2024,
  ]
created: 2026-04-22
updated: 2026-04-22
tags: [valuation, accounting, capital-allocation, metric]
status: complete
---

## Definition

**Owner earnings** are Buffett's alternative to GAAP net income, designed to answer the question: **"How much cash can the owner extract while maintaining the business's competitive position?"**

The formula, first presented in the [1986 Letter](../sources/buffett-letter-1986.md):

> **(a)** reported earnings  
> **plus (b)** depreciation, depletion, amortization, and other non-cash charges  
> **less (c)** the average annual amount of capitalized expenditures for plant and equipment that the business requires to maintain its competitive position and unit volume

Owner earnings = **(a) + (b) − (c)**

The critical insight: item **(c)** can be _less than_ or _greater than_ item **(b)**. GAAP treats depreciation as the cost of maintaining assets, but actual maintenance capital needs vary enormously by industry[^1].

## The 1986 Scott Fetzer Proof

Buffett used [Scott & Fetzer](../case-studies/scott-fetzer-acquisition.md) to demonstrate why GAAP earnings fail:

**Company O (actual business):** Pre-tax owner earnings of **$40.2M**
**Company N (GAAP-reported):** Earnings of only **$28.6M** — reduced by $11.6M in annual goodwill amortization on the $142.6M purchase premium

Both companies were _identical_ operationally. The $11.6M gap — purely an accounting artifact from acquisition accounting — would persist for 38 years, systematically understating the business's earning power by ~29%[^2].

### How Capital Requirements Vary

| Business Type                                                                                   | Depreciation vs. Maintenance Capex | Owner Earnings vs. GAAP                    |
| ----------------------------------------------------------------------------------------------- | ---------------------------------- | ------------------------------------------ |
| Capital-light consumer franchise ([See's Candies](../case-studies/sees-candies-acquisition.md)) | Depreciation > maintenance capex   | Owner earnings > GAAP earnings             |
| Capital-light diversified ([Scott Fetzer](../case-studies/scott-fetzer-acquisition.md))         | Depreciation >> maintenance capex  | Owner earnings >> GAAP earnings (29%+ gap) |
| Capital-intensive railroad ([BNSF](../case-studies/bnsf-acquisition.md))                        | Depreciation < maintenance capex   | Owner earnings < GAAP earnings             |
| Capital-intensive utility (BHE)                                                                 | Depreciation ≈ maintenance capex   | Owner earnings ≈ GAAP earnings             |

**[2015 Letter]** BNSF's capital needs consistently exceeded depreciation, meaning GAAP _overstated_ owner earnings — the reverse of the Scott Fetzer pattern. This is why Buffett emphasized "the difference between reported and economic depreciation can be enormous" — it can go either direction[^3].

## The Sainted Seven / Look-Through Earnings

**[1987 Letter]** Buffett introduced the "Sainted Seven" — Berkshire's seven largest non-insurance operating businesses — and reported their combined earnings to show the gap between GAAP per-share figures and actual earning power. By 1988, the group expanded (with Scott Fetzer) to what the letter called "the Sainted Seven Plus One."

**[1991 Letter]** "Look-through earnings" formalized the concept for stock investments: instead of counting only dividends received, count Berkshire's proportional share of each investee's total earnings. By the 2000s, look-through earnings were $600M+ above reported earnings — because investees like [Coca-Cola](../case-studies/coca-cola-investment.md) and [Gillette](../case-studies/gillette-investment.md) retained most earnings for reinvestment[^4].

## Connection to Operating Earnings

Owner earnings and [operating earnings](operating-earnings.md) are complementary but distinct metrics:

- **Operating earnings** strip out investment gains/losses to reveal the recurring earning power of Berkshire's businesses and investments
- **Owner earnings** adjust for the gap between GAAP depreciation and true maintenance capex to reveal how much cash an owner can actually extract
- A business can have strong operating earnings but weak owner earnings (if maintenance capex exceeds depreciation), or the reverse

For Berkshire's capital-light businesses (See's, Scott Fetzer, consumer brands), owner earnings consistently exceeded operating earnings. For capital-intensive businesses (BNSF, BHE), the reverse applied.

## Why It Matters for Valuation

**[2002 Letter]** Buffett: "Investors should understand that in all types of businesses, the returns that can be achieved by shareholders, through dividends or through retained earnings, depend on the _owner earnings_ of the enterprise."

The practical consequence for valuing Berkshire: GAAP book value understated the true value of capital-light subsidiaries (their owner earnings far exceeded reported earnings) while potentially overstating the value of capital-intensive ones. This is why Buffett insisted on [intrinsic value](intrinsic-value-vs-book-value.md) as the relevant measure — it incorporates owner economics that GAAP systematically obscures[^5].

## Sources

[^1]: [1986 Letter](../sources/buffett-letter-1986.md) — Owner-earnings formula; "(a) + (b) − (c)"; "the difference can be enormous"

[^2]: [1986 Letter](../sources/buffett-letter-1986.md) — Company O vs. Company N; $40.2M vs. $28.6M; 38-year distortion

[^3]: [2015 Letter](../sources/buffett-letter-2015.md) — BNSF capital needs exceed depreciation; infrastructure economics

[^4]: [1991 Letter](../sources/buffett-letter-1991.md) — Look-through earnings concept; gap between dividends received and proportional earnings

[^5]: [2002 Letter](../sources/buffett-letter-2002.md) — Owner earnings as foundation of shareholder returns and intrinsic value
