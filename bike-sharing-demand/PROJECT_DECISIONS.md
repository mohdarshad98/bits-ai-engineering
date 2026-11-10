# Project Decisions — Bike Sharing Demand Prediction

## 1. Business Context

### Original Business Request

We operate a bike-sharing service. Using historical hourly data, build a model that predicts bike rental demand so our operations team can plan bike availability.

### Intended Consumer

The primary intended consumer of the predictions is the operations team, which will use demand estimates to support fleet availability and operational planning.

## 2. Business Objectives

We will investigate two competing operational objectives.

### Scenario A — Fleet Efficiency and Cost Control

Minimise unnecessary fleet allocation, repositioning and associated operational costs while maintaining an acceptable level of bike availability.

- Overprediction may result in excess bikes, unnecessary repositioning and operational costs.
- Underprediction may result in insufficient availability and lost rental opportunities.

### Scenario B — Customer Availability and Service Level

Prioritise bike availability and minimise shortages, accepting potentially higher fleet allocation and operational costs.

- Underprediction may result in stockouts, lost rentals and customer dissatisfaction.
- Overprediction may result in excess bikes and additional operational costs.

## 3. Confirmed Decisions

| ID | Decision | Rationale |
|---|---|---|
| D-01 | Use the hourly dataset (`hour.csv`). | It provides more granular information about demand patterns than daily aggregates. |
| D-02 | Treat rental demand as a numerical prediction problem (regression). | The intended output is an estimate of rental counts rather than a demand category. |
| D-03 | Explore both fleet-efficiency and customer-availability optimization scenarios. | The operational consequences of overprediction and underprediction differ. |

## 4. Working Assumptions

| ID | Assumption | Reason | Revisit when |
|---|---|---|---|
| A-01 | Hourly demand prediction is the initial modelling direction. | The source data provides hourly observations. | Evidence from data investigation or modelling suggests a different prediction granularity is more appropriate. |
| A-02 | Actual rental count (`cnt`) is our initial target variable. | It represents observed total rentals. | We discover that observed rentals do not adequately represent actual demand. |

These assumptions are provisional and must not be treated as confirmed stakeholder requirements.

## 5. Open Business Questions

| ID | Question | Why it matters |
|---|---|---|
| Q-01 | Should predictions be hourly, daily or both? | Determines the required output granularity and operational usefulness. |
| Q-02 | How far ahead must demand be predicted? | Determines the prediction horizon and available input information. |
| Q-03 | How much advance notice does the operations team require? | Determines when predictions must be generated relative to the demand period. |
| Q-04 | Is future weather forecast information available at prediction time? | Determines whether weather variables can legitimately be used as future inputs. |
| Q-05 | Does the business require location-specific predictions? | Determines whether the available dataset has sufficient geographic detail. |
| Q-07 | Does observed rental count adequately represent actual customer demand? | Rentals may not capture customers who could not rent because bikes were unavailable. |

## 6. Closed Questions

| ID | Question | Resolution | Reason |
|---|---|---|---|
| Q-06 | What are the relative financial or operational costs of overprediction and underprediction? | Closed — no specific cost assumptions will be made. | There is no business stakeholder to provide actual cost information. Both optimization scenarios can still be investigated using prediction-error metrics and experimental penalty ratios without claiming to represent actual business costs. |

## 7. Initial Modelling Implications

1. `cnt` is the initial target variable.
2. `casual` and `registered` must not be used as predictors because they directly sum to `cnt` and would not be available before the rentals occur.
3. Historical weather observations cannot automatically be used as future weather inputs. Forecast availability and timing must be established.
4. `temp` and `atemp` require investigation for their relationship and operational availability.
5. Model validation must reflect the intended prediction scenario. For future-demand prediction, chronological validation should be considered instead of relying on random train/test splitting.
6. Model evaluation must consider both overprediction and underprediction, not just overall prediction accuracy.
7. Actual rental counts may underestimate latent demand when bike availability constrains rentals.

## 8. Decision History

| Date | Update |
|---|---|
| 2026-11-08 | Initial business objectives, optimization scenarios, confirmed decisions, working assumptions and open questions documented. |

This document will evolve as data investigation provides evidence and business assumptions are refined.

Decisions should be revisited when new evidence, constraints or requirements justify a change.