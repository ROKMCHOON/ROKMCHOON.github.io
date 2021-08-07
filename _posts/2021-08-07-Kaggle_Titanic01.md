```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

plt.style.use('seaborn')
sns.set(font_scale = 2.5)

import missingno as msno
import warnings
warnings.filterwarnings('ignore')

%matplotlib inline
```


```python
df_train = pd.read_csv('Tdata/train.csv')
df_test = pd.read_csv('Tdata/test.csv')
```


```python
df_train.head()
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
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>C85</td>
      <td>C</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
      <td>C123</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_train.describe()
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
      <td>891.000000</td>
      <td>891.000000</td>
      <td>891.000000</td>
      <td>714.000000</td>
      <td>891.000000</td>
      <td>891.000000</td>
      <td>891.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>446.000000</td>
      <td>0.383838</td>
      <td>2.308642</td>
      <td>29.699118</td>
      <td>0.523008</td>
      <td>0.381594</td>
      <td>32.204208</td>
    </tr>
    <tr>
      <th>std</th>
      <td>257.353842</td>
      <td>0.486592</td>
      <td>0.836071</td>
      <td>14.526497</td>
      <td>1.102743</td>
      <td>0.806057</td>
      <td>49.693429</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.420000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>223.500000</td>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>20.125000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>7.910400</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>446.000000</td>
      <td>0.000000</td>
      <td>3.000000</td>
      <td>28.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>14.454200</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>668.500000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>38.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>31.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>891.000000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>80.000000</td>
      <td>8.000000</td>
      <td>6.000000</td>
      <td>512.329200</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_test.describe()
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
      <td>332.000000</td>
      <td>418.000000</td>
      <td>418.000000</td>
      <td>417.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1100.500000</td>
      <td>2.265550</td>
      <td>30.272590</td>
      <td>0.447368</td>
      <td>0.392344</td>
      <td>35.627188</td>
    </tr>
    <tr>
      <th>std</th>
      <td>120.810458</td>
      <td>0.841838</td>
      <td>14.181209</td>
      <td>0.896760</td>
      <td>0.981429</td>
      <td>55.907576</td>
    </tr>
    <tr>
      <th>min</th>
      <td>892.000000</td>
      <td>1.000000</td>
      <td>0.170000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>996.250000</td>
      <td>1.000000</td>
      <td>21.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>7.895800</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1100.500000</td>
      <td>3.000000</td>
      <td>27.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>14.454200</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1204.750000</td>
      <td>3.000000</td>
      <td>39.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>31.500000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1309.000000</td>
      <td>3.000000</td>
      <td>76.000000</td>
      <td>8.000000</td>
      <td>9.000000</td>
      <td>512.329200</td>
    </tr>
  </tbody>
</table>
</div>



### 1.1 Null data check


```python
for col in df_train.columns:
    msg = 'column : {:>10}\t Percent of NaN value: {:.2f}%'.format(col, 100 * (df_train[col].isnull().sum() / df_train[col].shape[0]))
    print(msg)
```

    column : PassengerId	 Percent of NaN value: 0.00%
    column :   Survived	 Percent of NaN value: 0.00%
    column :     Pclass	 Percent of NaN value: 0.00%
    column :       Name	 Percent of NaN value: 0.00%
    column :        Sex	 Percent of NaN value: 0.00%
    column :        Age	 Percent of NaN value: 19.87%
    column :      SibSp	 Percent of NaN value: 0.00%
    column :      Parch	 Percent of NaN value: 0.00%
    column :     Ticket	 Percent of NaN value: 0.00%
    column :       Fare	 Percent of NaN value: 0.00%
    column :      Cabin	 Percent of NaN value: 77.10%
    column :   Embarked	 Percent of NaN value: 0.22%
    


```python
for col in df_test.columns:
    msg = 'column : {:>10}\t Percent of NaN value: {:.2f}%'.format(col, 100 * (df_test[col].isnull().sum() / df_test[col].shape[0]))
    print(msg)
