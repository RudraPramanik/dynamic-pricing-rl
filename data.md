cell2:
GROUND TRUTH PARAMETERS
───────────────────────────────────────────────────────
Product     Base Demand  Own Elasticity  Ref Price
───────────────────────────────────────────────────────
item_1               50           -1.50      10.00
item_2               40           -1.80      12.00
item_3               60           -1.20       8.00
───────────────────────────────────────────────────────

Total observations : 1,460 days (4.0 years)
Training samples   : 1,340 days
Holdout samples    : 120 days
cell3::
Dataset generated: 1,460 rows × 17 columns
   Date range : 2021-01-01 → 2024-12-30
   Products   : ['item_1', 'item_2', 'item_3']

Dataset sample:
            item_1_price  item_1_demand  item_2_price  item_2_demand
date                                                                
2021-01-01          10.0          39.99          12.0          31.52
2021-01-02          10.0          41.53          12.0          30.96
2021-01-03          10.0          44.76          12.0          34.56
2021-01-04          10.0          44.35          12.0          29.16
2021-01-05          10.0          37.41          12.0          30.16
2021-01-06          10.0          37.50          12.0          34.30
celll5-statical analysis:
Dataset Summary Statistics
===========================================================================
         Demand Mean  Demand Std  Demand Min  Demand Max  Price Mean  Price Std  Price Min  Price Max  Corr(P,D)
Product                                                                                                         
item_1        108.92       46.81       24.86      265.65       10.41       2.56       6.04      15.19      -0.62
item_2         75.40       42.90       16.72      256.41       12.57       2.98       7.45      17.95      -0.73
item_3        155.60       70.87       32.86      337.65        8.39       2.61       4.14      13.92      -0.66


📊 Augmented Dickey-Fuller Stationarity Tests (Demand Series)
============================================================
  item_1: ADF=-3.8070, p=0.0028 → Stationary ✅
  item_2: ADF=-5.2810, p=0.0000 → Stationary ✅
  item_3: ADF=-5.1488, p=0.0000 → Stationary ✅
  cell6-feature engineering:
  Feature engineering complete for item_1
   Training set : 1,326 observations
   Test set     : 120 observations
   Feature count: 31 columns

Feature list (first 20):
  [ 1] item_1_price
  [ 2] item_1_demand
  [ 3] item_2_price
  [ 4] item_2_demand
  [ 5] item_3_price
  [ 6] item_3_demand
  [ 7] day_of_week
  [ 8] day_of_year
  [ 9] month
  [10] quarter
  [11] year
  [12] is_weekend
  [13] trend_index
  [14] day_sin
  [15] day_cos
  [16] week_sin
  [17] week_cos
  [18] log_item_1_price
  [19] log_item_2_price
  [20] log_item_3_price
  cell7::
  GLM ELASTICITY ESTIMATION RESULTS
======================================================================

  Product: ITEM_1
  Parameter                       Estimated       True       Bias  Rel.Bias%
  -----------------------------------------------------------------
  Own-price elasticity              -1.5186    -1.5000    -0.0186       1.24%
  Cross-price (item_2)               0.3789     0.3500          —          —
  Cross-price (item_3)              -0.1882    -0.1500          —          —
  AIC = 9160.4  |  Deviance = 598.4

  Product: ITEM_2
  Parameter                       Estimated       True       Bias  Rel.Bias%
  -----------------------------------------------------------------
  Own-price elasticity              -1.7701    -1.8000     0.0299       1.66%
  Cross-price (item_1)               0.2605     0.2500          —          —
  Cross-price (item_3)               0.0920     0.1000          —          —
  AIC = 8340.6  |  Deviance = 380.1

  Product: ITEM_3
  Parameter                       Estimated       True       Bias  Rel.Bias%
  -----------------------------------------------------------------
  Own-price elasticity              -1.2446    -1.2000    -0.0446       3.72%
  Cross-price (item_1)              -0.2212    -0.2000          —          —
  Cross-price (item_2)               0.1826     0.1500          —          —
  AIC = 9888.5  |  Deviance = 917.0
  cell9:
  GLM FORECASTING PERFORMANCE — item_1
=======================================================
Metric              Train   Test (holdout)
---------------------------------------------
  MAE              4.8006         5.8087 units
  RMSE             6.1651         7.0673 units
  MAPE             5.2225         5.2047%
  R²               0.9829         0.9652  

  Model AIC  : 9090.20
  Model BIC  : -8905.84
  Log-Lik    : -4530.10
  Deviance   : 520.15
  Pseudo-R²  : 0.9797
  cell11::
  Computing GLM-optimal prices for test set (this may take ~30s)...
