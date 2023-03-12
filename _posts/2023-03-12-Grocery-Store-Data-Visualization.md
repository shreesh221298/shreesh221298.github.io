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
- [01. Code](#data-overview)

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