```

    column : PassengerId	 Percent of NaN value: 0.00%
    column :     Pclass	 Percent of NaN value: 0.00%
    column :       Name	 Percent of NaN value: 0.00%
    column :        Sex	 Percent of NaN value: 0.00%
    column :        Age	 Percent of NaN value: 20.57%
    column :      SibSp	 Percent of NaN value: 0.00%
    column :      Parch	 Percent of NaN value: 0.00%
    column :     Ticket	 Percent of NaN value: 0.00%
    column :       Fare	 Percent of NaN value: 0.24%
    column :      Cabin	 Percent of NaN value: 78.23%
    column :   Embarked	 Percent of NaN value: 0.00%
    


```python
msno.matrix(df=df_train.iloc[:,:], figsize=(8,8), color=(0.8, 0.5, 0.2))
```




    <AxesSubplot:>




    
![png](output_8_1.png)
    



```python
msno.bar(df=df_train.iloc[:,:], figsize=(8,8), color=(0.8, 0.5, 0.2))
```




    <AxesSubplot:>




    
![png](output_9_1.png)
    



```python
msno.bar(df=df_test.iloc[:,:], figsize=(8,8), color=(0.8, 0.5, 0.2))
```




    <AxesSubplot:>




    
![png](output_10_1.png)
    


### 1.2 Target label 확인


```python
f, ax = plt.subplots(1, 2, figsize=(18,8))
df_train['Survived'].value_counts().plot.pie(explode=[0,0.1], autopct='%1.1f%%', ax=ax[0], shadow=True)
ax[0].set_title('Pie plot - Survived')
ax[0].set_ylabel('')
sns.countplot('Survived', data=df_train, ax=ax[1])
ax[1].set_title('Count plot - Survived')

plt.show()
```


    
![png](output_12_0.png)
    


### 2. Exploratory data analysis
#### 2.1 Pclass


```python
df_train[['Pclass', 'Survived']].groupby(['Pclass'], as_index=True).count()
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
      <th>Survived</th>
    </tr>
    <tr>
      <th>Pclass</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>216</td>
    </tr>
    <tr>
      <th>2</th>
      <td>184</td>
    </tr>
    <tr>
      <th>3</th>
      <td>491</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_train[['Pclass', 'Survived']].groupby(['Pclass'], as_index=True).sum()
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
      <th>Survived</th>
    </tr>
    <tr>
      <th>Pclass</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>136</td>
    </tr>
    <tr>
      <th>2</th>
      <td>87</td>
    </tr>
    <tr>
      <th>3</th>
      <td>119</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.crosstab(df_train['Pclass'], df_train['Survived'], margins=True).style.background_gradient(cmap='summer_r')
