### 나이브 베이스 분류기
- 베이즈 정리를 적용한 확률적 분류 알고리즘
- 모든 특성들이 독립임을 가정(naive 가정)
- 입력 특성에 따라 3개의 분류기 존재
    - 가우시안 나이브 베이즈 분류기
    - 베르누이 나이브 베이즈 분류기
    - 다항 나이브 베이즈 분류기


```python
from sklearn.naive_bayes import GaussianNB, BernoulliNB, MultinomialNB
from sklearn.datasets import fetch_covtype, fetch_20newsgroups
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler, MinMaxScaler
from sklearn.feature_extraction.text import CountVectorizer, HashingVectorizer, TfidfVectorizer
from sklearn import metrics
```


```python
prior = [0.45, 0.3, 0.15, 0.1]
likelihood = [[0.3, 0.3, 0.4], [0.7, 0.2, 0.1], [0.15, 0.5, 0.35], [0.6, 0.2, 0.2]]

idx = 0
for c, xs in zip(prior, likelihood):
    result = 1.
    
    for x in xs:
        result *= x
    result *= c
    
    idx += 1
    print(f"{idx}번째 클래스의 가능성 : {result}")
```

### 산림 토양 데이터
- 산림 지역 토양의 특징 데이터
- 토양이 어떤 종류에 속하는지 예측


```python
covtype = fetch_covtype()
print(covtype.DESCR)
```


```python
pd.DataFrame(covtype.data)
```


```python
covtype.target
```

### 학습, 평가 데이터 분류


```python
covtype_X = covtype.data
covtype_y = covtype.target
```


```python
covtype_X_train, covtype_X_test, covtype_y_train, covtype_y_test = train_test_split(covtype_X, covtype_y, test_size = 0.2)
```


```python
print("전체 데이터 크기 : {}".format(covtype_X.shape))
print("학습 데이터 크기 : {}".format(covtype_X_train.shape))
print("평가 데이터 크기 : {}".format(covtype_X_test.shape))
```


```python
covtype_df = pd.DataFrame(data=covtype_X)
covtype_df.describe()
```


```python
covtype_train_df = pd.DataFrame(data=covtype_X_train)
covtype_train_df.describe()
```


```python
covtype_test_df = pd.DataFrame(data=covtype_X_test)
covtype_test_df.describe()
```

#### 전처리 과정


```python
scaler = StandardScaler()
covtype_X_train_scale = scaler.fit_transform(covtype_X_train)
covtype_X_test_scale = scaler.transform(covtype_X_test)
```

#### 전처리 후 데이터
- 평균은 0에 가깝게, 표준편차는 1에 가깝게 정규화


```python
covtype_train_df = pd.DataFrame(data=covtype_X_train_scale)
covtype_train_df.describe()
```


```python
covtype_test_df = pd.DataFrame(data=covtype_X_test_scale)
covtype_test_df.describe()
```

### 20 Newsgroup 데이터
- 뉴스 기사가 어느 그룹에 속하는지 분류
- 뉴스 기사는 텍스트 데이터이기 때문에 특별한 전처리 과정이 필요


```python
newsgroup = fetch_20newsgroups()
print(newsgroup.DESCR)
```


```python
newsgroup.data
```


```python
newsgroup.target_names
```

#### 학습, 평가 데이터 분류


```python
newsgroup_train = fetch_20newsgroups(subset = 'train')
newsgroup_test = fetch_20newsgroups(subset = 'test')
```


```python
X_train, y_train = newsgroup_train.data, newsgroup_train.target
X_test, y_test = newsgroup_test.data, newsgroup_test.target
```

### 벡터화
- 텍스트 데이터는 기계학습 모델에 입력 할 수 없음
- 벡터화는 텍스트 데이터를 실수 벡터로 변홯내 기계학습 모델에 입력 할 수 있도록 하는 전처리 과정
- Scikit-learn에서는 Count, Tf-idf, Hashing 세가지 방법을 지원

### CountVectorizer
- 문서에 나온 단어의 수를 세서 벡터 생성


```python
count_vectorizer = CountVectorizer()
```


