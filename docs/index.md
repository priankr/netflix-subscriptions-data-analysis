# Netflix Subscriptions Data Analysis

The data was downloaded from Kaggle:https://www.kaggle.com/prasertk/netflix-subscription-price-in-different-countries

The dataset contains details on Netflix subsription fees and content offerings across several countries where Netflix services are available. 

<i>Note: The dataset does not include ALL countries where Netflix operates. </i>

## Goal
Determining where Netflix customers potentially experience the most satisfaction from their subscription. 


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
```


```python
#Read the file
netflix_df = pd.read_csv('netflix dec 2021.csv')
```


```python
netflix_df.head()
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
      <th>Country_code</th>
      <th>Country</th>
      <th>Total Library Size</th>
      <th>No. of TV Shows</th>
      <th>No. of Movies</th>
      <th>Cost Per Month - Basic ($)</th>
      <th>Cost Per Month - Standard ($)</th>
      <th>Cost Per Month - Premium ($)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ar</td>
      <td>Argentina</td>
      <td>4760</td>
      <td>3154</td>
      <td>1606</td>
      <td>3.74</td>
      <td>6.30</td>
      <td>9.26</td>
    </tr>
    <tr>
      <th>1</th>
      <td>au</td>
      <td>Australia</td>
      <td>6114</td>
      <td>4050</td>
      <td>2064</td>
      <td>7.84</td>
      <td>12.12</td>
      <td>16.39</td>
    </tr>
    <tr>
      <th>2</th>
      <td>at</td>
      <td>Austria</td>
      <td>5640</td>
      <td>3779</td>
      <td>1861</td>
      <td>9.03</td>
      <td>14.67</td>
      <td>20.32</td>
    </tr>
    <tr>
      <th>3</th>
      <td>be</td>
      <td>Belgium</td>
      <td>4990</td>
      <td>3374</td>
      <td>1616</td>
      <td>10.16</td>
      <td>15.24</td>
      <td>20.32</td>
    </tr>
    <tr>
      <th>4</th>
      <td>bo</td>
      <td>Bolivia</td>
      <td>4991</td>
      <td>3155</td>
      <td>1836</td>
      <td>7.99</td>
      <td>10.99</td>
      <td>13.99</td>
    </tr>
  </tbody>
</table>
</div>




```python
netflix_df.columns
```




    Index(['Country_code', 'Country', 'Total Library Size', 'No. of TV Shows',
           'No. of Movies', 'Cost Per Month - Basic ($)',
           'Cost Per Month - Standard ($)', 'Cost Per Month - Premium ($)'],
          dtype='object')



## Checking for Missing Values


```python
#Checking the entire data frame for missing values
netflix_df.isna().any().any()
```




    False



## Creating new features

In this stage we are adding new features that will provide more insight into the data.

### Region 
We will identify the region each country falls under based on the country code.


```python
import pycountry_convert as pc

#Creating a function to identify the continent from the country code
def country_to_continent(country_name):
    country_alpha2 = pc.country_name_to_country_alpha2(country_name)
    country_continent_code = pc.country_alpha2_to_continent_code(country_alpha2)
    country_continent_name = pc.convert_continent_code_to_continent_name(country_continent_code)
    return country_continent_name

#Creating a column to indicate the continent for each country
netflix_df['Region'] = netflix_df['Country'].apply(lambda country_name: country_to_continent(country_name))

netflix_df.head()
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
      <th>Country_code</th>
      <th>Country</th>
      <th>Total Library Size</th>
      <th>No. of TV Shows</th>
      <th>No. of Movies</th>
      <th>Cost Per Month - Basic ($)</th>
      <th>Cost Per Month - Standard ($)</th>
      <th>Cost Per Month - Premium ($)</th>
      <th>Region</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ar</td>
      <td>Argentina</td>
      <td>4760</td>
      <td>3154</td>
      <td>1606</td>
      <td>3.74</td>
      <td>6.30</td>
      <td>9.26</td>
      <td>South America</td>
    </tr>
    <tr>
      <th>1</th>
      <td>au</td>
      <td>Australia</td>
      <td>6114</td>
      <td>4050</td>
      <td>2064</td>
      <td>7.84</td>
      <td>12.12</td>
      <td>16.39</td>
      <td>Oceania</td>
    </tr>
    <tr>
      <th>2</th>
      <td>at</td>
      <td>Austria</td>
      <td>5640</td>
      <td>3779</td>
      <td>1861</td>
      <td>9.03</td>
      <td>14.67</td>
      <td>20.32</td>
      <td>Europe</td>
    </tr>
    <tr>
      <th>3</th>
      <td>be</td>
      <td>Belgium</td>
      <td>4990</td>
      <td>3374</td>
      <td>1616</td>
      <td>10.16</td>
      <td>15.24</td>
      <td>20.32</td>
      <td>Europe</td>
    </tr>
    <tr>
      <th>4</th>
      <td>bo</td>
      <td>Bolivia</td>
      <td>4991</td>
      <td>3155</td>
      <td>1836</td>
      <td>7.99</td>
      <td>10.99</td>
      <td>13.99</td>
      <td>South America</td>
    </tr>
  </tbody>
</table>
</div>



To make the Dataframe more readable we can move the "Region" column right after the "Country" column


```python
#Creating a function to reposition columns
def movecol(df, cols_to_move=[], ref_col='', place='After'):
    
    #cols_to_move can be one or more columns we wish to move
    #ref_col is the column after or before which we want to reposition the column(s)
    
    cols = df.columns.tolist()
    if place == 'After':
        seg1 = cols[:list(cols).index(ref_col) + 1]
        seg2 = cols_to_move
    if place == 'Before':
        seg1 = cols[:list(cols).index(ref_col)]
        seg2 = cols_to_move + [ref_col]
    
    seg1 = [i for i in seg1 if i not in seg2]
    seg3 = [i for i in cols if i not in seg1 + seg2]
    
    return(df[seg1 + seg2 + seg3])
