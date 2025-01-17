# Ex-07-Feature-Selection
## AIM
To Perform the various feature selection techniques on a dataset and save the data to a file. 

# Explanation
Feature selection is to find the best set of features that allows one to build useful models.
Selecting the best features helps the model to perform well. 

# ALGORITHM
### STEP 1
Read the given Data
### STEP 2
Clean the Data Set using Data Cleaning Process
### STEP 3
Apply Feature selection techniques to all the features of the data set
### STEP 4
Save the data to the file

# CODE
```

Developed by: Harini.B
Register Number: 212221230035

from sklearn.datasets import load_boston
import pandas as pd
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
import seaborn as sns
import statsmodels.api as sm
%matplotlib inline
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.feature_selection import RFE
from sklearn.linear_model import RidgeCV, LassoCV, Ridge, Lasso

from sklearn.datasets import load_boston
boston = load_boston()

print(boston['DESCR'])

import pandas as pd
df = pd.DataFrame(boston['data'] )
df.head()

df.columns = boston['feature_names']
df.head()

df['PRICE']= boston['target']
df.head()

df.info()

plt.figure(figsize=(10, 8))
sns.distplot(df['PRICE'], rug=True)
plt.show()

#FILTER METHODS
X=df.drop("PRICE",1)
y=df["PRICE"]

from sklearn.feature_selection import SelectKBest, chi2
X, y = load_boston(return_X_y=True)
X.shape

#1.VARIANCE THRESHOLD
from sklearn.feature_selection import VarianceThreshold
selector = VarianceThreshold()
selector.fit_transform(X)

#2.INFORMATION GAIN/MUTUAL INFORMATION
from sklearn.feature_selection import mutual_info_regression
mi = mutual_info_regression(X, y);
mi = pd.Series(mi)
mi.sort_values(ascending=False)
mi.sort_values(ascending=False).plot.bar(figsize=(10, 4))

#3.SELECTKBEST METHOD
from sklearn.feature_selection import f_classif
from sklearn.feature_selection import SelectKBest,SelectPercentile
skb = SelectKBest(score_func=f_classif, k=2) 
X_data_new = skb.fit_transform(X, y)
print('Number of features before feature selection: {}'.format(X.shape[1]))
print('Number of features after feature selection: {}'.format(X_data_new.shape[1]))

#4.CORRELATION COEFFICIENT
cor=df.corr()
sns.heatmap(cor,annot=True)

#5.MEAN ABSOLUTE DIFFERENCE
mad=np.sum(np.abs(X-np.mean(X,axis=0)),axis=0)/X.shape[0]
plt.bar(np.arange(X.shape[1]),mad,color='teal')

#Processing data into array type.
from sklearn import preprocessing
lab = preprocessing.LabelEncoder()
y_transformed = lab.fit_transform(y)
print(y_transformed)

#6.CHI SQUARE TEST
X = X.astype(int)
chi2_selector = SelectKBest(chi2, k=2)
X_kbest = chi2_selector.fit_transform(X, y_transformed)
print('Original number of features:', X.shape[1])
print('Reduced number of features:', X_kbest.shape[1])

#7.SELECT PERCENTILE METHOD
X_new = SelectPercentile(chi2, percentile=10).fit_transform(X, y_transformed)
X_new.shape

#WRAPPER METHOD

#1.FORWARD FEATURE SELECTION

from mlxtend.feature_selection import SequentialFeatureSelector as SFS
from sklearn.linear_model import LinearRegression
sfs = SFS(LinearRegression(),
          k_features=10,
          forward=True,
          floating=False,
          scoring = 'r2',
          cv = 0)
sfs.fit(X, y)
sfs.k_feature_names_

#2.BACKWARD FEATURE ELIMINATION

sbs = SFS(LinearRegression(),
         k_features=10,
         forward=False,
         floating=False,
         cv=0)
sbs.fit(X, y)
sbs.k_feature_names_

#3.BI-DIRECTIONAL ELIMINATION

sffs = SFS(LinearRegression(),
         k_features=(3,7),
         forward=True,
         floating=True,
         cv=0)
sffs.fit(X, y)
sffs.k_feature_names_

#4.RECURSIVE FEATURE SELECTION

from sklearn.feature_selection import RFE
lr=LinearRegression()
rfe=RFE(lr,n_features_to_select=7)
rfe.fit(X, y)
print(X.shape, y.shape)
rfe.transform(X)
rfe.get_params(deep=True)
rfe.support_
rfe.ranking_

#EMBEDDED METHOD

#1.RANDOM FOREST IMPORTANCE

from sklearn.ensemble import RandomForestClassifier
model = RandomForestClassifier().fit(X,y_transformed)
importances=model.feature_importances_

final_df=pd.DataFrame({"Features":pd.DataFrame(X).columns,"Importances":importances})
final_df.set_index("Importances")
final_df=final_df.sort_values("Importances")
final_df.plot.bar(color="teal")
```
# OUPUT
![1](https://user-images.githubusercontent.com/93427253/169952267-113c27c1-dfb6-4842-b415-9bbbcd3a7f90.png)
### Analyzing the boston dataset:
![2](https://user-images.githubusercontent.com/93427253/169952489-1249c5f9-b03a-44ce-8b2b-f8b3a353d793.png)
![3](https://user-images.githubusercontent.com/93427253/169952541-a0bc28b7-461c-403e-ad34-df69a97ba0e2.png)
![4](https://user-images.githubusercontent.com/93427253/169953310-cf018d1c-f7fc-416f-86a5-06fb551b86ab.png)
### Analyzing dataset using Distplot:
![5](https://user-images.githubusercontent.com/93427253/169953248-83452be0-8696-4bfd-b4ea-e68768e21372.png)
## Filter Methods:
### Variance Threshold:
![6](https://user-images.githubusercontent.com/93427253/169953547-feffef87-405a-456d-830b-678ab11cb715.png)
### Information Gain:
![7](https://user-images.githubusercontent.com/93427253/169953685-347913d9-9add-4423-acee-4377b8b946a8.png)
### SelectKBest Model:
![8](https://user-images.githubusercontent.com/93427253/169953742-0e3aed23-3bfe-4787-8e9e-6c29e7d00167.png)
### Correlation Coefficient:
![9](https://user-images.githubusercontent.com/93427253/169953835-132aa4c7-be2a-4915-b750-4b847ae85b26.png)
### Mean Absolute difference:
![10](https://user-images.githubusercontent.com/93427253/169953926-2130461e-2c8a-412b-bd9e-fe51db739b96.png)
### Chi Square Test:
![11](https://user-images.githubusercontent.com/93427253/169954049-56eea74b-dfcd-4faa-9887-0da9612dfc8d.png)
![12](https://user-images.githubusercontent.com/93427253/169954103-4d9b05ec-1b81-4bd4-85d3-368e4edd65cd.png)
### SelectPercentile Method:
![13](https://user-images.githubusercontent.com/93427253/169954150-a44ae78a-c0f5-437b-942c-3533d37bb004.png)
## Wrapper Methods:
### Forward Feature Selection:
![14](https://user-images.githubusercontent.com/93427253/169954252-f3531c9e-fdf3-41c3-bfc1-9e318664a9e4.png)
### Backward Feature Selection:
![15](https://user-images.githubusercontent.com/93427253/169954323-de1d3d87-bdc7-4896-8826-e3e0312d28a5.png)
### Bi-Directional Elimination: 
![16](https://user-images.githubusercontent.com/93427253/169954374-6a1349a7-9fb4-4012-8016-00c9f0df7bd8.png)
### Recursive Feature Selection:
![17](https://user-images.githubusercontent.com/93427253/169954421-47f5d484-843a-4e9c-9d0c-8d3c53df8783.png)
## Embedded Methods:
### Random Forest Importance:
![18](https://user-images.githubusercontent.com/93427253/169954484-9234e548-2adf-40dd-8a57-af61b0f95ec3.png)

# RESULT:
Hence various feature selection techniques are applied to the given data set successfully and saved the data into a file.
