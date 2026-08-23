# Customer Churn Analysis

I wanted to work through a full churn analysis the way it'd actually happen at
a company — starting from messy raw data, cleaning it, digging into why
customers leave, building a model, and backing it up with proper statistical
testing instead of just eyeballing charts.

The dataset is IBM's Telco Customer Churn set from Kaggle (7,043 customers,
50 columns — demographics, services, billing, satisfaction score, and the
stated reason for leaving if they churned).

## What I was trying to answer

Who's likely to leave, why, and how much money is actually at stake.

## How it's organized

The notebook (`Customer_Churn_Analysis.ipynb`) has the whole thing end to end
with explanations along the way — that's the best place to start if you just
want to read through it.

The `.py` files break the same work into separate steps, roughly in the order
I did them:

- `1- Cleaning.ipynb` – handling missing values, figuring out which ones were
  actually missing vs. just meant "customer doesn't have this service"
- `2-Exploratory data analysis.ipynb` – digging into the raw patterns before building anything
- `3- Model.ipynb` – a logistic regression model, dropping anything that would
  leak the answer (like the churn score IBM already calculated)
- `4- Charts.ipynb` – the plots
- `5- Businessb questions.ipynb` – a few specific questions I wanted numbers for,
  like which customers are worth calling before they leave
- `6- Export for bi.ipynb` – cleaning up a version of the data for Power BI
- `statistical_analysis.R` – chi-square and t-tests to check that the patterns
  I found were actually statistically significant and not just noise

## What I found

Month-to-month customers churn at a much higher rate than people on longer
contracts — 51.7% vs. 2.6% for two-year contracts, which is a pretty stark
difference. Most of the churn happens early too; 60% of it is within the
first year.

Satisfaction score was the strongest single predictor I found, followed by
contract type and tenure. When people did leave, the most common reasons were
competitors offering better deals or devices, and the second most common was
just bad experiences with support.

The part I found most useful from a business angle: there are 320 customers
right now who haven't churned yet but have both a high lifetime value and a
high churn risk score. That's a pretty actionable list if you're trying to
decide who to call first.

Overall about $1.69M in revenue is tied to customers who left specifically
because of competitors, which on its own says something about where the
priority should be.

## The model

Logistic regression, mostly because it's interpretable — I wanted to be able
to say which factors mattered and by how much, not just get a black-box
prediction. It ended up performing well (ROC-AUC of 0.99, 96% accuracy), but
the interpretability was the actual point.

## Why R too

Python covers the modeling and EDA fine, but I wanted formal hypothesis tests
with real p-values to back up what looked like obvious patterns in the charts.
R's `glm()` gives you that directly without extra setup, so I used it for the
chi-square tests, t-tests, and a second logistic regression built for
inference rather than prediction.

## If I kept going

Probably would look at fiber optic customers specifically — that segment has
the worst combination of contract type and churn rate — and dig into whether
it's a pricing issue or a service quality issue. Also would want to actually
build the Power BI dashboard properly instead of just exporting a clean CSV
for it.
