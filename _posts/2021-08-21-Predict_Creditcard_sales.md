### 기초 탐색 및 데이터 준비

#### 학습 데이터 불러오기


```python
import os
os.chdir(r"C:\Users\ds990\Desktop\데이터분석\데이터 전처리\6. 실전 머신러닝 프로젝트\23. 상점 신용카드 매출 예측\데이터")
```


```python
# df의 시간 범위 : 2016-06-01 ~ 2019-02-28
df = pd.read_csv("funda_train.csv")
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
      <th>store_id</th>
      <th>card_id</th>
      <th>card_company</th>
      <th>transacted_date</th>
      <th>transacted_time</th>
      <th>installment_term</th>
      <th>region</th>
      <th>type_of_business</th>
      <th>amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>b</td>
      <td>2016-06-01</td>
      <td>13:13</td>
      <td>0</td>
      <td>NaN</td>
      <td>기타 미용업</td>
      <td>1857.142857</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>1</td>
      <td>h</td>
      <td>2016-06-01</td>
      <td>18:12</td>
      <td>0</td>
      <td>NaN</td>
      <td>기타 미용업</td>
      <td>857.142857</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>2</td>
      <td>c</td>
      <td>2016-06-01</td>
      <td>18:52</td>
      <td>0</td>
      <td>NaN</td>
      <td>기타 미용업</td>
      <td>2000.000000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>3</td>
      <td>a</td>
      <td>2016-06-01</td>
      <td>20:22</td>
      <td>0</td>
      <td>NaN</td>
      <td>기타 미용업</td>
      <td>7857.142857</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>4</td>
      <td>c</td>
      <td>2016-06-02</td>
      <td>11:06</td>
      <td>0</td>
      <td>NaN</td>
      <td>기타 미용업</td>
      <td>2000.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# submission_df의 범위 : 2019-03-01 ~ 2019-05-31
submission_df = pd.read_csv("submission.csv")
submission_df.head()
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
      <th>store_id</th>
      <th>amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



#### 변수 목록 탐색


```python
# 상점 ID는 일치
print(df['store_id'].unique())
print(submission_df['store_id'].unique())
```

    [   0    1    2 ... 2134 2135 2136]
    [   0    1    2 ... 2134 2135 2136]
    


```python
print(df.columns)
print(submission_df.columns)

# 일치하는 컬럼이 store_id와 amount 밖에 없으므로, 새로운 특징을 추출하는 것이 바람직함
```

    Index(['store_id', 'card_id', 'card_company', 'transacted_date',
           'transacted_time', 'installment_term', 'region', 'type_of_business',
           'amount'],
          dtype='object')
    Index(['store_id', 'amount'], dtype='object')
    

- 예측 대상은 3개월 합계이고, 가지고 있는 데이터는 분단위로 정리되어 있음
- t-2, t-1, t월의 데이터로 t+1, t+2, t+3월의 매출 합계를 예측하는 것으로 문제를 정의
- 따라서 거래 내역을 요약하여 월별로 데이터를 새로 정의하는 것이 중요

### 학습 데이터 구축

#### 년/월 추출


```python
df['transacted_year'] = df['transacted_date'].str.split('-', expand=True).iloc[:,0].astype(int) 
df['transacted_month'] = df['transacted_date'].str.split('-', expand=True).iloc[:,1].astype(int)
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
      <th>store_id</th>
      <th>card_id</th>
      <th>card_company</th>
      <th>transacted_date</th>
      <th>transacted_time</th>
      <th>installment_term</th>
      <th>region</th>
      <th>type_of_business</th>
      <th>amount</th>
      <th>transacted_year</th>
      <th>transacted_month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>b</td>
      <td>2016-06-01</td>
      <td>13:13</td>
      <td>0</td>
      <td>NaN</td>
      <td>기타 미용업</td>
      <td>1857.142857</td>
      <td>2016</td>
      <td>6</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>1</td>
      <td>h</td>
      <td>2016-06-01</td>
      <td>18:12</td>
      <td>0</td>
      <td>NaN</td>
      <td>기타 미용업</td>
      <td>857.142857</td>
      <td>2016</td>
      <td>6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>2</td>
      <td>c</td>
      <td>2016-06-01</td>
      <td>18:52</td>
      <td>0</td>
      <td>NaN</td>
      <td>기타 미용업</td>
      <td>2000.000000</td>
      <td>2016</td>
      <td>6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>3</td>
      <td>a</td>
      <td>2016-06-01</td>
      <td>20:22</td>
      <td>0</td>
      <td>NaN</td>
      <td>기타 미용업</td>
      <td>7857.142857</td>
      <td>2016</td>
      <td>6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>4</td>
      <td>c</td>
      <td>2016-06-02</td>
      <td>11:06</td>
      <td>0</td>
      <td>NaN</td>
      <td>기타 미용업</td>
      <td>2000.000000</td>
      <td>2016</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 데이터 병합을 위한 새로운 컬럼 생성 및 기존 시간 변수 삭제
df['t'] = (df['transacted_year'] - 2016) * 12 + df['transacted_month']
df.drop(['transacted_year', 'transacted_month', 'transacted_date', 'transacted_time'], axis=1, inplace=True)
```

#### 불필요한 변수 제거
- card_id, card_company는 특징으로 사용하기에는 너무 세분화될 수 있을 뿐만 아니라, 특징으로 유효할 가능성이 없다고 판단하여 삭제


```python
df.drop(['card_id', 'card_company'], axis=1, inplace=True)
```


```python
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
      <th>store_id</th>
      <th>installment_term</th>
      <th>region</th>
      <th>type_of_business</th>
      <th>amount</th>
      <th>t</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>기타 미용업</td>
      <td>1857.142857</td>
      <td>6</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>기타 미용업</td>
      <td>857.142857</td>
      <td>6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>기타 미용업</td>
      <td>2000.000000</td>
      <td>6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>기타 미용업</td>
      <td>7857.142857</td>
      <td>6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>기타 미용업</td>
      <td>2000.000000</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>



#### 업종 특성, 지역, 할부 평균 탐색


```python
df['installment_term'].value_counts().head() # 대부분이 일시불이므로, installment_term 변수를 할부인지 아닌지를 여부로 변환
```




    0    6327632
    3     134709
    2      42101
    5      23751
    6      10792
    Name: installment_term, dtype: int64




```python
df['installment_term'] = (df['installment_term'] > 0).astype(int)
df['installment_term'].value_counts()
```




    0    6327632
    1     228981
    Name: installment_term, dtype: int64




```python
# 상점별 평균 할부 비율
installment_term_per_store = df.groupby(['store_id'])['installment_term'].mean()
installment_term_per_store.head()
```




    store_id
    0    0.038384
    1    0.000000
    2    0.083904
    4    0.001201
    5    0.075077
    Name: installment_term, dtype: float64




```python
# groupoby에 결측을 포함시키기 위해, 결측을 문자로 대체
# 지역은 너무 많아서 그대로 활용하기 어려움, 따라서 그대로 더미화하지 않고, 이를 기반으로 한 새로운 변수를 파생해서 사용
df['region'].fillna('없음', inplace=True)
df['region'].value_counts().head()
```




    없음        2042766
    경기 수원시     122029
    충북 청주시     116766
    경남 창원시     107147
    경남 김해시     100673
    Name: region, dtype: int64




```python
df['type_of_business'].value_counts().head()
```




    한식 음식점업    745905
    두발 미용업     178475
    의복 소매업     158234
    기타 주점업     102413
    치킨 전문점      89277
    Name: type_of_business, dtype: int64




```python
# groupby에 결측을 포함시키기 위해, 결측을 문자로 대체
df['type_of_business'].fillna('없음', inplace=True)
```

#### 학습 데이터 구조 작성


```python
# 'store_id', 'region', 'type_of_business', 't'를 기준으로 중복을 제거한 뒤, 해당 컬럼만 가져옴
train_df = df.drop_duplicates(subset = ['store_id', 'region', 'type_of_business', 't'])[['store_id', 'region', 'type_of_business', 't']]
train_df.head()
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
      <th>store_id</th>
      <th>region</th>
      <th>type_of_business</th>
      <th>t</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>6</td>
    </tr>
    <tr>
      <th>145</th>
      <td>0</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>7</td>
    </tr>
    <tr>
      <th>323</th>
      <td>0</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>8</td>
    </tr>
    <tr>
      <th>494</th>
      <td>0</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>9</td>
    </tr>
    <tr>
      <th>654</th>
      <td>0</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>10</td>
    </tr>
  </tbody>
</table>
</div>



#### 평균 할부율 부착


```python
train_df['평균할부율'] = train_df['store_id'].replace(installment_term_per_store.to_dict())
```


```python
train_df.head()
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
      <th>store_id</th>
      <th>region</th>
      <th>type_of_business</th>
      <th>t</th>
      <th>평균할부율</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>6</td>
      <td>0.038384</td>
    </tr>
    <tr>
      <th>145</th>
      <td>0</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>7</td>
      <td>0.038384</td>
    </tr>
    <tr>
      <th>323</th>
      <td>0</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>8</td>
      <td>0.038384</td>
    </tr>
    <tr>
      <th>494</th>
      <td>0</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>9</td>
      <td>0.038384</td>
    </tr>
    <tr>
      <th>654</th>
      <td>0</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>10</td>
      <td>0.038384</td>
    </tr>
  </tbody>
</table>
</div>



#### t-1, t-2, t-3 시점의 매출 합계 부착


```python
# store_id와 t에 따른 amount 합계 계산 : amount_sum_per_t_and_sid
amount_sum_per_t_and_sid = df.groupby(['store_id', 't'], as_index=False)['amount'].sum()
amount_sum_per_t_and_sid.head()
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
      <th>store_id</th>
      <th>t</th>
      <th>amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>6</td>
      <td>7.470000e+05</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>7</td>
      <td>1.005000e+06</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>8</td>
      <td>8.715714e+05</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>9</td>
      <td>8.978571e+05</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>10</td>
      <td>8.354286e+05</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 몇몇 상점은 중간이 비어있음을 확인 => merge에서 문제가 생길 수 있음
amount_sum_per_t_and_sid.groupby(['store_id'])['t'].count().head(10)
```




    store_id
    0     33
    1     33
    2     33
    4     33
    5     33
    6     31
    7     31
    8     28
    9     29
    10    23
    Name: t, dtype: int64




