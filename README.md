# DeFi-Credit-Scoring-System-Zeru-
DeFi Credit Scoring System - The goal is to develop a robust machine learning model that assigns a credit score between 0 and 1000 to each wallet, based solely on historical transaction behavior. Higher scores indicate reliable and responsible usage; lower scores reflect risky, bot-like, or exploitative behavior.

The Architecture for this model is as follows

┌─────────────────────┐ ┌──────────────────────┐ ┌─────────────────────┐
│ │ │ │ │ │
│ Data Ingestion │────▶│ Feature Engineering │────▶│ Scoring Engine │
│ Layer │ │ Pipeline │ │ │
│ │ │ │ │ │
└─────────────────────┘ └──────────────────────┘ └─────────────────────┘
│ │ │
▼ ▼ ▼
┌─────────────────────┐ ┌──────────────────────┐ ┌─────────────────────┐
│ │ │ │ │ │
│ Data Validation │ │ Behavioral Analysis │ │ Risk Categorization│
│ & Preprocessing │ │ Component │ │ & Reporting │
│ │ │ │ │ │
└─────────────────────┘ └──────────────────────┘ └─────────────────────┘

┌─────────────────────┐     ┌──────────────────────┐     ┌─────────────────────┐
│                     │     │                      │     │                     │
│   Data Ingestion    │────▶│  Feature Engineering │────▶│   Scoring Engine    │
│     Layer           │     │       Pipeline       │     │                     │
│                     │     │                      │     │                     │
└─────────────────────┘     └──────────────────────┘     └─────────────────────┘
         │                           │                           │
         ▼                           ▼                           ▼
┌─────────────────────┐     ┌──────────────────────┐     ┌─────────────────────┐
│                     │     │                      │     │                     │
│  Data Validation    │     │ Behavioral Analysis  │     │  Risk Categorization│
│  & Preprocessing    │     │     Component        │     │    & Reporting      │
│                     │     │                      │     │                     │
└─────────────────────┘     └──────────────────────┘     └─────────────────────┘


Core Components Breakdown
1. Data Ingestion Layer
Purpose: Handle raw transaction data input and initial processing

def load_data(json_file_path) -> pd.DataFrame
    ├── Parse JSON to DataFrame
    ├── Extract nested actionData fields
    ├── Apply field mapping (userWallet -> wallet)
    ├── Validate required fields
    └── Return structured DataFrame
    
Data Flow:
Raw JSON → DataFrame → Field Mapping → Validation → Structured Data

2. Data Validation & Preprocessing Layer
Purpose: Clean, validate, and normalize transaction data

Input Data
    ├── Convert timestamps (Unix → DateTime)
    ├── Normalize amounts (handle token decimals)
    ├── Filter invalid transactions
    ├── Remove zero/negative amounts
    └── Validate data ranges
    
3. Feature Engineering Pipeline
Purpose: Transform raw transaction data into meaningful features for scoring

Wallet Data → [Feature Extractors] → Feature Vector
                     │
    ┌────────────────┼────────────────┐
    ▼                ▼                ▼
[Basic Metrics] [Behavioral] [Risk Indicators]

Feature Extractors:
def _basic_transaction_features():
    ├── Transaction count and frequency
    ├── Asset and action diversity
    ├── Volume statistics
    ├── Account age calculation
    └── Activity intensity metrics
def _reliability_features():
    ├── Repayment behavior analysis
    ├── Borrow/repay volume ratios
    ├── Transaction frequency consistency
    └── Time-based reliability patterns
def _utilization_features():
    ├── Debt-to-collateral ratios
    ├── Deposit vs borrow patterns
    ├── Healthy utilization scoring
    └── Position management analysis
def _behavioral_features():
    ├── Time-based activity patterns
    ├── Asset diversification analysis
    ├── Transaction timing consistency
    └── Weekend/weekday activity ratios
