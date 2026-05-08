# Monte-Carlo IBNR Reserving — Canadian P&C Market

**Author:** Reda Hakkani | PhD Candidate, Applied Mathematics | Montréal, QC  
**Domain:** Actuarial Reserving · Canadian P&C Insurance  
**Regulatory context:** OSFI MCT · OSFI A-4 · CIA Standards · IFRS 17

---

## Overview

IBNR reserve estimation using **Bootstrap Chain-Ladder** with **10,000 Monte-Carlo simulations** for Canadian personal auto bodily injury claims.

**Line of business:** Ontario Personal Auto — Bodily Injury (BI)  
**Accident years:** 2015–2024 (10-year triangle)  
**Regulatory requirement:** OSFI Minimum Capital Test (MCT) at 99.5% VaR

---

## Key Results

| Metric | Value |
|--------|-------|
| Development Triangle | 10 accident years × 10 development periods |
| **Chain-Ladder Best Estimate** | **CAD 772.6M** |
| **Bornhuetter-Ferguson** | **CAD 789.0M** |
| Bootstrap Mean Reserve | CAD 822.7M |
| Coefficient of Variation | 10.69% |
| VaR 75% (Going-concern CIA) | CAD 880.2M |
| VaR 90% | CAD 938.8M |
| **VaR 99.5% (OSFI MCT SCR)** | **CAD 1,068.1M** |
| Risk Margin above CL | CAD 295.5M (+38.3%) |
| Bootstrap Simulations | 10,000 |

---

## Development Triangle — Ontario Personal Auto BI (CAD millions)

|  | D1 | D2 | D3 | D4 | D5 | D6 | D7 | D8 | D9 | D10 |
|--|----|----|----|----|----|----|----|----|----|----|
| **AY2015** | 95.8 | 159.1 | 195.6 | 224.4 | 250.4 | 265.3 | 265.6 | 268.2 | 282.2 | **285.5** |
| **AY2016** | 99.8 | 170.0 | 196.9 | 233.1 | 262.4 | 274.9 | 277.8 | 285.3 | **296.7** | — |
| **AY2017** | 96.5 | 172.9 | 225.3 | 240.4 | 263.2 | 292.3 | 292.6 | **301.9** | — | — |
| **AY2018** | 93.6 | 165.9 | 216.1 | 247.1 | 269.7 | 272.3 | **288.1** | — | — | — |
| **AY2019** | 102.1 | 180.2 | 235.9 | 264.2 | 285.4 | **303.2** | — | — | — | — |
| **AY2020** | 117.9 | 191.3 | 245.9 | 274.8 | **302.8** | — | — | — | — | — |
| **AY2021** | 109.3 | 194.7 | 240.5 | **266.7** | — | — | — | — | — | — |
| **AY2022** | 114.0 | 201.8 | **259.8** | — | — | — | — | — | — | — |
| **AY2023** | 131.0 | **216.4** | — | — | — | — | — | — | — | — |
| **AY2024** | **129.7** | — | — | — | — | — | — | — | — | — |

*Bold = latest diagonal (observed). Blanks = IBNR to estimate.*

---

## Link Development Factors (LDFs) — Ontario BI Pattern

| D1→D2 | D2→D3 | D3→D4 | D4→D5 | D5→D6 | D6→D7 | D7→D8 | D8→D9 | D9→D10 | Tail |
|-------|-------|-------|-------|-------|-------|-------|-------|--------|------|
| 1.721 | 1.265 | 1.125 | 1.101 | 1.058 | 1.017 | 1.023 | 1.046 | 1.012 | 1.000 |

*Fast initial development (litigation + direct compensation) → slow tail (CAT BI, long-term disability)*

---

## Methodology

### Step 1 — Chain-Ladder (Volume-Weighted)
```
Observed cumulative triangle (10×10)
              │
              ▼
Age-to-Age LDF (volume-weighted average)
              │
              ▼
CDF-to-Ultimate by accident year
              │
              ▼
IBNR = Ultimate − Latest Paid Diagonal
```