```python
# 따라서 모든 값을 채우기 위해, 피벗 테이블을 생성하고 결측을 바로 앞 값으로 채움
amount_sum_per_t_and_sid = pd.pivot_table(df, values='amount', index='store_id', columns='t', aggfunc='sum')
amount_sum_per_t_and_sid.head(10)
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
      <th>t</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
      <th>11</th>
      <th>12</th>
      <th>13</th>
      <th>14</th>
      <th>15</th>
      <th>...</th>
      <th>29</th>
      <th>30</th>
      <th>31</th>
      <th>32</th>
      <th>33</th>
      <th>34</th>
      <th>35</th>
      <th>36</th>
      <th>37</th>
      <th>38</th>
    </tr>
    <tr>
      <th>store_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>747000.000000</td>
      <td>1.005000e+06</td>
      <td>871571.428571</td>
      <td>8.978571e+05</td>
      <td>8.354286e+05</td>
      <td>6.970000e+05</td>
      <td>7.618571e+05</td>
      <td>585642.857143</td>
      <td>7.940000e+05</td>
      <td>720257.142857</td>
      <td>...</td>
      <td>6.864286e+05</td>
      <td>7.072857e+05</td>
      <td>7.587143e+05</td>
      <td>6.798571e+05</td>
      <td>6.518571e+05</td>
      <td>7.390000e+05</td>
      <td>6.760000e+05</td>
      <td>8.745714e+05</td>
      <td>6.828571e+05</td>
      <td>5.152857e+05</td>
    </tr>
    <tr>
      <th>1</th>
      <td>137214.285714</td>
      <td>1.630000e+05</td>
      <td>118142.857143</td>
      <td>9.042857e+04</td>
      <td>1.180714e+05</td>
      <td>1.118571e+05</td>
      <td>1.155714e+05</td>
      <td>129642.857143</td>
      <td>1.602143e+05</td>
      <td>168428.571429</td>
      <td>...</td>
      <td>8.050000e+04</td>
      <td>7.828571e+04</td>
      <td>1.007857e+05</td>
      <td>9.214286e+04</td>
      <td>6.357143e+04</td>
      <td>9.500000e+04</td>
      <td>8.078571e+04</td>
      <td>8.528571e+04</td>
      <td>1.482857e+05</td>
      <td>7.742857e+04</td>
    </tr>
    <tr>
      <th>2</th>
      <td>260714.285714</td>
      <td>8.285714e+04</td>
      <td>131428.571429</td>
      <td>1.428571e+05</td>
      <td>1.097143e+05</td>
      <td>1.985714e+05</td>
      <td>1.600000e+05</td>
      <td>180714.285714</td>
      <td>1.542857e+05</td>
      <td>43571.428571</td>
      <td>...</td>
      <td>4.728571e+05</td>
      <td>3.542857e+05</td>
      <td>6.892857e+05</td>
      <td>4.578571e+05</td>
      <td>4.807143e+05</td>
      <td>5.100000e+05</td>
      <td>1.854286e+05</td>
      <td>3.407143e+05</td>
      <td>4.078571e+05</td>
      <td>4.968571e+05</td>
    </tr>
    <tr>
      <th>4</th>
      <td>733428.571429</td>
      <td>7.689286e+05</td>
      <td>698428.571429</td>
      <td>9.364286e+05</td>
      <td>7.627143e+05</td>
      <td>8.595714e+05</td>
      <td>1.069857e+06</td>
      <td>689142.857143</td>
      <td>1.050143e+06</td>
      <td>970285.714286</td>
      <td>...</td>
      <td>7.754286e+05</td>
      <td>8.812857e+05</td>
      <td>1.050929e+06</td>
      <td>8.492857e+05</td>
      <td>6.981429e+05</td>
      <td>8.284286e+05</td>
      <td>8.830000e+05</td>
      <td>9.238571e+05</td>
      <td>9.448571e+05</td>
      <td>8.822857e+05</td>
    </tr>
    <tr>
      <th>5</th>
      <td>342500.000000</td>
      <td>4.327143e+05</td>
      <td>263500.000000</td>
      <td>2.321429e+05</td>
      <td>2.115714e+05</td>
      <td>1.820857e+05</td>
      <td>1.475714e+05</td>
      <td>120957.142857</td>
      <td>1.864286e+05</td>
      <td>169000.000000</td>
      <td>...</td>
      <td>4.438571e+05</td>
      <td>5.637143e+05</td>
      <td>6.070714e+05</td>
      <td>4.828857e+05</td>
      <td>1.950000e+05</td>
      <td>3.249286e+05</td>
      <td>3.833000e+05</td>
      <td>3.995714e+05</td>
      <td>3.230000e+05</td>
      <td>2.155143e+05</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>568857.142857</td>
      <td>1.440143e+06</td>
      <td>1.238857e+06</td>
      <td>1.055429e+06</td>
      <td>9.268571e+05</td>
      <td>885642.857143</td>
      <td>8.003571e+05</td>
      <td>930714.285714</td>
      <td>...</td>
      <td>1.808357e+06</td>
      <td>1.752286e+06</td>
      <td>1.583786e+06</td>
      <td>1.628786e+06</td>
      <td>2.074071e+06</td>
      <td>1.907643e+06</td>
      <td>2.389143e+06</td>
      <td>2.230286e+06</td>
      <td>2.015500e+06</td>
      <td>2.463857e+06</td>
    </tr>
    <tr>
      <th>7</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>107857.142857</td>
      <td>3.756429e+05</td>
      <td>3.236429e+05</td>
      <td>3.450000e+05</td>
      <td>2.914286e+05</td>
      <td>231614.285714</td>
      <td>2.713571e+05</td>
      <td>249857.142857</td>
      <td>...</td>
      <td>2.657143e+05</td>
      <td>4.195429e+05</td>
      <td>4.628429e+05</td>
      <td>4.231286e+05</td>
      <td>3.203286e+05</td>
      <td>4.200286e+05</td>
      <td>3.143857e+05</td>
      <td>3.024143e+05</td>
      <td>1.364714e+05</td>
      <td>5.797143e+04</td>
    </tr>
    <tr>
      <th>8</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.925714e+05</td>
      <td>7.355000e+05</td>
      <td>467857.142857</td>
      <td>4.756429e+05</td>
      <td>603500.000000</td>
      <td>...</td>
      <td>1.837429e+06</td>
      <td>1.359857e+06</td>
      <td>1.213543e+06</td>
      <td>1.086000e+06</td>
      <td>1.369557e+06</td>
      <td>1.272071e+06</td>
      <td>1.260557e+06</td>
      <td>1.157257e+06</td>
      <td>1.134671e+06</td>
      <td>1.298329e+06</td>
    </tr>
    <tr>
      <th>9</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.071429e+05</td>
      <td>6.371429e+05</td>
      <td>6.035714e+05</td>
      <td>225428.571429</td>
      <td>2.871429e+05</td>
      <td>344428.571429</td>
      <td>...</td>
      <td>6.385714e+05</td>
      <td>2.765714e+05</td>
      <td>3.400000e+05</td>
      <td>2.542857e+05</td>
      <td>9.265714e+05</td>
      <td>8.714286e+05</td>
      <td>6.928571e+05</td>
      <td>6.628571e+05</td>
      <td>3.700000e+05</td>
      <td>4.057143e+05</td>
    </tr>
    <tr>
      <th>10</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>2.902857e+05</td>
      <td>6.078571e+05</td>
      <td>4.445714e+05</td>
      <td>6.414286e+05</td>
      <td>7.955714e+05</td>
      <td>4.992857e+05</td>
      <td>5.901429e+05</td>
      <td>5.184286e+05</td>
      <td>5.251429e+05</td>
      <td>6.548571e+05</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 33 columns</p>
</div>




```python
# 따라서 모든 값을 채우기 위해, 피벗 테이블을 생성하고 결측을 바로 앞 값으로 채움
amount_sum_per_t_and_sid = amount_sum_per_t_and_sid.fillna(method='ffill', axis=1).fillna(method='bfill', axis=1)
amount_sum_per_t_and_sid.head(10)
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
      <th>t</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
      <th>11</th>
      <th>12</th>
      <th>13</th>
      <th>14</th>
      <th>15</th>
      <th>...</th>
      <th>29</th>
      <th>30</th>
      <th>31</th>
      <th>32</th>
      <th>33</th>
      <th>34</th>
      <th>35</th>
      <th>36</th>
      <th>37</th>
      <th>38</th>
    </tr>
    <tr>
      <th>store_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>747000.000000</td>
      <td>1.005000e+06</td>
      <td>871571.428571</td>
      <td>8.978571e+05</td>
      <td>8.354286e+05</td>
      <td>6.970000e+05</td>
      <td>7.618571e+05</td>
      <td>585642.857143</td>
      <td>7.940000e+05</td>
      <td>720257.142857</td>
      <td>...</td>
      <td>6.864286e+05</td>
      <td>7.072857e+05</td>
      <td>7.587143e+05</td>
      <td>6.798571e+05</td>
      <td>6.518571e+05</td>
      <td>7.390000e+05</td>
      <td>6.760000e+05</td>
      <td>8.745714e+05</td>
      <td>6.828571e+05</td>
      <td>5.152857e+05</td>
    </tr>
    <tr>
      <th>1</th>
      <td>137214.285714</td>
      <td>1.630000e+05</td>
      <td>118142.857143</td>
      <td>9.042857e+04</td>
      <td>1.180714e+05</td>
      <td>1.118571e+05</td>
      <td>1.155714e+05</td>
      <td>129642.857143</td>
      <td>1.602143e+05</td>
      <td>168428.571429</td>
      <td>...</td>
      <td>8.050000e+04</td>
      <td>7.828571e+04</td>
      <td>1.007857e+05</td>
      <td>9.214286e+04</td>
      <td>6.357143e+04</td>
      <td>9.500000e+04</td>
      <td>8.078571e+04</td>
      <td>8.528571e+04</td>
      <td>1.482857e+05</td>
      <td>7.742857e+04</td>
    </tr>
    <tr>
      <th>2</th>
      <td>260714.285714</td>
      <td>8.285714e+04</td>
      <td>131428.571429</td>
      <td>1.428571e+05</td>
      <td>1.097143e+05</td>
      <td>1.985714e+05</td>
      <td>1.600000e+05</td>
      <td>180714.285714</td>
      <td>1.542857e+05</td>
      <td>43571.428571</td>
      <td>...</td>
      <td>4.728571e+05</td>
      <td>3.542857e+05</td>
      <td>6.892857e+05</td>
      <td>4.578571e+05</td>
      <td>4.807143e+05</td>
      <td>5.100000e+05</td>
      <td>1.854286e+05</td>
      <td>3.407143e+05</td>
      <td>4.078571e+05</td>
      <td>4.968571e+05</td>
    </tr>
    <tr>
      <th>4</th>
      <td>733428.571429</td>
      <td>7.689286e+05</td>
      <td>698428.571429</td>
      <td>9.364286e+05</td>
      <td>7.627143e+05</td>
      <td>8.595714e+05</td>
      <td>1.069857e+06</td>
      <td>689142.857143</td>
      <td>1.050143e+06</td>
      <td>970285.714286</td>
      <td>...</td>
      <td>7.754286e+05</td>
      <td>8.812857e+05</td>
      <td>1.050929e+06</td>
      <td>8.492857e+05</td>
      <td>6.981429e+05</td>
      <td>8.284286e+05</td>
      <td>8.830000e+05</td>
      <td>9.238571e+05</td>
      <td>9.448571e+05</td>
      <td>8.822857e+05</td>
    </tr>
    <tr>
      <th>5</th>
      <td>342500.000000</td>
      <td>4.327143e+05</td>
      <td>263500.000000</td>
      <td>2.321429e+05</td>
      <td>2.115714e+05</td>
      <td>1.820857e+05</td>
      <td>1.475714e+05</td>
      <td>120957.142857</td>
      <td>1.864286e+05</td>
      <td>169000.000000</td>
      <td>...</td>
      <td>4.438571e+05</td>
      <td>5.637143e+05</td>
      <td>6.070714e+05</td>
      <td>4.828857e+05</td>
      <td>1.950000e+05</td>
      <td>3.249286e+05</td>
      <td>3.833000e+05</td>
      <td>3.995714e+05</td>
      <td>3.230000e+05</td>
      <td>2.155143e+05</td>
    </tr>
    <tr>
      <th>6</th>
      <td>568857.142857</td>
      <td>5.688571e+05</td>
      <td>568857.142857</td>
      <td>1.440143e+06</td>
      <td>1.238857e+06</td>
      <td>1.055429e+06</td>
      <td>9.268571e+05</td>
      <td>885642.857143</td>
      <td>8.003571e+05</td>
      <td>930714.285714</td>
      <td>...</td>
      <td>1.808357e+06</td>
      <td>1.752286e+06</td>
      <td>1.583786e+06</td>
      <td>1.628786e+06</td>
      <td>2.074071e+06</td>
      <td>1.907643e+06</td>
      <td>2.389143e+06</td>
      <td>2.230286e+06</td>
      <td>2.015500e+06</td>
      <td>2.463857e+06</td>
    </tr>
    <tr>
      <th>7</th>
      <td>107857.142857</td>
      <td>1.078571e+05</td>
      <td>107857.142857</td>
      <td>3.756429e+05</td>
      <td>3.236429e+05</td>
      <td>3.450000e+05</td>
      <td>2.914286e+05</td>
      <td>231614.285714</td>
      <td>2.713571e+05</td>
      <td>249857.142857</td>
      <td>...</td>
      <td>2.657143e+05</td>
      <td>4.195429e+05</td>
      <td>4.628429e+05</td>
      <td>4.231286e+05</td>
      <td>3.203286e+05</td>
      <td>4.200286e+05</td>
      <td>3.143857e+05</td>
      <td>3.024143e+05</td>
      <td>1.364714e+05</td>
      <td>5.797143e+04</td>
    </tr>
    <tr>
      <th>8</th>
      <td>192571.428571</td>
      <td>1.925714e+05</td>
      <td>192571.428571</td>
      <td>1.925714e+05</td>
      <td>1.925714e+05</td>
      <td>1.925714e+05</td>
      <td>7.355000e+05</td>
      <td>467857.142857</td>
      <td>4.756429e+05</td>
      <td>603500.000000</td>
      <td>...</td>
      <td>1.837429e+06</td>
      <td>1.359857e+06</td>
      <td>1.213543e+06</td>
      <td>1.086000e+06</td>
      <td>1.369557e+06</td>
      <td>1.272071e+06</td>
      <td>1.260557e+06</td>
      <td>1.157257e+06</td>
      <td>1.134671e+06</td>
      <td>1.298329e+06</td>
    </tr>
    <tr>
      <th>9</th>
      <td>107142.857143</td>
      <td>1.071429e+05</td>
      <td>107142.857143</td>
      <td>1.071429e+05</td>
      <td>1.071429e+05</td>
      <td>6.371429e+05</td>
      <td>6.035714e+05</td>
      <td>225428.571429</td>
      <td>2.871429e+05</td>
      <td>344428.571429</td>
      <td>...</td>
      <td>6.385714e+05</td>
      <td>2.765714e+05</td>
      <td>3.400000e+05</td>
      <td>2.542857e+05</td>
      <td>9.265714e+05</td>
      <td>8.714286e+05</td>
      <td>6.928571e+05</td>
      <td>6.628571e+05</td>
      <td>3.700000e+05</td>
      <td>4.057143e+05</td>
    </tr>
    <tr>
      <th>10</th>
      <td>496714.285714</td>
      <td>4.967143e+05</td>
      <td>496714.285714</td>
      <td>4.967143e+05</td>
      <td>4.967143e+05</td>
      <td>4.967143e+05</td>
      <td>4.967143e+05</td>
      <td>496714.285714</td>
      <td>4.967143e+05</td>
      <td>496714.285714</td>
      <td>...</td>
      <td>2.902857e+05</td>
      <td>6.078571e+05</td>
      <td>4.445714e+05</td>
      <td>6.414286e+05</td>
      <td>7.955714e+05</td>
      <td>4.992857e+05</td>
      <td>5.901429e+05</td>
      <td>5.184286e+05</td>
      <td>5.251429e+05</td>
      <td>6.548571e+05</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 33 columns</p>
