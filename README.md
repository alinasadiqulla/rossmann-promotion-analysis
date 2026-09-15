# Rossmann Promotion Analysis
 
## Overview
This project investigates whether Rossmann's in-store promotions drive genuinely additional sales, or whether they mostly shift purchases that would have happened anyway into the promotion window. Using the Rossmann Store Sales dataset (~1 million store-day records), the analysis combines descriptive comparisons with a corrected day-matched methodology to test for a post-promotion sales dip.
 
## Research Question
Are sales patterns around promotions consistent with additional sales overall, or with purchases shifting into promotion periods?
 
## Data
- `train.csv` — daily sales, customer counts, and promo/holiday flags per store (not included in this repo; see [Kaggle's Rossmann Store Sales competition](https://www.kaggle.com/c/rossmann-store-sales) for the original data)
- `store.csv` — store-level metadata (type, competition distance, etc.)
## Methodology
1. **Data audit** — filtered out closed-store days (`Open == 0`), which would otherwise show $0 sales unrelated to promotions. Confirmed Rossmann does not run promotions on Saturdays or Sundays, limiting the promo-effect analysis to weekdays.
2. **Descriptive comparison** — compared average sales on promo vs. non-promo days, broken out by day of week, to check whether the promo effect holds independent of natural weekday demand patterns.
3. **Post-promotion shift check** — tested whether sales dip on the day immediately following a promotion, which would indicate demand was pulled forward rather than newly created. This required:
   - Sorting data chronologically per store
   - Flagging the actual last day of each promotion
   - Identifying the true next *calendar* day (accounting for store closures, which can skip a day)
   - Matching post-promotion days to a correctly aligned comparison group (same weekday, no overlap between groups)
## Key Finding
After correcting two methodological issues (a day-of-week mismatch, and an overlapping-baseline issue), post-promotion Saturdays showed a small, likely non-meaningful difference from normal Saturdays (5,806 vs. 5,776, roughly +0.5%), across large samples (71,031 vs. 66,082 observations). This suggests little evidence of a next-day sales dip following Friday promotions, though this has not been formally tested for statistical significance, and same-store observations across weeks are not fully independent.
 
## Limitations
- Analysis of demand shifting is currently limited to the Saturday-following-Friday-promo window; Monday through Thursday transitions have not yet been checked with the same corrected methodology
- No formal significance testing has been applied to the post-promotion comparison
- Regression analysis controlling for store-level differences and calendar effects (holidays, seasonality) is a planned next step, not yet complete
- `Promo2` (Rossmann's separate, longer-running promotion type) is out of scope for this analysis
## Next Steps
- Extend the corrected shift-check methodology to other days of the week
- Build a regression model (`Sales ~ Promo + DayOfWeek + StateHoliday + SchoolHoliday`) to test whether the promo effect holds after controlling for calendar effects
- Propose a follow-up randomized experiment (e.g., a promo holdout test) to establish causality
## Tools
Python, pandas, Matplotlib
 
## Author
Alina — NYU Gallatin, Data Science & Marketing Strategy
 
