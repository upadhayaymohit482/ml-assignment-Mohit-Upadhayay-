
# Part B: Business Case Analysis  
## Promotion Effectiveness at a Fashion Retail Chain  

---

# B1. Problem Formulation  

## (a) Machine Learning Formulation  

This problem can be formulated as a **supervised learning regression problem**.

- **Target Variable:**  
  `items_sold` (number of items sold in a store for a given month)

- **Input Features:**  
  - Store characteristics: `store_id`, `store_size`, `location_type`, `footfall`, `competition_density`  
  - Promotion details: `promotion_type`  
  - Temporal features: `month`, `year`, `is_weekend`, `is_festival`  
  - Customer behavior proxies: historical sales, visit frequency  
  - External factors: local competition, seasonality  

- **Type of Problem:**  
  This is a **regression problem** because the goal is to predict a continuous numeric value (items sold).

**Justification:**  
The company wants to maximise items sold, which is a continuous variable. Therefore, regression is appropriate instead of classification.

---

## (b) Why Use Items Sold Instead of Revenue  

Using **items sold (sales volume)** is more reliable than revenue because:

- Revenue can be distorted by pricing strategies (discounts, promotions)
- A high revenue does not necessarily mean high customer engagement
- Promotions like heavy discounts may reduce revenue but increase volume

**Broader Principle:**  
The target variable should align with the **true business objective**. In this case, the goal is to increase customer activity and product movement, not just revenue.

This illustrates that:
> Choosing the wrong target variable can lead to misleading model outcomes and poor business decisions.

---

## (c) Alternative Modelling Strategy  

Instead of one global model, a better approach is:

### **Segmented / Hierarchical Modelling**

- Build separate models for:
  - Urban stores
  - Semi-urban stores
  - Rural stores  

OR  

- Use a **store-cluster-based model**

**Justification:**
- Customer behavior differs significantly across locations
- Promotions have different effectiveness in different markets
- A single global model may average out important patterns

This improves:
- Model accuracy
- Interpretability
- Business relevance

---

# B2. Data and EDA Strategy  

## (a) Data Joining and Final Dataset  

### Data Sources:
1. Transactions  
2. Store attributes  
3. Promotion details  
4. Calendar  

### Joining Strategy:
- Join transactions with store attributes using `store_id`
- Join promotion details using `promotion_id` or date
- Join calendar using `transaction_date`

### Final Dataset Grain:
> **One row = one store per month**

### Aggregations:
- Total items sold per store per month
- Average basket size
- Total transactions
- Promotion applied that month
- Footfall (aggregated if needed)

---

## (b) Exploratory Data Analysis (EDA)  

### 1. Promotion vs Sales Plot
- Compare average items sold by promotion type  
- Helps identify which promotions are generally effective  

### 2. Time Series Plot
- Monthly sales trends  
- Detect seasonality and trends  

### 3. Correlation Heatmap
- Identify relationships between features  
- Helps detect multicollinearity  

### 4. Store Segmentation Analysis
- Compare performance across store types (urban vs rural)  
- Helps justify segmented models  

### 5. Distribution Plots
- Check skewness in `items_sold`  
- Decide if transformation is needed  

---

## (c) Handling Imbalance (80% No Promotion)  

### Problem:
- Model may learn bias towards "no promotion"
- Promotions may appear less effective than they actually are

### Solutions:
- Stratified sampling
- Use weighted models
- Create separate models for promotion vs non-promotion
- Feature engineering (promotion flag)

---

# B3. Model Evaluation and Deployment  

## (a) Train-Test Split  

### Strategy:
- Use **time-based split**
- Train: First ~80% of time (earlier months)
- Test: Last ~20% (recent months)

### Why Not Random Split:
- Causes data leakage
- Future information may enter training
- Unrealistic evaluation

### Evaluation Metrics:

- **RMSE (Root Mean Squared Error)**  
  Penalises large errors → useful for big mistakes  

- **MAE (Mean Absolute Error)**  
  Easy to interpret → average prediction error  

**Interpretation:**
- Lower RMSE → fewer large mistakes  
- Lower MAE → better overall prediction accuracy  

---

## (b) Explaining Model Recommendations  

Use **feature importance** from the model:

### Steps:
1. Identify top features affecting prediction  
2. Compare feature values for December vs March  
3. Check differences:
   - Seasonality
   - Festival flags
   - Customer activity  

### Communication:
Explain in simple terms:
- “In December, festivals and high demand make loyalty rewards effective”
- “In March, discounts work better due to lower demand”

---

## (c) Deployment Strategy  

### 1. Model Saving
- Save model using `joblib` or `pickle`

### 2. Monthly Prediction Pipeline
- Collect latest data
- Apply same preprocessing pipeline
- Generate predictions for all stores

### 3. Recommendation Generation
- For each store:
  - Predict items sold for each promotion type
  - Choose promotion with highest predicted sales

### 4. Monitoring

Track:
- Prediction error over time
- Data drift (feature distribution changes)
- Model performance decay

### 5. Retraining

Retrain when:
- Performance drops significantly
- New data patterns emerge
- Seasonal changes occur

---

# Final Conclusion  

This analysis translates a business problem into a structured ML workflow:

- Clear problem formulation  
- Thoughtful data design  
- Strong EDA strategy  
- Robust evaluation  
- Practical deployment plan  

This ensures the model is both **technically sound and business-relevant**.
