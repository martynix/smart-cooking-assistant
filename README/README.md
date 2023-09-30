# Introduction to IoT - Final Work
## Smart Cooking Assistant
## Author: Martyna Pitera

Final work plan is to specify and analyze of fictitious system that suggests recipes based on the ingredients available in the refrigerator and on the weather: Smart Cooking Assistant.

The Smart Cooking Assistant would be connected to other systems and would be able to recognize the ingredients placed in the refrigerator.

I’ve chosen this topic because I'm not the best cook myself. I've often found it tricky to come up with meal ideas and make cooking enjoyable. That’s the reason for creating a system that makes cooking easier and more exciting for people like me who struggle with it. I want to use technology to help people have a better time in the kitchen, including myself.

## Smart Cooking Assistand in IoT

The Smart Cooking Assistant leverages IoT technology by incorporating connected devices, such as cameras or sensors, to scan and identify ingredients in the user's refrigerator. This IoT integration enables the system to gather real-time data about available ingredients, enhancing its ability to provide personalized recipe recommendations. Additionaly, by integrating weather information, the system gains a contextual understanding of external conditions. This enables it to offer recipe suggestions that align with the current weather, ensuring that users receive recommendations tailored to factors like temperature, season, and potential outdoor activities.

## Some ideas for Smart Cooking Assistant

- the IoT-enabled device in the refrigerator uses cameras or sensors to scan and identify available ingredients,
- the system accesses real-time weather information through APIs or external sources,
- users have the option to manually input specific ingredients or preferences through a mobile app or voice command,
- based on the available ingredients, weather and user's preferences the system selects relevant recipes and ranks them according to factors like simplicity, popularity, and user preference,
- system sets timers and provides reminders to the user for different steps in the recipe,
- it collects user's feedback in order to improve performance,
- it helps reducing food waste

<div style="page-break-after: always;"></div>


## Analyze of the Smart Cooking Assistant as a system

Peculiar operation: provides personalized recipe recommendations based on available ingredients and current weather conditions

Conditions of production: IoT-enabled devices for ingredient recognition, AI algorithms for recipe recommendations, connections to external weather data sources, a database of recipes, internet connection

Conditions of reproduction: regular updates and maintenance, connection to the power grid, access to real-time weather data and connectivity to the recipe database

External conditions: Weather variability (changes in weather conditions affect the relevance of recipe suggestions. For example, the system may recommend hot soup recipes on a cold, rainy day and salads on a hot summer day), ingredient availability, user engagement


## Rationalities on using Smart Cooking Assistant in the context of food waste

Classical: We can see that food waste is a significant global issue (if food goes to the landfill and rots, it produces methane, but also waste of food is waste of energy etc. used to provide this food) and the importance of finding practical solutions to reduce food waste

Non-classical: It is possible to make mathematical models and estimate how much food people are wasting

Interventionist: Proposition of Smart Cooking Assistant, which may mitigate food waste


## Examples of different types of data

Analog: cooking utensils: tools and equipment used in the cooking process (pots, pans, etc.),

Digital: ingredient list provided by the IoT devices,

Primary: user input of preferences,

Secondary: incomplete cooking instructions: details about a cooking technique or step are missing,

Metadata: information about the original creator or source of a particular recipe,

Environmental: temperature outside measured with devices of system.

<div style="page-break-after: always;"></div>

## Signal path in the Smart Cooking Assistant

1. user's input of preferences
2. scanning and identifying available ingredients
3. getting a real-time weather conditions and temperature information
4. searching recipes based on ingredients and weather conditions
5. picking and ranking suitable recipes considering ingredients, weather, and user preferences
6. presenting information on the selected recipe, including ingredients and instructions
7. setting and reminding cooking times for each step in the recipe
8. collecting user's feedback


