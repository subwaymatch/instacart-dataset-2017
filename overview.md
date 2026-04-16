# Instacart Online Grocery Shopping Dataset 2017 — Case Study Overview

> **Course:** Advanced Business Analytics  
> **Dataset:** [The Instacart Online Grocery Shopping Dataset 2017](https://www.instacart.com/datasets/grocery-shopping-2017)  
> **Citation:** *"The Instacart Online Grocery Shopping Dataset 2017", Accessed from https://www.instacart.com/datasets/grocery-shopping-2017*

---

## 1. Introduction

### Background

Instacart is an American grocery delivery and pick-up service that connects consumers with personal shoppers who fulfil orders from local supermarkets. In May 2017, Instacart released an anonymized snapshot of its transaction history — one of the largest publicly available retail datasets — to support research in machine learning, consumer behaviour, and operations analytics.

The dataset captures **over 3 million grocery orders** placed by **more than 200,000 unique users**, recording every product purchased, the sequence in which items were added to the cart, and a set of temporal signals (day of week, hour of day, and days elapsed since the previous order). This richness makes the dataset ideal for studying the full lifecycle of the customer relationship: from a shopper's very first order through to their most recent one.

### Why This Dataset Matters for Business Analytics

Grocery retail operates on thin margins and intense competition. The ability to understand *what* customers buy, *when* they buy it, *how often* they return, and *which products they consistently reorder* translates directly into revenue-generating decisions:

- **Personalised recommendations** — surfacing products a customer is likely to buy next.
- **Assortment optimisation** — knowing which aisles drive the most volume and loyalty.
- **Promotional targeting** — identifying moments in a customer's lifecycle where intervention (e.g., a coupon) is most effective.
- **Demand forecasting** — predicting order volume by time of day and day of week to staff and stock efficiently.

By working through this case study, you will experience the full analytical pipeline that underpins these decisions.

---

## 2. Dataset Description

### Scale

| Dimension | Value |
|-----------|-------|
| Total orders | ~3.4 million |
| Unique users | ~206,000 |
| Products in catalogue | ~50,000 |
| Aisles | 134 |
| Departments | 21 |
| Order–product rows (train set) | ~1.38 million |

### Files Included

| File | Description |
|------|-------------|
| `orders.csv.zip` | One row per order — user ID, timing, and sequence info |
| `order_products_train.csv` | One row per item in a training-set order |
| `products.csv` | Product catalogue with aisle and department linkage |
| `aisles.csv` | Aisle ID → aisle name lookup |
| `departments.csv` | Department ID → department name lookup |

### Data Dictionary

#### `orders`
| Column | Type | Description |
|--------|------|-------------|
| `order_id` | int | Unique order identifier |
| `user_id` | int | Unique user (customer) identifier |
| `eval_set` | string | Which split this order belongs to (`prior`, `train`, or `test`) |
| `order_number` | int | Sequence number for this user (1 = first order, n = nth order) |
| `order_dow` | int | Day of the week the order was placed (0 = Saturday … 6 = Friday) |
| `order_hour_of_day` | int | Hour of the day the order was placed (0–23) |
| `days_since_prior_order` | float | Days elapsed since the user's previous order; capped at 30; NaN for `order_number = 1` |

#### `order_products_train`
| Column | Type | Description |
|--------|------|-------------|
| `order_id` | int | Foreign key → `orders.order_id` |
| `product_id` | int | Foreign key → `products.product_id` |
| `add_to_cart_order` | int | Position in which the product was added to the cart (1 = first) |
| `reordered` | int | 1 if this product was previously purchased by this user; 0 otherwise |

#### `products`
| Column | Type | Description |
|--------|------|-------------|
| `product_id` | int | Unique product identifier |
| `product_name` | string | Human-readable product name |
| `aisle_id` | int | Foreign key → `aisles.aisle_id` |
| `department_id` | int | Foreign key → `departments.department_id` |

#### `aisles`
| Column | Type | Description |
|--------|------|-------------|
| `aisle_id` | int | Unique aisle identifier |
| `aisle` | string | Aisle name (e.g., "fresh fruits", "yogurt") |

#### `departments`
| Column | Type | Description |
|--------|------|-------------|
| `department_id` | int | Unique department identifier |
| `department` | string | Department name (e.g., "produce", "dairy eggs") |

### Table Relationships

```
departments ──┐
              ├──► products ──► order_products_train ──► orders
aisles  ───────┘
```

`orders` is the central fact table. `order_products_train` is a many-to-many bridge between orders and products. `products`, `aisles`, and `departments` are dimension/lookup tables that enrich item-level records with human-readable names and hierarchical categorisation.

---

## 3. Types of Analyses Possible with This Dataset

The table below maps analytical techniques to the business questions they address. All of these can be performed using the data files included in this repository.

| Analysis Type | Technique | Business Question Addressed |
|---------------|-----------|----------------------------|
| **Exploratory Data Analysis (EDA)** | Descriptive statistics, histograms, bar charts | What does the data look like? Are there anomalies or patterns at a glance? |
| **Temporal Analysis** | Time-series aggregation, heatmaps | When do customers shop? Are there peak hours or days? |
| **Product Performance Analysis** | Value counts, ranked lists | Which products, aisles, and departments are most popular? |
| **Reorder / Loyalty Analysis** | Reorder rate by segment, cohort analysis | Which products and categories drive repeat purchases? How does loyalty evolve? |
| **Customer Segmentation** | RFM-style scoring, k-means clustering | Which customer groups exist based on purchasing behaviour? |
| **Market Basket Analysis** | Co-occurrence counting, Apriori algorithm, association rules | Which products are frequently bought together? How can this drive cross-selling? |
| **Cart Behaviour Analysis** | Add-to-cart position, basket size distribution | How do customers build their carts? What is bought first vs. last? |
| **Predictive Modelling** | Logistic regression, gradient boosting, neural networks | Can we predict whether a customer will reorder a specific product? |
| **Recommendation Systems** | Collaborative filtering, word2vec on item sequences | How can personalised product recommendations be generated? |
| **Demand Forecasting** | Time aggregations, regression | How does order volume vary by time, and can it be forecast? |

---

## 4. Case Study Objectives

By completing this case study, students will be able to:

1. **Load and merge** a multi-table relational dataset using `pandas` and reason about entity relationships and foreign keys.
2. **Perform a structured EDA** — including univariate, bivariate, and temporal analysis — to surface meaningful patterns in a large real-world dataset.
3. **Quantify customer loyalty** by calculating reorder rates and interpreting them at the product, aisle, and department level.
4. **Segment customers** based on behavioural signals (order frequency, basket size, reorder rate, shopping cadence) and profile each segment.
5. **Apply market basket analysis** to identify product affinities and translate findings into concrete cross-selling or promotional recommendations.
6. **Communicate analytical findings** through well-labelled, publication-quality visualisations and a concise written narrative.
7. **Evaluate the limitations** of the dataset (sample bias, absence of pricing and demographics, capped temporal variables) and discuss how those limitations affect business conclusions.

---

## 5. Case Study Instructions

### Prerequisites

- Python 3.8 or higher
- The following packages: `pandas`, `numpy`, `plotly`, `mlxtend`
- Jupyter Notebook or JupyterLab

Install all dependencies in one command:

```bash
pip install pandas numpy plotly mlxtend jupyter
```

### Repository Structure

```
instacart-dataset-2017/
├── overview.md                      ← This file (start here)
├── README.md                        ← Dataset background and data dictionary
├── eda_tutorial.ipynb               ← Step-by-step EDA walkthrough (start with this)
├── analysis.ipynb                   ← Pre-built visualisation reference notebook
├── customer_behavior_analysis.ipynb ← Market basket & customer segmentation notebook
├── orders.csv.zip                   ← Orders table (unzip before use)
├── order_products_train.csv         ← Order–product bridge table
├── products.csv                     ← Product catalogue
├── aisles.csv                       ← Aisle lookup
└── departments.csv                  ← Department lookup
```

### Suggested Workflow

Students are expected to work through the notebooks **in the following order**, completing the questions in Section 6 as they go:

| Step | Notebook / Activity | Focus |
|------|-------------------|-------|
| 1 | Read `overview.md` and `README.md` | Understand the dataset, tables, and objectives |
| 2 | `eda_tutorial.ipynb` | Load data, inspect quality, and build intuition through EDA |
| 3 | `analysis.ipynb` | Review pre-built charts and deepen pattern recognition |
| 4 | `customer_behavior_analysis.ipynb` | Segmentation and market basket analysis |
| 5 | Independent analysis notebook | Answer the case questions (Section 6) in your own notebook |
| 6 | Written report | Synthesise findings into a structured business memo (see Section 6) |

### Working with the Data

1. **Unzip the orders file.** The `orders.csv.zip` archive must be extracted before the data can be loaded. The tutorial notebooks do this automatically with Python's `zipfile` module.
2. **Use the train split.** This case study uses `order_products_train.csv`, which covers ~131,000 orders. The "prior" and "test" splits require the full dataset download from Instacart.
3. **Mind memory usage.** After merging all tables, the enriched dataset contains ~1.4 million rows. Standard laptop hardware handles this comfortably, but avoid storing redundant copies of large DataFrames.
4. **Preserve raw data.** Never overwrite the original CSV files. Assign all transformed frames to new variables.

---

## 6. Questions to Answer

Complete all questions in a **single Jupyter notebook** named `case_study_analysis.ipynb`. For each question, include:
- The Python code used to compute the answer.
- At least one supporting visualisation (where relevant).
- A 2–4 sentence written interpretation of the result.

Submit your notebook along with a **500–800 word business memo** addressed to a hypothetical Instacart VP of Product, summarising your three most actionable findings.

---

### Part A — Exploratory Data Analysis

**A1.** How many unique users, orders, and products are captured in the dataset? What is the average number of orders per user, and how does that distribution look?

**A2.** What percentage of items in `order_products_train` are reorders (i.e., `reordered == 1`)? What does this tell you about the typical Instacart customer?

**A3.** Identify the top 10 most-ordered products. Are any of these products surprising? What do the top products suggest about Instacart's primary customer base?

**A4.** Plot the distribution of `days_since_prior_order`. What are the most common shopping cadences? What might explain the spike at 30 days?

**A5.** Create a heatmap of order volume by day of week and hour of day. Identify the two peak ordering windows and hypothesise why they occur at those times.

---

### Part B — Product and Category Analysis

**B1.** Rank all 21 departments by total number of items ordered *and* by reorder rate. Which departments rank highly on both metrics? Which departments have low reorder rates despite high volume, and what might explain this?

**B2.** Within the **produce** department, compare the reorder rates of the top 10 aisles. Which produce aisles are most "sticky" (highest reorder rate), and what business implication does this carry for a loyalty programme?

**B3.** The `add_to_cart_order` column records where in the shopping session each item was added. Compute the average cart position for each department. Which departments are added first, and which are added last? Propose a hypothesis about customer shopping intent that explains this ordering.

**B4.** Select any one department and perform a deep-dive: show its top 10 products, its reorder rate trend, and its peak ordering hours. Write a brief profile of the "typical buyer" in this department.

---

### Part C — Customer Segmentation

**C1.** Build a customer-level summary table with the following features per user:
- Total number of orders placed
- Average basket size (items per order)
- Overall reorder rate
- Average days between orders

Report the mean, median, and standard deviation of each feature.

**C2.** Segment customers into at least three groups based on **order frequency** (total orders placed). Label each group (e.g., Light, Regular, Frequent, Power). For each segment, report the average basket size, reorder rate, and average days between orders. What are the defining behavioural characteristics of each segment?

**C3.** Visualise how a customer's reorder rate changes as a function of `order_number` (their nth order). Does loyalty — as measured by reorder rate — increase with tenure? At what order number does the trend stabilise?

**C4.** Identify the top 3 departments by share of orders for each customer segment. How do product preferences differ between Light and Power users? What does this imply for how Instacart should communicate with each segment?

---

### Part D — Market Basket Analysis

**D1.** Find the 10 most frequently co-purchased product pairs (products that appear together in the same order). For each pair, report the co-occurrence count and suggest a practical cross-selling or placement strategy.

**D2.** Using the Apriori algorithm (`mlxtend`), mine association rules from the dataset at the **aisle** level (each order is a basket of aisles). Filter to rules with **support ≥ 0.05** and **confidence ≥ 0.4**. Report the top 5 rules by lift and explain what each rule means in plain language.

**D3.** Choose one high-lift rule from D2. Describe a concrete promotional tactic (e.g., a bundled discount, a recommendation widget, a targeted push notification) that Instacart could implement to capitalise on this product affinity.

---

### Part E — Critical Evaluation

**E1.** This dataset is described by Instacart as "a heavily biased subset" of its production data. Identify at least **three specific ways** the sample may be biased and explain how each bias could lead an analyst to draw incorrect conclusions.

**E2.** The dataset contains no pricing, demographic, or geographic information. Pick **two business questions** that you were unable to fully answer because of these absences. For each, describe what additional data you would need and how you would collect or acquire it.

**E3.** Suppose Instacart wanted to use the insights from this case study to redesign their homepage for first-time users. What analytical findings from your work are directly applicable, and what additional research (e.g., A/B testing, user surveys) would be required before making the change?

---

## 7. Grading Rubric

| Component | Weight | Criteria |
|-----------|--------|---------|
| Code quality & reproducibility | 20% | Notebook runs top-to-bottom without errors; code is readable and well-commented |
| Analytical depth | 30% | Questions are answered completely; methodology is sound and clearly explained |
| Visualisation quality | 20% | Charts are appropriately chosen, labelled, and support the written interpretation |
| Written interpretation | 20% | Insights are accurate, clearly expressed, and grounded in evidence from the data |
| Business memo | 10% | Findings are synthesised into actionable recommendations with a clear executive audience in mind |

---

## 8. Useful References

- Instacart blog post: [3 Million Instacart Orders, Open Sourced](https://tech.instacart.com/3-million-instacart-orders-open-sourced-d40d29ead6f2)
- pandas documentation: https://pandas.pydata.org/docs/
- Plotly Express documentation: https://plotly.com/python/plotly-express/
- mlxtend — Frequent Patterns: https://rasbt.github.io/mlxtend/user_guide/frequent_patterns/
- Han, J., Pei, J., & Kamber, M. (2011). *Data Mining: Concepts and Techniques* (3rd ed.) — Chapter 6: Mining Frequent Patterns, Associations, and Correlations.
- Fader, P., Hardie, B., & Lee, K. (2005). "RFM and CLV: Using Iso-value Curves for Customer Base Analysis." *Journal of Marketing Research*, 42(4), 415–430.