```




<style  type="text/css" >
#T_94b6a_row0_col0,#T_94b6a_row1_col1,#T_94b6a_row1_col2{
            background-color:  #ffff66;
            color:  #000000;
        }#T_94b6a_row0_col1{
            background-color:  #cee666;
            color:  #000000;
        }#T_94b6a_row0_col2{
            background-color:  #f4fa66;
            color:  #000000;
        }#T_94b6a_row1_col0{
            background-color:  #f6fa66;
            color:  #000000;
        }#T_94b6a_row2_col0{
            background-color:  #60b066;
            color:  #000000;
        }#T_94b6a_row2_col1{
            background-color:  #dfef66;
            color:  #000000;
        }#T_94b6a_row2_col2{
            background-color:  #90c866;
            color:  #000000;
        }#T_94b6a_row3_col0,#T_94b6a_row3_col1,#T_94b6a_row3_col2{
            background-color:  #008066;
            color:  #f1f1f1;
        }</style><table id="T_94b6a_" ><thead>    <tr>        <th class="index_name level0" >Survived</th>        <th class="col_heading level0 col0" >0</th>        <th class="col_heading level0 col1" >1</th>        <th class="col_heading level0 col2" >All</th>    </tr>    <tr>        <th class="index_name level0" >Pclass</th>        <th class="blank" ></th>        <th class="blank" ></th>        <th class="blank" ></th>    </tr></thead><tbody>
                <tr>
                        <th id="T_94b6a_level0_row0" class="row_heading level0 row0" >1</th>
                        <td id="T_94b6a_row0_col0" class="data row0 col0" >80</td>
                        <td id="T_94b6a_row0_col1" class="data row0 col1" >136</td>
                        <td id="T_94b6a_row0_col2" class="data row0 col2" >216</td>
            </tr>
            <tr>
                        <th id="T_94b6a_level0_row1" class="row_heading level0 row1" >2</th>
                        <td id="T_94b6a_row1_col0" class="data row1 col0" >97</td>
                        <td id="T_94b6a_row1_col1" class="data row1 col1" >87</td>
                        <td id="T_94b6a_row1_col2" class="data row1 col2" >184</td>
            </tr>
            <tr>
                        <th id="T_94b6a_level0_row2" class="row_heading level0 row2" >3</th>
                        <td id="T_94b6a_row2_col0" class="data row2 col0" >372</td>
                        <td id="T_94b6a_row2_col1" class="data row2 col1" >119</td>
                        <td id="T_94b6a_row2_col2" class="data row2 col2" >491</td>
            </tr>
            <tr>
                        <th id="T_94b6a_level0_row3" class="row_heading level0 row3" >All</th>
                        <td id="T_94b6a_row3_col0" class="data row3 col0" >549</td>
                        <td id="T_94b6a_row3_col1" class="data row3 col1" >342</td>
                        <td id="T_94b6a_row3_col2" class="data row3 col2" >891</td>
            </tr>
    </tbody></table>




```python
df_train[['Pclass', 'Survived']].groupby(['Pclass'], as_index=True).mean().sort_values(by='Survived', ascending=False).plot.bar()
```




    <AxesSubplot:xlabel='Pclass'>




    
![png](output_17_1.png)
    



```python
y_position = 1.02
f, ax = plt.subplots(1, 2, figsize=(18,8))
df_train['Pclass'].value_counts().plot.bar(color=['#CD7F32', '#FFDF00', '#D3D3D3'], ax=ax[0])
ax[0].set_title('Number of Passengers By Pclass', y=y_position)
ax[0].set_ylabel('Count')
sns.countplot('Pclass', hue='Survived', data=df_train, ax=ax[1])
ax[1].set_title('Pclass: Survived vs Dead', y=y_position)
plt.show()
```


    
![png](output_18_0.png)
    


#### 2.2 Sex


```python
f, ax = plt.subplots(1,2, figsize=(18,8))
df_train[['Sex', 'Survived']].groupby(['Sex'], as_index=True).mean().plot.bar(ax=ax[0])
ax[0].set_title('Survived vs Sex')
sns.countplot('Sex', hue='Survived', data=df_train, ax=ax[1])
ax[1].set_title('Sex: Survived vs Dead')
plt.show()
```


    
![png](output_20_0.png)
    



```python
df_train[['Sex', 'Survived']].groupby(['Sex'], as_index=True).mean().sort_values(by='Survived', ascending = False)
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
      <th>Survived</th>
    </tr>
    <tr>
      <th>Sex</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>female</th>
      <td>0.742038</td>
    </tr>
    <tr>
      <th>male</th>
      <td>0.188908</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.crosstab(df_train['Sex'], df_train['Survived'], margins=True).style.background_gradient(cmap='summer_r')
