# EDA Cheat Sheet (Exploratory Data Analysis)

A concise, practical reference for performing EDA in **Python (pandas)** and **R (tidyverse)**.  
Covers univariate, bivariate, multivariate analysis, missing values, and quick visualizations.

---

# 1. Import + Basic Setup

## Python
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("data.csv")
df.head()
```

## R
```r
library(tidyverse)
df <- read_csv("data.csv")
head(df)
```

---

# 2. Dataset Overview

## Python
```python
df.shape
df.info()
df.describe(include='all')
df.nunique()
```

## R
```r
glimpse(df)
summary(df)
df %>% summarise_all(n_distinct)
```

---

# 3. Missing Values

## Python
```python
df.isna().sum()
df.dropna()
df.fillna(0)
```

## R
```r
colSums(is.na(df))
df %>% drop_na()
df %>% replace_na(list(col = 0))
```

---

# 4. Categorical Variables

## Python
```python
df['col'].value_counts()
df['col'].value_counts(normalize=True)
```

## R
```r
table(df$col)
prop.table(table(df$col))
```

---

# 5. Numerical Variables

## Python
```python
df['col'].describe()
df['col'].plot(kind='hist')
sns.boxplot(x=df['col'])
```

## R
```r
summary(df$col)
hist(df$col)
boxplot(df$col)
```

---

# 6. Bivariate Analysis

## Python
### Numeric vs Numeric
```python
df.corr()
sns.scatterplot(data=df, x='x', y='y')
sns.heatmap(df.corr(), annot=True)
```

### Categorical vs Numeric
```python
sns.boxplot(x='category', y='value', data=df)
sns.barplot(x='category', y='value', data=df)
```

## R
```r
df %>% select_if(is.numeric) %>% cor()
ggplot(df, aes(x, y)) + geom_point()
ggplot(df, aes(category, value)) + geom_boxplot()
```

---

# 7. Multivariate Analysis

## Python
```python
sns.pairplot(df.select_dtypes(include=np.number))
sns.heatmap(df.corr(), annot=True)
```

## R
```r
GGally::ggpairs(df)
corrplot::corrplot(cor(df %>% select_if(is.numeric)), method="color")
```

---

# 8. Outlier Detection

## Python
```python
Q1 = df['col'].quantile(0.25)
Q3 = df['col'].quantile(0.75)
IQR = Q3 - Q1
outliers = df[(df['col'] < Q1 - 1.5*IQR) | (df['col'] > Q3 + 1.5*IQR)]
```

## R
```r
Q1 <- quantile(df$col, 0.25)
Q3 <- quantile(df$col, 0.75)
IQR <- Q3 - Q1
outliers <- df %>% filter(col < Q1 - 1.5*IQR | col > Q3 + 1.5*IQR)
```

---

# 9. Quick EDA Pipelines

## Python (Pandas Profiling)
```python
from pandas_profiling import ProfileReport
ProfileReport(df)
```

## R (skimr)
```r
skimr::skim(df)
```

---

# 10. Handy One-Liners

| Task | Python | R |
|------|--------|---|
| Count duplicates | `df.duplicated().sum()` | `sum(duplicated(df))` |
| Drop duplicates | `df.drop_duplicates()` | `df %>% distinct()` |
| Rename columns | `df.rename(columns={"old":"new"})` | `df %>% rename(new = old)` |
| Filter rows | `df[df['col'] > 5]` | `df %>% filter(col > 5)` |
| Select columns | `df[['a','b']]` | `df %>% select(a,b)` |

---

# 11. Mini Checklist

- [ ] Load + inspect data  
- [ ] Check missing values  
- [ ] Examine variable types  
- [ ] Explore categorical variables  
- [ ] Explore numeric variables  
- [ ] Check correlations  
- [ ] Detect outliers  
- [ ] Visualize distributions  
- [ ] Document anomalies / insights  

---