```


```python
netflix_df = movecol(netflix_df, cols_to_move=['Region'], ref_col='Country', place='After')
```

### Value for Money

We will examine the relationship between the amount of content available and the basic subscription cost to determine which customers receive the most value for money.

A higher score indicates that the customers can access more content per dollar spent on their Basic Netflix Subsciption.


```python
netflix_df['Value for Money'] = (netflix_df['Total Library Size']/netflix_df['Cost Per Month - Basic ($)']).round()
```

# Exploratory Data Analysis

In this stage, we will examine the data to identify any patterns, trends and relationships between the variables. It will help us analyze the data and extract insights that can be used to make decisions.

Data Visualization will give us a clear idea of what the data means by giving it visual context.


```python
#Number of countries we have data for
len(netflix_df['Country'])
```




    65




```python
netflix_df['Region'].value_counts().to_frame()
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
      <th>Region</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Europe</th>
      <td>34</td>
    </tr>
    <tr>
      <th>Asia</th>
      <td>12</td>
    </tr>
    <tr>
      <th>South America</th>
      <td>10</td>
    </tr>
    <tr>
      <th>North America</th>
      <td>6</td>
    </tr>
    <tr>
      <th>Oceania</th>
      <td>2</td>
    </tr>
    <tr>
      <th>Africa</th>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
regions = netflix_df['Region'].value_counts()

regions.plot.pie(figsize=(8, 8), autopct='%1.0f%%',fontsize=15,labels=regions.index, pctdistance=0.85, startangle=55)
plt.title("Netflix Worldwide Coverage by Region", fontsize=16)

# autopct="%1.0f%%" shows the percentage to 0 decimal place 
# pctdistance controls how far the values appear from the center of the circle
# startangle controls the angle of the slices, allowing the pie chart to be rotated to improve readability
```




    Text(0.5, 1.0, 'Netflix Worldwide Coverage by Region')




    
![png](output_17_1.png)
    


We have data on <b>65</b> countries where Netflix operates across <b>6 regions</b>.
- <b>Europe</b> represents the biggest region for Netlfix 
- <b>Africa</b> represents the smallest region for Netlfix

## Content


```python
content_stats = netflix_df['Total Library Size'].describe().round().to_frame()
tv = netflix_df['No. of TV Shows'].describe().round()
movies = netflix_df['No. of Movies'].describe().round()
content_stats = pd.concat([content_stats, tv, movies], axis=1)
content_stats
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
      <th>Total Library Size</th>
      <th>No. of TV Shows</th>
      <th>No. of Movies</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>65.0</td>
      <td>65.0</td>
      <td>65.0</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>5314.0</td>
      <td>3519.0</td>
      <td>1795.0</td>
    </tr>
    <tr>
      <th>std</th>
      <td>980.0</td>
      <td>723.0</td>
      <td>327.0</td>
    </tr>
    <tr>
      <th>min</th>
      <td>2274.0</td>
      <td>1675.0</td>
      <td>373.0</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>4948.0</td>
      <td>3154.0</td>
      <td>1628.0</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>5195.0</td>
      <td>3512.0</td>
      <td>1841.0</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>5952.0</td>
      <td>3832.0</td>
      <td>1980.0</td>
    </tr>
    <tr>
      <th>max</th>
      <td>7325.0</td>
      <td>5234.0</td>
      <td>2387.0</td>
    </tr>
  </tbody>
</table>
</div>



- The average library size is <b>5314</b> with an average of <b>3519 TV Shows</b> and <b>1795 Movies</b>
- The content library size varies between <b>2274 and 7425</b>, which indicates that there is a significant difference in what content a netflix customer can see based on where they are accessing Netflix from. 
- <b>75% of customers</b> have access to a libary of at least <b>4948 TV Shows and Movies</b>.


```python
fig_dims = (10,10)
fig, ax = plt.subplots(figsize=fig_dims)

sns.histplot(data=netflix_df, x="Total Library Size",kde=True,binwidth=200)
plt.title("Total Netflix Content Library Size", fontsize=15)
```




    Text(0.5, 1.0, 'Total Netflix Content Library Size')




    
![png](output_21_1.png)
    



