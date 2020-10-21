## Heroes of Pymoli

![Fantasy](Images/Fantasy.png)

## Background
Congratulations! After a lot of hard work in the data munging mines, you've landed a job as Lead Analyst for an independent gaming company. You've been assigned the task of analyzing the data for their most recent fantasy game Heroes of Pymoli.

Like many others in its genre, the game is free-to-play, but players are encouraged to purchase optional items that enhance their playing experience. As a first task, the company would like you to generate a report that breaks down the game's purchasing data into meaningful insights.

## Observable Trends
* The most apparent trend in the data is the difference in gender percentages. In the Pymoli data, 84.03% of the users are male, while 14.06% are female.
* If one examines the Purchasing Analysis (Gender) data frame, men make 82.68% of the total purchases while women make 15.21%. What is significant about this is that women purchase 15.21% of the total while only making up 14.06% of the game population. Women are spending more than men.
* In the Purchasing Analysis (Age) data frame, we find that the 20-24-year-old age bracket is the largest but does not spend the most. The age group that purchases the most is the 35-39-year-old group; this group spends $4.76 per person. I would expect an older age bracket to buy more as they likely have more disposable income. Finally, the most surprising piece of data is the <10-year-old group comes in second with $4.54; this may be an artifact of the small sample size, though.
## Requirements, Code and Results
Your final report should include each of the following:

### Player Count
* Imports and read the csv file
```python
# Dependencies and Setup
import pandas as pd

# File to Load (Remember to Change These)
file_to_load = "Resources/purchase_data.csv"

# Read Purchasing File and store into Pandas data frame
purchase_data_df = pd.read_csv(file_to_load)
```
* Total Number of Players
```python
# Find the number of screen names "SN", for player count
player_count = len(purchase_data_df["SN"].unique())
# print(player_count)

# Create a data frame
player_count_df = pd.DataFrame({"Total Players": [player_count]})
player_count_df
```
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>576</td>
    </tr>
  </tbody>
</table>
</div>

### Purchasing Analysis (Total)

* Number of Unique Items
* Average Purchase Price
* Total Number of Purchases
* Total Revenue
```python
# Get the stats
count_unique_items_col = len(purchase_data_df["Item ID"].unique())
average_price_col = purchase_data_df["Price"].mean()
count_purchases_col = purchase_data_df["Purchase ID"].count()
total_revenue_col = purchase_data_df["Price"].sum()

# create data frame with specific columns
summary_df = pd.DataFrame({"Number of Unique Items": [count_unique_items_col],
                           "Average Price": [average_price_col], 
                           "Number of Purchases": [count_purchases_col], 
                           "Total Revenue": [total_revenue_col]
                         })

# format as currency
summary_df.style.format({"Average Price": '${:,.2f}',
                         "Total Revenue": '${:,.2f}'
                       })
```
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>179</td>
      <td>$3.05</td>
      <td>780</td>
      <td>$2,379.77</td>
    </tr>
  </tbody>
</table>
</div>

### Gender Demographics

* Percentage and Count of Male Players
* Percentage and Count of Female Players
* Percentage and Count of Other / Non-Disclosed
```python
# get a gender data frame group
gender_dfg = purchase_data_df.groupby("Gender")

# Get the stats
total_gender_dfu = gender_dfg.nunique()["SN"]
percent_of_players_dfu = total_gender_dfu / player_count * 100

# Create data frame
gender_demo_df = pd.DataFrame({"Total Count": total_gender_dfu, "Percentage of Players": percent_of_players_dfu})
gender_demo_df.index.name = None

# Format the values sorted by total count in descending order, and two decimal places for the percentage
gender_demo_df.sort_values(["Total Count"], ascending = False).style.format(
                            {"Percentage of Players":"{:.2f}%"})
```
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: right;">
      <th>Male</th>
      <td>484</td>
      <td>84.03%</td>
    </tr>
    <tr style="text-align: right;">
      <th>Female</th>
      <td>81</td>
      <td>14.06%</td>
    </tr>
    <tr style="text-align: right;">
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>1.91%</td>
    </tr>
  </tbody>
</table>
</div>

### Purchasing Analysis (Gender)
* The below each broken by gender
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value
  * Average Purchase Total per Person by Gender
```python
# Get the stats from the gender data frame 
purchase_count_col = gender_dfg["Purchase ID"].count()
avg_purchase_price_col = gender_dfg["Price"].mean()
avg_purchase_total_col = gender_dfg["Price"].sum()
avg_purchase_per_person_col = avg_purchase_total_col / total_gender_dfu

# Create data frame with the values above 
gender_sum_df = pd.DataFrame({
                               "Purchase Count": purchase_count_col, 
                               "Average Purchase Price": avg_purchase_price_col,
                               "Average Purchase Value":avg_purchase_total_col,
                               "Avg Purchase Total per Person": avg_purchase_per_person_col
                             })

# Make index in top left "Gender"
gender_sum_df.index.name = "Gender"

# Format with currency style
gender_sum_df.style.format({
                                "Average Purchase Price": "${:,.2f}",
                                "Average Purchase Value": "${:,.2f}",
                                "Avg Purchase Total per Person": "${:,.2f}"
                            })
```
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Average Purchase Value</th>
      <th>Avg Purchase Total per Person</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: right;">
      <th>Female</th>
      <td>113</td>
      <td>$3.20</td>
      <td>$361.94</td>
      <td>$4.47</td>
    </tr>
    <tr style="text-align: right;">
      <th>Male</th>
      <td>652</td>
      <td>$3.02</td>
      <td>$1967.64</td>
      <td>$4.07</td>
    </tr>
    <tr style="text-align: right;">
      <th>Other / Non-Disclosed</th>
      <td>15</td>
      <td>$3.35</td>
      <td>$50.19</td>
      <td>$4.56</td>
    </tr>
  </tbody>