### Step 2 — Bornhuetter-Ferguson (CIA Credibility Blend)
```
CIA a priori ELR (FSRA filing history)
              │
              ├── × % Unreported (1 − 1/CDF)
              │
              ▼
BF IBNR = A Priori × (1 − 1/CDF)

CIA recommendation:
- Recent years (AY2022+): high BF weight (low credibility)
- Older years (AY2015–2018): high CL weight (full credibility)
```

### Step 3 — Bootstrap Chain-Ladder (10,000 simulations)
```
Observed triangle
              │
              ▼
Pearson residuals (observed vs CL fitted)
              │
              ▼
Repeat 10,000 times:
  ├── Resample residuals (bootstrap with replacement)
  ├── Reconstruct pseudo-triangle
  ├── Re-estimate LDFs on pseudo-triangle
  ├── Project future payments + process variance
  │   (overdispersed Poisson structure)
  └── Store simulated IBNR
              │
              ▼
Reserve distribution
  → VaR 75%  (CIA going-concern)
  → VaR 90%
  → VaR 99.5% (OSFI MCT SCR requirement)
```

---

## IBNR by Accident Year

| Accident Year | Latest Paid | % Developed | CL Ultimate | IBNR (CL) | IBNR (BF) |
|---------------|------------|-------------|-------------|-----------|-----------|
| AY2015 | 285.5M | 100.0% | 285.5M | 0.0M | 0.0M |
| AY2016 | 296.7M | 98.8% | 300.2M | 3.5M | 3.8M |
| AY2017 | 301.9M | 94.5% | 319.5M | 17.6M | 18.1M |
| AY2018 | 288.1M | 92.4% | 311.9M | 23.8M | 24.5M |
| AY2019 | 303.2M | 90.8% | 334.0M | 30.8M | 32.1M |
| AY2020 | 302.8M | 85.8% | 352.8M | 50.1M | 53.4M |
| AY2021 | 266.7M | 78.0% | 342.1M | 75.4M | 78.9M |
| AY2022 | 259.8M | 69.3% | 374.9M | 115.1M | 121.2M |
| AY2023 | 216.4M | 54.8% | 395.0M | 178.6M | 184.3M |
| AY2024 | 129.7M | 31.8% | 407.4M | 277.7M | 272.7M |
| **TOTAL** | | | **3,423.3M** | **772.6M** | **789.0M** |

---

## Regulatory Framework

| Standard | Requirement | Application in this model |
|----------|-------------|--------------------------|
| **OSFI MCT** | Capital at 99.5% VaR | ✓ VaR 99.5% = CAD 1,068.1M |
| **OSFI A-4** | P&C reserve standards | ✓ Bootstrap uncertainty |
| **CIA P&C** | Appointed Actuary Report | ✓ CL + BF dual methods |
| **IFRS 17** | Risk adjustment | ✓ Reserve percentiles |
| **Solvency II** | SCR equivalent | ✓ Same 99.5% confidence |

---

## Installation

```bash
git clone https://github.com/RedaHakkani/monte-carlo-ibnr-reserving.git
cd monte-carlo-ibnr-reserving
pip install -r requirements.txt
python src/ibnr_reserving.py
```

## Requirements

```
numpy>=1.24.0
pandas>=2.0.0
scipy>=1.11.0
matplotlib>=3.7.0
```

## Output

- Full console report (triangle, LDFs, IBNR by year, VaR matrix)
- `ibnr_reserving_results.png` — 6-panel actuarial dashboard

---

## References

- England, P.D. & Verrall, R.J. (2002). *Stochastic Claims Reserving in General Insurance*. IoA.
- Mack, T. (1993). *Distribution-free Calculation of the Standard Error of Chain-Ladder Estimates*. ASTIN.
- CIA (2020). *Practice-Specific Standards for Property and Casualty Insurance*.
- OSFI (2022). *Guideline A-4 — Property and Casualty Insurance*.
- FSRA (2023). *Ontario Automobile Insurance Reporting Requirements*.

---

*Reda Hakkani — PhD Candidate, Applied Mathematics | Montréal, QC*  
*Available for actuarial and quantitative risk roles — hakkanireda@hotmail.com*
