---
layout: post
title:  "Kaggle: Machine Learning from Disaster (Binary Classification)"
date:   2020-03-16 17:28:03 -0400
categories: Interesting-projects
author: Ningxiao Zhang
permalink: "/posts/machine-learning-from-disaster/"
---

This is a Kaggle competition for a binary classification problem. You are asked to build a predictive model that can predict if a passenger or crew survived or not in the most infamous shipwreck -- The sinking of the Titanic. Your prediction is based on the passenger data (such as name, age, gender, socio-economic class, etc). I am going to use this project to indicate how I deal with binary classification problem.

I am going to generate models as follows:

1. [Prepare data](#Prepare-data)
2. [Perform exploratory data analysis](#perform-eda)
3. [Model data](#model-data)
4. [Submit result](#submit-res)

Import packages
```css
import pandas as pd
import matplotlib.pyplot as plt
```

## 1.Prepare data {#Prepare-data}

```css
df_train = pd.read_csv('data/train.csv')
df_test = pd.read_csv('data/test.csv')
```

Both train and test data have the same data structure. The train data has 12 columns: PassengerId, Survived, Pclass, Name, Sex, Age, SibSp, Parch, Ticket, Fare, Cabin, Embarked. The test data has similar columns except Survived which is the value we need to predict.

```css
df_train.head(10)
```
<img src="../../assets/img/titanic/data.png" alt="data" width="800"/>

```css
df_train.info()
df_test.info()
```
<img src="../../assets/img/titanic/train_info.png" alt="train_info" width="350"/><img src="../../assets/img/titanic/test_info.png" alt="test_info" width="350"/>

From the information above, in the train data the column 'Age', column 'Cabin', and column 'Embarked' have missing data and in the test data the column 'Age', column 'Fare', and column 'Cabin' have missing data.

1) Extract useful columns to new data frame

The passenger ID and Name are two columns that just for identify the person and might have few impact to our binary classification. Thus, I drop both of these two columns. The ticket and Cabin information can be represented by Fare and Embarked. So I also drop these two columns.

```css
data_train = df_train.copy()
data_test = df_test.copy()
drop_columns=['PassengerId','Name','Ticket','Cabin']
data_train.drop(drop_columns,axis=1,inplace=True)
data_test.drop(drop_columns,axis=1,inplace=True)
```
2) Check if there is any unreasonable data point

Sometimes a data set might have a wrong data (For example, a person with age 1000). To prevent this, we need to have a quick check of the data first. And our data looks good in this checking.

```css
data_train.describe()
data_test.describe()
```

<img src="../../assets/img/titanic/train_describe.png" alt="train_info" width="400"/><img src="../../assets/img/titanic/test_describe.png" alt="test_info" width="300"/>

3) Fill Nan value with reasonable value

I fill the nan value in Age and Fare as the mean age and fare of all the customers in each data set. And I fill the Embarked with the mode value.

```css
data_train['Age'].fillna(data_train['Age'].mean(),inplace=True)
data_test['Age'].fillna(data_test['Age'].mean(),inplace=True)
data_train['Embarked'].fillna(data_train['Embarked'].mode()[0],inplace=True)
data_test['Fare'].fillna(data_test['Fare'].mean(),inplace=True)
```

4) Data encoding

Some of the columns, like 'Sex' and 'Embarked', are not numerical data which might be hard to read by some models. Thus, I convert them with numbers by label encoding/one-hot encoding.

```css
label = LabelEncoder()
onehot = OneHotEncoder(sparse=False)

data_train['Sex_Code'] = label.fit_transform(data_train['Sex'])

integer_encoded = label.fit_transform(data_train['Embarked'])
integer_encoded = integer_encoded.reshape(len(integer_encoded), 1)
onehot_encoded = onehot.fit_transform(integer_encoded)
onehot_encoded = pd.DataFrame(onehot_encoded,columns=data_train['Embarked'].unique())

data_train = data_train.join(onehot_encoded)
data_train.drop(['Sex','Embarked'],axis=1,inplace=True)

data_test['Sex_Code'] = label.fit_transform(data_test['Sex'])

integer_encoded = label.fit_transform(data_test['Embarked'])
integer_encoded = integer_encoded.reshape(len(integer_encoded), 1)
onehot_encoded = onehot.fit_transform(integer_encoded)
onehot_encoded = pd.DataFrame(onehot_encoded,columns=data_test['Embarked'].unique())

data_test = data_test.join(onehot_encoded)
data_test.drop(['Sex','Embarked'],axis=1,inplace=True)
```

After these these steps, I prepared a data set with fully filled numerical data in useful columns for exploratory data analysis.