</div>




```python
# stack을 이용하여, 컬럼도 행 인덱스로 밀어넣고, 인덱스를 초기화하여 인덱스를 컬럼으로 가져옴
amount_sum_per_t_and_sid = amount_sum_per_t_and_sid.stack().reset_index()
amount_sum_per_t_and_sid.rename({0:"amount"}, axis=1, inplace=True)
```


```python
# t - k (k = 1,2,3) 시점의 부착
# train_df의 t는 amount_sum_per_t_and_sid의 t-k과 부착되어야 하므로, amount_sum_per_t_and_sid의 t에 k를 더함

for k in range(1,4):
    amount_sum_per_t_and_sid['t_{}'.format(k)] = amount_sum_per_t_and_sid['t'] + k
    train_df = pd.merge(train_df, amount_sum_per_t_and_sid.drop('t', axis=1), left_on = ['store_id', 't'], right_on = ['store_id', 't_{}'.format(k)])
    
    # 부착한 뒤, 불필요한 변수 제거 및 변수명 변경 : 다음 이터레이션에서의 병합이 잘되게 하기 위해서
    train_df.rename({"amount" : "{}_before_amount".format(k)}, axis=1, inplace=True)
    train_df.drop(['t_{}'.format(k)], axis=1, inplace=True)
    amount_sum_per_t_and_sid.drop(['t_{}'.format(k)], axis=1, inplace=True)
    
train_df.head()
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
      <th>store_id</th>
      <th>region</th>
      <th>type_of_business</th>
      <th>t</th>
      <th>평균할부율</th>
      <th>1_before_amount</th>
      <th>2_before_amount</th>
      <th>3_before_amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>9</td>
      <td>0.038384</td>
      <td>871571.428571</td>
      <td>1.005000e+06</td>
      <td>7.470000e+05</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>10</td>
      <td>0.038384</td>
      <td>897857.142857</td>
      <td>8.715714e+05</td>
      <td>1.005000e+06</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>11</td>
      <td>0.038384</td>
      <td>835428.571429</td>
      <td>8.978571e+05</td>
      <td>8.715714e+05</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>12</td>
      <td>0.038384</td>
      <td>697000.000000</td>
      <td>8.354286e+05</td>
      <td>8.978571e+05</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>13</td>
      <td>0.038384</td>
      <td>761857.142857</td>
      <td>6.970000e+05</td>
      <td>8.354286e+05</td>
    </tr>
  </tbody>
</table>
</div>



#### t-1, t-2, t-3 시점의 지역별 매출 합계 평균 부착


```python
# amount_sum_per_t_and_sid의 store_id를 region으로 대체시키기
store_to_region = df[['store_id', 'region']].drop_duplicates().set_index(['store_id'])['region'].to_dict()
amount_sum_per_t_and_sid['region'] = amount_sum_per_t_and_sid['store_id'].replace(store_to_region)

# 지역별 평균 매출 계산
amount_mean_per_t_and_region = amount_sum_per_t_and_sid.groupby(['region', 't'], as_index=False)['amount'].mean()
```


```python
# t - k (k = 1,2,3) 시점의 부착

for k in range(1,4):
    amount_mean_per_t_and_region['t_{}'.format(k)] = amount_mean_per_t_and_region['t'] + k
    train_df = pd.merge(train_df, amount_mean_per_t_and_region.drop('t', axis=1), left_on = ['region', 't'], right_on = ['region', 't_{}'.format(k)])
    train_df.rename({"amount" : "{}_before_amount_of_region".format(k)}, axis=1, inplace=True)
    
    train_df.drop(['t_{}'.format(k)], axis=1, inplace=True)
    amount_mean_per_t_and_region.drop(['t_{}'.format(k)], axis=1, inplace=True)
    
train_df.head()
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
      <th>store_id</th>
      <th>region</th>
      <th>type_of_business</th>
      <th>t</th>
      <th>평균할부율</th>
      <th>1_before_amount</th>
      <th>2_before_amount</th>
      <th>3_before_amount</th>
      <th>1_before_amount_of_region</th>
      <th>2_before_amount_of_region</th>
      <th>3_before_amount_of_region</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>9</td>
      <td>0.038384</td>
      <td>871571.428571</td>
      <td>1.005000e+06</td>
      <td>747000.000000</td>
      <td>761987.421532</td>
      <td>756108.674948</td>
      <td>739654.068323</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>없음</td>
      <td>없음</td>
      <td>9</td>
      <td>0.000000</td>
      <td>118142.857143</td>
      <td>1.630000e+05</td>
      <td>137214.285714</td>
      <td>761987.421532</td>
      <td>756108.674948</td>
      <td>739654.068323</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>없음</td>
      <td>없음</td>
      <td>9</td>
      <td>0.083904</td>
      <td>131428.571429</td>
      <td>8.285714e+04</td>
      <td>260714.285714</td>
      <td>761987.421532</td>
      <td>756108.674948</td>
      <td>739654.068323</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5</td>
      <td>없음</td>
      <td>의복 액세서리 및 모조 장신구 도매업</td>
      <td>9</td>
      <td>0.075077</td>
      <td>263500.000000</td>
      <td>4.327143e+05</td>
      <td>342500.000000</td>
      <td>761987.421532</td>
      <td>756108.674948</td>
      <td>739654.068323</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7</td>
      <td>없음</td>
      <td>없음</td>
      <td>9</td>
      <td>0.011558</td>
      <td>107857.142857</td>
      <td>1.078571e+05</td>
      <td>107857.142857</td>
      <td>761987.421532</td>
      <td>756108.674948</td>
      <td>739654.068323</td>
    </tr>
  </tbody>
</table>
</div>




```python
# amount_sum_per_t_and_sid의 store_id를 type_of_business으로 대체시키기
store_to_type_of_business = df[['store_id', 'type_of_business']].drop_duplicates().set_index(['store_id'])['type_of_business'].to_dict()
amount_sum_per_t_and_sid['type_of_business'] = amount_sum_per_t_and_sid['store_id'].replace(store_to_type_of_business)

# 지역별 평균 매출 계산
amount_mean_per_t_and_type_of_business = amount_sum_per_t_and_sid.groupby(['type_of_business', 't'], as_index=False)['amount'].mean()
```