```python
netflix_content_stats_df = pd.DataFrame(columns = ['Description', 'Country', 'Total Library Size', 'No. of TV Shows', 'No. of Movies','Cost Per Month - Basic ($)', 'Cost Per Month - Standard ($)','Cost Per Month - Premium ($)'])

#Largest Library
max_lib = netflix_df[netflix_df['Total Library Size'] == netflix_df['Total Library Size'].max()]
max_lib.insert(0, "Description", "Largest Content Library", True)

#Smallest Library
min_lib = netflix_df[netflix_df['Total Library Size'] == netflix_df['Total Library Size'].min()]
min_lib.insert(0, "Description", "Smallest Content Library", True)

#Largest TV Library
max_tv = netflix_df[netflix_df['No. of TV Shows'] == netflix_df['No. of TV Shows'].max()]
max_tv.insert(0, "Description", "Largest Library of TV Shows", True)

#Smallest TV Library
min_tv = netflix_df[netflix_df['No. of TV Shows'] == netflix_df['No. of TV Shows'].min()]
min_tv.insert(0, "Description", "Smallest Library of TV Shows", True)

#Largest Movie Library
max_mov = netflix_df[netflix_df['No. of Movies'] == netflix_df['No. of Movies'].max()]
max_mov.insert(0, "Description", "Largest Library of Movies", True)

#Smallest Movie Library
min_mov = netflix_df[netflix_df['No. of Movies'] == netflix_df['No. of Movies'].min()]
min_mov.insert(0, "Description", "Smallest Library of Movies", True)

#Adding columns to empty dataframe
netflix_content_stats_df = pd.concat([netflix_content_stats_df, max_lib, min_lib, max_tv, min_tv, max_mov, min_mov], axis=0)

#Removing extra columns
netflix_content_stats_df = netflix_content_stats_df[['Description', 'Country', 'Total Library Size', 'No. of TV Shows', 'No. of Movies']]

netflix_content_stats_df
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
      <th>Description</th>
      <th>Country</th>
      <th>Total Library Size</th>
      <th>No. of TV Shows</th>
      <th>No. of Movies</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>12</th>
      <td>Largest Content Library</td>
      <td>Czechia</td>
      <td>7325</td>
      <td>5234</td>
      <td>2091</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Smallest Content Library</td>
      <td>Croatia</td>
      <td>2274</td>
      <td>1675</td>
      <td>599</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Largest Library of TV Shows</td>
      <td>Czechia</td>
      <td>7325</td>
      <td>5234</td>
      <td>2091</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Smallest Library of TV Shows</td>
      <td>Croatia</td>
      <td>2274</td>
      <td>1675</td>
      <td>599</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Largest Library of Movies</td>
      <td>Malaysia</td>
      <td>5952</td>
      <td>3565</td>
      <td>2387</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Smallest Library of Movies</td>
      <td>San Marino</td>
      <td>2310</td>
      <td>1937</td>
      <td>373</td>
    </tr>
  </tbody>
</table>
</div>



As seen above, 
- <b>Czechia</b> has the Largest Content Library Overall and the Largest Library of TV Shows
- <b>Croatia</b> has the Smallest Content Library Overall and the Smallest Library of TV Shows
- <b>Malaysia</b> has the Largest Library of Movies
- <b>San Marino</b> has the Smallest Library of Movies


```python
sns.set_theme(style="whitegrid")

fig_dims = (10, 6)
fig, ax = plt.subplots(figsize=fig_dims)

#Creating a DataFrame with the Top 10 countires by library content
top_lib = netflix_df.sort_values(by=['Total Library Size'], ascending=False).head(10)

sns.set_color_codes("pastel")
sns.barplot(x=top_lib['Total Library Size'], y=top_lib['Country'], data=top_lib, label="All Content", color="b")
plt.title("Top Countries by Netflix Library Size", fontsize=15)

sns.set_color_codes("muted")
sns.barplot(x=top_lib['No. of TV Shows'], y=top_lib['Country'], data=top_lib, label="TV Shows", color="b")

sns.set_color_codes("pastel")
sns.barplot(x=top_lib['No. of Movies'], y=top_lib['Country'], data=top_lib, label="Movies", color="darkblue")

# Add a legend and informative axis label
ax.set(xlim=(0, 8000), ylabel="", xlabel="Library Size")
sns.despine(left=True, bottom=True)

# Put the legend out of the figure
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
```




    <matplotlib.legend.Legend at 0x1e78187c8e0>




    
![png](output_24_1.png)
    


In the top 10 countries by library size we see that the Movie content remains around 2000 and the main differentiator is the number of TV Shows available in each country. 


```python
sns.set_theme(style="whitegrid")

fig_dims = (10, 6)
fig, ax = plt.subplots(figsize=fig_dims)

#Creating a DataFrame with the Bottom 10 countires by library content
bot_lib = netflix_df.sort_values(by=['Total Library Size'], ascending=True).head(10)

sns.set_color_codes("pastel")
sns.barplot(x=bot_lib['Total Library Size'], y=bot_lib['Country'], data=bot_lib, label="All Content", color="b")
plt.title("Top Countries by Smallest Netflix Library Size", fontsize=15)

sns.set_color_codes("muted")
sns.barplot(x=bot_lib['No. of TV Shows'], y=bot_lib['Country'], data=bot_lib, label="TV Shows", color="b")

sns.set_color_codes("pastel")
sns.barplot(x=bot_lib['No. of Movies'], y=bot_lib['Country'], data=bot_lib, label="Movies", color="darkblue")

# Add a legend and informative axis label
ax.set(xlim=(0, 5000), ylabel="", xlabel="Library Size")
sns.despine(left=True, bottom=True)

# Put the legend out of the figure
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
```




    <matplotlib.legend.Legend at 0x1e781bdac40>




    
![png](output_26_1.png)
    


In the 10 countries by smallest library size we see that the Movie content remains around 1500 (except for Croatia and San Marino) and once again the main differentiator is the number of TV Shows available in each country. 

## Subscription


```python
subcription_stats = netflix_df['Cost Per Month - Basic ($)'].describe().round(2).to_frame()
standard = netflix_df['Cost Per Month - Standard ($)'].describe().round(2)
premium = netflix_df['Cost Per Month - Premium ($)'].describe().round(2)
subcription_stats = pd.concat([subcription_stats, standard, premium], axis=1)
subcription_stats
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
      <th>Cost Per Month - Basic ($)</th>
      <th>Cost Per Month - Standard ($)</th>
      <th>Cost Per Month - Premium ($)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>65.00</td>
      <td>65.00</td>
      <td>65.00</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>8.37</td>
      <td>11.99</td>
      <td>15.61</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.94</td>
      <td>2.86</td>
      <td>4.04</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.97</td>
      <td>3.00</td>
      <td>4.02</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>7.99</td>
      <td>10.71</td>
      <td>13.54</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>8.99</td>
      <td>11.49</td>
      <td>14.45</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>9.03</td>
      <td>13.54</td>
      <td>18.06</td>
    </tr>
    <tr>
      <th>max</th>
      <td>12.88</td>
      <td>20.46</td>
      <td>26.96</td>
    </tr>
  </tbody>