</table>
<div>

### Age Demographics

* The below each broken into bins of 4 years (i.e. &lt;10, 10-14, 15-19, etc.)
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value
  * Average Purchase Total per Person by Age Group
```python
# Create age bins
age_bins = [0, 9, 14, 19, 24, 29, 34, 39, 999]
age_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

# Create the "Age Group" Index
purchase_data_df["Age Group"] = pd.cut(purchase_data_df["Age"], age_bins, labels=age_names)

# Create new data frame group
ages_dfg = purchase_data_df.groupby("Age Group")

# Get the stats
total_age_count_col = ages_dfg["SN"].nunique()
age_bin_percent_col = total_age_count_col / player_count * 100

# Create the dataframe
age_summary_df = pd.DataFrame({"Total Count": total_age_count_col, "Percentage of Players": age_bin_percent_col})
age_summary_df.index.name = None
age_summary_df.style.format({"Percentage of Players": "{:.2f}%"})
```
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: right;">
      <th>&lt;10</th>
      <td>17</td>
      <td>2.95%</td>
    </tr>
    <tr style="text-align: right;">
      <th>10-14</th>
      <td>22</td>
      <td>3.82%</td>
    </tr>
    <tr style="text-align: right;">
      <th>15-19</th>
      <td>107</td>
      <td>18.58%</td>
    </tr>
    <tr style="text-align: right;">
      <th>20-24</th>
      <td>258</td>
      <td>44.79%</td>
    </tr>
    <tr style="text-align: right;">
      <th>25-29</th>
      <td>77</td>
      <td>13.37%</td>
    </tr>
    <tr style="text-align: right;">
      <th>30-34</th>
      <td>52</td>
      <td>9.03%</td>
    </tr>
    <tr style="text-align: right;">
      <th>35-39</th>
      <td>31</td>
      <td>5.38%</td>
    </tr>
    <tr style="text-align: right;">
      <th>40+</th>
      <td>12</td>
      <td>2.08%</td>
    </tr>
  </tbody>
</table>
</div>

### Purchasing Analysis (Age)

* Bin the purchase_data data frame by age
* Run basic calculations to obtain purchase count, avg. purchase price, avg. * * * purchase total per person etc. in the table below
* Create a summary data frame to hold the results
* Optional: give the displayed data cleaner formatting
* Display the summary data frame
```python
# Create new data frame group
purchase_analysis_dfg = purchase_data_df.groupby("Age Group")

# Get the stats
purchase_analysis_count_col = purchase_analysis_dfg["Purchase ID"].nunique()
purchase_analysis_avg_pp_col = purchase_analysis_dfg["Price"].mean()
purchase_analysis_tot_pp_col = purchase_analysis_dfg["Price"].sum()
purchase_analysis_avg_tot_pp_col = purchase_analysis_tot_pp_col / total_age_count_col

# Create the dataframe
age_analysis_df = pd.DataFrame({
                                "Purchase Count": purchase_analysis_count_col, 
                                "Average Purchase Price": purchase_analysis_avg_pp_col,
                                "Total Purchase Value": purchase_analysis_tot_pp_col,
                                "Avg Total Purchase per Person": purchase_analysis_avg_tot_pp_col
                              })

# Print the new dataframe
age_analysis_df.index.name = None
age_analysis_df.style.format({
                                "Average Purchase Price": "${:,.2f}",
                                "Total Purchase Value": "${:,.2f}",
                                "Avg Total Purchase per Person": "${:,.2f}"
                             })
```
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Avg Total Purchase per Person</th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: right;">
      <th>&lt;10</th>
      <td>23</td>
      <td>$3.35</td>
      <td>$77.13</td>
      <td>$4.54</td>
    </tr>
    <tr style="text-align: right;">
      <th>10-14</th>
      <td>28</td>
      <td>$2.96</td>
      <td>$82.78</td>
      <td>$3.76</td>
    </tr>
    <tr style="text-align: right;">
      <th>15-19</th>
      <td>136</td>
      <td>$3.04</td>
      <td>$412.89</td>
      <td>$3.86</td>
    </tr>
    <tr style="text-align: right;">
      <th>20-24</th>
      <td>365</td>
      <td>$3.05</td>
      <td>$1,114.06</td>
      <td>$4.32</td>
    </tr>
    <tr style="text-align: right;">
      <th>25-29</th>
      <td>101</td>
      <td>$2.90</td>
      <td>$293.00</td>
      <td>$3.81</td>
    </tr>
    <tr style="text-align: right;">
      <th>30-34</th>
      <td>73</td>
      <td>$2.93</td>
      <td>$214.00</td>
      <td>$4.12</td>
    </tr>
    <tr style="text-align: right;">
      <th>35-39</th>
      <td>41</td>
      <td>$3.60</td>
      <td>$147.67</td>
      <td>$4.76</td>
    </tr>
    <tr style="text-align: right;">
      <th>40+</th>
      <td>13</td>
      <td>$2.94</td>
      <td>$38.24</td>
      <td>$3.19</td>
    </tr>
  </tbody>
