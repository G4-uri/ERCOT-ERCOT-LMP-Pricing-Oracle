# ERCOT-LMP-Pricing-Oracle
Hawkes jump-diffusion and Extreme Value Theory pricing and collateral-risk model for ERCOT day-ahead LMPs, purpose-built for supplier-side bid sizing and collateral haircuts rather than execution




# ERCOT LMP Pricing Oracle

**File:** [`ERCOT_LMP_Oracle_EDF_Writeup.docx`](https://docs.google.com/document/d/1fVnq88tNagmZVM5VHRTg2PPQJv8VI80X/edit?usp=sharing&ouid=111928630189653190727&rtpof=true&sd=true)
**Author:** Gauri Nair · CHRIST (Deemed to be University), Bengaluru
**Status:** Oracle Server prototype, real-data validated (DAM); RTM validation ongoing

## What this is

A Hawkes jump-diffusion + Extreme Value Theory (EVT) pricing and collateral-risk model for the ERCOT electricity market. It is built for **supplier-side risk management** — day-ahead bid sizing and collateral haircut estimation — and is explicitly **not** an execution or order-routing system.

For a given ERCOT settlement point, the oracle produces:
- a fair-value LMP estimate for the next pricing interval
- a confidence interval around that estimate
- a collateral haircut combining a jump-diffusion VaR with a separate EVT tail-risk estimate

## Why it's methodologically non-trivial

Standard Hawkes/jump-diffusion models assume log-returns, which are undefined at zero or negative prices — a condition equities rarely hit but electricity prices do regularly. Four years of real ERCOT DAM_NORTH data contain 18 non-positive hourly observations, which silently broke the log-return formulation. The model was rebuilt in arithmetic (price-difference) terms, consistent with established electricity-pricing literature (Cartea & Figueroa, 2005).

## Headline results (real data, not synthetic)

| Test | Result |
|---|---|
| DAM walk-forward backtest | 4,274 out-of-sample hourly steps, 2024-10-31 to 2025-04-27 |
| Model MAE vs naive persistence | 2.6% edge ($6.089 vs $6.2545) |
| 95%-target CI coverage | 99.9% (conservative) |
| 1%-target VaR99 exceedance | 0.0% (conservative) |
| RTM (5-min, 10-day sample) | Inconclusive — no edge over naive persistence yet |

The DAM edge was independently re-verified after finding and fixing two calibration bugs; it reproduced at the same 2.6% figure.

## What's honestly unproven

- The self-exciting Hawkes component has not calibrated on real data at any resolution tested — real history pulled so far is short of the ~45–90+ days needed. Jump-diffusion and EVT currently carry the model's real predictive value.
- RTM (5-minute) results are preliminary and should not be read as validating short-horizon use.
- Validated on two settlement points only (DA_NORTH, HB_HOUSTON); other nodes are untested.
- Several parameters (Hawkes decay range, SCED-RTD trust weight, EIA-shock threshold) are reasoned defaults, not fitted.

Full detail, methodology, and the complete limitations list are in the write-up itself.
