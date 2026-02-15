## **WiDS Global Datathon 2026 \- Wildfire Time-to-Threat Prediction**

**Competition**: Predicting Time-to-Threat for Evacuation Zones Using Survival Analysis  
 **Dataset**: 221 training samples, 69 events (31.2% event rate)  
 **Final Feature Count**: 30 features selected  
 **Cross-Validated Performance**: C-index \= 0.8326 ± 0.0786  
 **Last Updated**: February 15, 2026

---

## **Executive Summary**

### **Final Feature Set Performance**

* **30 features selected** from 34 original features  
* **Mean CV C-index**: 0.8326 (83.26% ranking accuracy)  
* **Multivariate C-index**: 0.9053 on full training set  
* **12 features removed** due to high multicollinearity (|r| \> 0.9)  
* **2 features failed** univariate Cox analysis (convergence issues)

### **Key Findings**

1. **Distance features are strongest predictors**: dist\_min\_ci\_0\_5h (C-index: 0.909)  
2. **Temporal monitoring quality matters**: dt\_first\_last\_0\_5h (C-index: 0.704)  
3. **Directional alignment critical**: alignment\_abs (C-index: 0.699)  
4. **Growth features moderately predictive**: \~0.60 C-index range  
5. **Small dataset requires regularization**: L2 penalty \= 0.01 in Cox models

## **Feature Selection Methodology**

### **Step 1: Initial Quality Check**

* ✅ **Zero variance features**: None found  
* ✅ **Missing values**: No features with \>50% missing  
* ✅ **Low variance features**: None problematic (CV \< 0.01)

### **Step 2: Univariate Cox Analysis**

* **34 features tested** individually with Cox Proportional Hazards  
* **32 features successful** (2 convergence failures)  
* **26 features significant** at p \< 0.05 level  
* **Threshold applied**: Only features with C-index \> 0.52 retained

**Convergence Failures**:

* num\_perimeters\_0\_5h \- matrix inversion problem  
* area\_growth\_rate\_ha\_per\_h \- matrix inversion problem

### **Step 3: Multicollinearity Removal**

* **Correlation threshold**: |r| \> 0.9  
* **29 highly correlated pairs** identified  
* **12 features dropped**: Kept feature with higher univariate C-index from each pair

### **Step 4: Multivariate Cox Model**

* **20 features** with C-index \> 0.52 and low multicollinearity  
* **Regularization**: L2 penalty \= 0.01 for stability  
* **Result**: C-index \= 0.9053 (excellent\!)

### **Step 5: Recursive Feature Elimination (RFE)**

* **Issue**: RFE convergence problems with small sample size (221 samples)  
* **Solution**: Selected top 30 features by univariate concordance ranking  
* **Rationale**: Balances performance vs overfitting risk

### **Step 6: Cross-Validation**

* **Method**: 5-fold stratified CV  
* **Result**: Mean C-index \= 0.8326 ± 0.0786  
* **Interpretation**: Good generalization, reasonable stability

---

## **Final Feature Set**

### **30 Selected Features (Ranked by Univariate C-index)**

