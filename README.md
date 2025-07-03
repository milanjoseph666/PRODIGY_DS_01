# PRODIGY_DS_01
Bar chart or Histogram to visualize the Data 
#Fetch and prepare data 

import wbpy
import pandas as pd

# Fetch total population for selected countries and years
api = wbpy.IndicatorAPI()
countries = ["IND", "USA", "CHN", "BRA", "RUS"]
dataset = api.get_dataset("SP.POP.TOTL", countries, date="2010:2020")
data = pd.DataFrame(dataset.as_dict()).reset_index()
data = data.melt(id_vars=['index'], var_name='Country', value_name='Population')
data.columns = ['Year', 'Country', 'Population']
data['Population'] = data['Population'].astype(float)
data.to_csv("world_population_2010_2020.csv", index=False)

Visualize Distribution Histogram and Bar plot

import matplotlib.pyplot as plt
import seaborn as sns

sns.set(style="whitegrid")

# A) Histogram of Global Population Values
plt.figure(figsize=(10, 5))
sns.histplot(data['Population'], bins=20, kde=True, color='skyblue')
plt.title('Global Population Distribution (2010–2020)')
plt.xlabel('Population')
plt.ylabel('Frequency')
plt.tight_layout()
plt.savefig("population_histogram.png")
plt.close()

# B) Bar chart: Population by Country in 2020
latest = data[data['Year'] == '2020']
plt.figure(figsize=(8, 5))
sns.barplot(x='Country', y='Population', data=latest, palette='Set2')
plt.title('Total Population by Country in 2020')
plt.xlabel('Country')
plt.ylabel('Population')
plt.tight_layout()
plt.savefig("population_2020_by_country.png")
plt.close()
