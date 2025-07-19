# DeFi-Credit-Scoring-System-Zeru-
DeFi Credit Scoring System - The goal is to develop a robust machine learning model that assigns a credit score between 0 and 1000 to each wallet, based solely on historical transaction behavior. Higher scores indicate reliable and responsible usage; lower scores reflect risky, bot-like, or exploitative behavior.

This system calculates comprehensive credit scores (0-1000) for DeFi wallet addresses based on their transaction history and behavioral patterns. The scoring methodology combines multiple risk factors to provide a holistic creditworthiness assessment.

Score Components & Weights
Component	Weight	Description
Reliability Score	25%	Payment history, repayment ratios, and transaction consistency
Utilization Score	20%	Healthy debt-to-collateral ratios (optimal <80%)
Longevity Score	15%	Account age and sustained activity patterns
Liquidation Risk Score	15%	Historical liquidations and risk indicators (inverted)
Diversity Score	10%	Asset variety and action type diversification
Bot Behavior Score	10%	Anti-bot detection based on transaction patterns (inverted)
Volume Consistency	5%	Transaction size variability (moderate variation preferred)
Scoring Logic
Reliability Score (0-1)
Repayment Ratio (40%): repay_volume / borrow_volume

Frequency Consistency (30%): 1 - (daily_txn_std / daily_txn_mean)

Transaction Regularity (30%): 1 - (avg_days_between_txns / 30)

Utilization Score (0-1)
Healthy utilization = max(0, 1 - max(0, utilization_ratio - 0.8) * 5)

Penalizes over-leveraged positions (>80% utilization)

Longevity Score (0-1)
Account Age (60%): min(1, account_age_days / 365)

Activity Frequency (40%): min(1, transactions_per_day / 5)

Liquidation Risk Score (0-1, Inverted)
Base risk: 1 - liquidation_count * 0.2

Recent activity weighting for temporal relevance

Bot Detection Score (0-1, Inverted)
Round Number Bias: Frequency of round-number transactions

Time Regularity: Standard deviation of transaction intervals

Combined bot likelihood: round_ratio * 0.4 + regularity * 0.6

Score Categories
Range	Category	Risk Level
800-1000	Excellent	Low Risk
700-799	Good	Moderate Risk
600-699	Fair	Elevated Risk
500-599	Poor	High Risk
0-499	Very Poor	Very High Risk
Key Features
Data Processing
Handles nested JSON structures with actionData extraction

Automatic decimal conversion for token amounts (assumes 6-decimal tokens like USDC)

Robust error handling and data validation

Behavioral Analysis
Time-based pattern recognition (hourly/daily activity)

Asset and action diversification scoring

Liquidation event tracking and risk assessment

Anti-Manipulation
Bot detection through transaction pattern analysis

Round-number bias detection

Temporal regularity assessment

Usage
python
scorer = DeFiCreditScorer()
results_df, report = scorer.score_wallets('transactions.json')
Extensibility
Adding New Components
Define weight in feature_weights dictionary

Create feature calculation method in _calculate_wallet_features()

Add component scoring logic in calculate_component_scores()

Customizing Weights
Modify the feature_weights dictionary to adjust component importance based on specific use cases or risk appetites.

Data Source Flexibility
The load_data() method includes field mapping to accommodate different JSON structures. Update field_mapping dictionary for new data formats.

Validation & Transparency
Score Validation
All component scores normalized to 0-1 range

Final scores clipped to 0-1000 range

Comprehensive logging of data processing steps

Interpretability
Individual component scores preserved in output

Detailed transaction statistics for manual review

Clear categorization for risk assessment