</table>
</div>



- The average subscription cost for different tiers is as follows:
    - Basic: <b>8.37</b>
    - Standard: <b>11.99</b>
    - Premium: <b>15.61</b>
- The price variation for different tiers is as follows
    - Basic: Between 1.97 and 12.88, which is <b>10.91</b>
    - Standard: Between 3.00 and 20.46, which is <b>17.46</b>
    - Premium: Between 4.02 and 26.96, which is <b>22.94</b>
    
The cost of a customer's Netflix subscription tier increases by as much as $22.94 if they relocate to a different country. 


```python
fig_dims = (8,8)
fig, ax = plt.subplots(figsize=fig_dims)

sns.histplot(data=netflix_df, x="Cost Per Month - Basic ($)",kde=True,binwidth=0.5)
plt.title("Cost Per Month - Basic ($)", fontsize=15)
```




    Text(0.5, 1.0, 'Cost Per Month - Basic ($)')




    
![png](output_30_1.png)
    



```python
fig_dims = (8,8)
fig, ax = plt.subplots(figsize=fig_dims)

sns.histplot(data=netflix_df, x="Cost Per Month - Standard ($)",kde=True,binwidth=1)
plt.title("Cost Per Month - Standard ($)", fontsize=15)
```




    Text(0.5, 1.0, 'Cost Per Month - Standard ($)')




    
![png](output_31_1.png)
    



```python
fig_dims = (8,8)
fig, ax = plt.subplots(figsize=fig_dims)

sns.histplot(data=netflix_df, x="Cost Per Month - Premium ($)",kde=True,binwidth=1)
plt.title("Cost Per Month - Premium ($)", fontsize=15)
```




    Text(0.5, 1.0, 'Cost Per Month - Premium ($)')




    
![png](output_32_1.png)
    



```python
netflix_subscription_stats_df = pd.DataFrame(columns = ['Description', 'Country', 'Total Library Size', 'No. of TV Shows', 'No. of Movies','Cost Per Month - Basic ($)', 'Cost Per Month - Standard ($)','Cost Per Month - Premium ($)'])

#Highest basic cost per month
max_basic = netflix_df[netflix_df['Cost Per Month - Basic ($)'] == netflix_df['Cost Per Month - Basic ($)'].max()]
max_basic.insert(0, "Description", "Highest Subsription Costs", True)

#Smallest basic cost per month
min_basic = netflix_df[netflix_df['Cost Per Month - Basic ($)'] == netflix_df['Cost Per Month - Basic ($)'].min()]
min_basic.insert(0, "Description", "Lowest Subsription Costs", True)

#Highest standard cost per month
max_standard = netflix_df[netflix_df['Cost Per Month - Standard ($)'] == netflix_df['Cost Per Month - Standard ($)'].max()]
max_standard.insert(0, "Description", "Highest Standard Subsription", True)

#Smallest standard cost per month
min_standard = netflix_df[netflix_df['Cost Per Month - Standard ($)'] == netflix_df['Cost Per Month - Standard ($)'].min()]
min_standard.insert(0, "Description", "Lowest Standard Subsription", True)

#Highest premium cost per month
max_premium = netflix_df[netflix_df['Cost Per Month - Premium ($)'] == netflix_df['Cost Per Month - Premium ($)'].max()]
max_premium.insert(0, "Description", "Highest Premium Subsription", True)

#Smallest premium cost per month
min_premium = netflix_df[netflix_df['Cost Per Month - Premium ($)'] == netflix_df['Cost Per Month - Premium ($)'].min()]
min_premium.insert(0, "Description", "Lowest Premium Subsription", True)

#Adding columns to empty dataframe
netflix_subscription_stats_df = pd.concat([netflix_subscription_stats_df, max_basic, min_basic, max_standard, min_standard, max_premium, min_premium], axis=0)

#Removing extra columns
netflix_subscription_stats_df = netflix_subscription_stats_df[['Description', 'Country', 'Cost Per Month - Basic ($)', 'Cost Per Month - Standard ($)','Cost Per Month - Premium ($)']]

netflix_subscription_stats_df
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
      <th>Description</th>
      <th>Country</th>
      <th>Cost Per Month - Basic ($)</th>
      <th>Cost Per Month - Standard ($)</th>
      <th>Cost Per Month - Premium ($)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>33</th>
      <td>Highest Subsription Costs</td>
      <td>Liechtenstein</td>
      <td>12.88</td>
      <td>20.46</td>
      <td>26.96</td>
    </tr>
    <tr>
      <th>56</th>
      <td>Highest Subsription Costs</td>
      <td>Switzerland</td>
      <td>12.88</td>
      <td>20.46</td>
      <td>26.96</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Lowest Subsription Costs</td>
      <td>Turkey</td>
      <td>1.97</td>
      <td>3.00</td>
      <td>4.02</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Highest Standard Subsription</td>
      <td>Liechtenstein</td>
      <td>12.88</td>
      <td>20.46</td>
      <td>26.96</td>
    </tr>
    <tr>
      <th>56</th>
      <td>Highest Standard Subsription</td>
      <td>Switzerland</td>
      <td>12.88</td>
      <td>20.46</td>
      <td>26.96</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Lowest Standard Subsription</td>
      <td>Turkey</td>
      <td>1.97</td>
      <td>3.00</td>
      <td>4.02</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Highest Premium Subsription</td>
      <td>Liechtenstein</td>
      <td>12.88</td>
      <td>20.46</td>
      <td>26.96</td>
    </tr>
    <tr>
      <th>56</th>
      <td>Highest Premium Subsription</td>
      <td>Switzerland</td>
      <td>12.88</td>
      <td>20.46</td>
      <td>26.96</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Lowest Premium Subsription</td>
      <td>Turkey</td>
      <td>1.97</td>
      <td>3.00</td>
      <td>4.02</td>
    </tr>
  </tbody>
</table>
</div>



As seen above, 
- <b>Liechtenstein</b> & <b>Switzerland</b> have the Highest Subscription Costs Overall 
- <b>Turkey</b> has the Lowest Suscription Costs Overall


```python
sns.set_theme(style="whitegrid")