```




<style  type="text/css" >
#T_800f7_row0_col0,#T_800f7_row0_col2,#T_800f7_row1_col1{
            background-color:  #ffff66;
            color:  #000000;
        }#T_800f7_row0_col1{
            background-color:  #77bb66;
            color:  #000000;
        }#T_800f7_row1_col0{
            background-color:  #2c9666;
            color:  #000000;
        }#T_800f7_row1_col2{
            background-color:  #8bc566;
            color:  #000000;
        }#T_800f7_row2_col0,#T_800f7_row2_col1,#T_800f7_row2_col2{
            background-color:  #008066;
            color:  #f1f1f1;
        }</style><table id="T_800f7_" ><thead>    <tr>        <th class="index_name level0" >Survived</th>        <th class="col_heading level0 col0" >0</th>        <th class="col_heading level0 col1" >1</th>        <th class="col_heading level0 col2" >All</th>    </tr>    <tr>        <th class="index_name level0" >Sex</th>        <th class="blank" ></th>        <th class="blank" ></th>        <th class="blank" ></th>    </tr></thead><tbody>
                <tr>
                        <th id="T_800f7_level0_row0" class="row_heading level0 row0" >female</th>
                        <td id="T_800f7_row0_col0" class="data row0 col0" >81</td>
                        <td id="T_800f7_row0_col1" class="data row0 col1" >233</td>
                        <td id="T_800f7_row0_col2" class="data row0 col2" >314</td>
            </tr>
            <tr>
                        <th id="T_800f7_level0_row1" class="row_heading level0 row1" >male</th>
                        <td id="T_800f7_row1_col0" class="data row1 col0" >468</td>
                        <td id="T_800f7_row1_col1" class="data row1 col1" >109</td>
                        <td id="T_800f7_row1_col2" class="data row1 col2" >577</td>
            </tr>
            <tr>
                        <th id="T_800f7_level0_row2" class="row_heading level0 row2" >All</th>
                        <td id="T_800f7_row2_col0" class="data row2 col0" >549</td>
                        <td id="T_800f7_row2_col1" class="data row2 col1" >342</td>
                        <td id="T_800f7_row2_col2" class="data row2 col2" >891</td>
            </tr>
    </tbody></table>



#### 2.3 Both Sex and Pclass


```python
sns.factorplot('Pclass', 'Survived', hue = 'Sex', data=df_train, size=6, aspect=1.5);
```


    
![png](output_24_0.png)
    



```python
sns.factorplot(x='Sex', y='Survived', col='Pclass', data=df_train, satureation=.5, size=9, aspect=1);
```


    
![png](output_25_0.png)
    


#### 2.4 Age


```python
print('제일 나이 많은 탑승객 : {:.1f} Years'. format(df_train['Age'].max()))
print('제일 어린 탑승객 : {:.1f} Years'.format(df_train['Age'].min()))
print('탑승객 평균 나이 : {:.1f} Years'.format(df_train['Age'].mean()))
```

    제일 나이 많은 탑승객 : 80.0 Years
    제일 어린 탑승객 : 0.4 Years
    탑승객 평균 나이 : 29.7 Years
    


```python
fig, ax = plt.subplots(1,1, figsize=(9,5))
sns.kdeplot(df_train[df_train['Survived'] == 1]['Age'], ax=ax)
sns.kdeplot(df_train[df_train['Survived'] == 0]['Age'], ax=ax)
plt.legend(['Survived == 1', 'Survived == 0'])
plt.show()
```


    
![png](output_28_0.png)
    



```python
# Age distribution withing classes
plt.figure(figsize=(8,6))
df_train['Age'][df_train['Pclass'] == 1].plot(kind='kde')
df_train['Age'][df_train['Pclass'] == 2].plot(kind='kde')
df_train['Age'][df_train['Pclass'] == 3].plot(kind='kde')

plt.xlabel('Age')
plt.title('Age Distribution within classes')
plt.legend(['1st Class', '2nd Class', '3rd Class'])
```




    <matplotlib.legend.Legend at 0x220e537a8e0>




    
![png](output_29_1.png)
    



```python
cummulate_survival_ratio = []
for i in range(1, 80):
    cummulate_survival_ratio.append(df_train[df_train['Age'] < i]['Survived'].sum() / len(df_train[df_train['Age'] < i]['Survived']))
    