```python
# t - k (k = 1,2,3) 시점의 부착

for k in range(1,4):
    amount_mean_per_t_and_type_of_business['t_{}'.format(k)] = amount_mean_per_t_and_type_of_business['t'] + k
    train_df = pd.merge(train_df, amount_mean_per_t_and_type_of_business.drop('t', axis=1), left_on = ['type_of_business', 't'], right_on = ['type_of_business', 't_{}'.format(k)])
    train_df.rename({"amount" : "{}_before_amount_of_type_of_business".format(k)}, axis=1, inplace=True)
    
    train_df.drop(['t_{}'.format(k)], axis=1, inplace=True)
    amount_mean_per_t_and_type_of_business.drop(['t_{}'.format(k)], axis=1, inplace=True)
    
train_df.head()
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
      <th>store_id</th>
      <th>region</th>
      <th>type_of_business</th>
      <th>t</th>
      <th>평균할부율</th>
      <th>1_before_amount</th>
      <th>2_before_amount</th>
      <th>3_before_amount</th>
      <th>1_before_amount_of_region</th>
      <th>2_before_amount_of_region</th>
      <th>3_before_amount_of_region</th>
      <th>1_before_amount_of_type_of_business</th>
      <th>2_before_amount_of_type_of_business</th>
      <th>3_before_amount_of_type_of_business</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>9</td>
      <td>0.038384</td>
      <td>871571.428571</td>
      <td>1.005000e+06</td>
      <td>747000.000000</td>
      <td>7.619874e+05</td>
      <td>7.561087e+05</td>
      <td>7.396541e+05</td>
      <td>761025.0</td>
      <td>804979.761905</td>
      <td>679950.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>792</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>9</td>
      <td>0.218887</td>
      <td>681142.857143</td>
      <td>8.808571e+05</td>
      <td>733714.285714</td>
      <td>7.619874e+05</td>
      <td>7.561087e+05</td>
      <td>7.396541e+05</td>
      <td>761025.0</td>
      <td>804979.761905</td>
      <td>679950.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>23</td>
      <td>경기 안양시</td>
      <td>기타 미용업</td>
      <td>9</td>
      <td>0.048795</td>
      <td>879242.857143</td>
      <td>7.308571e+05</td>
      <td>845285.714286</td>
      <td>8.288317e+05</td>
      <td>5.887330e+05</td>
      <td>9.559733e+05</td>
      <td>761025.0</td>
      <td>804979.761905</td>
      <td>679950.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>192</td>
      <td>경기 화성시</td>
      <td>기타 미용업</td>
      <td>9</td>
      <td>0.100542</td>
      <td>579000.000000</td>
      <td>5.234286e+05</td>
      <td>551142.857143</td>
      <td>1.234460e+06</td>
      <td>1.227921e+06</td>
      <td>1.180455e+06</td>
      <td>761025.0</td>
      <td>804979.761905</td>
      <td>679950.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>536</td>
      <td>서울 광진구</td>
      <td>기타 미용업</td>
      <td>9</td>
      <td>0.014810</td>
      <td>96285.714286</td>
      <td>7.985714e+04</td>
      <td>99857.142857</td>
      <td>3.786820e+06</td>
      <td>3.397973e+06</td>
      <td>3.524075e+06</td>
      <td>761025.0</td>
      <td>804979.761905</td>
      <td>679950.0</td>
    </tr>
  </tbody>
</table>
</div>



#### 라벨 부착하기


```python
# 현 시점에서 t+1, t+2, t+3의 매출을 부착해야 함

amount_sum_per_t_and_sid.drop(['region', 'type_of_business'], axis=1, inplace=True)
for k in range(1,4):
    amount_sum_per_t_and_sid['t_{}'.format(k)] = amount_sum_per_t_and_sid['t'] - k
    train_df = pd.merge(train_df, amount_sum_per_t_and_sid.drop('t', axis=1), left_on = ['store_id', 't'], right_on = ['store_id', 't_{}'.format(k)])
    train_df.rename({"amount" : "Y_{}".format(k)}, axis=1, inplace=True)
                        
    train_df.drop(['t_{}'.format(k)], axis=1, inplace=True)
    amount_sum_per_t_and_sid.drop(['t_{}'.format(k)], axis=1, inplace=True)
```


```python
train_df["Y"] = train_df['Y_1'] + train_df['Y_2'] + train_df['Y_3']
```

### 학습 데이터 탐색 및 전처리

#### 특징과 라벨 분리


```python
X = train_df.drop(['store_id', 'region', 'type_of_business', 't', 'Y_1', 'Y_2', 'Y_3', 'Y'], axis=1)
Y = train_df['Y']
```

#### 데이터 분할 및 구조 탐색


```python
from sklearn.model_selection import train_test_split
Train_X, Test_X, Train_Y, Test_Y = train_test_split(X,Y)
Train_X.shape # 특징 대비 샘플이 많음
```




    (37673, 10)




```python
Train_Y.describe()
```




    count    3.767300e+04
    mean     3.437784e+06
    std      5.088019e+06
    min      0.000000e+00
    25%      1.136286e+06
    50%      2.219286e+06
    75%      4.114000e+06
    max      1.701386e+08
    Name: Y, dtype: float64



#### 이상치 제거


```python
def IQR_rule(val_list):
    # IQR 계산
    Q1 = np.quantile(val_list, 0.25)
    Q3 = np.quantile(val_list, 0.75)
    IQR = Q3 - Q1
    
    # IQR rule을 위배하지 않는 bool list 계산 (True: 이상치X, False: 이상치 O)
    not_outlier_condition = (Q3 + 1.5 * IQR > val_list) & (Q1 - 1.5 * IQR < val_list)
    return not_outlier_condition
```


```python
Y_condition = IQR_rule(Train_Y)
Train_Y = Train_Y[Y_condition]
Train_X = Train_X[Y_condition] 
```

#### 치우침 제거


```python
# 모두 좌로 치우침을 확인
Train_X.skew()
```




    평균할부율                                  3.002186
    1_before_amount                        2.383720
    2_before_amount                        2.226253
    3_before_amount                        2.322537
    1_before_amount_of_region              3.292319
    2_before_amount_of_region              3.236896
    3_before_amount_of_region              3.211631
    1_before_amount_of_type_of_business    1.840530
    2_before_amount_of_type_of_business    1.974181
    3_before_amount_of_type_of_business    2.226367
    dtype: float64




```python
# 치우침 제거
biased_variables = Train_X.columns[Train_X.skew().abs() > 1.5] # 왜도의 절댓값이 1.5 이상인 컬럼만 가져오기
Train_X[biased_variables] = Train_X[biased_variables] - Train_X[biased_variables].min() + 1
Train_X[biased_variables] = np.sqrt(Train_X[biased_variables])
```


```python
Train_X.skew()
```




    평균할부율                                  2.786951
    1_before_amount                        0.716774
    2_before_amount                        0.714163
    3_before_amount                        0.726775
    1_before_amount_of_region              1.841442
    2_before_amount_of_region              1.807969
    3_before_amount_of_region              1.794889
    1_before_amount_of_type_of_business   -0.254183
    2_before_amount_of_type_of_business   -0.228949
    3_before_amount_of_type_of_business   -0.169113
    dtype: float64



#### 스케일링 수행


```python
Train_X.max() - Train_X.min() # 특징 간 스케일 차이가 큼을 확인 => 스케일이 작은 특징은 영향을 거의 주지 못할 것이라 예상됨
```




    평균할부율                                     0.379933
    1_before_amount                        3694.924532
    2_before_amount                        3518.070636
    3_before_amount                        3428.035829
    1_before_amount_of_region              2588.236153
    2_before_amount_of_region              2588.236153
    3_before_amount_of_region              2588.236153
    1_before_amount_of_type_of_business    2581.717478
    2_before_amount_of_type_of_business    2614.039115
    3_before_amount_of_type_of_business    3025.776621
    dtype: float64




```python
# 스케일링 수행
from sklearn.preprocessing import MinMaxScaler
scaler = MinMaxScaler().fit(Train_X)
s_Train_X = scaler.transform(Train_X)
s_Test_X = scaler.transform(Test_X)

Train_X = pd.DataFrame(s_Train_X, columns=Train_X.columns)
Test_X = pd.DataFrame(s_Test_X, columns = Train_X.columns)

del s_Train_X, s_Test_X # 메모리 관리를 위해, 불필요한 값은 제거
```

#### 모델 학습

샘플 대비 특징이 적고, 특징의 타입이 전부 연속형으로 같음

- 모델 1. kNN
- 모델 2. RandomForestRegressor
- 모델 3. LightGBM
- 특징 선택: 3 ~ 10개 (기준:f_regression)


```python
from sklearn.model_selection import ParameterGrid
from sklearn.neighbors import KNeighborsRegressor as KNN
from sklearn.ensemble import RandomForestRegressor as RFR
from lightgbm import LGBMRegressor as LGB
from sklearn.feature_selection import *
```


```python
# 파라미터 그리드 생성
param_grid = dict()
# 입력 : 모델 함수, 출력: 모델의 하이퍼 파라미ㅓ 그리드

# 모델별 파라미터 그리드 생성
param_grid_for_knn = ParameterGrid({"n_neighbors": [1, 3, 5, 7],
                                   "metric": ['euclidean', 'cosine']})

param_grid_for_RFR = ParameterGrid({"max_depth": [1,2,3,4],
                                   "n_estimators":[100,200],
                                   "max_samples":[0.5, 0.6, 0.7, None]}) # 특징 대비 샘플이 많아서 붓스트랩 비율(max_sample)을 

param_grid_for_LGB = ParameterGrid({"max_depth":[1,2,3,4],
                                   "n_estimators":[100,200],
                                   "learning_rate": [0.05, 0.1, 0.15]})

# 모델 - 하이퍼 파라미터 그리드를 param_grid에 추가
param_grid[KNN] = param_grid_for_knn
param_grid[RFR] = param_grid_for_RFR
param_grid[LGB] = param_grid_for_LGB
```


```python
# 출력을 위한 max_iter_num 계산
max_iter_num = 0
for k in range(10, 2, -1):
    for M in param_grid.keys():
        for P in param_grid[M]:
            max_iter_num += 1
           
from sklearn.metrics import mean_absolute_error as MAE

best_score = 9999999999
iteration_num = 0
for k in range(10, 2, -1): # 메모리 부담 해소를 위해, 1씩 감소시킴
    selector = SelectKBest(f_regression, k = k).fit(Train_X, Train_Y)
    selected_features = Train_X.columns[selector.get_support()]

    Train_X = Train_X[selected_features]
    Test_X = Test_X[selected_features]
    
    for M in param_grid.keys():
        for P in param_grid[M]:
            # LightGBM에서 DataFrame이 잘 처리되지 않는 것을 방지하기 위해 .values를 사용
            model = M(**P).fit(Train_X.values, Train_Y.values)
            pred_Y = model.predict(Test_X.values)
            score = MAE(Test_Y.values, pred_Y)
            
            if score < best_score:
                best_score = score
                best_model = M
                best_paramter = P
                best_features = selected_features    
                
            iteration_num += 1
            print("iter_num:{}/{}, score: {}, best_score: {}".format(iteration_num, max_iter_num, round(score, 2), round(best_score, 2)))
