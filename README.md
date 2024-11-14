# Data---Science-Analyzing-the-Impact-of-Environmental-and-Factors-on-Housing-Prices-in-Boston

```
CODE :
import piplite
await piplite.install(['numpy'],['pandas'])
await piplite.install(['seaborn'])
import pandas as pd
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as pyplot
import scipy.stats
import statsmodels.api as sm
from statsmodels.formula.api import ols
from js import fetch
import io

URL = 'https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-ST0151EN-SkillsNetwork/labs/boston_housing.csv'
resp = await fetch(URL)
boston_url = io.BytesIO((await resp.arrayBuffer()).to_py())
boston_df=pd.read_csv(boston_url)

# Importing required libraries for plotting
import matplotlib.pyplot as plt

# Define the Null and Alternate Hypotheses for the impact of 'DIS' on 'MEDV'

# Null Hypothesis (H0): The distance to employment centers (DIS) has no effect on median home value (MEDV)
# Alternate Hypothesis (H1): The distance to employment centers (DIS) has a significant effect on median home value (MEDV)


# 1. Boxplot for "Median value of owner-occupied homes" (MEDV)
plt.figure(figsize=(8,6))
sns.boxplot(x=boston_df['MEDV'])
plt.title('Boxplot of Median Value of Owner-Occupied Homes (MEDV)')
plt.xlabel('MEDV')
plt.show()

# 2. Bar plot for the "Charles River" variable (binary: 1 if tract bounds the river, 0 otherwise)
plt.figure(figsize=(8,6))
sns.countplot(x='CHAS', data=boston_df)
plt.title('Bar Plot of Charles River (CHAS) Variable')
plt.xlabel('Charles River Boundary (1=Yes, 0=No)')
plt.ylabel('Count')
plt.show()

# 3. Boxplot for "MEDV" vs. "AGE" (Discretized AGE into 3 groups)
# Discretizing the AGE variable into 3 groups
bins = [0, 35, 70, boston_df['AGE'].max()]
labels = ['<= 35', '35-70', '>= 70']
boston_df['AGE_GROUP'] = pd.cut(boston_df['AGE'], bins=bins, labels=labels, right=False)

# Boxplot of MEDV vs. AGE_GROUP
plt.figure(figsize=(8,6))
sns.boxplot(x='AGE_GROUP', y='MEDV', data=boston_df)
plt.title('Boxplot of MEDV vs. AGE Group')
plt.xlabel('Age Group')
plt.ylabel('Median Value of Owner-Occupied Homes (MEDV)')
plt.show()

# 4. Scatter plot of Nitric oxide concentrations vs. Proportion of non-retail business acres
plt.figure(figsize=(8,6))
sns.scatterplot(x='NOX', y='INDUS', data=boston_df)
plt.title('Scatter Plot of Nitric Oxide Concentrations vs. Proportion of Non-Retail Business Acres')
plt.xlabel('Nitric Oxide Concentrations (NOX)')
plt.ylabel('Proportion of Non-Retail Business Acres (INDUS)')
plt.show()

# 5. Histogram for the pupil-to-teacher ratio (PTRATIO)
plt.figure(figsize=(8,6))
sns.histplot(boston_df['PTRATIO'], kde=True, bins=20)
plt.title('Histogram of Pupil-Teacher Ratio (PTRATIO)')
plt.xlabel('Pupil-Teacher Ratio')
plt.ylabel('Frequency')from scipy.stats import f_oneway

# Creating bins for AGE proportion (owner-occupied units built prior to 1940)
boston_df['AGE_BINS'] = pd.cut(boston_df['AGE'], bins=3, labels=["Low", "Medium", "High"])

# Grouping the data by the 'AGE_BINS' category
group1 = boston_df[boston_df['AGE_BINS'] == 'Low']['MEDV']
group2 = boston_df[boston_df['AGE_BINS'] == 'Medium']['MEDV']
group3 = boston_df[boston_df['AGE_BINS'] == 'High']['MEDV']

# Perform ANOVA test
f_stat, p_value = f_oneway(group1, group2, group3)

# Hypothesis testing
if p_value < alpha:
    conclusion = "Reject the null hypothesis. There is a significant difference in the median values of houses based on the proportion of units built before 1940."
else:
    conclusion = "Fail to reject the null hypothesis. There is no significant difference in the median values of houses."

f_stat, p_value, conclusion

plt.show()

from scipy.stats import ttest_ind

# Split the data into two groups based on CHAS
group1 = boston_df[boston_df['CHAS'] == 1]['MEDV']  # Houses near the Charles River
group2 = boston_df[boston_df['CHAS'] == 0]['MEDV']  # Houses not near the Charles River

# Perform the T-test
t_stat, p_value = ttest_ind(group1, group2)

# Hypothesis testing
alpha = 0.05
if p_value < alpha:
    conclusion = "Reject the null hypothesis. There is a significant difference in the median values of houses."
else:
    conclusion = "Fail to reject the null hypothesis. There is no significant difference in the median values of houses."

t_stat, p_value, conclusion



from scipy.stats import pearsonr

# Perform Pearson correlation
corr_stat, p_value = pearsonr(boston_df['NOX'], boston_df['INDUS'])

# Hypothesis testing
if p_value < alpha:
    conclusion = "Reject the null hypothesis. There is a significant relationship between Nitric Oxide concentrations and the proportion of non-retail business acres."
else:
    conclusion = "Fail to reject the null hypothesis. There is no significant relationship between Nitric Oxide concentrations and the proportion of non-retail business acres."

corr_stat, p_value, conclusion


import statsmodels.api as sm

# Define the independent variable (X) and dependent variable (y)
X = boston_df['DIS']
y = boston_df['MEDV']

# Add constant to the independent variable matrix
X = sm.add_constant(X)

# Perform regression analysis
model = sm.OLS(y, X).fit()

# Get the results
summary = model.summary()

# Hypothesis testing: check the p-value for 'DIS' to conclude significance
if model.pvalues['DIS'] < alpha:
    conclusion = "Reject the null hypothesis. The weighted distance to employment centers significantly impacts the median value of homes."
else:
    conclusion = "Fail to reject the null hypothesis. The weighted distance to employment centers does not significantly impact the median value of homes."

summary, conclusion


# Interpretation of the DIS coefficient
effect_interpretation = (
    f"The coefficient of DIS is {dis_coefficient:.3f}. This means that for every additional unit increase in "
    f"weighted distance to the five Boston employment centers, the median value of owner-occupied homes changes "
    f"by approximately {dis_coefficient:.3f} units. "
    f"{'This effect is statistically significant.' if dis_p_value < alpha else 'This effect is not statistically significant.'}"
)

# Display results
print(model.summary())
print("\nConclusion:", conclusion)
print("Effect Interpretation:", effect_interpretation)
```


## OUTPUT:

![Screenshot 2024-11-13 210336](https://github.com/user-attachments/assets/cc673601-299d-4759-8c9a-f74ae92f50eb)
![Screenshot 2024-11-13 210352](https://github.com/user-attachments/assets/76dd6315-a89d-4589-9591-75907f51479a)

![Screenshot 2024-11-13 210416](https://github.com/user-attachments/assets/60eb878d-9792-4b2b-98a4-fba9be1c30bf)

![Screenshot 2024-11-13 210520](https://github.com/user-attachments/assets/9d65c0d6-1c15-4883-a658-0dd3c28ac312)

![Screenshot 2024-11-13 210543](https://github.com/user-attachments/assets/7b1b1d2f-9c38-4dea-bc76-e88d5db35221)

![Screenshot 2024-11-13 210613](https://github.com/user-attachments/assets/50a3b9f7-061b-4375-8d79-d1f7c08a3cd2)






