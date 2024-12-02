# Hospital Supply Chain Inventory Optimization

## Table of Contents

1. [Project Overview](#project-overview)
2. [Data Sources](#data-sources)
3. [Tools](#tools)
4. [Data Cleaning/Preparation](#data-cleaningpreparation)
5. [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
6. [Data Analysis](#data-analysis)
7. [Results/Findings](#resultsfindings)
8. [Recommendations](#recommendations)


### Project Overview

This project analyzes hospital supply chain inventory management using data analytics to optimize stock levels, reduce waste, and ensure efficient restocking. By evaluating inventory data, vendor performance, and usage trends, we provide actionable insights to help hospitals maintain optimal inventory levels while minimizing costs.

### Data Sources 

**Dataset Source:**

The primary dataset used for this analysis is the **"inventory_data.csv"** [Download inventory_data.csv](https://github.com/rawatabhinav368/Hospital-Supply-Chain-Inventory-Optimization/blob/main/inventory_data.csv) file, which was downloaded from Kaggle's [Hospital Supply Chain Dataset](https://www.kaggle.com/datasets/vanpatangan/hospital-supply-chain).  This dataset contains detailed information about hospital inventory, including the following features:

- **Item_ID**: Unique identifier for each inventory item.
- **Item_Type**: The category or type of item (e.g., Medication, Surgical, Consumables, etc.).
- **Stock_Level**: The current quantity of the item in stock.
- **Usage_Rate**: The rate at which the item is consumed.
- **Cost**: The cost per unit of the item.
- **Vendor**: The supplier or vendor from which the item is sourced.
- **Lead_Time**: The time (in days) required to replenish the item after an order is placed.
- **Delivery_Time**: The actual time taken by the vendor to deliver the items.
- **Critical_Stock_Level**: The threshold below which stock is considered critical and requires reordering.

For more information, visit the Kaggle dataset page [here](https://www.kaggle.com/datasets/vanpatangan/hospital-supply-chain).


### Tools

1. **Python**: 
    Libraries Used: - Pandas , Numpy, Matplotlib, Seaborn, Plotly, Warnings.
  
2. **Tableau**: 
   - Tableau was used for data visualization. It helped in creating interactive dashboards to analyze hospital supply chain data, monitor inventory levels, and identify trends in real-time.
   - ## Hospital Supply Chain Inventory Dashboard

Click the image below to view the dashboard:

[![Hospital Supply Chain Inventory Dashboard](https://github.com/rawatabhinav368/Hospital-Supply-Chain-Inventory-Optimization/blob/main/hospial%20supply%20chain%20inventory%20data%20analysis%20dashboard.png)](https://github.com/rawatabhinav368/Hospital-Supply-Chain-Inventory-Optimization/blob/main/hospial%20supply%20chain%20inventory%20data%20analysis%20dashboard.png)

3. **PowerPoint**:
   - PowerPoint was used for creating presentations and communicating the insights derived from the analysis. It helped in effectively presenting findings and recommendations to stakeholders.
   - ## PowerPoint Presentation

You can download and view the PowerPoint presentation for this analysis using the link below:

[Download Hospital Supply Chain Inventory Data Analysis Presentation](https://github.com/rawatabhinav368/Hospital-Supply-Chain-Inventory-Optimization/blob/main/hospial%20supply%20chain%20inventory%20data%20analysis.pptx)

  ### Data Cleaning/Preparation

  In the initital data preparation pharase, we preformed the following tasks:
  
  1. Data Loading and inspection 
  
  2. Checking for missing values 

  ### Exploratory data analysis (EDA)
 
  EDA involves exploring the inventory data to answer key questions, such as:

  - Which inventory categories have higher turnover rates?
  - What items are prone to overstock or understock issues?
  - Which vendors are most dependent, and what are their lead times and costs?
  - How do monthly stock levels and daily usage trends vary?
  - What insights emerge from stock trends, vendor proportions, and correlations?
  - What are the optimal EOQ, safety stock, and replenishment strategies?

### Data Analysis 

Including some intresting code/features worked with 

```python
# Calculate inventory turnover
inventory_data['Inventory_Turnover'] = inventory_data['Avg_Usage_Per_Day'] / inventory_data['Current_Stock']

# Calculating stock differences
inventory_data['Stock_Difference'] = inventory_data['Current_Stock'] - inventory_data['Min_Required']
inventory_data['Overstock'] = inventory_data['Stock_Difference'].apply(lambda x: x if x > 0 else 0)
inventory_data['Understock'] = inventory_data['Stock_Difference'].apply(lambda x: -x if x < 0 else 0)

#Average Restock Lead Time by Vendor
vendor_lead_time = inventory_data.groupby('Vendor_ID')['Restock_Lead_Time'].mean().reset_index()


# Calculating monthly averages of stock and usage per day
monthly_data = inventory_data.groupby(['Year', 'Month']).agg({
    'Current_Stock': 'mean',  # Average current stock for the month
    'Avg_Usage_Per_Day': 'mean'  # Average usage per day for the month
}).reset_index()

# Economic Order Quantity (EOQ)
# Define the parameters
ordering_cost = 500 
holding_cost_per_unit = 2  

# Calculating EOQ for each item (using annual demand)
inventory_data['EOQ'] = np.sqrt((2 * inventory_data['Avg_Usage_Per_Day'] * 365 * ordering_cost) / holding_cost_per_unit)
inventory_data[['Item_Name', 'EOQ']].sort_values(by='EOQ', ascending=False)

# Safety Stock Calculation
z_score = 1.96  # For 95% service level
inventory_data['Std_Dev_Usage'] = inventory_data.groupby('Item_Name')['Avg_Usage_Per_Day'].transform('std')
inventory_data['Safety_Stock'] = z_score * inventory_data['Std_Dev_Usage'] * np.sqrt(inventory_data['Restock_Lead_Time'])
inventory_data[['Item_Name', 'Safety_Stock']].sort_values(by='Safety_Stock', ascending=False)

```

### Results/Findings

The analysis results are summarized as follows:
* *Consumables* have high turnover rates and are prone to overstocking, requiring precise management.
* *Equipment* demand is stable, with lower turnover rates.
* *Vendor V001* accounts for the largest share of inventory but has longer lead times and higher costs, while *V002* and *V003* offer shorter lead times and cost-efficiency, making them preferable for critical supplies.
* *Monthly stock fluctuations* indicate variability in planning, while daily usage remains consistent.
* *EOQ* and *safety stock* calculations highlight opportunities to optimize costs and reduce stockout risks.
* Strategic inventory management is essential to balance stock availability, cost efficiency, and vendor reliability.

  ## Recommendations
* *Improve Consumables Management*: Regularly adjust inventory levels based on demand patterns and maintain safety stock to prevent stockouts.
* *Diversify Vendors*: Reduce reliance on Vendor V001 by reallocating critical supplies to other vendors and establishing backup suppliers.
* *Optimize Inventory Costs*: Use EOQ to balance holding and ordering costs and minimize overstocking.
* *Enhance Vendor Contracts*: Negotiate for shorter lead times and reliable performance benchmarks.
