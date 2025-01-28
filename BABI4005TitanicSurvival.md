# Titanic Survival Data Analysis

## **Introduction**
This analysis explores survival patterns in the Titanic dataset. We aim to understand how different factors such as **gender, passenger class, age, and family size** influenced survival rates. The following visualizations will help uncover key insights.

---


## **Step 1: Load the Data**
We first import the dataset and display the first five rows.


```python
import pandas as pd

# Load the Titanic dataset
df = pd.read_csv(r"C:\Users\Hp\Downloads\titanic.csv")

# Display first few rows
from IPython.display import display
display(df.head())

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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>892</td>
      <td>0</td>
      <td>3</td>
      <td>Kelly, Mr. James</td>
      <td>male</td>
      <td>34.5</td>
      <td>0</td>
      <td>0</td>
      <td>330911</td>
      <td>7.8292</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>1</th>
      <td>893</td>
      <td>1</td>
      <td>3</td>
      <td>Wilkes, Mrs. James (Ellen Needs)</td>
      <td>female</td>
      <td>47.0</td>
      <td>1</td>
      <td>0</td>
      <td>363272</td>
      <td>7.0000</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>2</th>
      <td>894</td>
      <td>0</td>
      <td>2</td>
      <td>Myles, Mr. Thomas Francis</td>
      <td>male</td>
      <td>62.0</td>
      <td>0</td>
      <td>0</td>
      <td>240276</td>
      <td>9.6875</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>3</th>
      <td>895</td>
      <td>0</td>
      <td>3</td>
      <td>Wirz, Mr. Albert</td>
      <td>male</td>
      <td>27.0</td>
      <td>0</td>
      <td>0</td>
      <td>315154</td>
      <td>8.6625</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>896</td>
      <td>1</td>
      <td>3</td>
      <td>Hirvonen, Mrs. Alexander (Helga E Lindqvist)</td>
      <td>female</td>
      <td>22.0</td>
      <td>1</td>
      <td>1</td>
      <td>3101298</td>
      <td>12.2875</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>


## **Step 2: Understanding Data Structure**
- There are 418 passengers in this dataset.
- The **"Age"** and **"Cabin"** columns have missing values.
- The **"Survived"** column (0 = No, 1 = Yes) is our target variable.



```python
df.shape  # This gives (number_of_rows, number_of_columns)

```




    (418, 12)




```python
# Check dataset structure
df.info()

```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 418 entries, 0 to 417
    Data columns (total 12 columns):
     #   Column       Non-Null Count  Dtype  
    ---  ------       --------------  -----  
     0   PassengerId  418 non-null    int64  
     1   Survived     418 non-null    int64  
     2   Pclass       418 non-null    int64  
     3   Name         418 non-null    object 
     4   Sex          418 non-null    object 
     5   Age          332 non-null    float64
     6   SibSp        418 non-null    int64  
     7   Parch        418 non-null    int64  
     8   Ticket       418 non-null    object 
     9   Fare         417 non-null    float64
     10  Cabin        91 non-null     object 
     11  Embarked     418 non-null    object 
    dtypes: float64(2), int64(5), object(5)
    memory usage: 39.3+ KB
    

## **Step 3: Handling Missing Values**
- **Age** has missing values, so we will fill them with the median age.
- **Cabin** has many missing values, so we will drop it.



```python
# Count missing values in each column
df.isnull().sum()
```




    PassengerId      0
    Survived         0
    Pclass           0
    Name             0
    Sex              0
    Age             86
    SibSp            0
    Parch            0
    Ticket           0
    Fare             1
    Cabin          327
    Embarked         0
    dtype: int64



## **Step 4: Data Summary**
This table provides an overview of the numerical columns:

- The **average passenger age** is approximately **30.27 years**.
- The **highest fare paid** was **$512.33**.
- The **survival rate** is approximately **36.37%** (since the mean of "Survived" is 0.363636, meaning about 36% of passengers survived).




```python
# Summary statistics
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>418.000000</td>
      <td>418.000000</td>
      <td>418.000000</td>
      <td>332.000000</td>
      <td>418.000000</td>
      <td>418.000000</td>
      <td>417.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1100.500000</td>
      <td>0.363636</td>
      <td>2.265550</td>
      <td>30.272590</td>
      <td>0.447368</td>
      <td>0.392344</td>
      <td>35.627188</td>
    </tr>
    <tr>
      <th>std</th>
      <td>120.810458</td>
      <td>0.481622</td>
      <td>0.841838</td>
      <td>14.181209</td>
      <td>0.896760</td>
      <td>0.981429</td>
      <td>55.907576</td>
    </tr>
    <tr>
      <th>min</th>
      <td>892.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.170000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>996.250000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>21.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>7.895800</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1100.500000</td>
      <td>0.000000</td>
      <td>3.000000</td>
      <td>27.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>14.454200</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1204.750000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>39.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>31.500000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1309.000000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>76.000000</td>
      <td>8.000000</td>
      <td>9.000000</td>
      <td>512.329200</td>
    </tr>
  </tbody>
</table>
</div>



# Titanic Data Analysis:


## **Understanding Survival Patterns Through Visualizations**
In this section, we explore **various factors that influenced survival rates** on the Titanic using data visualizations. The following charts will help us analyze:
- **Overall survival distribution**
- **Impact of passenger class on survival**
- **Effect of age on survival chances**
- **How family size influenced survival rates**

