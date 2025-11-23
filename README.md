# Risk-Adjusted-Technical-Pricing-Framework-for-Auto-Insurance
This project simulates a real-world Pricing Adequacy Review for personal auto insurance portfolio.
The objective is to determine whether current premiums sufficiently cover the expected cost of claims, and to design a data-driven pricing strategy informed by risk segmentation.

# 📊 Dataset

The dataset represents an auto insurance portfolio with:

1. Policyholder demographics (Age, Marital Status, Senior Status, Loyalty)
2. Coverage and pricing attributes (Policy Type, Premium Amount, Discounts)
3. Risk indicators (Risk Tier, Credit Score, Engagement Score, Region)
4. Claims data (Frequency, Severity, Total Paid)

Total records: 10,000 policies

# 🔧 Phase 1 — Data Preparation

Key transformations:

1. Standardized categorical fields (e.g., Is_Senior, Marital_Status, Policy_Type)
2. Ordinal encoding for loyalty & experience attributes
3. One-hot encoding for nominal attributes (e.g., Source_of_Lead, Region)
4. Created exposure-normalised claim indicators:

This produced a model-ready dataset with well-defined risk features and claim outcomes.

# 📈 Phase 2 — Frequency Modelling

A Poisson GLM was fitted to estimate claim frequency using risk drivers:

𝐸[Claims] = exp(𝛽0 + 𝛽1𝑋1 + ⋯ + 𝛽𝑘𝑋𝑘)

Key predictors retained after model selection:

1. Risk Tier (strongest driver)
2. Region
3. Loyalty Band
4. Prior Insurance Tenure
5. Policy Type
6. Multi-Policy & Safe Driving Discounts

Model dispersion ≈ 0.84, indicating no overdispersion → Poisson GLM appropriate.

Calibration Results:
Predicted frequency aligned well with observed frequency across deciles.

# 💰 Phase 3 — Severity Modelling

A Gamma GLM was fitted conditional on claim occurrence:

Severity placeholders mapped as:

Claim Severity	Cost Assigned
Low	            500
Medium	        1,500
High	          15,000

Severity strongly driven by:

1. Risk Tier
2. Policy Type
3. Credit Score

Model demonstrated excellent monotonic calibration across risk deciles.

# 🧮 Phase 4 — Pure Premium (Expected Loss Cost)

Expected Loss Cost = Frequency × Severity

Then compared to the actual Premium_Amount:
Loss Ratio = Expected Loss Cost / Written Premium

​
# 📌 Key Findings

#### Segment             | 	      Finding	                 |         Interpretation
1. High Risk Tier	       |     Loss Ratio ≈ 6.1	           |     Severely underpriced
  
2. Medium Risk Tier	     |     LR ≈ 0.51	                 |       Adequately priced

3. Low Risk Tier	       |     LR ≈ 0.08	                 |       Significantly overpriced

4. Full Coverage Policies|	   Better performance than liability-only |   Indicates liability underpricing

5. Region                |    Rural ≈ Suburban ≈ Urban performance   | Region is not a major pricing lever
                             
                        		

# 🎯 Pricing Recommendations
#### Action	                 |         Rationale	                    |        Expected Effect
1. +20–45% premium uplift for High-Risk Tier  |   Align price with expected loss cost  |       Improves portfolio adequacy


2. -8–15% reduction for Low-Risk Tier 	|    Avoid adverse selection &  improve competitiveness	          |      Retention improvement
               

3. +3–7% uplift for Liability-Only policies	    |    Address under-costing of minimal coverage	|  Reduces cross-subsidy distortion


# Resulting Portfolio Impact:

Portfolio Loss Ratio improves from ~0.86 → ~0.70 post-adjustment.