```

    iter_num:1/512, score: 3897555.64, best_score: 3897555.64
    iter_num:2/512, score: 3241733.99, best_score: 3241733.99
    iter_num:3/512, score: 3596607.91, best_score: 3241733.99
    iter_num:4/512, score: 3678092.07, best_score: 3241733.99
    iter_num:5/512, score: 2031247.71, best_score: 2031247.71
    iter_num:6/512, score: 1753129.82, best_score: 1753129.82
    iter_num:7/512, score: 1679245.92, best_score: 1679245.92
    iter_num:8/512, score: 1652203.08, best_score: 1652203.08
    iter_num:9/512, score: 3127679.84, best_score: 1652203.08
    iter_num:10/512, score: 3125282.99, best_score: 1652203.08
    iter_num:11/512, score: 3125547.5, best_score: 1652203.08
    iter_num:12/512, score: 3123663.51, best_score: 1652203.08
    iter_num:13/512, score: 3127227.55, best_score: 1652203.08
    iter_num:14/512, score: 3132080.53, best_score: 1652203.08
    iter_num:15/512, score: 3129735.26, best_score: 1652203.08
    iter_num:16/512, score: 3127231.28, best_score: 1652203.08
    iter_num:17/512, score: 3865594.75, best_score: 1652203.08
    iter_num:18/512, score: 3884355.75, best_score: 1652203.08
    iter_num:19/512, score: 3885041.35, best_score: 1652203.08
    iter_num:20/512, score: 3884598.98, best_score: 1652203.08
    iter_num:21/512, score: 3881793.27, best_score: 1652203.08
    iter_num:22/512, score: 3880608.21, best_score: 1652203.08
    iter_num:23/512, score: 3881512.55, best_score: 1652203.08
    iter_num:24/512, score: 3884453.31, best_score: 1652203.08
    iter_num:25/512, score: 4287752.24, best_score: 1652203.08
    iter_num:26/512, score: 4288343.06, best_score: 1652203.08
    iter_num:27/512, score: 4278944.95, best_score: 1652203.08
    iter_num:28/512, score: 4272619.72, best_score: 1652203.08
    iter_num:29/512, score: 4286836.71, best_score: 1652203.08
    iter_num:30/512, score: 4274370.1, best_score: 1652203.08
    iter_num:31/512, score: 4283658.64, best_score: 1652203.08
    iter_num:32/512, score: 4284466.48, best_score: 1652203.08
    iter_num:33/512, score: 4557991.99, best_score: 1652203.08
    iter_num:34/512, score: 4537814.29, best_score: 1652203.08
    iter_num:35/512, score: 4551897.93, best_score: 1652203.08
    iter_num:36/512, score: 4525646.26, best_score: 1652203.08
    iter_num:37/512, score: 4552967.43, best_score: 1652203.08
    iter_num:38/512, score: 4549152.85, best_score: 1652203.08
    iter_num:39/512, score: 4531208.07, best_score: 1652203.08
    iter_num:40/512, score: 4534477.75, best_score: 1652203.08
    iter_num:41/512, score: 4221596.06, best_score: 1652203.08
    iter_num:42/512, score: 4527332.63, best_score: 1652203.08
    iter_num:43/512, score: 4485904.7, best_score: 1652203.08
    iter_num:44/512, score: 4506996.74, best_score: 1652203.08
    iter_num:45/512, score: 4487000.75, best_score: 1652203.08
    iter_num:46/512, score: 4518144.49, best_score: 1652203.08
    iter_num:47/512, score: 4623364.1, best_score: 1652203.08
    iter_num:48/512, score: 4565402.64, best_score: 1652203.08
    iter_num:49/512, score: 4531526.07, best_score: 1652203.08
    iter_num:50/512, score: 4547257.81, best_score: 1652203.08
    iter_num:51/512, score: 4415296.04, best_score: 1652203.08
    iter_num:52/512, score: 4013008.33, best_score: 1652203.08
    iter_num:53/512, score: 4371449.64, best_score: 1652203.08
    iter_num:54/512, score: 4203310.74, best_score: 1652203.08
    iter_num:55/512, score: 4231881.77, best_score: 1652203.08
    iter_num:56/512, score: 4093514.16, best_score: 1652203.08
    iter_num:57/512, score: 4581836.14, best_score: 1652203.08
    iter_num:58/512, score: 4401684.33, best_score: 1652203.08
    iter_num:59/512, score: 4427312.41, best_score: 1652203.08
    iter_num:60/512, score: 4010338.26, best_score: 1652203.08
    iter_num:61/512, score: 4735618.27, best_score: 1652203.08
    iter_num:62/512, score: 4582673.04, best_score: 1652203.08
    iter_num:63/512, score: 4029426.53, best_score: 1652203.08
    iter_num:64/512, score: 4554429.45, best_score: 1652203.08
    iter_num:65/512, score: 3890092.11, best_score: 1652203.08
    iter_num:66/512, score: 3227243.63, best_score: 1652203.08
    iter_num:67/512, score: 3577446.88, best_score: 1652203.08
    iter_num:68/512, score: 3670972.6, best_score: 1652203.08
    iter_num:69/512, score: 1991078.61, best_score: 1652203.08
    iter_num:70/512, score: 1730160.96, best_score: 1652203.08
    iter_num:71/512, score: 1662932.84, best_score: 1652203.08
    iter_num:72/512, score: 1636492.75, best_score: 1636492.75
    iter_num:73/512, score: 3118992.43, best_score: 1636492.75
    iter_num:74/512, score: 3123173.5, best_score: 1636492.75
    iter_num:75/512, score: 3125094.01, best_score: 1636492.75
    iter_num:76/512, score: 3124921.64, best_score: 1636492.75
    iter_num:77/512, score: 3125749.69, best_score: 1636492.75
    iter_num:78/512, score: 3127090.76, best_score: 1636492.75
    iter_num:79/512, score: 3125478.15, best_score: 1636492.75
    iter_num:80/512, score: 3125954.21, best_score: 1636492.75
    iter_num:81/512, score: 3882069.06, best_score: 1636492.75
    iter_num:82/512, score: 3884598.94, best_score: 1636492.75
    iter_num:83/512, score: 3882771.19, best_score: 1636492.75
    iter_num:84/512, score: 3890029.68, best_score: 1636492.75
    iter_num:85/512, score: 3887615.63, best_score: 1636492.75
    iter_num:86/512, score: 3884898.49, best_score: 1636492.75
    iter_num:87/512, score: 3876972.33, best_score: 1636492.75
    iter_num:88/512, score: 3882073.6, best_score: 1636492.75
    iter_num:89/512, score: 4285446.43, best_score: 1636492.75
    iter_num:90/512, score: 4290917.87, best_score: 1636492.75
    iter_num:91/512, score: 4284726.44, best_score: 1636492.75
    iter_num:92/512, score: 4271793.14, best_score: 1636492.75
    iter_num:93/512, score: 4286014.92, best_score: 1636492.75
    iter_num:94/512, score: 4271999.93, best_score: 1636492.75
    iter_num:95/512, score: 4290192.5, best_score: 1636492.75
    iter_num:96/512, score: 4276163.98, best_score: 1636492.75
    iter_num:97/512, score: 4557395.14, best_score: 1636492.75
    iter_num:98/512, score: 4540876.84, best_score: 1636492.75
    iter_num:99/512, score: 4539415.87, best_score: 1636492.75
    iter_num:100/512, score: 4525852.57, best_score: 1636492.75
    iter_num:101/512, score: 4530029.03, best_score: 1636492.75
    iter_num:102/512, score: 4535953.07, best_score: 1636492.75
    iter_num:103/512, score: 4528201.04, best_score: 1636492.75
    iter_num:104/512, score: 4536185.51, best_score: 1636492.75
    iter_num:105/512, score: 4221596.06, best_score: 1636492.75
    iter_num:106/512, score: 4595250.95, best_score: 1636492.75
    iter_num:107/512, score: 4624454.84, best_score: 1636492.75
    iter_num:108/512, score: 4360731.96, best_score: 1636492.75
    iter_num:109/512, score: 4330972.37, best_score: 1636492.75
    iter_num:110/512, score: 4010836.77, best_score: 1636492.75
    iter_num:111/512, score: 4021305.99, best_score: 1636492.75
    iter_num:112/512, score: 3810846.91, best_score: 1636492.75
    iter_num:113/512, score: 4601337.44, best_score: 1636492.75
    iter_num:114/512, score: 4645607.5, best_score: 1636492.75
    iter_num:115/512, score: 4266863.13, best_score: 1636492.75
    iter_num:116/512, score: 3611536.93, best_score: 1636492.75
    iter_num:117/512, score: 3967317.82, best_score: 1636492.75
    iter_num:118/512, score: 3526023.96, best_score: 1636492.75
    iter_num:119/512, score: 3863049.18, best_score: 1636492.75
    iter_num:120/512, score: 3510631.05, best_score: 1636492.75
    iter_num:121/512, score: 4684441.4, best_score: 1636492.75
    iter_num:122/512, score: 4498736.8, best_score: 1636492.75
    iter_num:123/512, score: 3904950.88, best_score: 1636492.75
    iter_num:124/512, score: 3477146.22, best_score: 1636492.75
    iter_num:125/512, score: 3828681.11, best_score: 1636492.75
    iter_num:126/512, score: 3388565.54, best_score: 1636492.75
    iter_num:127/512, score: 3783193.61, best_score: 1636492.75
    iter_num:128/512, score: 3794577.08, best_score: 1636492.75
    iter_num:129/512, score: 4219667.65, best_score: 1636492.75
    iter_num:130/512, score: 3534224.41, best_score: 1636492.75
    iter_num:131/512, score: 3812406.74, best_score: 1636492.75
    iter_num:132/512, score: 4034521.79, best_score: 1636492.75
    iter_num:133/512, score: 1991532.4, best_score: 1636492.75
    iter_num:134/512, score: 1731364.81, best_score: 1636492.75
    iter_num:135/512, score: 1661450.66, best_score: 1636492.75
    iter_num:136/512, score: 1633572.56, best_score: 1633572.56
    iter_num:137/512, score: 3125373.51, best_score: 1633572.56
    iter_num:138/512, score: 3125936.23, best_score: 1633572.56
    iter_num:139/512, score: 3128092.46, best_score: 1633572.56
    iter_num:140/512, score: 3124412.45, best_score: 1633572.56
    iter_num:141/512, score: 3125965.38, best_score: 1633572.56
    iter_num:142/512, score: 3124152.45, best_score: 1633572.56
    iter_num:143/512, score: 3126403.2, best_score: 1633572.56
    iter_num:144/512, score: 3130177.44, best_score: 1633572.56
    iter_num:145/512, score: 3878463.71, best_score: 1633572.56
    iter_num:146/512, score: 3880450.49, best_score: 1633572.56
    iter_num:147/512, score: 3877994.1, best_score: 1633572.56
    iter_num:148/512, score: 3873651.49, best_score: 1633572.56
    iter_num:149/512, score: 3882482.11, best_score: 1633572.56
    iter_num:150/512, score: 3877653.69, best_score: 1633572.56
    iter_num:151/512, score: 3871811.03, best_score: 1633572.56
    iter_num:152/512, score: 3880776.29, best_score: 1633572.56
    iter_num:153/512, score: 4293653.66, best_score: 1633572.56
    iter_num:154/512, score: 4280952.39, best_score: 1633572.56
    iter_num:155/512, score: 4267424.45, best_score: 1633572.56
    iter_num:156/512, score: 4281450.14, best_score: 1633572.56
    iter_num:157/512, score: 4271648.81, best_score: 1633572.56
    iter_num:158/512, score: 4275960.95, best_score: 1633572.56
    iter_num:159/512, score: 4273550.77, best_score: 1633572.56
    iter_num:160/512, score: 4281048.22, best_score: 1633572.56
    iter_num:161/512, score: 4548358.58, best_score: 1633572.56
    iter_num:162/512, score: 4533294.48, best_score: 1633572.56
    iter_num:163/512, score: 4536892.43, best_score: 1633572.56
    iter_num:164/512, score: 4542977.32, best_score: 1633572.56
    iter_num:165/512, score: 4509862.41, best_score: 1633572.56
    iter_num:166/512, score: 4523918.59, best_score: 1633572.56
    iter_num:167/512, score: 4531547.64, best_score: 1633572.56
    iter_num:168/512, score: 4530494.75, best_score: 1633572.56
    iter_num:169/512, score: 4221596.06, best_score: 1633572.56
    iter_num:170/512, score: 4592428.79, best_score: 1633572.56
    iter_num:171/512, score: 4603179.26, best_score: 1633572.56
    iter_num:172/512, score: 4320876.02, best_score: 1633572.56
    iter_num:173/512, score: 4330657.57, best_score: 1633572.56
    iter_num:174/512, score: 4010874.62, best_score: 1633572.56
    iter_num:175/512, score: 4125056.56, best_score: 1633572.56
    iter_num:176/512, score: 3717549.32, best_score: 1633572.56
    iter_num:177/512, score: 4611382.03, best_score: 1633572.56
    iter_num:178/512, score: 4653695.52, best_score: 1633572.56
    iter_num:179/512, score: 4185270.4, best_score: 1633572.56
    iter_num:180/512, score: 3616586.79, best_score: 1633572.56
    iter_num:181/512, score: 3954910.8, best_score: 1633572.56
    iter_num:182/512, score: 3625422.75, best_score: 1633572.56
    iter_num:183/512, score: 3717993.78, best_score: 1633572.56
    iter_num:184/512, score: 3499005.04, best_score: 1633572.56
    iter_num:185/512, score: 4658946.2, best_score: 1633572.56
    iter_num:186/512, score: 4518264.42, best_score: 1633572.56
    iter_num:187/512, score: 3900523.73, best_score: 1633572.56
    iter_num:188/512, score: 3400197.03, best_score: 1633572.56
    iter_num:189/512, score: 3830830.56, best_score: 1633572.56
    iter_num:190/512, score: 3653789.58, best_score: 1633572.56
    iter_num:191/512, score: 3533225.57, best_score: 1633572.56
    iter_num:192/512, score: 3583114.54, best_score: 1633572.56
    iter_num:193/512, score: 4540536.34, best_score: 1633572.56
    iter_num:194/512, score: 4183092.63, best_score: 1633572.56
    iter_num:195/512, score: 4052040.89, best_score: 1633572.56
    iter_num:196/512, score: 4333872.19, best_score: 1633572.56
    iter_num:197/512, score: 1996136.18, best_score: 1633572.56
    iter_num:198/512, score: 1739143.16, best_score: 1633572.56
    iter_num:199/512, score: 1667337.78, best_score: 1633572.56
    iter_num:200/512, score: 1635971.98, best_score: 1633572.56
    iter_num:201/512, score: 3123806.55, best_score: 1633572.56
    iter_num:202/512, score: 3126822.17, best_score: 1633572.56
    iter_num:203/512, score: 3123654.53, best_score: 1633572.56
    iter_num:204/512, score: 3126646.47, best_score: 1633572.56
    iter_num:205/512, score: 3127244.86, best_score: 1633572.56
    iter_num:206/512, score: 3123833.76, best_score: 1633572.56
    iter_num:207/512, score: 3125198.5, best_score: 1633572.56
    iter_num:208/512, score: 3124727.72, best_score: 1633572.56
    iter_num:209/512, score: 3876035.44, best_score: 1633572.56
    iter_num:210/512, score: 3873240.06, best_score: 1633572.56
    iter_num:211/512, score: 3879398.82, best_score: 1633572.56
    iter_num:212/512, score: 3883459.33, best_score: 1633572.56
    iter_num:213/512, score: 3879311.2, best_score: 1633572.56
    iter_num:214/512, score: 3876426.33, best_score: 1633572.56
    iter_num:215/512, score: 3878776.93, best_score: 1633572.56
    iter_num:216/512, score: 3883443.92, best_score: 1633572.56
    iter_num:217/512, score: 4274316.05, best_score: 1633572.56
    iter_num:218/512, score: 4282201.63, best_score: 1633572.56
    iter_num:219/512, score: 4286735.44, best_score: 1633572.56
    iter_num:220/512, score: 4279168.15, best_score: 1633572.56
    iter_num:221/512, score: 4277317.0, best_score: 1633572.56
    iter_num:222/512, score: 4280910.93, best_score: 1633572.56
    iter_num:223/512, score: 4272784.89, best_score: 1633572.56
    iter_num:224/512, score: 4269462.93, best_score: 1633572.56
    iter_num:225/512, score: 4544941.52, best_score: 1633572.56
    iter_num:226/512, score: 4541781.22, best_score: 1633572.56
    iter_num:227/512, score: 4548411.65, best_score: 1633572.56
    iter_num:228/512, score: 4543863.26, best_score: 1633572.56
    iter_num:229/512, score: 4548965.24, best_score: 1633572.56
    iter_num:230/512, score: 4548984.94, best_score: 1633572.56
    iter_num:231/512, score: 4519241.61, best_score: 1633572.56
    iter_num:232/512, score: 4527950.33, best_score: 1633572.56
    iter_num:233/512, score: 4221596.06, best_score: 1633572.56
    iter_num:234/512, score: 4592428.79, best_score: 1633572.56
    iter_num:235/512, score: 4603922.93, best_score: 1633572.56
    iter_num:236/512, score: 4264536.12, best_score: 1633572.56
    iter_num:237/512, score: 4364956.89, best_score: 1633572.56
    iter_num:238/512, score: 3962697.65, best_score: 1633572.56
    iter_num:239/512, score: 4081764.23, best_score: 1633572.56
    iter_num:240/512, score: 3771672.72, best_score: 1633572.56
    iter_num:241/512, score: 4607377.08, best_score: 1633572.56
    iter_num:242/512, score: 4643359.75, best_score: 1633572.56
    iter_num:243/512, score: 4214180.84, best_score: 1633572.56
    iter_num:244/512, score: 3709090.53, best_score: 1633572.56
    iter_num:245/512, score: 3883720.2, best_score: 1633572.56
    iter_num:246/512, score: 3573906.58, best_score: 1633572.56
    iter_num:247/512, score: 3639747.81, best_score: 1633572.56
    iter_num:248/512, score: 3488870.81, best_score: 1633572.56
    iter_num:249/512, score: 4667776.75, best_score: 1633572.56
    iter_num:250/512, score: 4515614.9, best_score: 1633572.56
    iter_num:251/512, score: 3956639.48, best_score: 1633572.56
    iter_num:252/512, score: 3417014.59, best_score: 1633572.56
    iter_num:253/512, score: 3824816.04, best_score: 1633572.56
    iter_num:254/512, score: 3714358.51, best_score: 1633572.56
    iter_num:255/512, score: 3666102.53, best_score: 1633572.56
    iter_num:256/512, score: 3733592.16, best_score: 1633572.56
    iter_num:257/512, score: 5953811.56, best_score: 1633572.56
    iter_num:258/512, score: 4409638.81, best_score: 1633572.56
    iter_num:259/512, score: 4345609.81, best_score: 1633572.56
    iter_num:260/512, score: 4427139.3, best_score: 1633572.56
    iter_num:261/512, score: 2219843.5, best_score: 1633572.56
    iter_num:262/512, score: 1934590.4, best_score: 1633572.56
    iter_num:263/512, score: 1843755.15, best_score: 1633572.56
    iter_num:264/512, score: 1804607.77, best_score: 1633572.56
    iter_num:265/512, score: 3124262.4, best_score: 1633572.56
    iter_num:266/512, score: 3126170.94, best_score: 1633572.56
    iter_num:267/512, score: 3126574.24, best_score: 1633572.56
    iter_num:268/512, score: 3129812.16, best_score: 1633572.56
    iter_num:269/512, score: 3123649.42, best_score: 1633572.56
    iter_num:270/512, score: 3125249.38, best_score: 1633572.56
    iter_num:271/512, score: 3127252.48, best_score: 1633572.56
    iter_num:272/512, score: 3127199.82, best_score: 1633572.56
    iter_num:273/512, score: 3893430.5, best_score: 1633572.56
    iter_num:274/512, score: 3882720.54, best_score: 1633572.56
    iter_num:275/512, score: 3893683.54, best_score: 1633572.56
    iter_num:276/512, score: 3888664.75, best_score: 1633572.56
    iter_num:277/512, score: 3884399.92, best_score: 1633572.56
    iter_num:278/512, score: 3883428.5, best_score: 1633572.56
    iter_num:279/512, score: 3890138.19, best_score: 1633572.56
    iter_num:280/512, score: 3882942.64, best_score: 1633572.56
    iter_num:281/512, score: 4283489.67, best_score: 1633572.56
    iter_num:282/512, score: 4271848.27, best_score: 1633572.56
    iter_num:283/512, score: 4286837.25, best_score: 1633572.56
    iter_num:284/512, score: 4276616.54, best_score: 1633572.56
    iter_num:285/512, score: 4277677.94, best_score: 1633572.56
    iter_num:286/512, score: 4287571.0, best_score: 1633572.56
    iter_num:287/512, score: 4271750.84, best_score: 1633572.56
    iter_num:288/512, score: 4276218.58, best_score: 1633572.56
    iter_num:289/512, score: 4532464.08, best_score: 1633572.56
    iter_num:290/512, score: 4535669.87, best_score: 1633572.56
    iter_num:291/512, score: 4534844.26, best_score: 1633572.56
    iter_num:292/512, score: 4549754.03, best_score: 1633572.56
    iter_num:293/512, score: 4545055.33, best_score: 1633572.56
    iter_num:294/512, score: 4541013.04, best_score: 1633572.56
    iter_num:295/512, score: 4529847.82, best_score: 1633572.56
    iter_num:296/512, score: 4536619.54, best_score: 1633572.56
    iter_num:297/512, score: 4221596.06, best_score: 1633572.56
    iter_num:298/512, score: 4584486.81, best_score: 1633572.56
    iter_num:299/512, score: 4589120.18, best_score: 1633572.56
    iter_num:300/512, score: 4558720.2, best_score: 1633572.56
    iter_num:301/512, score: 4483569.76, best_score: 1633572.56
    iter_num:302/512, score: 4325904.87, best_score: 1633572.56
    iter_num:303/512, score: 4348132.13, best_score: 1633572.56
    iter_num:304/512, score: 4379862.44, best_score: 1633572.56
    iter_num:305/512, score: 4584841.34, best_score: 1633572.56
    iter_num:306/512, score: 4573702.1, best_score: 1633572.56
    iter_num:307/512, score: 4521972.79, best_score: 1633572.56
    iter_num:308/512, score: 4275892.46, best_score: 1633572.56
    iter_num:309/512, score: 4293875.46, best_score: 1633572.56
    iter_num:310/512, score: 4226972.87, best_score: 1633572.56
    iter_num:311/512, score: 4363991.44, best_score: 1633572.56
    iter_num:312/512, score: 4283177.42, best_score: 1633572.56
    iter_num:313/512, score: 4618531.04, best_score: 1633572.56
    iter_num:314/512, score: 4522098.26, best_score: 1633572.56
    iter_num:315/512, score: 4297025.42, best_score: 1633572.56
    iter_num:316/512, score: 4196721.45, best_score: 1633572.56
    iter_num:317/512, score: 4314436.51, best_score: 1633572.56
    iter_num:318/512, score: 4269688.35, best_score: 1633572.56
    iter_num:319/512, score: 4350640.71, best_score: 1633572.56
    iter_num:320/512, score: 4305303.1, best_score: 1633572.56
    iter_num:321/512, score: 4559218.36, best_score: 1633572.56
    iter_num:322/512, score: 4956745.63, best_score: 1633572.56
    iter_num:323/512, score: 4620170.28, best_score: 1633572.56
    iter_num:324/512, score: 4384270.07, best_score: 1633572.56
    iter_num:325/512, score: 2235195.84, best_score: 1633572.56
    iter_num:326/512, score: 1965079.86, best_score: 1633572.56
    iter_num:327/512, score: 1889726.64, best_score: 1633572.56
    iter_num:328/512, score: 1839710.57, best_score: 1633572.56
    iter_num:329/512, score: 3124194.91, best_score: 1633572.56
    iter_num:330/512, score: 3127081.31, best_score: 1633572.56
    iter_num:331/512, score: 3119424.24, best_score: 1633572.56
    iter_num:332/512, score: 3123570.0, best_score: 1633572.56
    iter_num:333/512, score: 3125755.38, best_score: 1633572.56
    iter_num:334/512, score: 3122244.61, best_score: 1633572.56
    iter_num:335/512, score: 3129768.24, best_score: 1633572.56
    iter_num:336/512, score: 3126697.97, best_score: 1633572.56
    iter_num:337/512, score: 3898658.59, best_score: 1633572.56
    iter_num:338/512, score: 3872631.35, best_score: 1633572.56
    iter_num:339/512, score: 3880950.57, best_score: 1633572.56
    iter_num:340/512, score: 3879838.52, best_score: 1633572.56
    iter_num:341/512, score: 3881618.29, best_score: 1633572.56
    iter_num:342/512, score: 3880430.4, best_score: 1633572.56
    iter_num:343/512, score: 3879504.69, best_score: 1633572.56
    iter_num:344/512, score: 3879360.06, best_score: 1633572.56
    iter_num:345/512, score: 4290246.75, best_score: 1633572.56
    iter_num:346/512, score: 4292513.61, best_score: 1633572.56
    iter_num:347/512, score: 4278748.14, best_score: 1633572.56
    iter_num:348/512, score: 4283300.65, best_score: 1633572.56
    iter_num:349/512, score: 4293361.53, best_score: 1633572.56
    iter_num:350/512, score: 4279212.23, best_score: 1633572.56
    iter_num:351/512, score: 4267242.33, best_score: 1633572.56
    iter_num:352/512, score: 4271459.86, best_score: 1633572.56
    iter_num:353/512, score: 4545200.95, best_score: 1633572.56
    iter_num:354/512, score: 4543471.55, best_score: 1633572.56
    iter_num:355/512, score: 4550615.35, best_score: 1633572.56
    iter_num:356/512, score: 4528584.65, best_score: 1633572.56
    iter_num:357/512, score: 4506321.0, best_score: 1633572.56
    iter_num:358/512, score: 4547380.65, best_score: 1633572.56
    iter_num:359/512, score: 4530614.79, best_score: 1633572.56
    iter_num:360/512, score: 4538585.78, best_score: 1633572.56
    iter_num:361/512, score: 4221596.06, best_score: 1633572.56
    iter_num:362/512, score: 4578368.33, best_score: 1633572.56
    iter_num:363/512, score: 4592379.85, best_score: 1633572.56
    iter_num:364/512, score: 4520123.19, best_score: 1633572.56
    iter_num:365/512, score: 4494288.16, best_score: 1633572.56
    iter_num:366/512, score: 4502236.68, best_score: 1633572.56
    iter_num:367/512, score: 4345540.99, best_score: 1633572.56
    iter_num:368/512, score: 4560640.6, best_score: 1633572.56
    iter_num:369/512, score: 4583443.68, best_score: 1633572.56
    iter_num:370/512, score: 4551900.48, best_score: 1633572.56
    iter_num:371/512, score: 4530924.73, best_score: 1633572.56
    iter_num:372/512, score: 4276649.75, best_score: 1633572.56
    iter_num:373/512, score: 4443148.32, best_score: 1633572.56
    iter_num:374/512, score: 4652512.58, best_score: 1633572.56
    iter_num:375/512, score: 4550378.5, best_score: 1633572.56
    iter_num:376/512, score: 4555321.53, best_score: 1633572.56
    iter_num:377/512, score: 4610701.74, best_score: 1633572.56
    iter_num:378/512, score: 4496916.07, best_score: 1633572.56
    iter_num:379/512, score: 4294752.16, best_score: 1633572.56
    iter_num:380/512, score: 4215761.9, best_score: 1633572.56
    iter_num:381/512, score: 4597967.62, best_score: 1633572.56
    iter_num:382/512, score: 4666445.9, best_score: 1633572.56
    iter_num:383/512, score: 4425483.39, best_score: 1633572.56
    iter_num:384/512, score: 4506485.95, best_score: 1633572.56
    iter_num:385/512, score: 5284585.44, best_score: 1633572.56
    iter_num:386/512, score: 5100967.35, best_score: 1633572.56
    iter_num:387/512, score: 4841498.78, best_score: 1633572.56
    iter_num:388/512, score: 4579555.4, best_score: 1633572.56
    iter_num:389/512, score: 2242123.57, best_score: 1633572.56
    iter_num:390/512, score: 1990534.79, best_score: 1633572.56
    iter_num:391/512, score: 1897679.5, best_score: 1633572.56
    iter_num:392/512, score: 1848076.03, best_score: 1633572.56
    iter_num:393/512, score: 3121014.66, best_score: 1633572.56
    iter_num:394/512, score: 3126055.14, best_score: 1633572.56
    iter_num:395/512, score: 3131564.1, best_score: 1633572.56
    iter_num:396/512, score: 3126876.37, best_score: 1633572.56
    iter_num:397/512, score: 3132145.91, best_score: 1633572.56
    iter_num:398/512, score: 3128718.73, best_score: 1633572.56
    iter_num:399/512, score: 3124002.16, best_score: 1633572.56
    iter_num:400/512, score: 3125267.96, best_score: 1633572.56
    iter_num:401/512, score: 3872673.02, best_score: 1633572.56
    iter_num:402/512, score: 3875761.28, best_score: 1633572.56
    iter_num:403/512, score: 3886434.53, best_score: 1633572.56
    iter_num:404/512, score: 3881045.8, best_score: 1633572.56
    iter_num:405/512, score: 3887633.88, best_score: 1633572.56
    iter_num:406/512, score: 3880077.68, best_score: 1633572.56
    iter_num:407/512, score: 3890282.02, best_score: 1633572.56
    iter_num:408/512, score: 3886465.69, best_score: 1633572.56
    iter_num:409/512, score: 4279461.21, best_score: 1633572.56
    iter_num:410/512, score: 4276664.11, best_score: 1633572.56
    iter_num:411/512, score: 4286738.09, best_score: 1633572.56
    iter_num:412/512, score: 4287970.57, best_score: 1633572.56
    iter_num:413/512, score: 4288435.71, best_score: 1633572.56
    iter_num:414/512, score: 4278391.7, best_score: 1633572.56
    iter_num:415/512, score: 4269790.73, best_score: 1633572.56
    iter_num:416/512, score: 4271359.12, best_score: 1633572.56
    iter_num:417/512, score: 4545786.78, best_score: 1633572.56
    iter_num:418/512, score: 4504403.13, best_score: 1633572.56
    iter_num:419/512, score: 4530460.3, best_score: 1633572.56
    iter_num:420/512, score: 4550613.12, best_score: 1633572.56
    iter_num:421/512, score: 4542149.37, best_score: 1633572.56
    iter_num:422/512, score: 4544085.82, best_score: 1633572.56
    iter_num:423/512, score: 4535609.37, best_score: 1633572.56
    iter_num:424/512, score: 4540646.93, best_score: 1633572.56
    iter_num:425/512, score: 4221596.06, best_score: 1633572.56
    iter_num:426/512, score: 4577887.92, best_score: 1633572.56
    iter_num:427/512, score: 4595582.4, best_score: 1633572.56
    iter_num:428/512, score: 4480122.33, best_score: 1633572.56
    iter_num:429/512, score: 4548426.34, best_score: 1633572.56
    iter_num:430/512, score: 4445112.47, best_score: 1633572.56
    iter_num:431/512, score: 4488525.23, best_score: 1633572.56
    iter_num:432/512, score: 4425906.31, best_score: 1633572.56
    iter_num:433/512, score: 4583303.66, best_score: 1633572.56
    iter_num:434/512, score: 4521978.44, best_score: 1633572.56
    iter_num:435/512, score: 4480116.13, best_score: 1633572.56
    iter_num:436/512, score: 4447452.91, best_score: 1633572.56
    iter_num:437/512, score: 4426200.07, best_score: 1633572.56
    iter_num:438/512, score: 4580668.58, best_score: 1633572.56
    iter_num:439/512, score: 4403603.9, best_score: 1633572.56
    iter_num:440/512, score: 4411113.03, best_score: 1633572.56
    iter_num:441/512, score: 4592862.61, best_score: 1633572.56
    iter_num:442/512, score: 4491254.3, best_score: 1633572.56
    iter_num:443/512, score: 4413321.6, best_score: 1633572.56
    iter_num:444/512, score: 4458637.64, best_score: 1633572.56
    iter_num:445/512, score: 4460498.95, best_score: 1633572.56
    iter_num:446/512, score: 4479366.98, best_score: 1633572.56
    iter_num:447/512, score: 4362415.76, best_score: 1633572.56
    iter_num:448/512, score: 4421543.64, best_score: 1633572.56
    iter_num:449/512, score: 3948657.8, best_score: 1633572.56
    iter_num:450/512, score: 3369956.1, best_score: 1633572.56
    iter_num:451/512, score: 3558257.01, best_score: 1633572.56
    iter_num:452/512, score: 3273629.13, best_score: 1633572.56
    iter_num:453/512, score: 2781566.55, best_score: 1633572.56
    iter_num:454/512, score: 2478047.29, best_score: 1633572.56
    iter_num:455/512, score: 2406646.66, best_score: 1633572.56
    iter_num:456/512, score: 2367109.93, best_score: 1633572.56
    iter_num:457/512, score: 3126611.91, best_score: 1633572.56
    iter_num:458/512, score: 3125139.02, best_score: 1633572.56
    iter_num:459/512, score: 3118047.43, best_score: 1633572.56
    iter_num:460/512, score: 3130671.27, best_score: 1633572.56
    iter_num:461/512, score: 3123181.22, best_score: 1633572.56
    iter_num:462/512, score: 3129903.98, best_score: 1633572.56
    iter_num:463/512, score: 3122887.91, best_score: 1633572.56
    iter_num:464/512, score: 3125969.02, best_score: 1633572.56
    iter_num:465/512, score: 3880300.18, best_score: 1633572.56
    iter_num:466/512, score: 3878997.34, best_score: 1633572.56
    iter_num:467/512, score: 3870084.67, best_score: 1633572.56
    iter_num:468/512, score: 3881375.46, best_score: 1633572.56
    iter_num:469/512, score: 3884928.3, best_score: 1633572.56
    iter_num:470/512, score: 3885718.15, best_score: 1633572.56
    iter_num:471/512, score: 3884263.38, best_score: 1633572.56
    iter_num:472/512, score: 3883267.31, best_score: 1633572.56
    iter_num:473/512, score: 4286741.59, best_score: 1633572.56
    iter_num:474/512, score: 4267822.83, best_score: 1633572.56
    iter_num:475/512, score: 4302187.79, best_score: 1633572.56
    iter_num:476/512, score: 4295599.1, best_score: 1633572.56
    iter_num:477/512, score: 4297424.89, best_score: 1633572.56
    iter_num:478/512, score: 4282196.37, best_score: 1633572.56
    iter_num:479/512, score: 4260398.03, best_score: 1633572.56
    iter_num:480/512, score: 4281865.74, best_score: 1633572.56
    iter_num:481/512, score: 4570960.52, best_score: 1633572.56
    iter_num:482/512, score: 4548003.61, best_score: 1633572.56
    iter_num:483/512, score: 4548061.74, best_score: 1633572.56
    iter_num:484/512, score: 4537366.35, best_score: 1633572.56
    iter_num:485/512, score: 4544956.07, best_score: 1633572.56
    iter_num:486/512, score: 4533843.88, best_score: 1633572.56
    iter_num:487/512, score: 4531378.33, best_score: 1633572.56
    iter_num:488/512, score: 4519623.21, best_score: 1633572.56
    iter_num:489/512, score: 4221596.06, best_score: 1633572.56
    iter_num:490/512, score: 4583058.5, best_score: 1633572.56
    iter_num:491/512, score: 4622878.37, best_score: 1633572.56
    iter_num:492/512, score: 4497974.31, best_score: 1633572.56
    iter_num:493/512, score: 4600219.81, best_score: 1633572.56
    iter_num:494/512, score: 4469348.01, best_score: 1633572.56
    iter_num:495/512, score: 4584320.09, best_score: 1633572.56
    iter_num:496/512, score: 4320327.07, best_score: 1633572.56
    iter_num:497/512, score: 4592108.44, best_score: 1633572.56
    iter_num:498/512, score: 4542076.52, best_score: 1633572.56
    iter_num:499/512, score: 4480012.75, best_score: 1633572.56
    iter_num:500/512, score: 4431292.25, best_score: 1633572.56
    iter_num:501/512, score: 4444353.94, best_score: 1633572.56
    iter_num:502/512, score: 4340680.28, best_score: 1633572.56
    iter_num:503/512, score: 4324137.03, best_score: 1633572.56
    iter_num:504/512, score: 4265164.14, best_score: 1633572.56
    iter_num:505/512, score: 4614924.93, best_score: 1633572.56
    iter_num:506/512, score: 4553539.14, best_score: 1633572.56
    iter_num:507/512, score: 4439568.67, best_score: 1633572.56
    iter_num:508/512, score: 4398323.16, best_score: 1633572.56
    iter_num:509/512, score: 4398685.19, best_score: 1633572.56
    iter_num:510/512, score: 4348089.31, best_score: 1633572.56
    iter_num:511/512, score: 4322741.54, best_score: 1633572.56
    iter_num:512/512, score: 4251483.0, best_score: 1633572.56
    

### 최종 모델 학습


```python
def pipeline(X):
    X[biased_variables] = X[biased_variables] - X[biased_variables].min() + 1
    X[biased_variables] = np.sqrt(X[biased_variables])
    X = pd.DataFrame(scaler.transform(X), columns = X.columns)
    X = X[best_features]
    return X