Each visualization is accompanied by a brief explanation of its key insights.

---





```python
import seaborn as sns
import matplotlib.pyplot as plt

# Count of survivors with labeled bars
ax = sns.countplot(x="Survived", data=df, palette="coolwarm")
plt.title("Survival Count")

# Remove x-axis label
plt.xlabel("")  # This removes the default "Survived" label under the x-axis

plt.ylabel("Count")
plt.xticks([0, 1], ["Did Not Survive", "Survived"])  # Custom labels

# Add labels on bars
for p in ax.patches:
    ax.annotate(f"{int(p.get_height())}",  
                (p.get_x() + p.get_width() / 2, p.get_height() + 5),  # Center and space text
                ha="center", fontsize=12, fontweight="bold", color="black")

plt.ylim(0, max(df["Survived"].value_counts()) + 30)  # Extend y-axis for spacing
plt.show()

```


    
![png](output_11_0.png)
    


## **Survival Count**
- **266 passengers did not survive**, while **152 passengers survived**.
- The survival rate is **approximately 36%**, meaning most passengers did not make it.
- This highlights the **severity of the Titanic disaster** and the **limited number of lifeboats**.
- The graph makes it clear that **survival was not evenly distributed**, and many people lost their lives.




```python
# Survival rate by passenger class with labeled bars
ax = sns.barplot(x="Pclass", y="Survived", data=df, palette="magma", ci=None)
plt.title("Survival Rate by Passenger Class")
plt.xlabel("Passenger Class (1 = First, 2 = Second, 3 = Third)")
plt.ylabel("Survival Probability")

# Add labels on bars
for p in ax.patches:
    ax.annotate(f"{p.get_height():.2f}", (p.get_x() + 0.3, p.get_height() + 0.02))

plt.show()

```


    
![png](output_13_0.png)
    


## **Survival Rate by Passenger Class**
- **First-class passengers had the highest survival probability (47%).**
- **Second-class passengers had a survival probability of 32%.**
- **Third-class passengers had a survival probability of 33%, only slightly higher than second-class.**
- This confirms that **wealth and social status played a role in survival chances** on the Titanic.
- **First-class passengers had better access to lifeboats**, increasing their chances of survival.



```python
plt.figure(figsize=(8, 6)) 

# Plot non-survivors
sns.histplot(df[df["Survived"] == 0]["Age"], bins=20, kde=True, color="red", label="Did Not Survive", alpha=0.6)
# Plot survivors 
sns.histplot(df[df["Survived"] == 1]["Age"], bins=20, kde=True, color="green", label="Survived", alpha=0.8)


plt.legend(facecolor="white", loc="upper right")  
plt.title("Age Distribution of Survivors vs. Non-Survivors")
plt.xlabel("Age")
plt.ylabel("Count")
plt.show()



```


    
![png](output_15_0.png)
    


## **Age Distribution of Survivors vs. Non-Survivors**
- **Children (0-10 years) had a higher survival rate**, likely due to the **"Women and children first"** policy.
- **Young adults (20-40 years) had the highest number of deaths**, as seen from the high concentration of red bars.
- **Elderly passengers (60+) had low survival rates**, indicating that older individuals were less likely to escape.
- The overall trend shows that **survival chances varied significantly by age**, reinforcing the idea that **younger passengers were prioritized for rescue**.



```python
# Create FamilySize column
df["FamilySize"] = df["SibSp"] + df["Parch"] + 1  # +1 includes the passenger themselves

# Group large family sizes into categories
df["FamilyGroup"] = df["FamilySize"].apply(lambda x: "1" if x == 1 else 
                                                    "2-4" if 2 <= x <= 4 else 
                                                    "5+")

# Plot the updated family survival rate
ax = sns.barplot(x="FamilyGroup", y="Survived", data=df, palette="coolwarm", ci=None)

plt.title("Survival Rate by Family Size Group")
plt.xlabel("Family Size Group")
plt.ylabel("Survival Probability")
plt.ylim(0, 1)  # Keep survival rates between 0-1 for better readability

# Add labels on bars
for p in ax.patches:
    ax.annotate(f"{p.get_height():.2f}", (p.get_x() + 0.3, p.get_height() + 0.02))

plt.show()

```


    
![png](output_17_0.png)
    


## **Survival Rate by Family Size**
- **Small families (2-4 members) had the highest survival rate (~52%).**  
- **Single travelers had a lower survival rate (~27%).**  
- **Large families (5+ members) had the lowest survival rate (~40%).**  
- Having **some family helped**, but **larger families struggled to evacuate together**, reducing their chances of survival.  
- This trend suggests that **being alone was risky, but too many family members also reduced survival chances** due to logistical challenges.



# **Final Summary & Conclusion**
- **Most passengers did not survive** (~36% survival rate).
- **Women had a much higher survival rate than men** (supporting "Women and children first").
- **First-class passengers had the best survival rate**, while third-class had the worst.
- **Children had a higher chance of survival**, while adults (especially men) had lower survival rates.
- **Small families had better survival chances than solo travelers or large families.**

These insights confirm that **social status, gender, and family size played a key role in survival.**



```python

```
