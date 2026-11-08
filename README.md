# Sunfield Solar – Project Finance Model

An interactive project finance model for a 50 MWp utility-scale solar PV farm in the UK. It sizes senior debt by DSCR sculpting, solves the tax–interest circularity, and calculates equity and project returns. Every output updates live as the assumptions change.

**Live demo:** https://kuzeyoznr.github.io/solar-pf-model

---

## Features

- **Sculpted debt sizing.** Senior debt is sized so that CFADS ÷ debt service equals a target DSCR in every year of the tenor, capped by a maximum gearing.
- **Circularity solved by iteration.** Interest reduces tax, tax changes CFADS, and CFADS changes the debt size. The model iterates until debt converges, the same job a copy-paste macro does in Excel.
- **Returns and credit metrics.** Equity IRR, project IRR, NPV, LCOE, minimum and average DSCR, and equity payback.
- **Scenarios.** One-click Downside, Base and Upside cases.
- **Sensitivities.** An equity IRR tornado chart and a two-way data table (PPA price × capex).
- **Cash flow waterfall.** Full annual breakdown from generation to cash to equity.
- **Formula bar.** Click any output to see how it is calculated.
- **Spreadsheet conventions.** Blue cells are hardcoded inputs; black values are calculated.

## Base case results

| Metric | Value |
|---|---|
| Capex | £27.5m |
| Senior debt | £20.6m (75% gearing) |
| Equity IRR | 14.1% |
| Project IRR | 8.9% |
| LCOE | ~£67/MWh |
| Min DSCR | 1.41x |

### Scenario comparison

| Scenario | Equity IRR | Gearing | Min DSCR | Binding constraint |
|---|---|---|---|---|
| Downside | 5.5% | 53% | 1.30x | DSCR |
| Base | 14.1% | 75% | 1.41x | Gearing cap |
| Upside | 24.6% | 75% | 1.90x | Gearing cap |

In the downside case, weaker cash flows make the DSCR target the binding constraint, so the project can support less debt. In the base and upside cases, the gearing cap binds first and the DSCR sits above target.

## Methodology

**1. Operations (each year *t*)**
- Generation = Capacity × 8,760 h × Capacity factor × (1 − Degradation)^(t−1)
- Revenue = Generation × PPA price, indexed at CPI
- EBITDA = Revenue − Opex (opex also indexed at CPI)

**2. Debt sizing**
- Sculpted debt service = CFADS ÷ Target DSCR, for each year of the tenor
- Debt capacity = PV of sculpted debt service at the interest rate
- Senior debt = MIN(Debt capacity, Max gearing × Capex)
- Interest = Opening balance × Rate; Principal = Debt service − Interest

**3. Tax**
- Taxable profit = EBITDA − Depreciation − Interest
- Straight-line depreciation over the operating life
- Losses carried forward

**4. Returns**
- CFADS = EBITDA − Tax
- Cash to equity = CFADS − Debt service
- Equity IRR = IRR(−Equity invested, cash to equity)
- Project IRR = IRR(−Capex, EBITDA − unlevered tax)
- LCOE = (Capex + PV of opex) ÷ PV of generation

## Base case assumptions

| Input | Value |
|---|---|
| Installed capacity | 50 MWp |
| Net capacity factor | 11% |
| Annual degradation | 0.4% |
| Operating life | 30 years |
| PPA price (year 1) | £68/MWh |
| CPI indexation | 2.5% p.a. |
| Capex | £550k/MWp |
| Opex (year 1) | £12k/MWp p.a. |
| Max gearing | 75% |
| Target DSCR | 1.30x |
| All-in interest rate | 6.5% |
| Debt tenor | 18 years |
| Corporation tax | 25% |
| Discount rate | 7.5% |

All figures are illustrative.

## Simplifications

- No construction period or interest during construction (IDC)
- No debt service reserve account (DSRA)
- Straight-line tax depreciation rather than UK capital allowances
- Unlimited loss carry-forward
- Single PPA price with no merchant tail

Possible extensions: construction phase with IDC, DSRA, capital allowance pools, merchant price curve after the PPA term, and P50/P90 energy cases.

## How to run

No installation needed.

1. Download `index.html`
2. Open it in any modern browser

## Tech

Plain HTML, CSS and JavaScript in a single file. No frameworks or build step. Charts are drawn as inline SVG.

## Author

**Kuzey Öznur**, Electronics Engineering, King's College London
[LinkedIn](https://linkedin.com/in/kuzey-oznur-ab7b551a8) · [GitHub](https://github.com/kuzeyoznr)