```python
X_train_count = count_vectorizer.fit_transform(X_train)
X_test_count = count_vectorizer.transform(X_test)
```

#### 데이터를 희소 행렬 형태로 표현


```python
X_train_count
```


```python
for v in X_train_count[0]:
    print(v)
```

#### HashingVectorizer
- 각 단어를 해쉬 값으로 표현
- 미리 정해진 크기의 벡터로 표현


```python
hash_vectorizer = HashingVectorizer(n_features = 1000)
```


```python
X_train_hash = hash_vectorizer.fit_transform(X_train)
X_test_hash = hash_vectorizer.transform(X_test)
```


```python
X_train_hash
```


```python
print(X_train_hash[0])
```

#### TfidfVectorzier
- 문서에 나온 단어 빈도(term frequency)와 역문서 빈도(inverse document frequency)를 곱해서 구함
- 각 빈도는 일반적으로 로그 스케일링 후 사용


```python
tfidf_vectorizer = TfidfVectorizer()
```


```python
X_train_tfidf = tfidf_vectorizer.fit_transform(X_train)
X_test_tfidf = tfidf_vectorizer.transform(X_test)
```


```python
X_train_tfidf
```


```python
for v in X_train_tfidf[0]:
    print(v)
```

### 가우시안 나이브 베이즈
- 입력 특성이 가우시안(정규) 분포를 갖는다고 가정


```python
model = GaussianNB()
model.fit(covtype_X_train_scale, covtype_y_train)
```


```python
predict = model.predict(covtype_X_train_scale)
acc = metrics.accuracy_score(covtype_y_train, predict)
f1 = metrics.f1_score(covtype_y_train, predict, average = None)

print("Train Accuracy : {}".format(acc))
print("Train F1 score : {}".format(f1))
```


```python
predict = model.predict(covtype_X_test_scale)
acc = metrics.accuracy_score(covtype_y_test, predict)
f1 = metrics.f1_score(covtype_y_test, predict, average = None)

print("Test Accuracy : {}".format(acc))
print("Test F1 score : {}".format(f1))
```


```python
from sklearn.datasets import make_blobs
plt.style.use(['seaborn-whitegrid'])
```


```python
def make_meshgrid(x, y, h=.02):
    x_min, x_max = x.min() -1, x.max() +1
    y_min, y_max = y.min() -1, y.max() +1
    xx, yy = np.meshgrid(np.arange(x_min, x_max, h),
                         np.arange(y_min, y_max, h))
    
    return xx, yy

def plot_contours(clf, xx, yy, **params):
    Z = clf.predict(np.c_[xx.ravel(), yy.ravel()])
    Z = Z.reshape(xx.shape)
    out = plt.contourf(xx, yy, Z, **params)
    
    return out
```


```python
X, y = make_blobs(n_samples = 1000)
```


```python
plt.scatter(X[:,0], X[:,1], c=y, cmap = plt.cm.coolwarm, s=20, edgecolors='k');
```


```python
model = GaussianNB()
model.fit(X,y)
```


```python
xx, yy = make_meshgrid(X[:,0], X[:,1])
plot_contours(model, xx, yy, cmap = plt.cm.coolwarm, alpha=0.8)
plt.scatter(X[:,0], X[:,1], c=y, cmap = plt.cm.coolwarm, s=20, edgecolors='k');
```

### 베르누이 나이브 베이즈
- 입력 특성이 베르누이 분포에 의해 생성된 이진 값을 갖는다고 가정

#### 학습 및 평가 (Count)


```python
model = BernoulliNB()
model.fit(X_train_count, y_train)
```


```python
predict = model.predict(X_train_count)
acc = metrics.accuracy_score(y_train, predict)
f1 = metrics.f1_score(y_train, predict, average = None)

print('Train Accuracy : {}'.format(acc))
print('Train F1 score : {}'.format(f1))
```


```python
predict = model.predict(X_test_count)
acc = metrics.accuracy_score(y_test, predict)
f1 = metrics.f1_score(y_test, predict, average = None)

print('Test Accuracy : {}'.format(acc))
print('Test F1 score : {}'.format(f1))
```