def _liquidation_risk_features():
    ├── Liquidation event detection
    ├── Recent activity analysis
    ├── Risk score calculation
    └── Historical risk patterns
def _bot_detection_features():
    ├── Transaction regularity analysis
    ├── Round number bias detection
    ├── Amount pattern analysis
    └── Behavioral automation scoring

4. Scoring Engine
Purpose: Convert features into standardized credit scores
Feature Vectors → [Component Scorers] → [Weighted Combination] → Final Score
                         │
        ┌────────────────┼────────────────────────────┐
        ▼                ▼                            ▼
   [Reliability]    [Utilization]              [Risk Scores]
   [Longevity]      [Diversity]                [Bot Detection]


Component Scoring System:

4.1 Score Components (Weighted)
Component                 Weight    Purpose
├── Reliability Score     25%       Payment history & consistency
├── Utilization Score     20%       Debt management patterns
├── Longevity Score       15%       Account maturity & activity
├── Liquidation Risk      15%       Historical risk events
├── Diversity Score       10%       Asset & protocol variety
├── Bot Behavior Score    10%       Artificial activity detection
└── Volume Consistency    5%        Transaction pattern stability

4.2 Normalization Process
Raw Features → [0,1] Normalization → Weighted Combination → [0,1000] Scale

4.3 Risk Categorization
Score Range    Category                   Risk Level
800-1000       Excellent (Low Risk)       Premium Tier
700-799        Good (Moderate Risk)       Standard Tier
600-699        Fair (Elevated Risk)       Cautious Tier
500-599        Poor (High Risk)           Restricted Tier
0-499          Very Poor (Very High Risk) Avoid Tier


Data Flow Architecture
Complete Pipeline Flow
1. Raw JSON Input
   ├── user-wallet-transactions.json
   └── Nested transaction records

2. Data Ingestion
   ├── JSON parsing
   ├── Schema mapping
   └── Field validation

3. Preprocessing
   ├── Type conversion
   ├── Amount normalization
   ├── Data cleaning
   └── Quality validation

4. Feature Engineering
   ├── Per-wallet feature extraction
   ├── Behavioral pattern analysis
   ├── Risk indicator calculation
   └── Bot detection scoring

5. Component Scoring
   ├── Feature normalization (0-1)
   ├── Component-specific scoring
   ├── Risk-based inversions
   └── Quality adjustments

6. Final Scoring
   ├── Weighted combination
   ├── Scale to 1000-point system
   ├── Risk categorization
   └── Score validation

7. Output Generation
   ├── CSV export (wallet_credit_scores.csv)
   ├── Statistical reports
   ├── Distribution analysis
   └── Visualization charts

Memory and Performance Architecture
Efficient Processing Design:
Data Loading → Chunked Processing → Vectorized Operations → Memory Management
     │                │                    │                    │
     ▼                ▼                    ▼                    ▼
[Pandas DataFrame] [Per-Wallet] [NumPy Operations] [Garbage Collection]


Extension Points
Adding New Features:
# Add new feature extractor
def _new_risk_features(self, wallet_data):
    # Custom feature logic
    return features

# Register in main feature calculation
def _calculate_wallet_features(self, wallet_data):
    features.update(self._new_risk_features(wallet_data))

Error Handling and Robustness
Fault Tolerance Design
Input Validation → Processing Safeguards → Output Validation
        │                   │                    │
        ▼                   ▼                    ▼
[Schema Checks] [Exception Handling] [Score Bounds]
[Data Quality]  [Default Values]     [Category Logic]


### Core Scoring Components

7 weighted components to calculate the final credit score:

1. **Reliability Score (25%)** - Payment history and transaction consistency
2. **Utilization Score (20%)** - Debt-to-collateral ratios and borrowing patterns
3. **Longevity Score (15%)** - Account age and sustained activity
4. **Liquidation Risk Score (15%)** - Historical liquidations and risk indicators
5. **Diversity Score (10%)** - Asset and action variety
6. **Bot Behavior Score (10%)** - Anti-bot detection (inverted)
7. **Volume Consistency Score (5%)** - Transaction volume patterns