## 2.Perform exploratory data analysis {#perform-eda}

1) Extract new features

SibSp and Parch are two columns that represent family size. So I add an additional column Family as the sum of these two columns.

```css
data_train['Family'] = data_train['SibSp']+data_train['Parch']
data_test['Family'] = data_test['SibSp']+data_test['Parch']
```

2) Visualize the data

To see the relationship between the dependent variable and each predictor, I made plots to show the percentage of survivors in each category.

<img src="../../assets/img/titanic/visualize.png" alt="visualize" width="800"/>

3) Check correlation

Before we fit the model, we need to check if there is any correlation between each feature.

```css
def correlation_heatmap(df):
    _ , ax = plt.subplots(figsize =(14, 12))
    colormap = sns.diverging_palette(220, 10, as_cmap = True)

    _ = sns.heatmap(
        df.corr(),
        cmap = colormap,
        square=True,
        cbar_kws={'shrink':.9 },
        ax=ax,
        annot=True,
        linewidths=0.1,vmax=1.0, linecolor='white',
        annot_kws={'fontsize':12 }
    )

    plt.title('Correlation of Features', y=1.05, size=15)

correlation_heatmap(data_train)
```

<img src="../../assets/img/titanic/correlation.png" alt="correlation" width="800"/>

## 3.Model data {#model-data}

There is no best model for different project. So I apply models and compare with them to pick up the best one. In this section, I use the script from LD Freeman on Kaggle to show the fitting result from different models.

```css
MLA = [
    #Ensemble Methods
    ensemble.AdaBoostClassifier(),
    ensemble.BaggingClassifier(),
    ensemble.ExtraTreesClassifier(),
    ensemble.GradientBoostingClassifier(),
    ensemble.RandomForestClassifier(),

    #Gaussian Processes
    gaussian_process.GaussianProcessClassifier(),

    #GLM
    linear_model.LogisticRegressionCV(),
    linear_model.PassiveAggressiveClassifier(),
    linear_model.RidgeClassifierCV(),
    linear_model.SGDClassifier(),
    linear_model.Perceptron(),

    #Navies Bayes
    naive_bayes.BernoulliNB(),
    naive_bayes.GaussianNB(),

    #Nearest Neighbor
    neighbors.KNeighborsClassifier(),

    #SVM
    svm.SVC(probability=True),
    svm.NuSVC(probability=True),
    svm.LinearSVC(),

    #Trees    
    tree.DecisionTreeClassifier(),
    tree.ExtraTreeClassifier(),

    #Discriminant Analysis
    discriminant_analysis.LinearDiscriminantAnalysis(),
    discriminant_analysis.QuadraticDiscriminantAnalysis(),

    #xgboost
    XGBClassifier()    
    ]

    x = [ 'Pclass', 'Age', 'SibSp', 'Parch', 'Fare', 'Sex_Code', 'S', 'C', 'Q', 'Family']
y = [ 'Survived']

MLA_columns = ['MLA Name','MLA Train Accuracy Mean', 'MLA Test Accuracy Mean']
MLA_compare = pd.DataFrame(columns = MLA_columns)

cv_split = model_selection.ShuffleSplit(n_splits = 10, test_size = .3, train_size = .6, random_state = 0 )
row_index = 0
for alg in MLA:
    cv_results = model_selection.cross_validate(alg, data_train[x], data_train[y], cv  = cv_split, return_train_score = True)

    MLA_name = alg.__class__.__name__
    MLA_compare.loc[row_index, 'MLA Name'] = MLA_name
    MLA_compare.loc[row_index, 'MLA Train Accuracy Mean'] = cv_results['train_score'].mean()
    MLA_compare.loc[row_index, 'MLA Test Accuracy Mean'] = cv_results['test_score'].mean()   
    row_index+=1

MLA_compare.sort_values(by = ['MLA Test Accuracy Mean'], ascending = False, inplace = True)

sns.barplot(x='MLA Test Accuracy Mean', y = 'MLA Name', data = MLA_compare, color = 'm')

plt.title('Machine Learning Algorithm Accuracy Score \n')
plt.xlabel('Accuracy Score (%)')
plt.ylabel('Algorithm')
```

<img src="../../assets/img/titanic/model.png" alt="model" width="800"/>

## 4.Submit result {#submit-res}

After we figure out the best model for this test now. We use the model to predict the test sample and generate the predict result.

```css
alg = ensemble.GradientBoostingClassifier()
alg.fit(data_train[x], data_train[y])
df_test['Survived'] = alg.predict(data_test)

res = df_test[['PassengerId','Survived']]
res.to_csv("res.csv", index=False)
```