```python
import networkx as nx
import matplotlib.pyplot as plt

G = nx.DiGraph()

nodes = [
    "user's input",
    "ingredient recognition",
    "weather information",
    "searching recipes",
    "selecting and ranking recipes",
    "recipe details",
    "cooking timer",
    "user feedback"
]

edges = [
    ("user's input", "ingredient recognition"),
    ("ingredient recognition", "weather information"),
    ("weather information", "searching recipes"),
    ("searching recipes", "selecting and ranking recipes"),
    ("selecting and ranking recipes", "recipe details"),
    ("recipe details", "cooking timer"),
    ("cooking timer", "user feedback"),
    ("user feedback", "user's input")
]

G.add_nodes_from(nodes)
G.add_edges_from(edges)

pos = nx.spring_layout(G, seed=42)
nx.draw(G, pos, with_labels=True, node_size=1500, node_color='pink', 
        font_size=8, font_color='black', font_weight='bold', arrowsize=20)

plt.title("Smart Cooking Assistant Signal Path", fontsize=12)

plt.show()
```


    
![png](output_2_0.png)
    


<div style="page-break-after: always;"></div>

## Parameters of the topology

Degree of nodes indicates how many links to another one some node has. In this topology every node has degree 2.

Adjacent matrix presents the connections between the nodes.

Diameter represents the maximum number of edges that must be traversed to go from one node to another in the graph. In this case, diameter is 7.

Clustering coefficient of any node is 0, because one node's neighbours have have zero links between each other.


```python
print(G.degree())

A=nx.adjacency_matrix(G)
print(A.todense())

print(nx.diameter(G))

print(nx.clustering(G))
```

    [("user's input", 2), ('ingredient recognition', 2), ('weather information', 2), ('searching recipes', 2), ('selecting and ranking recipes', 2), ('recipe details', 2), ('cooking timer', 2), ('user feedback', 2)]
    [[0 1 0 0 0 0 0 0]
     [0 0 1 0 0 0 0 0]
     [0 0 0 1 0 0 0 0]
     [0 0 0 0 1 0 0 0]
     [0 0 0 0 0 1 0 0]
     [0 0 0 0 0 0 1 0]
     [0 0 0 0 0 0 0 1]
     [1 0 0 0 0 0 0 0]]
    7
    {"user's input": 0, 'ingredient recognition': 0, 'weather information': 0, 'searching recipes': 0, 'selecting and ranking recipes': 0, 'recipe details': 0, 'cooking timer': 0, 'user feedback': 0}
    

## Analysis of the topology as a communication network

Information flows in a clear and organized way. It starts with the user's input and goes through stages. This structured flow ensures that information reaches the right stages in the cooking process.
The loop involving user feedback and input makes the system more adaptable to individual preferences. It allows users to have a say in the cooking process, which can lead to a more personalized and user-friendly experience. 
While the system is robust in adapting to variations in user preferences, it is sensitive to failures at any stage. If any stage fails, the system stops working. But in this case, it may could be considered as a positive aspect. For example, if the device can't scan the ingredients, any recipe won't be proposed, which makes logical sense. We don't want to get a random recipe, but based on the ingredients.

<div style="page-break-after: always;"></div>

## Global food waste

