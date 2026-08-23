# Superstore Sales — What Actually Drives Profit?

A regression analysis of the Superstore retail dataset, exploring whether discounting drives profit down — and what the data reveals once product category and order size are accounted for.

## Project Summary

Started with a simple question: **does discount rate drive profit down?**

A single-variable regression confirmed the direction (more discount → less profit) but explained only **7% of profit's variation** (R² = 0.07). Directionally right, practically incomplete.

Adding Sales, Quantity, and Product Sub-Category (16 categories, one-hot encoded) lifted R² to **25%** — a 4x improvement. The bigger driver wasn't discount alone — it was *what* was being sold.

Earlier exploratory analysis had flagged **"Tables"** as the top loss-making sub-category. Controlling for discount level told a more precise story: Tables is discounted **~67% more heavily than average** (26% vs. 16%), which inflated its apparent losses in raw numbers. Once that's accounted for, the real structural drag is **Machines** — and **Copiers** turns out to be a strong, previously unrecognized profit driver.

To check the model wasn't just fitting noise, I ran **LassoCV** (cross-validated L1 regularization). It kept 16 of 19 features — only three low-volume categories (Envelopes, Labels, Fasteners) were dropped as non-contributory. Once features were standardized, **Discount emerged as the single strongest driver of profit loss** in the entire model.

Even at its best, the model explains ~25% of profit variation — a meaningful signal, not a complete picture. Region, Customer Segment, and shipping cost are logical next variables to test.

## Key Visuals

| Naive Model (R²=0.07) | Model V2 Coefficients | Predicted vs. Actual |
|---|---|---|
| Discount barely tracks Profit | Copiers ↑, Machines/Discount/Tables ↓ | Model captures signal, underestimates extremes |

*(see notebook for full-resolution charts)*

## Methodology

1. **EDA** — explored profit by Region, Category, and Sub-Category; identified initial loss patterns
2. **Model V1** — simple linear regression, `Profit ~ Discount`
3. **Model V2** — multivariable regression with one-hot encoded Sub-Category, Sales, and Quantity
4. **Regularization** — Lasso (manual alpha exploration) and LassoCV (cross-validated, properly scaled) to test feature robustness
5. **Validation** — sanity-checked dropped features against raw profit distributions
6. **Visualization** — coefficient impact chart, predicted-vs-actual performance chart

## Tools

Python · pandas · numpy · scikit-learn · seaborn · matplotlib · Google Colab

## Files

- `Sales_EDA.ipynb` — full analysis notebook (EDA → modeling → regularization → visualization)
- `Sample - Superstore.csv` - raw dataset for these models
- `README.md` — this file

## Limitations & Next Steps

- Model explains ~25% of profit variation — most of the story is still untold
- Candidate next features: Region, Customer Segment, Ship Mode, shipping cost
- Next model type to explore: Decision Trees / Random Forests (non-linear relationships, interaction effects)

---
*Part of an ongoing data science portfolio — built while preparing for the MSc Data Science & AI Strategy program at emlyon Business School.*