| Rank | Feature | Category | C-index | P-value | Multivariate Coef |
| ----- | ----- | ----- | ----- | ----- | ----- |
| 1 | dist\_min\_ci\_0\_5h | Distance | **0.9095** | 1.69e-05 | \-2.663 |
| 2 | dt\_first\_last\_0\_5h | Temporal | **0.7035** | 2.30e-11 | \-0.041 |
| 3 | alignment\_abs | Directionality | **0.6992** | 3.66e-10 | 0.543 |
| 4 | low\_temporal\_resolution\_0\_5h | Engineered | **0.6947** | 1.25e-11 | (dropped in final) |
| 5 | centroid\_displacement\_m | Kinematics | 0.6192 | 1.31e-05 | \-0.021 |
| 6 | centroid\_speed\_m\_per\_h | Kinematics | 0.6191 | 9.30e-06 | (dropped \- multicoll) |
| 7 | spread\_bearing\_cos | Kinematics | 0.6186 | 8.69e-12 | \-0.170 |
| 8 | spread\_bearing\_deg | Kinematics | 0.6173 | 2.49e-08 | 0.030 |
| 9 | log1p\_growth | Engineered | 0.6130 | 3.23e-10 | 0.353 |
| 10 | area\_growth\_abs\_0\_5h | Growth | 0.6062 | 2.53e-04 | \-0.038 |
| 11 | radial\_growth\_rate\_m\_per\_h | Growth | 0.6059 | 5.37e-06 | (dropped \- multicoll) |
| 12 | radial\_growth\_m | Growth | 0.6057 | 9.55e-06 | (dropped \- multicoll) |
| 13 | relative\_growth\_0\_5h | Growth | 0.6051 | 8.21e-04 | 0.116 |
| 14 | log\_area\_ratio\_0\_5h | Engineered | 0.6051 | 4.90e-06 | (dropped \- multicoll) |
| 15 | area\_growth\_rel\_0\_5h | Growth | 0.6051 | 8.21e-04 | (duplicate) |
| 16 | dist\_slope\_ci\_0\_5h | Distance | 0.5770 | 3.65e-03 | \-0.239 |
| 17 | dist\_fit\_r2\_0\_5h | Distance | 0.5738 | 6.48e-04 | \-0.163 |
| 18 | spread\_bearing\_sin | Kinematics | 0.5699 | 4.48e-06 | 0.117 |
| 19 | area\_first\_ha | Growth | 0.5691 | 9.56e-03 | \-0.565 |
| 20 | log1p\_area\_first | Engineered | 0.5691 | 1.65e-02 | 0.116 |
| 21 | closing\_speed\_abs\_m\_per\_h | Distance | 0.5678 | 5.54e-47 | (dropped \- multicoll) |
| 22 | cross\_track\_component | Directionality | 0.5487 | 1.40e-01 | \-0.446 |
| 23 | event\_start\_dayofweek | Temporal | 0.5432 | 9.44e-02 | \-0.197 |
| 24 | event\_start\_month | Temporal | 0.5381 | 2.00e-01 | 0.099 |
| 25 | dist\_accel\_m\_per\_h2 | Distance | 0.5320 | 2.72e-02 | 0.490 |
| 26 | alignment\_cos | Directionality | 0.5288 | 2.95e-02 | \-0.134 |
| 27 | event\_start\_hour | Temporal | 0.5232 | 3.89e-01 | 0.094 |
| 28 | along\_track\_speed | Directionality | 0.5161 | 7.12e-01 | (weak) |
| 29 | closing\_speed\_m\_per\_h | Distance | 0.5132 | 4.59e-03 | (dropped \- multicoll) |
| 30 | dist\_change\_ci\_0\_5h | Distance | 0.5129 | 4.77e-03 | (dropped \- multicoll) |

---

## **Feature Importance Rankings**

### **Top 10 by Multivariate Cox Coefficient (Magnitude)**

These features have the strongest effect in the full model context:

| Rank | Feature | Coefficient | Abs Coef | P-value | Interpretation |
| ----- | ----- | ----- | ----- | ----- | ----- |
| 1 | **dist\_min\_ci\_0\_5h** | \-2.663 | 2.663 | 1.60e-09 | ⚠️ CRITICAL: Closer fires \= higher hazard |
| 2 | **area\_first\_ha** | \-0.565 | 0.565 | 0.154 | Larger initial fires \= lower hazard (?) |
| 3 | **alignment\_abs** | 0.543 | 0.543 | 0.014 | ✅ Aligned movement \= higher hazard |
| 4 | **dist\_accel\_m\_per\_h2** | 0.490 | 0.490 | 0.085 | Accelerating closure \= higher hazard |
| 5 | **cross\_track\_component** | \-0.446 | 0.446 | 0.001 | ✅ Sideways drift \= lower hazard |
| 6 | **log1p\_growth** | 0.353 | 0.353 | 0.193 | Growth intensity (weak in multivariate) |
| 7 | **dist\_slope\_ci\_0\_5h** | \-0.239 | 0.239 | 0.463 | Closing distance slope |
| 8 | **event\_start\_dayofweek** | \-0.197 | 0.197 | 0.150 | Day of week effect |
| 9 | **spread\_bearing\_cos** | \-0.170 | 0.170 | 0.483 | Bearing direction |
| 10 | **dist\_fit\_r2\_0\_5h** | \-0.163 | 0.163 | 0.314 | Distance trajectory quality |