fig_dims = (10, 6)
fig, ax = plt.subplots(figsize=fig_dims)

#Creating a DataFrame with the Top 10 countires by Subscription Cost
top_cost = netflix_df.sort_values(by=['Cost Per Month - Premium ($)'], ascending=False).head(10)

sns.set_color_codes("pastel")
sns.barplot(x=top_cost['Cost Per Month - Premium ($)'], y=top_cost['Country'], data=top_lib, label="Premium", color="b")
plt.title("Top Countries by Netflix Subscription Cost", fontsize=15)

sns.set_color_codes("muted")
sns.barplot(x=top_cost['Cost Per Month - Standard ($)'], y=top_cost['Country'], data=top_lib, label="Standard", color="b")

sns.set_color_codes("pastel")
sns.barplot(x=top_cost['Cost Per Month - Basic ($)'], y=top_cost['Country'], data=top_lib, label="Basic", color="darkblue")

# Add a legend and informative axis label
ax.set(xlim=(0, 30), ylabel="", xlabel="Subscription Cost ($)")
sns.despine(left=True, bottom=True)

# Put the legend out of the figure
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
```




    <matplotlib.legend.Legend at 0x1e782008880>




    
![png](output_35_1.png)
    


In the Top 10 countries by Subscription Cost we see that the subscription cost remains almost the same across tiers (except for Switzerland and Liechtenstein) 

## Region


```python
netflix_region_df = netflix_df.groupby('Region')['Total Library Size'].mean().round()

#Converting the series netflix_region_df to a dataframe
netflix_region_df = netflix_region_df.to_frame()

#Identifying average number of TV shows offered in each region
netflix_region_df['Average No. of TV Shows'] = netflix_df.groupby('Region')['No. of TV Shows'].mean().round()

#Identifying average number of Movies offered in each region
netflix_region_df['Average No. of Movies'] = netflix_df.groupby('Region')['No. of Movies'].mean().round()

#Identifying average basic subscription cost in each region
netflix_region_df['Average Basic Subscription Cost ($)'] = netflix_df.groupby('Region')['Cost Per Month - Basic ($)'].mean().round(2)

#Identifying average standard subscription cost in each region
netflix_region_df['Average Standard Subscription Cost ($)'] = netflix_df.groupby('Region')['Cost Per Month - Standard ($)'].mean().round(2)

#Identifying average premium subscription cost in each region
netflix_region_df['Average Premium Subscription Cost ($)'] = netflix_df.groupby('Region')['Cost Per Month - Premium ($)'].mean().round(2)

#Identifying average value for money in each region
netflix_region_df['Average Value for Money'] = netflix_df.groupby('Region')['Value for Money'].mean().round()

#Renaming columns
netflix_region_df.rename(columns={'Total Library Size':'Average Overall Library Size'}, inplace=True)

#The index for this dataframe contains the station names so we want to put that information in a seaprate column
#Resetting the index (also creates a new column called 'index')
netflix_region_df .reset_index(level=0, inplace=True)

netflix_region_df
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
      <th>Region</th>
      <th>Average Overall Library Size</th>
      <th>Average No. of TV Shows</th>
      <th>Average No. of Movies</th>
      <th>Average Basic Subscription Cost ($)</th>
      <th>Average Standard Subscription Cost ($)</th>
      <th>Average Premium Subscription Cost ($)</th>
      <th>Average Value for Money</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Africa</td>
      <td>5736.0</td>
      <td>3686.0</td>
      <td>2050.0</td>
      <td>6.26</td>
      <td>10.05</td>
      <td>12.58</td>
      <td>916.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Asia</td>
      <td>5347.0</td>
      <td>3377.0</td>
      <td>1970.0</td>
      <td>7.64</td>
      <td>10.40</td>
      <td>12.97</td>
      <td>900.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Europe</td>
      <td>5361.0</td>
      <td>3652.0</td>
      <td>1709.0</td>
      <td>9.23</td>
      <td>13.30</td>
      <td>17.55</td>
      <td>599.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>North America</td>
      <td>5299.0</td>
      <td>3459.0</td>
      <td>1840.0</td>
      <td>8.08</td>
      <td>11.88</td>
      <td>15.20</td>
      <td>661.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Oceania</td>
      <td>6099.0</td>
      <td>4026.0</td>
      <td>2072.0</td>
      <td>8.32</td>
      <td>12.32</td>
      <td>16.66</td>
      <td>736.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>South America</td>
      <td>4927.0</td>
      <td>3156.0</td>
      <td>1771.0</td>
      <td>6.71</td>
      <td>9.62</td>
      <td>12.56</td>
      <td>802.0</td>
    </tr>
  </tbody>
</table>
</div>



### Content


