# Demand Forecasting Model Comparison: Accuracy vs. Cost

I compared forecast accuracy with the financial consequences of forecast errors. Comparing three forecasting methods across five retail product categories, and testing whether the model with the best statistical accuracy is actually the best financial decision.

**Tools:** Excel (data cleaning, forecasting models, financial modeling) · Tableau (dashboard)
## Model Performance

-**Best Accuracy**
    Simple Exponential Smoothing
    13.75% MAPE

-**Lowest Estimated Cost**
    3-Month Moving Average
    £313,571

-**Cost Difference**
    Simple Exp. vs Moving Avg
    £39,714
    
![Dashboard showing MAPE and cost comparison across five product categories](Demand%20Forecasting%20Model%20Comparison%20Dashboard.png)
https://public.tableau.com/app/profile/abdulazeez.bankole/viz/DemandForecastingModelComparison/DemandForecastingModelComparisonAccuracyvs_Cost

## Table of Contents

- [Business Problem](#business-problem)
- [Approach](#approach)
- [Key Findings](#key-findings)
- [Recommendation](#recommendation)
- [Limitations](#limitations)

## Business Problem

Getting demand forecasts wrong is expensive in two different ways. Overstocking ties up cash in inventory sitting in a warehouse and adds ongoing storage costs. Understocking is worse, it delays order fulfillment and manufacturing schedules, and ultimately costs lost sales. Because these two failure modes carry very different costs, no single forecasting method can be assumed "correct" without testing it against alternatives.

This analysis compares three forecasting methods across five product categories to answer two questions: which model actually saves the most money, and does the model with the best statistical accuracy (MAPE) also make the best financial decision?

## Approach

Starting from raw transactional data (orders, order line items, and product cost/price records), I built a clean monthly demand history for five product categories spanning 23 months (Jan 2023–Nov 2024; one partial final month was excluded to avoid distorting the results). Data was split into a 17-month training period and a 6-month holdout test period, so every model's accuracy was judged only on data it hadn't seen.

**Models tested:**

| Model | Description |
|---|---|
| 3-Month Moving Average | Trailing average baseline |
| Simple Exponential Smoothing | Smoothing factor (α) tuned per category |
| Holt-Winters Seasonal Method | Excel `FORECAST.ETS`; accounts for trend and seasonality |

Accuracy was scored using **Mean Absolute Percentage Error (MAPE)**. MAPE was then translated into estimated **financial cost**:
- **Understocking** → costed as lost margin per unit short
- **Overstocking** → Overstocking → unit cost × an assumed 20% annual holding rate, prorated monthly.
## Key Findings

### MAPE by category and model

| Category | Moving Average | Simple Exp. Smoothing | Holt-Winters |
|---|---|---|---|
| Outdoor & Sports | 16.09% | **12.42%** | 14.69% |
| Health & Beauty | 9.41% | **8.62%** | 10.73% |
| Home & Kitchen | 19.66% | **16.22%** | 24.81% |
| Office Supplies | 17.93% | 14.43% | **12.90%** |
| Electronics | 18.11% | 17.08% | **15.81%** |
| **Average** | 16.24% | **13.75%** | 15.79% |

### Estimated cost by category and model

| Category | Moving Average | Simple Exp. Smoothing | Holt-Winters |
|---|---|---|---|
| Outdoor & Sports | £98,106 | £126,226 | **£97,315** |
| Health & Beauty | £32,391 | **£20,917** | £22,384 |
| Home & Kitchen | **£49,064** | £63,106 | £72,369 |
| Office Supplies | **£83,309** | £86,281 | £88,690 |
| Electronics | £50,701 | £56,754 | **£39,185** |
| **Total** | **£313,571** | £353,285 | £319,942 |

*(Bold = best result in that row)*

**Takeaways:**

- **No single model won on accuracy across every category.** Simple Exponential Smoothing had the best average MAPE, but Holt-Winters won outright in Office Supplies and Electronics, and Moving Average stayed competitive despite being the simplest method.
- **The most accurate model wasn't the cheapest.** Simple Exponential Smoothing's better average MAPE didn't translate into lower cost summed across all five categories, it was actually the most expensive option, about £40,000 more than Moving Average. MAPE treats overstock and understock errors as equally "wrong," but financially they aren't: a stockout costs lost margin, typically far more expensive than the modest monthly holding cost of excess stock.
- **Model performance is category-specific.** Only 3 of 5 categories had the same model win on both MAPE and cost, an argument against a one-size-fits-all model choice.
- **Holt-Winters struggled where history was thin.** Holt-Winters performed poorly in some categories, particularly Home & Kitchen, where it produced the highest MAPE (24.81%). With only 23 months of history, the dataset contains fewer than two complete annual seasonal cycles, making seasonal patterns harder to estimate reliably. These results should therefore be reassessed as more historical data becomes available.

## Recommendation

Model selection should be driven by estimated financial cost, not accuracy metrics alone. A practical approach: run all three models per category, translate each into estimated cost using category-specific margin and holding-cost figures, and select per category rather than committing to one method business-wide. Where a single default is needed for simplicity, Where a single default is required for simplicity, the 3-month Moving Average provides the lowest estimated aggregate cost in this analysis, although it has a higher average MAPE than Simple Exponential Smoothing.

## Limitations

- The 20% annual holding-cost rate is an assumed benchmark rather than a company-specific figure. Because holding costs vary substantially by industry, product characteristics, and capital costs, the cost results should be interpreted as scenario estimates rather than actual business costs.
- 23 months of history is right at the edge of what's needed to reliably estimate seasonality, the Holt-Winters results should be revisited once 2+ years of clean data are available.
- Two categories (Outdoor & Sports, Home & Kitchen) showed a sharp demand spike in Aug–Sep 2024 that every model under-forecast. With only one prior year for comparison, it's unclear whether this is a recurring seasonal pattern or a one-off event.