</table>
</div>

### Top Spenders

* Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
  * SN
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value
```python
# Create new data frame group
names_dfg = purchase_data_df.groupby("SN")

# Get the stats
top_purchase_count_col = names_dfg["Purchase ID"].count()
top_average_pp_col = names_dfg["Price"].mean()
top_total_purchase_value_col = names_dfg["Price"].sum()

# Create data frame with the values above 
top_sum_df = pd.DataFrame({
                            "Purchase Count": top_purchase_count_col, 
                            "Average Purchase Price": top_average_pp_col,
                            "Total Purchase Value": top_total_purchase_value_col
                          })

# Format with currency style
top_sum_df.sort_values(['Total Purchase Value'], ascending=False).head().style.format({
      "Average Purchase Price": "${:,.2f}",
      "Total Purchase Value": "${:,.2f}"
      })
```
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: right;">
      <th>Lisosia93</th>
      <td>5</td>
      <td>$3.79</td>
      <td>$18.96</td>
    </tr>
    <tr style="text-align: right;">
      <th>Idastidru52</th>
      <td>4</td>
      <td>$3.86</td>
      <td>$15.45</td>
    </tr>
    <tr style="text-align: right;">
      <th>Chamjask73</th>
      <td>3</td>
      <td>$4.61</td>
      <td>$13.83</td>
    </tr>
    <tr style="text-align: right;">
      <th>Iral74</th>
      <td>4</td>
      <td>$3.40</td>
      <td>$13.62</td>
    </tr>
    <tr style="text-align: right;">
      <th>Iskadarya95</th>
      <td>3</td>
      <td>$4.37</td>
      <td>$13.10</td>
    </tr>
  </tbody>
</table>
</div>

### Most Popular Items

* Identify the 5 most popular items by purchase count, then list (in a table):
  * Item ID
  * Item Name
  * Purchase Count
  * Item Price
  * Total Purchase Value
```python
# Create new dataframe
items_df = purchase_data_df[['Item ID', 'Item Name', 'Price']]

# Create the dataframe group
items_dfg = items_df.groupby(['Item ID', 'Item Name'])

# Get the stats
mostpop_purchase_count_col = items_dfg["Item ID"].count()
mostpop_total_purchase_value_col = items_dfg["Price"].sum()
mostpop_total_avg_item_price_col = mostpop_total_purchase_value_col / mostpop_purchase_count_col

# Create data frame with the values above 
mostpop_sum_df = pd.DataFrame({
                               "Purchase Count": mostpop_purchase_count_col, 
                               "Item Price": mostpop_total_avg_item_price_col,
                               "Total Purchase Value": mostpop_total_purchase_value_col
                             })

# Format with currency style
mostpop_sum_df.sort_values(['Purchase Count'], ascending=False).head().style.format({"Item Price": "${:,.2f}", "Total Purchase Value": "${:,.2f}"})
```  
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr style="text-align: right;">
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: right;">
      <th>92</th>
      <th>Final Critic</th>
      <td>13</td>
      <td>$4.61</td>
      <td>$59.99</td>
    </tr>
    <tr style="text-align: right;">
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr style="text-align: right;">
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr style="text-align: right;">
      <th>132</th>
      <th>Persuasion</th>
      <td>9</td>
      <td>$3.22</td>
      <td>$28.99</td>
    </tr>
    <tr style="text-align: right;">
      <th>108</th>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <td>9</td>
      <td>$3.53</td>
      <td>$31.77</td>
    </tr>
  </tbody>
</table>
</div>

### Most Profitable Items
* Identify the 5 most profitable items by total purchase value, then list (in a table):
  * Item ID
  * Item Name
  * Purchase Count
  * Item Price
  * Total Purchase Value
```python
# Format with currency style 
mostpop_sum_df.sort_values(['Total Purchase Value'], ascending=False).head().style.format({"Item Price": "${:,.2f}", "Total Purchase Value": "${:,.2f}"})
```
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr style="text-align: right;">
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: right;">
      <th>92</th>
      <th>Final Critic</th>
      <td>13</td>
      <td>$4.61</td>
      <td>$59.99</td>
    </tr>
    <tr style="text-align: right;">
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr style="text-align: right;">
      <th>82</th>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr style="text-align: right;">
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr style="text-align: right;">
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>8</td>
      <td>$4.35</td>
      <td>$34.80</td>
    </tr>
  </tbody>
</table>
</div>