plt.figure(figsize=(7,7))
plt.plot(cummulate_survival_ratio)
plt.title('Survival rate change depending on range of Age', y=1.02)
plt.ylabel('Survival rate')
plt.xlabel('Range of Age(0~x)')
plt.show()
```


    
![png](output_30_0.png)
    



```python
f, ax = plt.subplots(1,2, figsize=(18,8))
sns.violinplot("Pclass","Age", hue="Survived", data=df_train, scale='count', split=True, ax=ax[0])
ax[0].set_title("Pclass and Age vs Survived")
ax[0].set_yticks(range(0,110, 10))
sns.violinplot("Sex","Age", hue="Survived", data=df_train, scale='count', split=True, ax=ax[1])
ax[1].set_title("Sex and Age vs Survived")
ax[1].set_yticks(range(0,110,10))
plt.show()
```


    
![png](output_31_0.png)
    


#### 2.6 Embarked


```python
f, ax= plt.subplots(1,1, figsize=(7,7))
df_train[['Embarked', 'Survived']].groupby(['Embarked'], as_index=True).mean().sort_values(by='Survived', ascending=False).plot.bar(ax=ax)
```




    <AxesSubplot:xlabel='Embarked'>




    
![png](output_33_1.png)
    



```python
f,ax = plt.subplots(2,2,figsize=(20,15))
sns.countplot('Embarked', data=df_train, ax=ax[0,0])
ax[0,0].set_title('(1)NO. Of Passengers Boarded')
sns.countplot('Embarked', hue='Sex', data=df_train, ax=ax[0,1])
ax[0,1].set_title('(2) Male-Female Split for Embarked')
sns.countplot('Embarked', hue='Survived', data=df_train, ax=ax[1,0])
ax[1,0].set_title('(3) Embarked vs Survived')
sns.countplot('Embarked', hue='Pclass', data=df_train, ax=ax[1,1])
ax[1,1].set_title('(4) Embakred vs Pclass')
plt.subplots_adjust(wspace=0.2, hspace=0.5)
plt.show()
```


    
![png](output_34_0.png)
    


#### 2.7 Family - SibSp(형제 자매) + Parch(부모, 자녀)


```python
df_train['FamilySize'] = df_train['SibSp'] + df_train['Parch'] + 1
df_test['FamilySize'] = df_train['SibSp'] + df_train['Parch'] + 1
```


```python
print("Maximum size of Family : ", df_train['Familysize'].max())
print("Minimum size of Family : ", df_train['Familysize'].min())
```

    Maximum size of Family :  11
    Minimum size of Family :  1
    


```python
f, ax = plt.subplots(1,3, figsize=(40,10))
sns.countplot('FamilySize', data=df_train, ax=ax[0])
ax[0].set_title('(1) No. Of Passenger Boarded', y=1.02)

sns.countplot('FamilySize', hue='Survived', data=df_train, ax=ax[1])
ax[1].set_title('(2) Survived countplot depending on FamilySize', y=1.02)

df_train[['FamilySize', 'Survived']].groupby(['FamilySize'], as_index=True).mean().sort_values(by='Survived', ascending=False).plot.bar(ax=ax[2])
ax[2].set_title('(3) Survived rate depending on FamilySize', y=1.02)

plt.subplots_adjust(wspace=0.2, hspace=0.5)
plt.show()
```


    
![png](output_38_0.png)
    


#### 2.8 Fare


```python
df_test.loc[df_test.Fare.isnull(), 'Fare'] = df_test['Fare'].mean()

df_train['Fare'] = df_train['Fare'].map(lambda i : np.log(i) if i > 0 else 0)
df_test['Fare'] = df_test['Fare'].map(lambda i : np.log(i) if i > 0 else 0)
```


```python
fig, ax = plt.subplots(1, 1, figsize=(8,8))
g = sns.distplot(df_train['Fare'], color='b', label='Skewness : {:.2f}'.format(df_train['Fare'].skew()), ax=ax)
g = g.legend(loc='best')
```


    
![png](output_41_0.png)
    


#### 2.9 Cabin


```python
df_train.head()
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
      <th>Fmailysize</th>
      <th>Familysize</th>
      <th>FamilySize</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>1.981001</td>
      <td>NaN</td>
      <td>S</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>4.266662</td>
      <td>C85</td>
      <td>C</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>2.070022</td>
      <td>NaN</td>
      <td>S</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>3.972177</td>
      <td>C123</td>
      <td>S</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>2.085672</td>
      <td>NaN</td>
      <td>S</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
