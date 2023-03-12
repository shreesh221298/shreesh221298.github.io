---
layout: post
title: Grocery store data Visualization to improve business
image: "/posts/Grocery Store Data Visualization.jpg"
tags: [Data Visualization, Python, Business Analysis]
---

ACME Corp is a mock grocery store client that needs a data scientist to use its data and recommend ways to improve the business

# Table of contents

- [00. Project Overview](#overview-main)
    - [Data](#overview-data)
    - [Actions](#overview-actions)
    - [Results](#overview-results)
    - [Next Steps](#overview-growth)
    - [Key Definition](#overview-definition)
- [01. Python Code](#python-code)

___

# Project Overview  <a name="overview-main"></a>

### Data <a name="overview-context"></a>
The data provided was from ACME Corp, a Canadian grocery store chain. There were 12 CSV files in total: 10 trans_fact files, 1 location file and 1 product file. There may be missing sales and units in some trans_fact files that need to be replaced with zero value.

<br>
![alt text](/img/posts/Data.png "Data")
<br>
### Actions <a name="overview-actions"></a>
Objective: Analyze ACME Corp’s data and recommend actions to improve performance

Step 1: Ingest & Clean the Data
Ingest and prepare the data by combining the 10 trans_fact files and replacing any null sales and null units with zero.
Step 2: Transform the data in memory 
i.	which provinces and stores are performing well and 
ii.	how much are the top stores in each province performing compared with the average store of the province.
Step 3: Transform the data in memory 
iii.	how customers in the loyalty program are performing compared to non-loyalty customers and 
iv.	what category of products is contributing to most of ACME’s sales
<br>
<br>

### Results <a name="overview-results"></a>
Top Performing Provinces

Top Performers: Alberta, Ontario
Below Average: British Columbia, Saskatchewan

<br>
![alt text](/img/posts/Top performing Provinces.png "Top Performing Provinces")

<br>
Recommendations: 
* Build a loyal customer base to maintain sales in high performing provinces
* Introduce deals and discounts in below par provinces to boost sales and build customer loyalty


<br>
Top Performing Stores

Observation: 80% of sales are coming from 5 out of the 47 stores in the country

<br>
![alt text](/img/posts/Top Performing Stores.png "Top Performing Stores")
<br>
Recommendation: 
* Conduct surveys and get feedback from customers, to understand the strengths and weaknesses of various stores

* Determine steps for improving sales, based on customer feedback from surveys

<br>
Top Store vs Average Store in the Province

Observation: There are 9 stores performing above provincial average out of the 47 stores in the country.
<br>
![alt text](/img/posts/Provincial Average.png "Top Store vs. Average Store ")

<br>
Recommendation:
* Build an employee incentive program for stores that perform above average to maintain store performance and employee satisfaction in the top performing stores and motivate employees in other stores to perform better

<br>

Loyalty vs Non-Loyalty Customers

Observation: 82% sales are coming from non-loyalty customers. Hence, there is lot of room for building a loyal customer base

<br>
![alt text](/img/posts/Customer sales distribution.png "Customer Sales Distribution")

<br>

Recommendation: 
* Constantly update rewards program to maintain customer satisfaction

* Incentivize customers to join the loyalty program

* Use machine learning to understand customer behaviour and initiate customer suited deals to build a loyal customer base

Top Performing Product Category
Observation: The sales distribution is shown in the graph below
<br>
![alt text](/img/posts/Product category sales.png "Product Category Sales")

<br>
Recommendation: 
* Ensure product quality and customer service on top categories to maintain sales
* Review pricing and experiment with new brands in below par products
### Next Steps <a name="overview-growth"></a>

* Determine growth in sales after implementing the mentioned changes

* Analyze customer feedback surveys to understand strengths and weaknesses of the business

* Research customer behavior to increase spending in loyal customers


<br>
<br>
### Key Definition  <a name="overview-definition"></a>

The *loyalty score* metric measures the % of grocery spend (market level) that each customer allocates to the client vs. all of the competitors.  

Example 1: Customer X has a total grocery spend of $100 and all of this is spent with our client. Customer X has a *loyalty score* of 1.0

Example 2: Customer Y has a total grocery spend of $200 but only 20% is spent with our client.  The remaining 80% is spend with competitors.  Customer Y has a *customer loyalty score* of 0.2
<br>
<br>
___

# Python Code  <a name="python-code"></a>

I have used python to visualize the grocery store data. Here’s the code for my visualizations. 
```python
#####Analytics Engineer Technical Test#####
#Import Packages
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib.gridspec import GridSpec
import seaborn as sns

###STEP 1: Ingest and Clean Data###
#Extracting customer transactions files
trans_fact_1 = pd.read_csv("trans_fact_1.csv")
trans_fact_2 = pd.read_csv("trans_fact_2.csv")
trans_fact_3 = pd.read_csv("trans_fact_3.csv")
trans_fact_4 = pd.read_csv("trans_fact_4.csv")
trans_fact_5 = pd.read_csv("trans_fact_5.csv")
trans_fact_6 = pd.read_csv("trans_fact_6.csv")
trans_fact_7 = pd.read_csv("trans_fact_7.csv")
trans_fact_8 = pd.read_csv("trans_fact_8.csv")
trans_fact_9 = pd.read_csv("trans_fact_9.csv")
trans_fact_10 = pd.read_csv("trans_fact_10.csv")
In [3]:

#Combining Files
grocery_transactions_data = pd.concat([trans_fact_1,trans_fact_2,trans_fact_3,trans_fact_4,
                                       trans_fact_5,trans_fact_6,trans_fact_7,trans_fact_8,
                                       trans_fact_9,trans_fact_10], axis = 0)

grocery_transactions_data.isna().sum()

#Replacing Null values with 0
grocery_transactions_data["sales"].fillna(value = 0, inplace=True)
grocery_transactions_data["units"].fillna(value = 0, inplace=True)
grocery_transactions_data.isna().sum()


grocery_transactions_data[['sales', 'units']].describe()


###STEP 2: Top Stores and provinces###
grocery_transactions_data['trans_dt'] = pd.to_datetime(grocery_transactions_data['trans_dt'])

#Extracting location data and merging with customer transactions data
location_data=pd.read_csv("location.csv")
data_merged=pd.merge(grocery_transactions_data,location_data, how="left", on="store_location_id")
data_merged.drop(['region', 'postal_code','banner'], axis = 1,  inplace =True)
data_merged.isna().sum()

#Checking Number of Provinces
provinces = data_merged['province'].unique()
print(provinces)


#Total Sales calculation by Province
provincial_total_sales = data_merged.groupby('province').agg({
                                                       "sales":"sum",
                                                       "units":"sum",
                                                       "transaction_id": "count"}).reset_index()

provincial_total_sales.columns=["province","total_sales", "total_items_sold", "transaction_count"]
provincial_total_sales.sort_values(by = ["total_sales"], inplace = True, ascending =False)

#Bar Chart depicting total sales by Province
plt.figure(figsize=(8,5))
plt.minorticks_on()
plt.bar(provincial_total_sales["province"],provincial_total_sales["total_sales"]/10**6)
plt.title("TOTAL SALES IN EACH PROVINCE", fontsize=16)
plt.ylabel("Sales (Million $)", fontsize=16)
plt.xlabel("Provinces",fontsize=16)
plt.grid(axis='y')
plt.grid(which='minor', axis='y', linewidth=0.4)
plt.show()

#Pie chart showing sales distribution by province
plt.figure(1, figsize=(20,10)) 
the_grid = GridSpec(2, 2)

cmap = plt.get_cmap('Spectral')
colors = [cmap(i) for i in np.linspace(0, 1, 8)]

plt.subplot(the_grid[0, 1], aspect=1, title='Provincial Sales Distribution')
type_show_ids = plt.pie(provincial_total_sales['total_sales'], labels=provincial_total_sales["province"],
                        autopct='%1.1f%%', shadow=True, colors=colors)
plt.show()

#Storewise sales in Canada
Canada_sales_summary = data_merged.groupby(['province','city','store_location_id']).agg({
                                                       "sales":"sum",
                                                       "units":"sum",
                                                       "transaction_id": "count"}).reset_index()
Canada_sales_summary.columns=["province","city","store_location_id","total_sales", "total_items_sold", "transaction_count"]
Canada_sales_summary.sort_values(by = ["total_sales"], inplace = True, ascending =False)
Canada_sales_summary.head()

#Pie chart showing store wise sales distribution
plt.figure(1, figsize=(20,10)) 
the_grid = GridSpec(2, 2)

cmap = plt.get_cmap('Spectral')
colors = [cmap(i) for i in np.linspace(0, 1, 8)]

plt.subplot(the_grid[0, 1], aspect=1, title='Store Wise Sales Distribution')
type_show_ids = plt.pie(Canada_sales_summary['total_sales'], labels=Canada_sales_summary['store_location_id'], 
                        autopct='%1.1f%%', shadow=True, colors=colors)
plt.show()

#Sales data by province and stores
province_data = data_merged.groupby(['province', 'store_location_id']).agg({ "sales":"sum",
                                                                            "units":"sum",
                                                                            "transaction_id": "count"}).reset_index()
province_data.columns=["province", "store_location_id","total_sales", "total_items_sold", "transaction_count"]


#Box plot for sales distribution by province
plt.figure(figsize=(10,5))
sns.boxplot(x=province_data['province'], y=province_data['total_sales']/10**6, showfliers=False, showmeans=True, 
            meanprops={"marker":"+",
                       "markerfacecolor":"white", 
                       "markeredgecolor":"black",
                       "markersize":"7"})
plt.ylabel("Sales in (Million $)", fontsize=16)
plt.xlabel("Provinces",fontsize=16)
plt.title("Sales Distribution by Province", fontsize=16)
plt.minorticks_on()
plt.grid(axis='y')
plt.grid(which='minor', axis='y', linewidth=0.4)
plt.show()


#Provincial Statistics
province_data_stats = province_data.groupby(province_data['province']).agg(
                                            max_sales = ('total_sales', lambda x: x.max()),
                                            mean_sales = ('total_sales', lambda x: x.mean()),
                                            max_items = ('total_items_sold', lambda x: x.max()),
                                            mean_items = ('total_items_sold', lambda x: x.mean()), 
                                            max_transactions = ('transaction_count', lambda x: x.max()),
                                            mean_transactions = ('transaction_count', lambda x: x.mean())).reset_index()
province_data_stats


#Chart to compare max and mean sales in province
plt.figure(figsize=(10,5))
plt.bar(province_data_stats.province, province_data_stats.max_sales/10**6)
plt.plot(province_data_stats.province, province_data_stats.mean_sales/10**6, label='mean', marker='o', color='orange')
plt.ylabel("Sales (Million $)", fontsize=16)
plt.xlabel("Provinces",fontsize=16)
plt.title("Comparison of Max and Mean sales per store)", fontsize=16)
plt.legend(loc=0)
plt.minorticks_on()
plt.grid()
plt.grid(which='minor', axis='y', linewidth=0.4)
plt.show()


###STEP 3: Loyal customers vs Non-loyal customers & Top product categories ###
products = pd.read_csv("product.csv")
product_sales_data=pd.merge(grocery_transactions_data,products, how="inner", on="product_id")
product_sales_data.drop(['sku','upc','item_name','item_description','department'], axis = 1,  inplace =True)
product_sales_data.isna().sum()


#Loyal Customers data
loyal_customers = grocery_transactions_data[grocery_transactions_data['customer_id'] > 0]
loyal_sales_summary=loyal_customers.groupby('transaction_id').agg({
                                                       "sales":"sum",
                                                       "units":"sum"
                                                       }).reset_index()

loyal_sales_summary.columns=["transaction_id","total_sales", "total_items_sold"]

#Chronological loyal customer data
chronological_loyal_sales = loyal_customers.groupby('trans_dt').agg({
                                                       "sales":"sum",
                                                       "units":"sum"
                                                       }).reset_index()

chronological_loyal_sales.columns=["date","total_sales", "total_items_sold"]
chronological_loyal_sales.sort_values(by=["date"],inplace = True, ascending = True)


#Daily Loyal customer sales chart
plt.figure(figsize=(12,6))
plt.grid()
plt.plot(chronological_loyal_sales["date"],chronological_loyal_sales["total_sales"])
plt.title('DAILY LOYAL CUSTOMER SALES', fontsize=16)
plt.xlabel('Date', fontsize=16)
plt.ylabel('Sales in $', fontsize=16)
plt.show()


#Loyal Customers data
non_loyal_customers = grocery_transactions_data[grocery_transactions_data['customer_id'] < 0]
non_loyal_sales_summary=non_loyal_customers.groupby('transaction_id').agg({
                                                       "sales":"sum",
                                                       "units":"sum"
                                                       }).reset_index()

non_loyal_sales_summary.columns=["transaction_id","total_sales", "total_items_sold"]

#Chronological loyal customer data
chronological_non_loyal_sales = non_loyal_customers.groupby('trans_dt').agg({
                                                       "sales":"sum",
                                                       "units":"sum"
                                                       }).reset_index()

chronological_non_loyal_sales.columns=["date","total_sales", "total_items_sold"]
chronological_non_loyal_sales.sort_values(by=["date"],inplace = True, ascending = True)


#Daily Non Loyal customer sales chart
plt.figure(figsize=(12,6))
plt.grid()
plt.plot(chronological_non_loyal_sales["date"],chronological_non_loyal_sales["total_sales"])
plt.title('DAILY NON LOYAL CUSTOMER SALES', fontsize=16)
plt.xlabel('Date', fontsize=16)
plt.ylabel('Sales in $', fontsize=16)
plt.show()

#Bar chart for Loyal vs Non Loyal customer sales 
plt.figure(figsize=(8,5))
plt.minorticks_on()
plt.bar(["Loyal Customers","Non-Loyal Customers"],[chronological_loyal_sales["total_sales"].sum()/10**6,
                                                   chronological_non_loyal_sales["total_sales"].sum()/10**6])
plt.title("Loyal vs Non Loyal Customer Sales", fontsize = 16)
plt.ylabel("Sales (Million $)", fontsize = 16)
plt.grid(axis='y')
plt.grid(which='minor', axis='y', linewidth=0.4)
plt.show()


#Pie chart for customer sales distribution
plt.figure(1, figsize=(20,10)) 
the_grid = GridSpec(2, 2)

cmap = plt.get_cmap('Spectral')
colors = [cmap(i) for i in np.linspace(0, 1, 8)]

plt.subplot(the_grid[0, 1], aspect=1, title='Customer Sales Distribution')
type_show_ids = plt.pie([chronological_loyal_sales["total_sales"].sum(),
                         chronological_non_loyal_sales["total_sales"].sum()], 
                        labels=["Loyal Customers","Non-Loyal Customers"], autopct='%1.1f%%', shadow=True, colors=colors)
plt.show()


#Category sales data
categories = product_sales_data["category"].unique()
category_sales_summary=product_sales_data.groupby('category').agg({
                                                       "sales":"sum",
                                                       "units":"sum"
                                                       }).reset_index()

category_sales_summary.columns=["category","total_sales", "total_items_sold"]


#Product Categories Bar Chart
plt.figure(figsize=(10,25))
category_sales_summary.sort_values(by=['total_sales'], ascending=[False], inplace=True)
sns.barplot(data=category_sales_summary, y='category', x='total_sales')
plt.ylabel("Product Category ID", fontsize=16)
plt.xlabel("Total Sales ($)",fontsize=16)
plt.minorticks_on()
plt.grid(axis='x')
plt.grid(which='minor', axis='x', linewidth=0.4)

```

![image](https://user-images.githubusercontent.com/47675756/224532638-65ee7ec2-0d88-435f-be76-43b2a5ff5917.png)