model = best_model(**best_paramter).fit(pipeline(X).values, Y)
```


```python
# 2019-03-01 ~ 2019-05-31
submission_df['t'] = (2019-2016) * 12 + 2
```


```python
# region 변수와 type_of_business 변수 부착
submission_df['region'] = submission_df['store_id'].replace(store_to_region)
submission_df['type_of_business'] = submission_df['store_id'].replace(store_to_type_of_business)
```

#### 특징 부착


```python
submission_df['평균할부율'] = submission_df['store_id'].replace(installment_term_per_store.to_dict())
submission_df.head()
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
      <th>store_id</th>
      <th>amount</th>
      <th>t</th>
      <th>region</th>
      <th>type_of_business</th>
      <th>평균할부율</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>38</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>0.038384</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0</td>
      <td>38</td>
      <td>없음</td>
      <td>없음</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0</td>
      <td>38</td>
      <td>없음</td>
      <td>없음</td>
      <td>0.083904</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>0</td>
      <td>38</td>
      <td>서울 종로구</td>
      <td>없음</td>
      <td>0.001201</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>38</td>
      <td>없음</td>
      <td>의복 액세서리 및 모조 장신구 도매업</td>
      <td>0.075077</td>
    </tr>
  </tbody>
</table>
</div>




```python
submission_df.drop('amount', axis=1, inplace=True)
```