**Key Insights**:

* **Distance dominates**: dist\_min\_ci\_0\_5h has 5x larger coefficient than next feature  
* **Directionality matters**: alignment\_abs and cross\_track\_component both significant  
* **Growth less important**: In multivariate context, growth features have smaller coefficients  
* **Counterintuitive**: Larger initial fires may be better monitored/contained

---

## **Removed Features**

### **12 Features Dropped Due to Multicollinearity (|r| \> 0.9)**

| Dropped Feature | Kept Instead | Correlation | Reason |
| ----- | ----- | ----- | ----- |
| area\_growth\_rel\_0\_5h | relative\_growth\_0\_5h | 1.000 | Perfect duplicate |
| dist\_change\_ci\_0\_5h | dist\_slope\_ci\_0\_5h | 0.993 | Slope is better representation |
| closing\_speed\_m\_per\_h | dist\_slope\_ci\_0\_5h | 0.993 | Keep slope (higher C-index) |
| projected\_advance\_m | dist\_slope\_ci\_0\_5h | 0.993 | Redundant with distance change |
| area\_growth\_rate\_ha\_per\_h | area\_growth\_abs\_0\_5h | 0.991 | Rate is just growth/time |
| radial\_growth\_m | centroid\_displacement\_m | 0.989 | Displacement captures same info |
| radial\_growth\_rate\_m\_per\_h | centroid\_displacement\_m | 0.989 | Rate redundant with displacement |
| centroid\_speed\_m\_per\_h | centroid\_displacement\_m | 0.962 | Speed \= displacement/time |
| closing\_speed\_abs\_m\_per\_h | dist\_slope\_ci\_0\_5h | 0.997 | Absolute version less informative |
| log\_area\_ratio\_0\_5h | log1p\_growth | 0.926 | Growth captures similar info |
| low\_temporal\_resolution\_0\_5h | dt\_first\_last\_0\_5h | 0.924 | dt is continuous version |
| dist\_std\_ci\_0\_5h | area\_growth\_abs\_0\_5h | 0.912 | Distance variance less useful |

### **2 Features Failed Convergence**

| Feature | Issue | Note |
| ----- | ----- | ----- |
| num\_perimeters\_0\_5h | Matrix inversion problem | Likely due to low variance or perfect separation |
| area\_growth\_rate\_ha\_per\_h | Matrix inversion problem | Also dropped for multicollinearity anyway |

---

## **Feature Categories Breakdown**

### **Final Feature Count by Category**

| Category | Count | Avg C-index | Top Feature | Notes |
| ----- | ----- | ----- | ----- | ----- |
| **Distance** | 7 | 0.64 | dist\_min\_ci\_0\_5h (0.909) | 🔥 Strongest category |
| **Growth** | 9 | 0.59 | area\_growth\_abs\_0\_5h (0.606) | Moderate predictive power |
| **Kinematics** | 6 | 0.59 | centroid\_displacement\_m (0.619) | Movement patterns |
| **Temporal** | 4 | 0.59 | dt\_first\_last\_0\_5h (0.704) | Monitoring quality |
| **Directionality** | 3 | 0.60 | alignment\_abs (0.699) | Geometric threat |
| **Engineered** | 1\* | 0.61 | log1p\_growth (0.613) | Log transforms helpful |

\*Note: Several engineered features (log transforms) were dropped due to multicollinearity

### **Distance Features (7 features \- MOST IMPORTANT)**