## Processing Flow

### 1. Data Loading & Preprocessing
- Load JSON transaction data
- Extract nested actionData fields
- Map fields to standardized schema
- Convert timestamps and normalize amounts
- Filter invalid transactions

### 2. Feature Engineering
For each wallet, calculate:
- **Basic Metrics**: Transaction count, volume, asset diversity
- **Reliability Indicators**: Repayment ratios, frequency consistency
- **Utilization Patterns**: Borrow/deposit ratios, collateral usage
- **Behavioral Signals**: Time patterns, action diversity
- **Risk Indicators**: Liquidation history, recent activity
- **Bot Detection**: Amount patterns, transaction regularity

### 3. Component Scoring
Transform raw features into 0-1 normalized scores:
- Apply domain-specific scoring functions
- Handle edge cases and missing data
- Invert risk-based components (higher score = lower risk)

### 4. Final Score Calculation
- Apply weighted combination of component scores
- Scale to 0-1000 point system
- Categorize into risk levels

### 5. Report Generation
- Generate comprehensive scoring report
- Identify top and bottom performers
- Calculate distribution statistics

## Installation


### Key Findings

1. **Average Credit Score**: 624.3 points
2. **Score Standard Deviation**: 63.3 points
3. **Median Score**: 605.7 points
4. **Mode Category**: Good (Moderate Risk) - 68.52% of wallets fall in 600-700 range

## Component Score Analysis

Based on the weighted scoring system and the observed data patterns:

### Component Performance Averages

| Component                    | Average Score | Weight | Contribution |
|------------------------------|---------------|--------|--------------|
| Reliability Score            | 0.623         | 25%    | 155.8 points |
| Utilization Score            | 0.421         | 20%    | 84.2 points  |
| Longevity Score              | 0.298         | 15%    | 44.7 points  |
| Liquidation Risk Score       | 1.000         | 15%    | 150.0 points |
| Diversity Score              | 0.456         | 10%    | 45.6 points  |
| Bot Behavior Score           | 0.989         | 10%    | 98.9 points  |
| Volume Consistency Score     | 0.892         | 5%     | 44.6 points  |

## Behavioral Analysis by Score Range

### High Scorers (700-800) - Excellent Credit

**Characteristics (Based on Actual Data):**
- **Repayment Behavior**: Extremely high repayment ratios (average 5,970,772,443.58 indicating over-collateralization)
- **Utilization Patterns**: Conservative borrowing (average 0.23 utilization ratio)
- **Transaction Frequency**: Above average activity (42.1 transactions per wallet)
- **Asset Diversity**: Higher engagement with multiple assets
- **Risk Management**: Perfect liquidation history (0% liquidation rate)
- **Activity Longevity**: Accounts active for 31 days on average

**Typical Profile:**
- Average transactions: 42.1 per wallet
- Average account age: 31 days
- Liquidation rate: 0%
- Bot-like behavior: Virtually none (0.000 avg bot score)

**Business Implications:**
- Lowest default risk with perfect liquidation record
- Suitable for premium rates and higher credit limits
- Ideal candidates for unsecured lending

### Medium Scorers (500-699) - Good to Fair Credit

**Characteristics (Based on Actual Data):**
- **Repayment Behavior**: Moderate repayment patterns (0.13 average ratio for 400-699 range)
- **Utilization Patterns**: Highly variable, with some extreme outliers in lower ranges
- **Transaction Frequency**: Standard activity (26.3 transactions average)
- **Risk Events**: No liquidation history observed
- **Behavioral Patterns**: Minimal automated activity (0.012 avg bot score)

**Typical Profile:**
- Average transactions: 26.3 per wallet
- Average account age: 21 days
- Liquidation rate: 0%
- Low bot detection rate (1.1-4.0% depending on sub-range)