```python
# t - k (k = 1,2,3) 시점의 부착
# submission_df의 t는 amount_sum_per_t_and_sid의 t - k과 부착되어야 하므로, amount_sum_per_t_and_sid의 t에 k를 더함

for k in range(1,4):
    amount_sum_per_t_and_sid['t_{}'.format(k)] = amount_sum_per_t_and_sid['t'] + k
    submission_df = pd.merge(submission_df, amount_sum_per_t_and_sid.drop('t', axis=1), left_on=['store_id', 't'], right_on=['store_id', 't_{}'.format(k)])
    submission_df.rename({'amount':'{}_before_amount'.format(k)}, axis=1, inplace=True)
    submission_df.drop(['t_{}'.format(k)], axis=1, inplace=True)
    amount_sum_per_t_and_sid.drop(['t_{}'.format(k)], axis=1, inplace=True)
```


```python
# 지역 관련 변수 부착
for k in range(1,4):
    amount_mean_per_t_and_region['t_{}'.format(k)] = amount_mean_per_t_and_region['t'] + k
    submission_df = pd.merge(submission_df, amount_mean_per_t_and_region.drop('t', axis=1), left_on=['region', 't'], right_on=['region', 't_{}'.format(k)])
    submission_df.rename({'amount':'{}_before_amount_of_region'.format(k)}, axis=1, inplace=True)
    
    submission_df.drop(['t_{}'.format(k)], axis=1, inplace=True)
    amount_mean_per_t_and_region.drop(['t_{}'.format(k)], axis=1, inplace=True)
```