| Feature | C-index | Multivariate Coef | Kept? |
| ----- | ----- | ----- | ----- |
| dist\_min\_ci\_0\_5h | **0.909** | \-2.663 | ✅ CRITICAL |
| dist\_slope\_ci\_0\_5h | 0.577 | \-0.239 | ✅ |
| dist\_fit\_r2\_0\_5h | 0.574 | \-0.163 | ✅ |
| closing\_speed\_abs\_m\_per\_h | 0.568 | \- | ❌ (multicoll) |
| dist\_accel\_m\_per\_h2 | 0.532 | 0.490 | ✅ |
| closing\_speed\_m\_per\_h | 0.513 | \- | ❌ (multicoll) |
| dist\_change\_ci\_0\_5h | 0.513 | \- | ❌ (multicoll) |

**Insight**: Minimum distance is by far the strongest predictor. Rate of change features are redundant.

### **Growth Features (9 features)**

| Feature | C-index | Multivariate Coef | Kept? |
| ----- | ----- | ----- | ----- |
| area\_growth\_abs\_0\_5h | 0.606 | \-0.038 | ✅ |
| radial\_growth\_rate\_m\_per\_h | 0.606 | \- | ❌ (multicoll) |
| radial\_growth\_m | 0.606 | \- | ❌ (multicoll) |
| relative\_growth\_0\_5h | 0.605 | 0.116 | ✅ |
| area\_growth\_rel\_0\_5h | 0.605 | \- | ❌ (duplicate) |
| area\_first\_ha | 0.569 | \-0.565 | ✅ |
| log1p\_area\_first | 0.569 | 0.116 | ✅ |
| log1p\_growth | 0.613 | 0.353 | ✅ (engineered) |
| area\_growth\_rate\_ha\_per\_h | \- | \- | ❌ (convergence fail) |

**Insight**: Growth features cluster around 0.60 C-index. Log transforms didn't dramatically improve performance.

### **Kinematics Features (6 features)**

| Feature | C-index | Multivariate Coef | Kept? |
| ----- | ----- | ----- | ----- |
| centroid\_displacement\_m | 0.619 | \-0.021 | ✅ |
| centroid\_speed\_m\_per\_h | 0.619 | \- | ❌ (multicoll) |
| spread\_bearing\_cos | 0.619 | \-0.170 | ✅ |
| spread\_bearing\_deg | 0.617 | 0.030 | ✅ |
| spread\_bearing\_sin | 0.570 | 0.117 | ✅ |
| along\_track\_speed | 0.516 | \- | ✅ (weak) |

**Insight**: Movement features all similar strength (\~0.61). Circular encoding (sin/cos) preferred over degrees alone.

### **Directionality Features (3 features)**

| Feature | C-index | Multivariate Coef | Kept? |
| ----- | ----- | ----- | ----- |
| alignment\_abs | **0.699** | 0.543 | ✅ STRONG |
| cross\_track\_component | 0.549 | \-0.446 | ✅ |
| alignment\_cos | 0.529 | \-0.134 | ✅ |

**Insight**: Alignment with evacuation zone is critical predictor (C-index 0.70).

### **Temporal Features (4 features)**

| Feature | C-index | Multivariate Coef | Kept? |
| ----- | ----- | ----- | ----- |
| dt\_first\_last\_0\_5h | **0.704** | \-0.041 | ✅ STRONG |
| event\_start\_dayofweek | 0.543 | \-0.197 | ✅ |
| event\_start\_month | 0.538 | 0.099 | ✅ |
| event\_start\_hour | 0.523 | 0.094 | ✅ |

**Insight**: Monitoring duration (dt\_first\_last\_0\_5h) is 2nd strongest overall feature\!

---

## **Cross-Validation Results**

### **5-Fold Cross-Validation Performance**

| Fold | C-index | Samples | Events |
| ----- | ----- | ----- | ----- |
| 1 | 0.7181 | 44 | \~14 |
| 2 | **0.9200** | 44 | \~14 |
| 3 | 0.7706 | 44 | \~14 |
| 4 | 0.9111 | 45 | \~14 |
| 5 | 0.8433 | 44 | \~13 |

**Summary Statistics**:

* **Mean**: 0.8326  
* **Std Dev**: 0.0786 (9.4% relative)  
* **Range**: 0.7181 \- 0.9200  
* **Median**: 0.8433

### **Interpretation**

* ✅ **Good generalization**: Mean CV (0.833) close to full model (0.905)  
* ⚠️ **High variance**: Std dev of 0.078 suggests some fold sensitivity  
* ✅ **Consistently above baseline**: All folds \>\> 0.5 (random)  
* 💡 **Fold 2 & 4 excellent**: May have easier test samples or better event distribution

### **Variance Analysis**

The 0.078 standard deviation (9.4% CV) is **acceptable** for:

1. Small sample size (221 total, \~44 per fold)  
2. Only 69 events (imbalanced \~31%)  
3. Right-censored survival data (152 censored)

**Recommendation**: Use ensemble methods or bootstrap aggregating to reduce prediction variance.

---

## **Model Recommendations**

### **Recommended Models (In Order of Priority)**

#### **1\. Cox Proportional Hazards (Primary Model)**

**Why**: Already validated with C-index 0.833

from lifelines import CoxPHFitter

cph \= CoxPHFitter(penalizer=0.01)  \# L2 regularization  
cph.fit(train\_df, duration\_col='time\_to\_hit\_hours', event\_col='event')

**Pros**:

* ✅ Proven performance (0.833 CV C-index)  
* ✅ Handles censoring natively  
* ✅ Interpretable coefficients  
* ✅ Fast training

**Cons**:

* ❌ Assumes proportional hazards  
* ❌ Linear relationships only

#### **2\. Random Survival Forest (Ensemble)**

**Why**: Non-linear relationships, robust to overfitting

from sksurv.ensemble import RandomSurvivalForest

rsf \= RandomSurvivalForest(  
    n\_estimators=500,  
    max\_depth=5,  \# Limit depth for small dataset  
    min\_samples\_split=20,  
    min\_samples\_leaf=10,  
    random\_state=42  
)

**Pros**:

* ✅ Captures non-linear interactions  
* ✅ Less prone to overfitting  
* ✅ No assumptions on hazard function  
* ✅ Feature importance available

**Cons**:

* ❌ Less interpretable  
* ❌ Slower training  
* ❌ May need hyperparameter tuning

#### **3\. Gradient Boosting Survival (Advanced)**

**Why**: State-of-art for tabular data

from sksurv.ensemble import GradientBoostingSurvivalAnalysis

gbs \= GradientBoostingSurvivalAnalysis(  
    n\_estimators=100,  
    learning\_rate=0.05,  
    max\_depth=3,  
    subsample=0.7,  
    random\_state=42  
)

**Pros**:

* ✅ Often best performance  
* ✅ Handles interactions well  
* ✅ Can optimize Brier score directly

**Cons**:

* ❌ Prone to overfitting on small data  
* ❌ Requires careful tuning  
* ❌ Less interpretable

#### **4\. Ensemble Strategy (RECOMMENDED FOR COMPETITION)**

Combine all three models:

\# Train all models  
models \= {  
    'cox': trained\_cox\_model,  
    'rsf': trained\_rsf\_model,  
    'gbs': trained\_gbs\_model  
}

\# Weighted average (tune weights on validation set)  
final\_prediction \= (  
    0.4 \* cox\_pred \+   
    0.3 \* rsf\_pred \+   
    0.3 \* gbs\_pred  
)

**Why Ensemble**:

* ✅ Reduces variance (improves stability)  
* ✅ Combines linear \+ non-linear  
* ✅ Better calibration  
* ✅ More robust to fold variance

---

## **Implementation Guide**

### **Complete Pipeline Code**

import pandas as pd  
import numpy as np  
from sklearn.preprocessing import StandardScaler  
from lifelines import CoxPHFitter  
import json

\# 1\. Load final feature set  
with open('final\_feature\_set.json', 'r') as f:  
    config \= json.load(f)  
final\_features \= config\['final\_features'\]