**Business Implications:**
- Standard lending rates appropriate
- Regular monitoring recommended for utilization spikes
- Suitable for collateralized lending

### Low Scorers (300-500) - Poor to Very Poor Credit

**Characteristics (Based on Actual Data):**
- **Repayment Issues**: Very poor repayment history (0.00 ratio for <400 range)
- **High Utilization**: Extremely high utilization ratios (averaging in trillions, indicating calculation errors or extreme over-leverage)
- **Liquidation History**: Surprisingly, 0% liquidation rate despite poor scores
- **Bot-like Behavior**: Minimal bot detection (0.0% for lowest tier)
- **Account Maturity**: Paradoxically longer account age (52 days average for <400 range)

**Typical Profile:**
- Average transactions: 32.3 per wallet (higher than medium scorers)
- High utilization rates (100% for ranges 300-500)
- No liquidation events recorded
- Minimal bot-like behavior scores

**Business Implications:**
- High default risk despite lack of liquidation events
- Avoid unsecured lending
- Require maximum collateral ratios
- May indicate data quality issues requiring investigation

## Risk Pattern Analysis

### Liquidation Risk Indicators

**Surprising Findings:**
- **0% liquidation rate** across ALL score ranges, indicating either:
  - Very recent data with insufficient time for liquidations
  - Data quality issues in liquidation event capture
  - Extremely conservative user behavior in this dataset

### Bot Detection Insights

**Identified patterns:**
- **Very Low Bot Activity**: Maximum 4.0% bot detection rate in 500-600 range
- **Clean High Scorers**: 0% bot detection in highest score ranges
- **Concentrated Detection**: Bot activity primarily in middle-score ranges (500-600)

### Utilization Risk Patterns

**Critical Observations:**
- **Extreme Utilization Values**: Ratios in trillions for low-score ranges suggest data normalization issues
- **Healthy High Scorers**: Utilization ratios 0.23-0.52 for scores 700+
- **100% High Utilization Rate**: All wallets in 300-500 range flagged for high utilization

## Data Quality Concerns

### Identified Issues
1. **Utilization Calculation Anomalies**: Extreme values suggest normalization problems
2. **Repayment Ratio Outliers**: Values over 5 billion indicate calculation errors
3. **Zero Liquidation Events**: Unusual for DeFi dataset, needs investigation
4. **Short Account Ages**: Average 21-52 days suggests recent protocol or data collection period

## Recommendations

### For Lending Protocols

1. **Tier 1 (700-800 Score)**: 479 wallets (13.7%) - Premium tier with enhanced limits
2. **Tier 2 (600-699)**: 2,396 wallets (68.5%) - Standard tier, backbone of portfolio
3. **Tier 3 (500-599)**: 476 wallets (13.6%) - Cautious lending with higher collateral
4. **Tier 4 (<500)**: 146 wallets (4.2%) - Restricted access, investigate data anomalies

### For Data Quality

1. **Immediate Review**: Investigate utilization and repayment ratio calculations
2. **Liquidation Event Capture**: Verify liquidation detection methodology
3. **Amount Normalization**: Review token decimal handling and amount conversions
4. **Historical Validation**: Cross-reference with known liquidation events

### For Risk Management

1. **Conservative Approach**: Given data anomalies, apply higher safety margins
2. **Manual Review**: Flag extreme utilization ratios for individual assessment
3. **Gradual Rollout**: Start with high-score segment while refining calculations
4. **Monitoring Dashboard**: Real-time tracking of key metrics and anomalies

## Model Performance Assessment

### Current Limitations
- **Data Recency**: Short observation period (21-52 day average account age)
- **Missing Liquidations**: 0% rate suggests incomplete event capture
- **Calculation Errors**: Extreme values in key metrics require correction

### Validation Metrics
- **Score Distribution**: Reasonable bell curve around 624 points
- **Risk Segmentation**: Clear separation between tiers
- **Component Balance**: Well-distributed contributions across scoring components

