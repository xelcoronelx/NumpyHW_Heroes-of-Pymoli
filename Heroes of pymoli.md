

```python
import pandas as pd
import numpy as np

```


```python
purchase_data= pd.read_json('purchase_data.json')
purchase_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
#need to add the bins for one of the excercises
# AGE  - BINS & LABELS
bins = [0, 9.90, 14.90, 19.90, 24.90, 29.90, 34.90, 39.90, 99999]
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

purchase_data["Age Ranges"] = pd.cut(purchase_data["Age"], bins, labels=group_names)
purchase_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
      <th>Age Ranges</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
      <td>35-39</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
      <td>30-34</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
      <td>20-24</td>
    </tr>
  </tbody>
</table>
</div>




```python
#total player count
unique_players = len(purchase_data['SN'].unique())
total_players = len(purchase_data['SN'])

print(unique_players)
print(total_players)
```

    573
    780



```python
#I assume you wanted the unique players so going forward I will use that
pd.DataFrame({'Total Players': [unique_players]})
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
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
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
demographics = purchase_data.loc[:,['Gender', 'SN','Age','Age Ranges']].drop_duplicates()

#demographics.head()
```


```python
# total purchasing analysis - gather stats
unique_items = len(purchase_data["Item ID"].unique())
average_purchase_price = purchase_data["Price"].mean()
purchases_count = purchase_data["Price"].count()
total_value = purchase_data["Price"].sum()



```


```python
# total purchasing analysis - creating the data frame

Purchasing_Summary_Total = pd.DataFrame({'Number of Unique Items': [unique_items],
                                        'Average Purchase Price': [average_purchase_price],                            
                                        'Total Number of Purchases': [purchases_count],
                                        'Total Revenue': [total_value]}).round(2)

```


```python
# total Purchasing analysis - cleaning the formats

Purchasing_Summary_Total = Purchasing_Summary_Total
Purchasing_Summary_Total['Average Purchase Price']= Purchasing_Summary_Total['Average Purchase Price'].map("${:,.2f}".format)
Purchasing_Summary_Total['Total Number of Purchases']= Purchasing_Summary_Total['Total Number of Purchases'].map("{:,}".format)
Purchasing_Summary_Total['Total Revenue']= Purchasing_Summary_Total['Total Revenue'].map("${:,.2f}".format)


```


```python
# total Purchasing analysis final output

Purchasing_Summary_Total = Purchasing_Summary_Total[['Number of Unique Items','Average Purchase Price', 'Total Number of Purchases', 'Total Revenue']]
Purchasing_Summary_Total
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Unique Items</th>
      <th>Average Purchase Price</th>
      <th>Total Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
#genders demographics

gender_total = demographics["Gender"].value_counts()
gender_percents = gender_total / unique_players * 100
gender_DF = pd.DataFrame({"Total Count": gender_total, "Percentage of Players": gender_percents}).round(2)

gender_DF

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
#gender purchase analysis - create variables
gender_purchases = purchase_data.groupby(["Gender"]).count()["Price"].rename("Purchase Count")
gender_average_amounts = purchase_data.groupby(["Gender"]).mean()["Price"].rename("Average Purchase Price")
gender_totals = purchase_data.groupby(["Gender"]).sum()["Price"].rename("Total Purchase Value")
normalized = gender_totals / gender_DF['Total Count']

```


```python
#gender purchase analysis - create data frame & cleaned it up
Gender_Purchase_analysis = pd.DataFrame({"Purchase Count": gender_purchases, "Average Purchase Price": gender_average_amounts, "Total Purchase Value": gender_totals, "Normalized Totals": normalized})

Gender_Purchase_analysis["Purchase Count"] = Gender_Purchase_analysis["Purchase Count"].map("{:,}".format)
Gender_Purchase_analysis["Average Purchase Price"] = Gender_Purchase_analysis["Average Purchase Price"].map("${:,.2f}".format)
Gender_Purchase_analysis["Total Purchase Value"] = Gender_Purchase_analysis["Total Purchase Value"].map("${:,.2f}".format)
Gender_Purchase_analysis["Normalized Totals"] = Gender_Purchase_analysis["Normalized Totals"].map("${:,.2f}".format)