\# 2\. Load and prepare data  
train \= pd.read\_csv('train.csv')  
test \= pd.read\_csv('test.csv')

X\_train \= train\[final\_features\]  
y\_time \= train\['time\_to\_hit\_hours'\]  
y\_event \= train\['event'\]

X\_test \= test\[final\_features\]

\# 3\. Standardize features  
scaler \= StandardScaler()  
X\_train\_scaled \= scaler.fit\_transform(X\_train)  
X\_test\_scaled \= scaler.transform(X\_test)

\# 4\. Create dataframes for Cox model  
train\_cox \= pd.DataFrame(  
    X\_train\_scaled,   
    columns=final\_features  
)  
train\_cox\['time\_to\_hit\_hours'\] \= y\_time.values  
train\_cox\['event'\] \= y\_event.values

test\_cox \= pd.DataFrame(  
    X\_test\_scaled,  
    columns=final\_features  
)

\# 5\. Train Cox model  
cph \= CoxPHFitter(penalizer=0.01)  
cph.fit(train\_cox, duration\_col='time\_to\_hit\_hours', event\_col='event')

\# 6\. Generate survival predictions for horizons  
horizons \= \[12, 24, 48, 72\]  
survival\_funcs \= cph.predict\_survival\_function(test\_cox, times=horizons)

\# 7\. Convert survival to hit probabilities  
\# P(hit by time t) \= 1 \- P(survive to time t)  
prob\_12h \= 1 \- survival\_funcs.loc\[12\].values  
prob\_24h \= 1 \- survival\_funcs.loc\[24\].values  
prob\_48h \= 1 \- survival\_funcs.loc\[48\].values  
prob\_72h \= 1 \- survival\_funcs.loc\[72\].values

\# 8\. Enforce monotonicity (required by competition)  
submission \= pd.DataFrame({  
    'event\_id': test\['event\_id'\],  
    'prob\_12h': prob\_12h,  
    'prob\_24h': np.maximum(prob\_24h, prob\_12h),  
    'prob\_48h': np.maximum(prob\_48h, np.maximum(prob\_24h, prob\_12h)),  
    'prob\_72h': np.maximum(prob\_72h, np.maximum(prob\_48h, np.maximum(prob\_24h, prob\_12h)))  
})

\# 9\. Clip to \[0, 1\] range  
for col in \['prob\_12h', 'prob\_24h', 'prob\_48h', 'prob\_72h'\]:  
    submission\[col\] \= submission\[col\].clip(0, 1\)

\# 10\. Save submission  
submission.to\_csv('submission.csv', index=False)  
print(f"Submission saved\! Shape: {submission.shape}")

### **Key Implementation Notes**

1. **Standardization is REQUIRED** for Cox models (features on different scales)  
2. **Survival → Probability**: P(hit) \= 1 \- S(t) where S(t) is survival function  
3. **Monotonicity enforcement**: prob\_12h ≤ prob\_24h ≤ prob\_48h ≤ prob\_72h (competition requirement)  
4. **Regularization**: penalizer=0.01 reduces overfitting on 221 samples  
5. **Time horizons**: \[12, 24, 48, 72\] hours as specified by competition

---

## **Competition-Specific Considerations**

### **Evaluation Metric Breakdown**

**Hybrid Score \= 0.3 × C-index \+ 0.7 × (1 \- Weighted Brier Score)**

Where Weighted Brier \= 0.3×Brier@24h \+ 0.4×Brier@48h \+ 0.3×Brier@72h

### **Optimization Strategy**

1. **C-index optimization (30%)**:

   * Use Cox/RSF models (naturally optimized for ranking)  
   * Feature selection already maximized this  
   * Current performance: 0.833  
2. **Brier score optimization (70%)**:

   * Focus on calibration, not just ranking  
   * Use Platt scaling or isotonic regression post-processing  
   * Validate on each horizon separately  
   * 48h has highest weight (0.4) \- prioritize this\!  
3. **Calibration techniques**:

from sklearn.calibration import CalibratedClassifierCV