```python
sns.set_theme(style="whitegrid")

fig_dims = (10, 6)
fig, ax = plt.subplots(figsize=fig_dims)

#Creating a DataFrame with the Top 10 countires by library content
region_lib = netflix_region_df.sort_values(by=['Average Overall Library Size'], ascending=False).head(10)

sns.set_color_codes("pastel")
sns.barplot(x=region_lib['Average Overall Library Size'], y=region_lib['Region'], label="All Content", color="b")
plt.title("Regions by Netflix Library Size", fontsize=15)

sns.set_color_codes("muted")
sns.barplot(x=region_lib['Average No. of TV Shows'], y=region_lib['Region'], label="TV Shows", color="b")

sns.set_color_codes("pastel")
sns.barplot(x=region_lib['Average No. of Movies'], y=region_lib['Region'], label="Movies", color="darkblue")

# Add a legend and informative axis label
ax.set(xlim=(0, 8000), ylabel="", xlabel="Library Size")
sns.despine(left=True, bottom=True)

# Put the legend out of the figure
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
```




    <matplotlib.legend.Legend at 0x1e78225f7c0>




    
![png](output_39_1.png)
    


- <b>Oceania</b> has the largest overall content library, TV Show & Movie Library
- <b>South America</b> has the smallest overall content library, TV Show & Movie Library

### Subscription Cost


```python
sns.set_theme(style="whitegrid")

fig_dims = (10, 6)
fig, ax = plt.subplots(figsize=fig_dims)

#Creating a DataFrame with the countires by Subscription Cost
region_cost = netflix_region_df.sort_values(by=['Average Premium Subscription Cost ($)'], ascending=False)

sns.set_color_codes("pastel")
sns.barplot(x=region_cost['Average Premium Subscription Cost ($)'], y=region_cost['Region'], label="Premium", color="b")
plt.title("Regions by Netflix Subscription Cost", fontsize=15)

sns.set_color_codes("muted")
sns.barplot(x=region_cost['Average Standard Subscription Cost ($)'], y=region_cost['Region'], label="Standard", color="b")

sns.set_color_codes("pastel")
sns.barplot(x=region_cost['Average Basic Subscription Cost ($)'], y=region_cost['Region'], label="Basic", color="darkblue")

# Add a legend and informative axis label
ax.set(xlim=(0, 20), ylabel="", xlabel="Subscription Cost ($)")
sns.despine(left=True, bottom=True)

# Put the legend out of the figure
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
```




    <matplotlib.legend.Legend at 0x1e7823130a0>




    
![png](output_41_1.png)
    


- <b>Europe</b> has the largest overall subscription cost
- <b>South America</b> has the lowest standard and premium subscription cost
- <b>Africa</b> has the lowest basic subscription cost

## Value for Money


```python
netflix_df['Value for Money'].describe().round().to_frame()
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
      <th>Value for Money</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>65.0</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>700.0</td>
    </tr>
    <tr>
      <th>std</th>
      <td>342.0</td>
    </tr>
    <tr>
      <th>min</th>
      <td>237.0</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>555.0</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>628.0</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>753.0</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2355.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
netflix_df[(netflix_df['Value for Money']==237)|(netflix_df['Value for Money']==2355)]
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
      <th>Country_code</th>
      <th>Country</th>
      <th>Region</th>
      <th>Total Library Size</th>
      <th>No. of TV Shows</th>
      <th>No. of Movies</th>
      <th>Cost Per Month - Basic ($)</th>
      <th>Cost Per Month - Standard ($)</th>
      <th>Cost Per Month - Premium ($)</th>
      <th>Value for Money</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>33</th>
      <td>li</td>
      <td>Liechtenstein</td>
      <td>Europe</td>
      <td>3048</td>
      <td>1712</td>
      <td>1336</td>
      <td>12.88</td>
      <td>20.46</td>
      <td>26.96</td>
      <td>237.0</td>
    </tr>
    <tr>
      <th>59</th>
      <td>tr</td>
      <td>Turkey</td>
      <td>Asia</td>
      <td>4639</td>
      <td>2930</td>
      <td>1709</td>
      <td>1.97</td>
      <td>3.00</td>
      <td>4.02</td>
      <td>2355.0</td>
    </tr>
  </tbody>
</table>
</div>



The average Netflix customer has access to <b>700 TV Shows and Movies</b> for every dollar spent on their subscription.
- 75% of customers ss to <b>753 TV Shows and Movies</b> for every dollar spent on their subscription, which is just above average.
- On the high end, customers in certain countries have access to <b>2355 TV Shows and Movies</b> for every dollar spent on their subscription.
- On the low end, customers in certain countries have access to <b>237 TV Shows and Movies</b> for every dollar spent on their subscription.

Therefore, a cutomer in Turkey country is getting <b>10 times the value</b> from their Netflix subscription compared to a customer in Liechtenstein


```python
sns.set_theme(style="whitegrid")

fig_dims = (10, 6)
fig, ax = plt.subplots(figsize=fig_dims)

#Creating a DataFrame with the countires by Subscription Cost
value = netflix_df.sort_values(by=['Value for Money'], ascending=False).head(10)

sns.set_color_codes("pastel")
sns.barplot(x=value['Value for Money'], y=value['Country'], color="b")
plt.title("Top Countries by Customer Value for Money", fontsize=15)
```




    Text(0.5, 1.0, 'Top Countries by Customer Value for Money')




    
![png](output_46_1.png)
    


Customers in <b>Turkey</b> and <b>India</b> have the greatest value for money for their Basic Netflix Subscription


```python
sns.set_theme(style="whitegrid")

fig_dims = (10, 6)
fig, ax = plt.subplots(figsize=fig_dims)

#Creating a DataFrame with the countires by Subscription Cost
value = netflix_df.sort_values(by=['Value for Money'], ascending=True).head(10)