```python
# t - k (k = 1,2,3) 시점의 부착
# submission_df의 t는 amount_sum_per_t_and_sid의 t - k과 부착되어야 하므로, amount_sum_per_t_and_sid의 t에 k를 더함

for k in range(1,4):
    amount_mean_per_t_and_type_of_business['t_{}'.format(k)] = amount_mean_per_t_and_type_of_business['t'] + k
    submission_df = pd.merge(submission_df, amount_mean_per_t_and_type_of_business.drop('t', axis=1), left_on=['type_of_business', 't'], right_on=['type_of_business', 't_{}'.format(k)])
    submission_df.rename({'amount':'{}_before_amount_of_type_of_business'.format(k)}, axis=1, inplace=True)
    
    submission_df.drop(['t_{}'.format(k)], axis=1, inplace=True)
    amount_mean_per_t_and_type_of_business.drop(['t_{}'.format(k)], axis=1, inplace=True)

submission_df.head()
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
      <th>store_id</th>
      <th>t</th>
      <th>region</th>
      <th>type_of_business</th>
      <th>평균할부율</th>
      <th>1_before_amount</th>
      <th>2_before_amount</th>
      <th>3_before_amount</th>
      <th>1_before_amount_of_region</th>
      <th>2_before_amount_of_region</th>
      <th>3_before_amount_of_region</th>
      <th>1_before_amount_of_type_of_business</th>
      <th>2_before_amount_of_type_of_business</th>
      <th>3_before_amount_of_type_of_business</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>38</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>0.038384</td>
      <td>682857.142857</td>
      <td>874571.428571</td>
      <td>676000.000000</td>
      <td>9.468777e+05</td>
      <td>1.000725e+06</td>
      <td>9.886195e+05</td>
      <td>585125.0</td>
      <td>650055.952381</td>
      <td>558241.666667</td>
    </tr>
    <tr>
      <th>1</th>
      <td>792</td>
      <td>38</td>
      <td>없음</td>
      <td>기타 미용업</td>
      <td>0.218887</td>
      <td>743214.285714</td>
      <td>871071.428571</td>
      <td>973857.142857</td>
      <td>9.468777e+05</td>
      <td>1.000725e+06</td>
      <td>9.886195e+05</td>
      <td>585125.0</td>
      <td>650055.952381</td>
      <td>558241.666667</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1828</td>
      <td>38</td>
      <td>경기 용인시</td>
      <td>기타 미용업</td>
      <td>0.195502</td>
      <td>953000.000000</td>
      <td>816857.142857</td>
      <td>911957.142857</td>
      <td>1.801051e+06</td>
      <td>2.009936e+06</td>
      <td>1.897275e+06</td>
      <td>585125.0</td>
      <td>650055.952381</td>
      <td>558241.666667</td>
    </tr>
    <tr>
      <th>3</th>
      <td>23</td>
      <td>38</td>
      <td>경기 안양시</td>
      <td>기타 미용업</td>
      <td>0.048795</td>
      <td>660857.142857</td>
      <td>999285.714286</td>
      <td>827571.428571</td>
      <td>7.843780e+05</td>
      <td>6.421832e+05</td>
      <td>6.788446e+05</td>
      <td>585125.0</td>
      <td>650055.952381</td>
      <td>558241.666667</td>
    </tr>
    <tr>
      <th>4</th>
      <td>192</td>
      <td>38</td>
      <td>경기 화성시</td>
      <td>기타 미용업</td>
      <td>0.100542</td>
      <td>467571.428571</td>
      <td>550571.428571</td>
      <td>399142.857143</td>
      <td>1.209348e+06</td>
      <td>1.125181e+06</td>
      <td>1.049587e+06</td>
      <td>585125.0</td>
      <td>650055.952381</td>
      <td>558241.666667</td>
    </tr>
  </tbody>
</table>
</div>




```python
submission_X = submission_df[X.columns]
submission_X = pipeline(submission_X)

pred_Y = model.predict(submission_X)

result = pd.DataFrame({"store_id":submission_df['store_id'].values,
                      "pred_amount":pred_Y})
```

    C:\Users\ds990\anaconda3\lib\site-packages\pandas\core\frame.py:3191: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      self[k1] = value[k2]
    


```python
result.sort_values(by='store_id')
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
      <th>store_id</th>
      <th>pred_amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>5.168055e+06</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1</td>
      <td>1.315465e+06</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2</td>
      <td>1.564663e+06</td>
    </tr>
    <tr>
      <th>612</th>
      <td>4</td>
      <td>4.736389e+06</td>
    </tr>
    <tr>
      <th>1187</th>
      <td>5</td>
      <td>4.337580e+06</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>609</th>
      <td>2132</td>
      <td>3.245156e+06</td>
    </tr>
    <tr>
      <th>610</th>
      <td>2133</td>
      <td>1.507945e+06</td>
    </tr>
    <tr>
      <th>1186</th>
      <td>2134</td>
      <td>1.221278e+06</td>
    </tr>
    <tr>
      <th>611</th>
      <td>2135</td>
      <td>1.777776e+06</td>
    </tr>
    <tr>
      <th>1463</th>
      <td>2136</td>
      <td>8.980647e+06</td>
    </tr>
  </tbody>
</table>
<p>1967 rows × 2 columns</p>
</div>




```python

```