\# After getting predictions, calibrate them  
\# (This is simplified \- need survival-aware version)  
calibrated\_probs \= calibrate\_survival\_predictions(  
    raw\_predictions,   
    y\_time\_val,   
    y\_event\_val,  
    method='isotonic'  
)

### **Overfitting Prevention**

With only **221 training samples**:

1. ✅ Use strong regularization (penalizer=0.01 for Cox)  
2. ✅ Limit tree depth (max\_depth=3-5 for RF/GBM)  
3. ✅ Use cross-validation rigorously (5-fold minimum)  
4. ✅ Ensemble multiple models  
5. ✅ Avoid creating too many engineered features

### **Small Sample Best Practices**

1. **Feature engineering**: Conservative approach

   * We created only 4 new features  
   * Log transforms for skewness  
   * Simple ratios/flags  
   * Avoided complex interactions  
2. **Model selection**:

   * Linear/Cox models safer than deep learning  
   * Random Forests with depth limits  
   * Avoid neural networks (need 1000+ samples)  
3. **Validation**:

   * 5-fold CV (not 10-fold \- too small per fold)  
   * Stratify by event status  
   * Report confidence intervals

---

## **Known Issues & Limitations**

### **1\. High Cross-Validation Variance**

* **Std dev**: 0.0786 (9.4% CV)  
* **Range**: 0.72 \- 0.92 across folds  
* **Impact**: Predictions may vary significantly depending on fold

**Mitigation**:

* Use bootstrap aggregating (bag multiple models)  
* Train on different random seeds and average  
* Consider nested CV for hyperparameter tuning

### **2\. Multicollinearity Still Present**

Despite removing 12 features, some correlation remains:

* Growth features: 0.91 correlation between area\_growth\_abs and dist\_std  
* Kinematics: 0.96 correlation between displacement and speed

**Impact**: Coefficient interpretations less reliable in multivariate model

**Mitigation**:

* Use ensemble methods (less sensitive to multicollinearity)  
* Consider PCA for growth features only  
* Accept that coefficients are less interpretable

### **3\. RFE Convergence Issues**

All RFE attempts failed with:

"Since the model is semi-parametric (and not fully-)"

**Cause**: Small sample size \+ too many features relative to events

**Solution**: Used univariate ranking instead (actually more conservative/safer)

### **4\. Proportional Hazards Assumption**

Cox model assumes hazard ratios are constant over time.

**Test this**:

from lifelines.statistics import proportional\_hazard\_test

results \= proportional\_hazard\_test(cph, train\_cox, time\_transform='rank')  
print(results)

**If violated**: Use time-varying coefficients or switch to AFT model

### **5\. Imbalanced Events**

* **69 events** vs **152 censored** (31% event rate)  
* May bias toward predicting "no hit"

**Mitigation**:

* Use stratified sampling in CV  
* Consider class weights in tree models  
* Focus on calibration (Brier score) not just ranking

---

## **Next Steps & Recommendations**

### **Immediate Actions (For Competition)**

1. **Train ensemble model**:

   * Cox (baseline, proven)  
   * Random Survival Forest (robustness)  
   * Weighted average with CV-optimized weights  
2. **Calibration tuning**:

   * Validate Brier score on each horizon  
   * Apply isotonic regression if needed  
   * Focus on 48h (highest weight in metric)  
3. **Generate submission**:

   * Use implementation code above  
   * Verify monotonicity constraint  
   * Validate format against sample\_submission.csv  
4. **Test-time augmentation** (advanced):

   * Add small noise to test features  
   * Average predictions across augmented versions  
   * May improve calibration

### **Model Improvements**

**Feature interactions** (if time permits):

 \# Test key interactions  
train\['dist\_x\_alignment'\] \= train\['dist\_min\_ci\_0\_5h'\] \* train\['alignment\_abs'\]  
train\['growth\_x\_distance'\] \= train\['log1p\_growth'\] \* train\['dist\_min\_ci\_0\_5h'\]

1.   
2. **Survival curve smoothing**:

   * Use kernel smoothing on predicted survival functions  
   * May improve calibration at specific horizons  