sns.set_color_codes("pastel")
sns.barplot(x=value['Value for Money'], y=value['Country'], color="b")
plt.title("Top Countries by Lowest Customer Value for Money", fontsize=15)
```




    Text(0.5, 1.0, 'Top Countries by Lowest Customer Value for Money')




    
![png](output_48_1.png)
    


Customers in <b>Liechtenstein, Croatia and San Marino</b> have the least value for money for their Basic Netflix Subscription

### Region


```python
sns.set_theme(style="whitegrid")

fig_dims = (10, 6)
fig, ax = plt.subplots(figsize=fig_dims)

#Creating a DataFrame with the countires by Subscription Cost
value_region = netflix_region_df.sort_values(by=['Average Value for Money'], ascending=False)

sns.set_color_codes("pastel")
sns.barplot(x=value_region['Average Value for Money'], y=value_region['Region'], color="b")
plt.title("Customer Value for Money by Region", fontsize=15)
```




    Text(0.5, 1.0, 'Customer Value for Money by Region')




    
![png](output_50_1.png)
    


- Customers in <b>Africa and Asia</b> have the greatest value for money for their Basic Netflix Subscription
- Customers in <b>Europe</b> have the least value for money for their Basic Netflix Subscription

### Content vs. Subscription 

Another way to determine the customer's value for money is to look at countries where customers get access to above average library size and below average basic subsciption cost.


```python
#Identifying countries with below average basic cost and above average library size
highlight = netflix_df[(netflix_df['Cost Per Month - Basic ($)']<netflix_df['Cost Per Month - Basic ($)'].mean())&(netflix_df['Total Library Size']>netflix_df['Total Library Size'].mean())]

len(highlight)
```




    10



We have <b>10 countries</b> where customers get access to above average library size and below average basic subsciption cost.


```python
fig_dims = (15, 5)
fig, ax = plt.subplots(figsize=fig_dims)

sns.scatterplot(x='Total Library Size',y='Cost Per Month - Basic ($)', data=netflix_df, s=50, color="lightblue")
sns.scatterplot(x='Total Library Size',y='Cost Per Month - Basic ($)', data=highlight, s=50, color="green")

plt.title("Content vs. Subsciption Cost",fontsize=15)
```




    Text(0.5, 1.0, 'Content vs. Subsciption Cost')




    
![png](output_54_1.png)
    


Points highlighted in <b>green</b> indicate countries with access to an above average library size at a below average basic subscription cost. 


```python
#Identifying the top 3 countries by Total Library Size
highlight.sort_values(by=['Total Library Size'], ascending=True).head(3)
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
      <th>Country_code</th>
      <th>Country</th>
      <th>Region</th>
      <th>Total Library Size</th>
      <th>No. of TV Shows</th>
      <th>No. of Movies</th>
      <th>Cost Per Month - Basic ($)</th>
      <th>Cost Per Month - Standard ($)</th>
      <th>Cost Per Month - Premium ($)</th>
      <th>Value for Money</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>60</th>
      <td>ua</td>
      <td>Ukraine</td>
      <td>Europe</td>
      <td>5336</td>
      <td>3261</td>
      <td>2075</td>
      <td>5.64</td>
      <td>8.46</td>
      <td>11.29</td>
      <td>946.0</td>
    </tr>
    <tr>
      <th>48</th>
      <td>ru</td>
      <td>Russia</td>
      <td>Europe</td>
      <td>5711</td>
      <td>3624</td>
      <td>2087</td>
      <td>8.13</td>
      <td>10.84</td>
      <td>13.56</td>
      <td>702.0</td>
    </tr>
    <tr>
      <th>52</th>
      <td>za</td>
      <td>South Africa</td>
      <td>Africa</td>
      <td>5736</td>
      <td>3686</td>
      <td>2050</td>
      <td>6.26</td>
      <td>10.05</td>
      <td>12.58</td>
      <td>916.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Identifying the top 3 countries by Basic Subscription Cost
highlight.sort_values(by=['Cost Per Month - Basic ($)'], ascending=True).head(3)
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
      <th>Country_code</th>
      <th>Country</th>
      <th>Region</th>
      <th>Total Library Size</th>
      <th>No. of TV Shows</th>
      <th>No. of Movies</th>
      <th>Cost Per Month - Basic ($)</th>
      <th>Cost Per Month - Standard ($)</th>
      <th>Cost Per Month - Premium ($)</th>
      <th>Value for Money</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>26</th>
      <td>in</td>
      <td>India</td>
      <td>Asia</td>
      <td>5843</td>
      <td>3718</td>
      <td>2125</td>
      <td>2.64</td>
      <td>6.61</td>
      <td>8.60</td>
      <td>2213.0</td>
    </tr>
    <tr>
      <th>60</th>
      <td>ua</td>
      <td>Ukraine</td>
      <td>Europe</td>
      <td>5336</td>
      <td>3261</td>
      <td>2075</td>
      <td>5.64</td>
      <td>8.46</td>
      <td>11.29</td>
      <td>946.0</td>
    </tr>
    <tr>
      <th>52</th>
      <td>za</td>
      <td>South Africa</td>
      <td>Africa</td>
      <td>5736</td>
      <td>3686</td>
      <td>2050</td>
      <td>6.26</td>
      <td>10.05</td>
      <td>12.58</td>
      <td>916.0</td>
    </tr>
  </tbody>
</table>
</div>



In terms of Content, the top 3 countries for customers are <b> Ukraine, Russia and South Africa</b> 

In terms of Basic Subscription Cost, the top 3 countries for customers are <b> India, Ukraine and South Africa</b> 

Taking both Content and Subscription Cost into consideration <b>Ukrainian and South African</b> Customers receive the most value for their Netflix Subscription.

# Summary of Netflix Data

