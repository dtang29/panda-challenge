

```python
#import dependencies
import pandas as pd
import os
```


```python
#set file_path for items_complete.csv
file_path_text = os.path.join("..", "generated_data","items_complete.csv")

#set file_path for players_complete.csv
file_path_players = os.path.join("..", "generated_data","players_complete.csv")

#set file_path for purchase_data_3.csv
file_path_purchase = os.path.join("..", "generated_data","purchase_data_3.csv")

```


```python
#read in text
items = pd.read_csv(file_path_text)
items.head()

#read in players
players = pd.read_csv(file_path_players)
players.head()

#read in purhcase
purchase = pd.read_csv(file_path_purchase)
purchase.head()
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
      <th>Purchase ID</th>
      <th>SN</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Iloni35</td>
      <td>20</td>
      <td>Male</td>
      <td>93</td>
      <td>Apocalyptic Battlescythe</td>
      <td>4.49</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Aidaira26</td>
      <td>21</td>
      <td>Male</td>
      <td>12</td>
      <td>Dawne</td>
      <td>3.36</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Irim47</td>
      <td>17</td>
      <td>Male</td>
      <td>5</td>
      <td>Putrid Fan</td>
      <td>2.63</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Irith83</td>
      <td>17</td>
      <td>Male</td>
      <td>123</td>
      <td>Twilight's Carver</td>
      <td>2.55</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Philodil43</td>
      <td>22</td>
      <td>Male</td>
      <td>154</td>
      <td>Feral Katana</td>
      <td>4.11</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Player Count
total_players = pd.DataFrame({"Total Players": players["SN"].count()}, index=[0] )
total_players
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
      <td>1163</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Total)

#Number of Unique Items
unique_items = items["Item Name"].nunique()
#Average Purchase Price
avg_purchase_price = purchase["Price"].mean()
#Total Number of Purchases
total_num_purchase = purchase["Purchase ID"].count()
#Total Revenue
total_revenue = purchase["Price"].sum()

purchasing_analysis = pd.DataFrame({"Number of Unique Items":unique_items,
                                   "Average Price":"${:.2f}".format(avg_purchase_price),
                                   "Number of Purchases":total_num_purchase,
                                   "Total Revenue":"${:.2f}".format(total_revenue)}, index=[0])

purchasing_analysis[["Number of Unique Items", "Average Price", "Number of Purchases", "Total Revenue"]]


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
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>186</td>
      <td>$2.92</td>
      <td>78</td>
      <td>$228.10</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender Demographics

count_players = players["Gender"].value_counts()
total_players = players["Player ID"].count()
percentage = round((count_players / total_players)*100, 2).map("{:}%".format)


demographics = pd.DataFrame({"Percentage of Players": percentage, "Total Count": count_players})
demographics
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
      <td>82.03%</td>
      <td>954</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>16.08%</td>
      <td>187</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.89%</td>
      <td>22</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Gender) - The below each broken by gender

grouped = purchase.groupby("Gender")

#Purchase Count
count_purchase = grouped["Purchase ID"].count()
#Average Purchase Price
average_purchase = grouped["Price"].mean()
#Total Purchase Value
total_purchase = count_purchase * average_purchase

#Create a dataframe
purchasing_analysis = pd.DataFrame({"Purchase Count":count_purchase,
                                   "Average Purchase Price":average_purchase,
                                   "Total Purchase Value":total_purchase,
                                   })


#---Calculate min-max normalization between the average purchase price for genders to map values from range 0 to 1---

#get max and min average purchase price between female, male, other 
max_purchase_price = purchasing_analysis["Average Purchase Price"].max()
min_purchase_price = purchasing_analysis["Average Purchase Price"].min()

#Calculate a min-max normalization on average purchase price
normalized_totals = (purchasing_analysis["Average Purchase Price"] - min_purchase_price) / (max_purchase_price - min_purchase_price)

#Add new column named Normalized Totals to df 
purchasing_analysis["Normalized Totals"] = normalized_totals

#format columns to include $ signs
purchasing_analysis["Average Purchase Price"] = purchasing_analysis["Average Purchase Price"].map("$ {:,.2f}".format)
purchasing_analysis["Total Purchase Value"] = purchasing_analysis["Total Purchase Value"].map("$ {:,.2f}".format)

#reorder columns
purchasing_analysis = purchasing_analysis[["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]
purchasing_analysis

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
      <td>13</td>
      <td>$ 3.18</td>
      <td>$ 41.38</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>64</td>
      <td>$ 2.88</td>
      <td>$ 184.60</td>
      <td>0.719021</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1</td>
      <td>$ 2.12</td>
      <td>$ 2.12</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Demographics - The below each broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.)

bins = [0,10,14,19,24,29,34,39,100]
group_names = ["<10", "10-14" , "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

#cut players and place Age in bins and put it in new column called age group
players["Age Group"] = pd.cut(players["Age"],bins,labels=group_names)

#group by summary bin column
group = players.groupby("Age Group")

#get count of players per bin
age_per_bin = group["Player ID"].count()

#get the total count of players
total = age_per_bin.sum()

#calculate percentage of players per bin and format it with percentages
percentage_of_players = round(age_per_bin/total*100,2).map("{:}%".format)

age_demographics = pd.DataFrame({"Percentage of Players": percentage_of_players,
                                 "Total Count": age_per_bin})
age_demographics

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
    <tr>
      <th>Age Group</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>5.33%</td>
      <td>62</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>3.96%</td>
      <td>46</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.54%</td>
      <td>204</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>41.79%</td>
      <td>486</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>16.25%</td>
      <td>189</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.17%</td>
      <td>95</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>5.16%</td>
      <td>60</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1.81%</td>
      <td>21</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing_Analysis (Age)

#cut purchase and place Age in bins and put it in new column called summary
purchase["Age Group"] = pd.cut(purchase["Age"],bins,labels=group_names)

grouped_purchase = purchase.groupby("Age Group")

#Purchase Count
count_purchase_age = grouped_purchase["Purchase ID"].count()
#Average Purchase Price
average_purchase_age = grouped_purchase["Price"].mean()
#Total Purchase Value
total_purchase_age = count_purchase_age * average_purchase_age


#create new dataframe to hold everything
purchasing_analysis_age = pd.DataFrame({"Purchase Count": count_purchase_age,
                                       "Average Purchase Price": average_purchase_age,
                                      "Total Purchase Value": total_purchase_age})




#---Calculate min-max normalization between the average purchase price for genders to map values from range 0 to 1---

#get max and min average purchase price between ages
max_purchase_price_age = purchasing_analysis_age["Average Purchase Price"].max()
min_purchase_price_age = purchasing_analysis_age["Average Purchase Price"].min()

#Calculate a min-max normalization on average purchase price
normalized_totals_age = (purchasing_analysis_age["Average Purchase Price"] - min_purchase_price_age) / (max_purchase_price_age - min_purchase_price_age)

#Add new column named Normalized Totals to df 
purchasing_analysis_age["Normalized Totals"] = normalized_totals_age

#format columns to include $ signs
purchasing_analysis_age["Average Purchase Price"] = purchasing_analysis_age["Average Purchase Price"].map("$ {:,.2f}".format)
purchasing_analysis_age["Total Purchase Value"] = purchasing_analysis_age["Total Purchase Value"].map("$ {:,.2f}".format)

#reorder columns
purchasing_analysis_age = purchasing_analysis_age[["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]
purchasing_analysis_age

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
      <th>Age Group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>5</td>
      <td>$ 2.76</td>
      <td>$ 13.82</td>
      <td>0.292497</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>3</td>
      <td>$ 2.99</td>
      <td>$ 8.96</td>
      <td>0.376027</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>11</td>
      <td>$ 2.76</td>
      <td>$ 30.41</td>
      <td>0.292702</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>36</td>
      <td>$ 3.02</td>
      <td>$ 108.89</td>
      <td>0.390303</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>9</td>
      <td>$ 2.90</td>
      <td>$ 26.11</td>
      <td>0.343932</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>7</td>
      <td>$ 1.98</td>
      <td>$ 13.89</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>6</td>
      <td>$ 3.56</td>
      <td>$ 21.37</td>
      <td>0.591729</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1</td>
      <td>$ 4.65</td>
      <td>$ 4.65</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top Spenders

#Identify the the top 5 spenders in the game by total purchase value, then list (in a table):

#SN
grouped_sn = purchase.groupby("SN")

#Purchase Count
count_purchase_sn = grouped_sn["Purchase ID"].count()

#Average Purchase Price
average_purchase_sn = grouped_sn["Price"].mean()

#Total Purchase Value
total_purchase_sn = grouped_sn["Price"].sum()

#Create dataframe to hold the values
top_spenders = pd.DataFrame({"Purchase Count": count_purchase_sn,
                           "Average Purchase Price": average_purchase_sn.map("$ {:,.2f}".format),
                          "Total Purchase Value": total_purchase_sn.map("$ {:,.2f}".format)})
#Rearrange the columns
top_spenders = top_spenders[["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]


#Sort the values by Total Purchase Value descending and call the first 5 rows
top_spenders.sort_values("Total Purchase Value", ascending=False).head(5)

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
      <th>Sundaky74</th>
      <td>2</td>
      <td>$ 3.71</td>
      <td>$ 7.41</td>
    </tr>
    <tr>
      <th>Aidaira26</th>
      <td>2</td>
      <td>$ 2.56</td>
      <td>$ 5.13</td>
    </tr>
    <tr>
      <th>Eusty71</th>
      <td>1</td>
      <td>$ 4.81</td>
      <td>$ 4.81</td>
    </tr>
    <tr>
      <th>Chanirra64</th>
      <td>1</td>
      <td>$ 4.78</td>
      <td>$ 4.78</td>
    </tr>
    <tr>
      <th>Alarap40</th>
      <td>1</td>
      <td>$ 4.71</td>
      <td>$ 4.71</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Popular Items

#Identify the 5 most popular items by purchase count, then list (in a table):

#Item ID
#Item Name
#Purchase Count
#Item Price
#Total Purchase Value

#Item Name
grouped_item = purchase.groupby(["Item ID","Item Name","Price"])

#Purchase Count
count_purchase_item = grouped_item["Purchase ID"].count()

#Create a dataframe to hold all values
items = pd.DataFrame({"Purchase Count": count_purchase_item})
items = items.reset_index()

#Sort top 5 items by Purchase Count
popular_items = items.sort_values("Purchase Count", ascending=False).head(5)

#Add a new column called Total Purchase Value
popular_items["Total Purchase Value"] = popular_items["Price"] * popular_items["Purchase Count"]

#Add $ signs after it has been sorted
popular_items["Price"] = popular_items["Price"].map(("$ {:,.2f}".format))
popular_items["Total Purchase Value"] = popular_items["Total Purchase Value"].map(("$ {:,.2f}".format))

#Rearrange columns
popular_items = popular_items[["Item ID", "Item Name", "Purchase Count", "Price", "Total Purchase Value"]]
popular_items


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
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>31</th>
      <td>94</td>
      <td>Mourning Blade</td>
      <td>3</td>
      <td>$ 3.64</td>
      <td>$ 10.92</td>
    </tr>
    <tr>
      <th>28</th>
      <td>90</td>
      <td>Betrayer</td>
      <td>2</td>
      <td>$ 4.12</td>
      <td>$ 8.24</td>
    </tr>
    <tr>
      <th>38</th>
      <td>111</td>
      <td>Misery's End</td>
      <td>2</td>
      <td>$ 1.79</td>
      <td>$ 3.58</td>
    </tr>
    <tr>
      <th>21</th>
      <td>64</td>
      <td>Fusion Pummel</td>
      <td>2</td>
      <td>$ 2.42</td>
      <td>$ 4.84</td>
    </tr>
    <tr>
      <th>50</th>
      <td>154</td>
      <td>Feral Katana</td>
      <td>2</td>
      <td>$ 4.11</td>
      <td>$ 8.22</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Identify the 5 most profitable items by total purchase value, then list (in a table):

#Add new column with Total Purchase Value
items["Total Purchase Value"] = items["Price"] * items["Purchase Count"]

# #Sort top 5 items by Purchase Count
profitable_items = items.sort_values("Total Purchase Value", ascending=False).head(5)

# #Add $ signs after it has been sorted
profitable_items["Price"] = profitable_items["Price"].map(("$ {:,.2f}".format))
profitable_items["Total Purchase Value"] = profitable_items["Total Purchase Value"].map(("$ {:,.2f}".format))


# #Rearrange columns
profitable_items = profitable_items[["Item ID", "Item Name", "Purchase Count", "Price", "Total Purchase Value"]]
profitable_items
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
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>31</th>
      <td>94</td>
      <td>Mourning Blade</td>
      <td>3</td>
      <td>$ 3.64</td>
      <td>$ 10.92</td>
    </tr>
    <tr>
      <th>39</th>
      <td>117</td>
      <td>Heartstriker, Legacy of the Light</td>
      <td>2</td>
      <td>$ 4.71</td>
      <td>$ 9.42</td>
    </tr>
    <tr>
      <th>30</th>
      <td>93</td>
      <td>Apocalyptic Battlescythe</td>
      <td>2</td>
      <td>$ 4.49</td>
      <td>$ 8.98</td>
    </tr>
    <tr>
      <th>28</th>
      <td>90</td>
      <td>Betrayer</td>
      <td>2</td>
      <td>$ 4.12</td>
      <td>$ 8.24</td>
    </tr>
    <tr>
      <th>50</th>
      <td>154</td>
      <td>Feral Katana</td>
      <td>2</td>
      <td>$ 4.11</td>
      <td>$ 8.22</td>
    </tr>
  </tbody>
</table>
</div>