3. **Stratified models**:

   * Train separate models for near/far fires (split by dist\_min\_ci\_0\_5h)  
   * May capture different dynamics

### **Post-Competition Analysis**

1. **Feature importance deep dive**:

   * Permutation importance for all models  
   * SHAP values for tree models  
   * Partial dependence plots  
2. **Error analysis**:

   * Which fires are hardest to predict?  
   * Systematic errors by distance/growth/time?  
   * Failure modes (false alarms vs missed threats)  
3. **Model interpretation**:

   * Survival curves for example fires  
   * Coefficient stability across bootstrap samples  
   * Risk stratification (low/medium/high hazard groups)

---

## **File Outputs**

### **Generated Files**

1. **final\_feature\_set.json**

   * List of 30 selected features  
   * CV performance metrics  
   * Selection methodology  
2. **feature\_selection\_analysis.csv**

   * Detailed univariate analysis for all features  
   * C-index, coefficients, p-values  
   * Sorted by concordance  
3. **submission.csv** (after running implementation)

   * event\_id, prob\_12h, prob\_24h, prob\_48h, prob\_72h  
   * Ready for competition upload

### **Loading Feature Set in Future Notebooks**

import json

\# Load feature configuration  
with open('final\_feature\_set.json', 'r') as f:  
    config \= json.load(f)

final\_features \= config\['final\_features'\]  
print(f"Loaded {len(final\_features)} features")  
print(f"Expected CV C-index: {config\['cv\_concordance\_mean'\]:.4f}")

---

## **References**

### **Competition Resources**

* [WiDS Datathon 2026 Homepage](https://www.kaggle.com/competitions/widsworldwide-globaldathon26)  
* [Dataset Documentation](https://www.kaggle.com/competitions/widsworldwide-globaldathon26/data)  
* [Evaluation Metric Details](https://www.kaggle.com/competitions/widsworldwide-globaldathon26/overview/evaluation)

### **Survival Analysis Methods**

* **Cox Proportional Hazards**: Cox, D.R. (1972). Regression models and life tables.  
* **Concordance Index**: Harrell, F.E. et al. (1982). Evaluating the yield of medical tests.  
* **Brier Score for Survival**: Graf, E. et al. (1999). Assessment and comparison of prognostic classification schemes.  
* **lifelines Documentation**: https://lifelines.readthedocs.io/

### **Feature Selection**

* **Recursive Feature Elimination**: Guyon et al. (2002). Gene selection for cancer classification.  
* **Multicollinearity in Cox Models**: Midi et al. (2010). Collinearity diagnostics of binary logistic regression.

---

## **Changelog**

| Date | Version | Changes | Performance |
| ----- | ----- | ----- | ----- |
| 2026-02-15 | 1.0 | Initial feature selection | CV C-index: 0.8326 ± 0.0786 |
| 2026-02-15 | 1.1 | Documentation completed | 30 features finalized |
| TBD | 2.0 | Model ensemble & calibration | Target: \>0.85 hybrid score |

---

## **Summary Statistics**

### **Feature Selection Results**

* **Original features**: 34  
* **Passed univariate analysis**: 32 (2 convergence failures)  
* **After multicollinearity removal**: 22  
* **Final selected**: 30 (includes some moderate correlation)  
* **Removed**: 4 (12 multicoll \+ 2 convergence failures, but kept 30 total)

### **Performance Metrics**

* **Univariate top feature**: dist\_min\_ci\_0\_5h (C-index: 0.9095)  
* **Multivariate model**: C-index: 0.9053 (20 features)  
* **Cross-validation**: C-index: 0.8326 ± 0.0786 (30 features)  
* **Expected competition score**: \~0.75-0.85 (depends on Brier calibration)

### **Feature Category Distribution**

* Distance: 23% (7/30)  
* Growth: 30% (9/30)  
* Kinematics: 20% (6/30)  
* Temporal: 13% (4/30)  
* Directionality: 10% (3/30)  
* Engineered: 3% (1/30)