#Gender_Purchase_analysis.head()
```


```python
#GENDER BREAKDOWN
Gender_Purchase_analysis = Gender_Purchase_analysis.loc[:,["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]
Gender_Purchase_analysis
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
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
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$3.83</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1,867.68</td>
      <td>$4.02</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$4.47</td>
    </tr>
  </tbody>
</table>
</div>




```python
#AGE demos and df. not asked to do this on instructions but saw it on
#example solutions
AGE_total = demographics["Age Ranges"].value_counts()
AGE_percents = AGE_total / unique_players * 100
AGE_DF1 = pd.DataFrame({"Total Count": AGE_total, "Percentage of Players": AGE_percents}).round(2)

#AGE STATS
AGE_purchases = purchase_data.groupby(["Age Ranges"]).count()["Price"].rename("Purchase Count")
AGE_average_amounts = purchase_data.groupby(["Age Ranges"]).mean()["Price"].rename("Average Purchase Price")
AGE_totals = purchase_data.groupby(["Age Ranges"]).sum()["Price"].rename("Total Purchase Value")
AGE_normalized = AGE_totals / AGE_DF1['Total Count']


```


```python
#age stats to df plus cleaning up format
AGE_DF2 = pd.DataFrame({"Purchase Count": AGE_purchases, "Average Purchase Price": AGE_average_amounts, "Total Purchase Value": AGE_totals, "Normalized Totals": AGE_normalized})

AGE_DF2["Purchase Count"] = AGE_DF2["Purchase Count"].map("{:,}".format)
AGE_DF2["Average Purchase Price"] = AGE_DF2["Average Purchase Price"].map("${:,.2f}".format)
AGE_DF2["Total Purchase Value"] = AGE_DF2["Total Purchase Value"].map("${:,.2f}".format)
AGE_DF2["Normalized Totals"] = AGE_DF2["Normalized Totals"].map("${:,.2f}".format)

```


```python
#AGE RESULTS
AGE_DF2= AGE_DF2.loc[:,["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]

AGE_DF2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10-14</th>
      <td>35</td>
      <td>$2.77</td>
      <td>$96.95</td>
      <td>$4.22</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
      <td>$3.78</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
      <td>$4.26</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>$4.20</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
      <td>$4.42</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>17</td>
      <td>$3.16</td>
      <td>$53.75</td>
      <td>$4.89</td>
    </tr>
    <tr>
      <th>&lt;10</th>
      <td>28</td>
      <td>$2.98</td>
      <td>$83.46</td>
      <td>$4.39</td>
    </tr>
  </tbody>
</table>
</div>




```python
#TOP spenders stats
Spender_purchases = purchase_data.groupby(["SN"]).count()["Price"].rename("Purchase Count")
Spender_average_amounts = purchase_data.groupby(["SN"]).mean()["Price"].rename("Average Purchase Price")
Spender_total = purchase_data.groupby(["SN"]).sum()["Price"].rename("Total Purchase Value")

```


```python
#spenders stats to df plus cleaning up format
SPENDER_DF =  pd.DataFrame({"Purchase Count": Spender_purchases, "Average Purchase Price": Spender_average_amounts, "Total Purchase Value": Spender_total})

SPENDER_DF["Average Purchase Price"] = SPENDER_DF["Average Purchase Price"].map("${:,.2f}".format)
SPENDER_DF["Total Purchase Value"] = SPENDER_DF["Total Purchase Value"].map("${:,.2f}".format)

SPENDER_DF.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
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
    <tr>
      <th>Adairialis76</th>
      <td>$2.46</td>
      <td>1</td>
      <td>$2.46</td>
    </tr>
    <tr>
      <th>Aduephos78</th>
      <td>$2.23</td>
      <td>3</td>
      <td>$6.70</td>
    </tr>
    <tr>
      <th>Aeduera68</th>
      <td>$1.93</td>
      <td>3</td>
      <td>$5.80</td>
    </tr>
    <tr>
      <th>Aela49</th>
      <td>$2.46</td>
      <td>1</td>
      <td>$2.46</td>
    </tr>
    <tr>
      <th>Aela59</th>
      <td>$1.27</td>
      <td>1</td>
      <td>$1.27</td>
    </tr>
  </tbody>
</table>
</div>




```python

#TOP SPENDERS SUMMARY
SPENDER_DF = SPENDER_DF.loc[:,["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]

SPENDER_DF.sort_values('Total Purchase Value', ascending=False).head()
# SPENDER_DF.sort_values('Purchase Count', ascending=False).head(15)
# SPENDER_DF.sort_values('Average Purchase Price', ascending=False).head(15)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
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
    <tr>
      <th>Qarwen67</th>
      <td>4</td>
      <td>$2.49</td>
      <td>$9.97</td>
    </tr>
    <tr>
      <th>Sondim43</th>
      <td>3</td>
      <td>$3.13</td>
      <td>$9.38</td>
    </tr>
    <tr>
      <th>Tillyrin30</th>
      <td>3</td>
      <td>$3.06</td>
      <td>$9.19</td>
    </tr>
    <tr>
      <th>Lisistaya47</th>
      <td>3</td>
      <td>$3.06</td>
      <td>$9.19</td>
    </tr>
    <tr>
      <th>Tyisriphos58</th>
      <td>2</td>
      <td>$4.59</td>
      <td>$9.18</td>
    </tr>
  </tbody>
</table>
</div>




```python
# RAN OUT OF TIME TO FINISH THE ASSIGMENT
```