This dataset (https://www.kaggle.com/datasets/joebeachcapital/food-waste) contains information about food waste from every country worldwide. It has been collected from different sources, such as retailers, households, and restaurants. The data includes food waste totals, both per-capita and absolute numbers, as well as totals for household, retail and food service sources respectively. This food waste data is from 2021.



```python
#! pip install numpy

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import warnings
import plotly.express as px
warnings.filterwarnings('ignore')

df = pd.read_csv("df.csv")

df.head()
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
      <th>Country</th>
      <th>combined figures (kg/capita/year)</th>
      <th>Household estimate (kg/capita/year)</th>
      <th>Household estimate (tonnes/year)</th>
      <th>Retail estimate (kg/capita/year)</th>
      <th>Retail estimate (tonnes/year)</th>
      <th>Food service estimate (kg/capita/year)</th>
      <th>Food service estimate (tonnes/year)</th>
      <th>Confidence in estimate</th>
      <th>M49 code</th>
      <th>Region</th>
      <th>Source</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>126</td>
      <td>82</td>
      <td>3109153</td>
      <td>16</td>
      <td>594982</td>
      <td>28</td>
      <td>1051783</td>
      <td>Very Low Confidence</td>
      <td>4</td>
      <td>Southern Asia</td>
      <td>https://www.unep.org/resources/report/unep-foo...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>127</td>
      <td>83</td>
      <td>238492</td>
      <td>16</td>
      <td>45058</td>
      <td>28</td>
      <td>79651</td>
      <td>Very Low Confidence</td>
      <td>8</td>
      <td>Southern Europe</td>
      <td>https://www.unep.org/resources/report/unep-foo...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>135</td>
      <td>91</td>
      <td>3918529</td>
      <td>16</td>
      <td>673360</td>
      <td>28</td>
      <td>1190335</td>
      <td>Very Low Confidence</td>
      <td>12</td>
      <td>Northern Africa</td>
      <td>https://www.unep.org/resources/report/unep-foo...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Andorra</td>
      <td>123</td>
      <td>84</td>
      <td>6497</td>
      <td>13</td>
      <td>988</td>
      <td>26</td>
      <td>1971</td>
      <td>Low Confidence</td>
      <td>20</td>
      <td>Southern Europe</td>
      <td>https://www.unep.org/resources/report/unep-foo...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Angola</td>
      <td>144</td>
      <td>100</td>
      <td>3169523</td>
      <td>16</td>
      <td>497755</td>
      <td>28</td>
      <td>879908</td>
      <td>Very Low Confidence</td>
      <td>24</td>
      <td>Sub-Saharan Africa</td>
      <td>https://www.unep.org/resources/report/unep-foo...</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.describe()
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
      <th>combined figures (kg/capita/year)</th>
      <th>Household estimate (kg/capita/year)</th>
      <th>Household estimate (tonnes/year)</th>
      <th>Retail estimate (kg/capita/year)</th>
      <th>Retail estimate (tonnes/year)</th>
      <th>Food service estimate (kg/capita/year)</th>
      <th>Food service estimate (tonnes/year)</th>
      <th>M49 code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>214.000000</td>
      <td>214.000000</td>
      <td>2.140000e+02</td>
      <td>214.000000</td>
      <td>2.140000e+02</td>
      <td>214.000000</td>
      <td>2.140000e+02</td>
      <td>214.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>126.794393</td>
      <td>84.294393</td>
      <td>2.658896e+06</td>
      <td>15.116822</td>
      <td>5.520454e+05</td>
      <td>27.383178</td>
      <td>1.138859e+06</td>
      <td>433.971963</td>
    </tr>
    <tr>
      <th>std</th>
      <td>22.157879</td>
      <td>18.313705</td>
      <td>8.596906e+06</td>
      <td>5.767840</td>
      <td>2.195578e+06</td>
      <td>6.508420</td>
      <td>5.380459e+06</td>
      <td>252.185786</td>
    </tr>
    <tr>
      <th>min</th>
      <td>61.000000</td>
      <td>33.000000</td>
      <td>8.500000e+02</td>
      <td>3.000000</td>
      <td>1.380000e+02</td>
      <td>3.000000</td>
      <td>2.760000e+02</td>
      <td>4.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>115.000000</td>
      <td>74.000000</td>
      <td>6.831550e+04</td>
      <td>13.000000</td>
      <td>1.250925e+04</td>
      <td>26.000000</td>
      <td>2.239125e+04</td>
      <td>219.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>123.000000</td>
      <td>80.000000</td>
      <td>5.205080e+05</td>
      <td>16.000000</td>
      <td>1.006500e+05</td>
      <td>28.000000</td>
      <td>1.884660e+05</td>
      <td>432.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>137.750000</td>
      <td>95.000000</td>
      <td>2.111314e+06</td>
      <td>16.000000</td>
      <td>3.568158e+05</td>
      <td>28.000000</td>
      <td>6.307618e+05</td>
      <td>645.250000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>260.000000</td>
      <td>189.000000</td>
      <td>9.164621e+07</td>
      <td>79.000000</td>
      <td>2.242470e+07</td>
      <td>90.000000</td>
      <td>6.537774e+07</td>
      <td>894.000000</td>
    </tr>
  </tbody>
</table>
</div>



## Which region wastes the largest amount of food?


```python
waste_by_region = df.groupby('Region')['combined figures (kg/capita/year)'].sum().sort_values(ascending=False)
waste_by_region.plot(kind='barh',title='Food wastage by region')
plt.xlabel('Food wastage (kg/capita/year)')
plt.show()
```


    
![png](output_9_0.png)
    


Exploring the data and visualising it, we find out that the region with the largest food wastage is Sub-Saharan Africa. On the other hand, Australia and New Zealand waste the least.

<div style="page-break-after: always;"></div>

## Which country wastes the most per capita (considering household estimate)?


```python
df_sorted = df.sort_values(by='Household estimate (kg/capita/year)', ascending=False)
top_10_countries = df_sorted.head(10)
plt.xticks(rotation=45) 
sns.scatterplot(x=top_10_countries['Country'], 
                y=top_10_countries['Household estimate (kg/capita/year)'])
```




    <Axes: xlabel='Country', ylabel='Household estimate (kg/capita/year)'>




    
![png](output_11_1.png)
    


<div style="page-break-after: always;"></div>

## Which country wastes the least per capita (considering household estimate)?


```python
df_sorted = df.sort_values(by='Household estimate (kg/capita/year)', ascending=True)
top_10_countries = df_sorted.head(10)
plt.xticks(rotation=45) 
sns.scatterplot(x=top_10_countries['Country'], 
                y=top_10_countries['Household estimate (kg/capita/year)'])
```




    <Axes: xlabel='Country', ylabel='Household estimate (kg/capita/year)'>




    
![png](output_13_1.png)
    


As we can see, the country which waste the most is Nigeria, and the one which waste the least is Russia.

The reason for such big food waste in Nigeria might be inadequate transport and storage facilities due to poor access to power, cold storage and drying facilities. A lot of food goes to waste because it's hard to keep it fresh.

<div style="page-break-after: always;"></div>

## How can The Smart Cooking Assistant contribute to reducing food waste?

Food waste is a big problem, but the Smart Cooking Assistant can help. This clever system suggests recipes based on what you have in your fridge, which means you'll use up ingredients instead of letting them go to waste. Even if you don't have all the ingredients for a recipe the system can suggest recipes that work with what you've got, reducing the need to buy extra stuff. It also checks the weather and suggests meals that match the conditions. On hot days, it might suggest cooler dishes to prevent fresh food from spoiling too soon. In conclusion, it is like having a personal cooking coach that's good for wallet and the environment.

## Self-evaluation of the final work

Creating the Smart Cooking Assistant was driven by a personal need. As someone who doesn't consider themselves a creative cook, I wanted a system that could suggest recipes based on what's in my fridge. The system makes cooking easier and more exciting for people like me who struggle with it. For this reason, developing it was a bit fun, since I could bring a lot of functionalties to it. 

I found some data about food waste in the world and tried to find some key information about where the largest amount of food is wasted. 

In my final project, I used everything I learned in the course, and I think it turned out pretty well. It was cool to take what I knew and make something useful out of it.

### References

https://www.kaggle.com/datasets/joebeachcapital/food-waste

https://agrilinks.org/post/nigerian-companies-are-using-innovative-approaches-reduce-food-loss-and-boost-smallholder#:~:text=Farmers%20in%20Nigeria%20lose%20about,drying%20facilities%20are%20major%20challenges.