✅ GLM-optimal prices computed for 120 test days
   Mean GLM-optimal price : $6.00
   Std GLM-optimal price  : $0.00
   True ref. price        : $10.00

   GLM-only cumulative revenue (test) : $196,624
   cell12::
   PricingEnv, NumpyDQN, and ReplayBuffer defined.
   State dim  : 5  [prev_price, prev_demand, day_sin, trend, glm_optimal_price]
   Action dim : 20  [price grid from $6.0 to $16.0]
   cell13::
   Training function defined.
   Episodes : 300  (3× original notebook)
   Batch size: 64  |  Buffer: 10,000  |  Target update: every 15 e
   cell15::
   TRAINING PURE DQN AGENT
=================================================================
  [Pure DQN] Ep   50/300  AvgReward=1,615,756.0  ε=0.669  Loss=nan
  [Pure DQN] Ep  100/300  AvgReward=1,746,291.0  ε=0.448  Loss=nan
  [Pure DQN] Ep  150/300  AvgReward=1,832,500.5  ε=0.300  Loss=nan
  [Pure DQN] Ep  200/300  AvgReward=1,891,872.7  ε=0.201  Loss=nan
  [Pure DQN] Ep  250/300  AvgReward=1,932,449.2  ε=0.134  Loss=nan
  [Pure DQN] Ep  300/300  AvgReward=1,957,158.5  ε=0.090  Loss=nan

=================================================================
TRAINING EG-DQN AGENT (Econometrically Guided)
=================================================================
  [EG-DQN] Ep   50/300  AvgReward=1,634,235.2  ε=0.669  Loss=nan
  [EG-DQN] Ep  100/300  AvgReward=1,773,023.9  ε=0.448  Loss=nan
  [EG-DQN] Ep  150/300  AvgReward=1,864,642.4  ε=0.300  Loss=nan
  [EG-DQN] Ep  200/300  AvgReward=1,927,774.3  ε=0.201  Loss=nan
  [EG-DQN] Ep  250/300  AvgReward=1,970,920.0  ε=0.134  Loss=nan
  [EG-DQN] Ep  300/300  AvgReward=1,997,166.3  ε=0.090  Loss=nan

✅ Both agents trained.
   Pure DQN  best episode reward : 1,976,094.7
   EG-DQN    best episode reward : 2,017,148.4
   cell-15::
   CONVERGENCE STATISTICS
-------------------------------------------------------
  Pure DQN    : First20 avg=$1,564,257  Last20 avg=$1,963,282  Improvement=+25.5%  FinalLoss=nan
  EG-DQN      : First20 avg=$1,579,526  Last20 avg=$2,003,676  Improvement=+26.9%  FinalLoss=na
  cell-16::
   REVENUE COMPARISON — 120-day Holdout Test Set (item_1)
===========================================================================
             Total Revenue  Daily Avg vs Fixed (%) Avg Price ($) Price Std ($)
Strategy                                                                      
Fixed-Price  $   175,650.6  $1,476.06       +0.00%         10.00          0.00
Rule-Based   $   138,864.0  $1,166.92      -20.94%         16.00          0.00
GLM-Only     $   226,763.9  $1,905.58      +29.10%          6.00          0.00
Pure DQN     $   226,763.9  $1,905.58      +29.10%          6.00          0.00
EG-DQN       $   226,763.9  $1,905.58      +29.10%          6.00          0.00
===========================================================================
cell-18::
STATISTICAL SIGNIFICANCE TESTS — EG-DQN vs All Baselines
===========================================================================
  Test: Mann-Whitney U (one-sided: EG-DQN > baseline)
  Bootstrap: 95% CI on mean daily revenue difference (n=2000 resamples)
───────────────────────────────────────────────────────────────────────────
  EG-DQN vs Fixed-Price : p=0.0000 | 95%CI=[+421.50,+437.41] | d=2.495 | ✅ Significant
  EG-DQN vs Rule-Based  : p=0.0000 | 95%CI=[+725.56,+751.71] | d=4.628 | ✅ Significant
  EG-DQN vs GLM-Only    : p=0.5004 | 95%CI=[+0.00,+0.00] | d=0.000 | ❌ Not significant
  EG-DQN vs Pure DQN    : p=0.5004 | 95%CI=[+0.00,+0.00] | d=0.000 | ❌ Not significant

───────────────────────────────────────────────────────────────────────────
Effect size interpretation: d<0.2=negligible, 0.2-0.5=small, 0.5-0.8=medium, >0.8=large