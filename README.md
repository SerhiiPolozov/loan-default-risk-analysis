# Loan Default Risk Analysis

Exploratory data analysis in Python of what drives loan defaults, ending with practical approval recommendations (credit score tiers and a DTI review trigger).

**Tools:** Python, pandas, NumPy, seaborn, matplotlib, Jupyter Notebook

## Business Questions

1. What is the overall default rate, and how does it vary by credit score range?
2. Is there a relationship between debt-to-income (DTI) ratio and default? What cutoff would you recommend?
3. Which loan purposes have the highest default rates, and does loan amount differ between defaulted and non-defaulted loans?
4. How do employment status and years employed affect default risk?

## Dataset

Two CSV files in the `data/` folder, joined on `borrower_id`:

| File | Rows | Key columns |
|---|---|---|
| `borrower_profiles.csv` | 500 borrowers | age, state, education_level, employment_status, years_employed, annual_income, credit_score, home_ownership, dependents, existing_monthly_debt |
| `loan_applications.csv` | 601 loans | loan_purpose, loan_amount, term_months, interest_rate, monthly_payment, dti_ratio, loan_status, days_delinquent, defaulted |

Some borrowers have two loans. The original source of the dataset is unknown, so I treated it as a practice dataset.

## Approach

- Checked both tables for missing values and duplicate loan IDs, then merged them (601 rows, no nulls).
- Grouped credit scores into five ranges and grouped DTI and interest rate into quintiles.
- Compared default rates across groups with tables and point/bar plots (error bars are 95% confidence intervals).
- Tested candidate DTI cutoffs (40 to 60) by comparing default rates above and below each, and how many loans each would flag.
- Used correlations with `defaulted` to rank factors. `days_delinquent` was excluded because it is an outcome of default, not a cause.

## Key Findings

- **Overall default rate: 24%.**
- **Credit score is the strongest factor.** Default rate falls from 49% (scores 520-599) to about 12% (750+). Borrowers below 600 are the highest risk; 700+ are the safest.
- **DTI risk rises gradually, with no sharp breakpoint.** About 13% default below a DTI of 40 vs about 32% above it. For any cutoff from 40 to 60, the gap is 16 to 20 percentage points. A cutoff of 50 would flag 270 of 601 loans (45%), so I recommend using it as a trigger for **manual review** rather than automatic rejection.
- **Interest rate** correlates with default about as much as DTI, but lenders likely assign higher rates to riskier borrowers, so it reflects risk rather than causing it.
- **Employment:** employment status shows no meaningful difference, but borrowers with less than 2 years of employment default more often (35% vs 23%).
- **Loan purpose:** wedding (32%), home improvement (29%), and auto (27%) loans look slightly riskier, but samples per purpose are small and confidence intervals overlap, so this is not conclusive.
- **Loan amount:** nearly identical for defaulted and non-defaulted loans (about $22.6K vs $22.0K), so it is not a useful approval criterion.

## Limitations

- Small sample (601 loans), so some group-level differences are uncertain.
- Results show associations, not causation, and I did not run formal statistical tests.
- Dataset source is unknown, so findings may not generalize to real lending data.

## How to Run

1. Clone this repository.
2. Install the requirements: `pip install pandas numpy seaborn matplotlib jupyter`
3. Open `Loan_Default_Risk_Analysis.ipynb` in Jupyter **from the project folder**, so the relative path `data/...` works, and run all cells.

## Repository Structure

```
.
├── Loan_Default_Risk_Analysis.ipynb
├── README.md
└── data/
    ├── borrower_profiles.csv
    └── loan_applications.csv
```
