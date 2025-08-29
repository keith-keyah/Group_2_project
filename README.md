# `Group_2_project is a repo containing the work done by group 2`

# ANALYSIS OF CROP YIELD IN KARAMOJA REGION


# Business Understanding
An NGO seeks to provide technical support as well as farm inputs to the farmers experiencing extremely low yield in the Karamoja region of Uganda.

# Problem statement
The NGO lacks visibility into the overall state of the region and often needs to rely on some very local sources of information to prioritize their activities. Dalberg Data Insights (DDI) has been requested to develop a new food security monitoring tool to support the decision making of the NGO

# Objectives
- Total Yield per region
- Crop Type grown per region
- Yield of the different crops per region
- Correlation between population size and yield
- Correlation between allocated land and yield

# Success Criteria
Understanding the areas that have the lowest yields in order for the NGO to prioritize in terms of resource distribution.

## Data Understanding

Importing the neccessary libraries

```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
```
#

```python
# Loading the data sets
district_crop_yield = pd.read_csv('/content/Uganda_Karamoja_District_Crop_Yield_Population.csv')
district_crop_yield.head()
```

#

- Checking the structure of the dataset

```python
district_crop_yield.info()
```

```pthon
subcounty_crop_yield.info()
```

#

- Summary of descriptive statistics

```python
district_crop_yield.describe()
```

# 

- Showing and visualizing the correlation of the `numerical columns only` and presenting it in a `heat map`

```python
district_crop_yield.select_dtypes(include='number').corr()
```

```python
sns.heatmap(district_crop_yield.select_dtypes(include='number').corr(), annot=True)
plt.rcParams['figure.figsize']=(20,10)
plt.show()
```
<img width="1526" height="819" alt="image" src="https://github.com/user-attachments/assets/141e0251-635f-4583-9f13-57eb8da91c34" />

#

`1.0` - perfect positive correlation

`0.0` - no correlation

`-1.0` - perfect negative correlation

The same can be done for `subcounty_crop_yield`

## DATA CLEANING

- checking for null or missing values in both datasets

```python
district_crop_yield.isna().sum()
```
```python
subcounty_crop_yield.isna().sum()
```

There are no null values found in both datasets

#

- Renaming the `district column` under a common name to enable data blending in `Tableu`

```python
district_crop_yield = district_crop_yield.rename(columns={'NAME':'DISTRICT_NAME'})
district_crop_yield.head()
```

#

- Creating new fields to show total production per district and sub-county

```python
district_crop_yield['Total_Yield'] = district_crop_yield['M_Prod_Tot'] + district_crop_yield['S_Prod_Tot']
district_crop_yield.head()
```

Same can be done for the `subcounty` dataset

#

- Checking for duplicates in both datasets

```python
district_crop_yield.duplicated().sum()
```
```python
subcounty_crop_yield.duplicated().sum()
```

## EDA - Exploratory Data Analysis

- Creating a bar chart to compare the `mean` for maize and sorghum yields for `district yield` 

```python
mean_S_Yield_dist = district_crop_yield['S_Yield_Ha'].mean()
mean_M_Yield_dist = district_crop_yield['M_Yield_Ha'].mean()

labels = ['Mean Sorghum Yield', 'Mean Maize Yield']
means = [mean_S_Yield_dist, mean_M_Yield_dist]

plt.bar(labels, means, color=['skyblue', 'lightgreen'])
plt.ylabel('Yield (Kg/Ha)')
plt.title('Comparison of Mean Sorghum and Maize Yields Across Districts')
plt.show()
```
<img width="1634" height="836" alt="image" src="https://github.com/user-attachments/assets/c1883910-8d11-42c3-b49d-1ebb9874b0dc" />

#

- To find out if A HIGHER POPULATION LEADs TO A HIGHER CROP YIELD IN AN AREA, We are going to work on comparing the mean total district yields to the mean total population in order to come up with a sound conclusion.

```python
#mean district population
mean_dist_population = district_crop_yield['POP'].mean()
```
output = np.float64(214943.57142857142)
```python
#total yields
district_crop_yield['total_yields'] = district_crop_yield['S_Yield_Ha'] + district_crop_yield['M_Yield_Ha']
```
```python
#mean total crop yields
district_crop_yield['total_yields'] = district_crop_yield['S_Yield_Ha'] + district_crop_yield['M_Yield_Ha']
district_crop_yield['total_yields'].mean()
```
output = np.float64(1255.4285714285713)

- plotting a bar graph comparing the mean total yield against the mean population

<img width="490" height="490" alt="image" src="https://github.com/user-attachments/assets/99c49766-eb5d-4547-aa39-14cd7684c4f1" />

#

- `Scatter plot` showing the correlation of population density against total yields

<img width="1634" height="855" alt="image" src="https://github.com/user-attachments/assets/88708cce-d6fd-4a38-b2a6-cad6712b8bd8" />

#

A higher population concentration in an area does not, in fact, lead to a higher count of crop yields in said area, even though they may seem highly related when using a bar graph.

#

HOW DOES THE AMOUNT OF LAND ALLOCATED TO CROP GROWTH IMPACT INTEGRAL QUANTITY OF CROP YIELDS IN THE DISTRICTS.

- Here we will come up with three visualisations comparing:

- maize yields against maize area per hectares
- sougham yields against sougham area per hectares
- total yields against total crop area per hectares

- `finding mean of sougham area per hectare district-wise`
- `finding mean of maize area per hectare district-wise`
- `finding the total crop area`
- `finding mean of the total crop area`

#

- Plotting a bar graph to compare mean soghurm area and mean soghurm yield

```python
fig, ax1 = plt.subplots(figsize=(8, 6))

labels = ['Mean Sorghum Area', 'Mean Sorghum Yield']
means_area = [mean_sougham_area]
means_yield = [mean_S_Yield_dist]

# Bar chart for mean sorghum area on the left y-axis
rects1 = ax1.bar(labels[0], means_area, color='salmon', width=0.4)
ax1.set_ylabel('Area (Ha)')
ax1.set_title('Comparison of Mean Sorghum Area and Mean Sorghum Yield')
ax1.tick_params(axis='y')

# Create a second y-axis for mean sorghum yield
ax2 = ax1.twinx()
rects2 = ax2.bar(labels[1], means_yield, color='skyblue', width=0.4)
ax2.set_ylabel('Yield (Kg/Ha)')
ax2.tick_params(axis='y')

fig.tight_layout()
plt.show()
```
#
<img width="790" height="590" alt="image" src="https://github.com/user-attachments/assets/7f088f77-3275-42d3-a0d2-cd0eea725254" />

#

- Comparing mean maize area and mean maize yield

<img width="790" height="590" alt="image" src="https://github.com/user-attachments/assets/cf14e68b-8014-4dcf-a68a-86126183d880" />

#

- finding the correlation of total crop area against total crop yields and plotting a scatter plot for visualization

```python
corr_disttca_v_disttcy = np.corrcoef(district_crop_yield['total_crop_area'],district_crop_yield['total_yields'])
corr_disttca_v_disttcy[0,1]
```
`output = np.float64(0.2953194074238365)`

#
<img width="1634" height="855" alt="image" src="https://github.com/user-attachments/assets/0a9598b0-44ad-4389-8734-1d58b61e3528" />

#
*The amount of land allocated to crop growth does indeed affect the total crop yield of the districts, though not greatly, due to the possibility of underlying factors such as soil fertility and climate change in the districts.*

#

Now we will look into how the Sub-county crop yield relates with certain factors such as population and total area allocated for plant growth.

*Firstly we will check how the total yield of the subcounties relates to the total area allocated to crop growth.*