#### 학습 및 평가(Hash)


```python
model = BernoulliNB()
model.fit(X_train_hash, y_train)
```


```python
predict = model.predict(X_train_hash)
acc = metrics.accuracy_score(y_train, predict)
f1 = metrics.f1_score(y_train, predict, average = None)

print('Train Accuracy : {}'.format(acc))
print('Train F1 score : {}'.format(f1))
```


```python
predict = model.predict(X_test_hash)
acc = metrics.accuracy_score(y_test, predict)
f1 = metrics.f1_score(y_test, predict, average = None)

print('Test Accuracy : {}'.format(acc))
print('Test F1 score : {}'.format(f1))
```

#### 학습 및 평가(Tf-idf)


```python
model = BernoulliNB()
model.fit(X_train_tfidf, y_train)
```


```python
predict = model.predict(X_train_tfidf)
acc = metrics.accuracy_score(y_train, predict)
f1 = metrics.f1_score(y_train, predict, average = None)

print('Train Accuracy : {}'.format(acc))
print('Train F1 score : {}'.format(f1))
```


```python
predict = model.predict(X_test_tfidf)
acc = metrics.accuracy_score(y_test, predict)
f1 = metrics.f1_score(y_test, predict, average = None)

print('Test Accuracy : {}'.format(acc))
print('Test F1 score : {}'.format(f1))
```


```python
X, y = make_blobs(n_samples = 1000)
```


```python
plt.scatter(X[:,0], X[:,1], c=y, cmap=plt.cm.coolwarm, s=20, edgecolors='k');
```


```python
model = BernoulliNB()
model.fit(X,y)
```


```python
xx, yy = make_meshgrid(X[:,0], X[:,1])
plot_contours(model, xx, yy, cmap=plt.cm.coolwarm, alpha=0.8)
plt.scatter(X[:,0],X[:,1], c=y, cmap=plt.cm.coolwarm, s=20, edgecolors='k');
```

#### 다항 나이브 베이즈
- 입력 특성이 다항분포에 의해 생성된 빈도수 값을 갖는다고 가정

#### 학습 및 평가(Count)


```python
model = MultinomialNB()
model.fit(X_train_count, y_train)
```


```python
predict = model.predict(X_train_count)
acc = metrics.accuracy_score(y_train, predict)
f1 = metrics.f1_score(y_train, predict, average = None)

print("Train Accuracy : {}".format(acc))
print("Train F1 Score : {}".format(f1))
```


```python
predict = model.predict(X_test_count)
acc = metrics.accuracy_score(y_test, predict)
f1 = metrics.f1_score(y_test, predict, average = None)

print("test Accuracy : {}".format(acc))
print("test F1 Score : {}".format(f1))
```

#### 학습 및 평가(Tf-idf)


```python
model = MultinomialNB()
model.fit(X_train_tfidf, y_train)
```


```python
predict = model.predict(X_train_tfidf)
acc = metrics.accuracy_score(y_train, predict)
f1 = metrics.f1_score(y_train, predict, average = None)

print("Train Accuracy : {}".format(acc))
print("Train F1 Score : {}".format(f1))
```


```python
predict = model.predict(X_test_tfidf)
acc = metrics.accuracy_score(y_test, predict)
f1 = metrics.f1_score(y_test, predict, average = None)

print("test Accuracy : {}".format(acc))
print("test F1 Score : {}".format(f1))
```

#### 시각화


```python
X, y = make_blobs(n_samples=1000)
```


```python
scaler = MinMaxScaler()
X = scaler.fit_transform(X)
```


```python
plt.scatter(X[:,0],X[:,1], c=y, cmap=plt.cm.coolwarm, edgecolors='k');
```


```python
model = MultinomialNB()
model.fit(X,y)
```


```python
xx, yy = make_meshgrid(X[:,0], X[:,1])
plot_contours(model, xx, yy, cmap=plt.cm.coolwarm, alpha=0.8)
plt.scatter(X[:,0], X[:,1], c=y, cmap=plt.cm.coolwarm, s=20, edgecolor='k');
```


```python

```
