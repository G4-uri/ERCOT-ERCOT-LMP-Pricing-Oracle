# Calibrating Tail Risk for ERCOT Supplier Collateral

**Time-of-day and volatility-filtered VaR for 5-minute and day-ahead LMP exposure**

Gauri Santhosh Nair · CHRIST (Deemed to be University), Bengaluru

This project asks one question: **how much collateral should a supplier hold against ERCOT price exposure?** It does not try to predict the next price. It sizes a 99% Value-at-Risk (VaR99) for a 50 MW position and tests it walk-forward, with no look-ahead, on real ERCOT data.

> **Status.** The full methodology, parameters and diagnostics are being prepared for a paper and are intentionally not published here. This page reports headline findings only.

> **Correction notice.** An earlier version of this work reported a 2.6% day-ahead forecasting edge over persistence. That result came from an implementation error and is **withdrawn**. The corrected model is statistically tied with persistence, and the project is now framed as risk calibration, not price forecasting.

## Headline results

| | Static parametric VaR | Volatility-filtered, time-of-day VaR |
|---|---|---|
| Breach rate, all hours (target 1%) | 0.83% | 1.15% |
| Breach rate, 17:00–22:00 evening ramp | **2.31%** | **1.20%** |
| Breach severity (mean loss / VaR) | 2.65× | 1.33× |
| Pinball loss (τ = 0.99) | 18.46 | 7.38 |
| VaR as share of notional | 49.7% | 29.3% |

Real-time, HB_HOUSTON, 31,119 five-minute walk-forward steps.

1. **A static VaR looks right and is wrong.** Its aggregate breach rate hides a much higher rate in the evening ramp, where the largest losses occur, and its breaches cluster.
2. **Conditioning on time of day and current volatility fixes most of it.** The evening breach rate moves close to target, breach severity roughly halves, and the result holds across the alternative settings tested.
3. **Day-ahead, no method beats posting full notional** at 99% while keeping breaches near target, across 4,275 causal steps at DA_NORTH.
4. **No point-forecast edge** over persistence in either market.
5. **Hawkes self-excitation is not supported** as a risk driver on the real data tested, beyond what time of day already explains.

## Approach

Loss is the one-sided drop a seller bears on a fixed 50 MW position. Several VaR constructions are scored against each other on identical steps, using standard coverage and independence tests, pinball loss, capital efficiency and breach severity. All calibration is walk-forward: nothing fitted on a test step is used to score it. Methods, parameters, window lengths and diagnostics are withheld pending the paper.

## Data

Real ERCOT data throughout: 5-minute real-time LMP at HB_HOUSTON, and hourly day-ahead LMP with load, wind and solar at DA_NORTH. Results are labelled real or synthetic and never mixed.

## Limitations

1. One node and one season per horizon.
2. A fixed one-sided 50 MW position held one step; buyer-side risk, multi-step holding and ERCOT's actual collateral rules are not modelled.
3. Only the 99% level is tested; expected-shortfall haircuts are untested.
4. Breaches still cluster day-ahead, and coverage tests are optimistic where breaches are not independent.
5. Some settings were examined after seeing results and are exploratory.
6. The Hawkes finding is "not supported at current sample sizes", not "disproven".

## Next steps

More nodes and a longer history, fundamentals-conditioned day-ahead risk, adaptive calibration, and an expected-shortfall haircut.

## Rights

© Gauri Santhosh Nair. All rights reserved. Please do not reproduce or build on the methodology without permission. Contact: gauri.santhoshnair@bscdsmh.christuniversity.in