We have data on <b>65</b> countries where Netflix operates across <b>6 regions</b>.

In terms of number of countries served,
- <b>Europe</b> represents the biggest region for Netlfix
- <b>Africa</b> represents the smallest region for Netlfix

## Content

- The average library size is <b>5314</b> with an average of <b>3519 TV Shows</b> and <b>1795 Movies</b>
- The content library size varies between <b>2274 and 7425</b>, which indicates that there is a significant difference in what content a netflix customer can see based on where they are accessing Netflix from. 
- <b>75% of customers</b> have access to a library of at least <b>4948 TV Shows and Movies</b>.

Based on the data we can say that the majority of Netflix's content in any given country consists of TV Shows.
- The standard deviation in TV Shows available is 723, while the standard deviation in Movies available is 327 
- Therefore, Movies available to users in different countries vary by a smaller margin than TV Shows avaialable to users in different countries. 
- TV Shows are the main differentiator in terms of content between each country. 

## Subscription Cost 

- The average subscription cost for different tiers is as follows:
    - Basic: <b>8.37</b>
    - Standard: <b>11.99</b>
    - Premium: <b>15.61</b>
    
The cost of a customer's Netflix subscription tier increases by as much as $22.94 if they relocate to a different country. 

## Value for Money

The average Netflix customer has access to <b>700 TV Shows and Movies</b> for every dollar spent on their subscription.
- 75% of customers ss to <b>753 TV Shows and Movies</b> for every dollar spent on their subscription, which is just above average.
- On the high end, customers in certain countries have access to <b>2355 TV Shows and Movies</b> for every dollar spent on their subscription.
- On the low end, customers in certain countries have access to <b>237 TV Shows and Movies</b> for every dollar spent on their subscription.

Thereofore, the disparity between the value a customer experiences from country to country may be almost <b>as high as 10 times the value for money</b>

## Regional Breakdown

### Europe

<u>Content</u>: European countries on average have access to the <i>3rd largest content library</i>, only slightly bigger than Asian countries.

<u>Subscription Cost and Value for Money</u>: Customers experience the <i>least value for money</i> for their Basic Netflix Subscription despite the fact that it has the <i>highest overall subscription cost</i>

<u>Country Details</u>:
- <b>Czechia</b> has the Largest Content Library Overall and the Largest Library of TV Shows
- <b>Croatia</b> has the Smallest Content Library Overall and the Smallest Library of TV Shows
- <b>San Marino</b> has the Smallest Library of Movies
- <b>Liechtenstein</b> has the Highest Subscription Costs Overall and customers in <b>Liechtenstein</b> experience the least value for money for their Basic Netflix Subscription
- Taking both Content and Subscription Cost into consideration, customers in <b>Ukraine</b> receive the most value for their Basic Netflix Subscription.

### Asia
<u>Content</u>: Asian countries on average have access to the <i>4rd largest content library</i>

<u>Subscription Cost and Value for Money</u>: Customers experience the <i>2nd highest value for money</i> for their Basic Netflix Subscription and have the <i> 4th highest overall subscription cost</i>

<u>Country Details</u>:
- <b>Malaysia</b> has the Largest Library of Movies
- <b>Turkey</b> has the Lowest Suscription Costs Overall
- Taking purely just value for money into consideration, customers in <b>Turkey</b> receive the greatest value for money for their Basic Netflix Subscription


### Africa
<u>Content</u>: African countries on average have access to the <i>2nd largest content library</i>

<u>Subscription Cost and Value for Money</u>: Customers experience the <i>greatest value for money</i> for their Basic Netflix Subscription and it has the <i>lowest basic subscription cost</i>

### North America
<u>Content</u>: North American countries on average have access to the <i>2nd smallest overall content library</i>

<u>Subscription Cost and Value for Money</u>:Customers experience the <i>2nd lowest value for money</i> for their Basic Netflix Subscription and it has the <i>3rd highest overall subscription cost</i>. 

### South America
<u>Content</u>: South American Countries on average have access to the <i>smallest overall content library</i>

<u>Subscription Cost and Value for Money</u>: Customers experience the <i>3rd highest value for money</i> for their Basic Netflix Subscription and it has the <i>lowest overall subscription cost</i>. 

### Oceania
<u>Content</u>: Oceanian countries on average have access to the <i>largest overall content library</i>

<u>Subscription Cost and Value for Money</u>: Customers experience the <i>4th highest value for money</i> for their Basic Netflix Subscription and it has the <i>2nd highest overall subscription cost</i>. 

# Where do Netflix customers experience the highest satisfaction?

A Netflix customer's primary concern is determining a balance between the value of entertainment and the cost of that entertainment. Ideally, the customer is seeking to pay the lowest amount possible for access to the most content. Based on the information above we can see that Cutomers in Africa would experience the most satisfaction for three key reasons:

<u>Content</u>: Access to the <i>2nd largest content library</i><br>
<u>Subscription Cost</u>: It has the <i>lowest basic subscription cost</i><br>
<u>Value for Money</u>: Region with the <i>greatest value for money</i> 

In particular, <b>South Africa</b>, which ranks 3rd in terms of above average access to content and below average basic subscription cost is the best location for Netflix Customers.

<b>Additional Data necessary</b><br>
The data only tells how much content is available to customers in each country and how much the different subscription tiers cost. For a holistic comparison, we need to consider factors such as 
- Number of customers in each country
- Number of customers actively using Netflix in each country (including Total hours of content watched, frequency of use etc.)
- Details on the type of content available
- Details on customer preferences (in terms of content)<br>
- And much more...

Together this data can help paint a detailed picture of what customers enjoy the most and where the most satisfied customers may be located. 



```python

```
