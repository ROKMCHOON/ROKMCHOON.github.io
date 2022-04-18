### 최근접 이웃(KNN)
- 특별한 예측 모델 없이 가장 가까운 데이터 포인트를 기반으로 예측을 수행하는 방법
- 분류와 회귀 모두 지원


```python
import multiprocessing
plt.style.use(['seaborn-whitegrid'])
```


```python
from sklearn.neighbors import KNeighborsClassifier, KNeighborsRegressor
from sklearn.manifold import TSNE
from sklearn.datasets import load_iris, load_breast_cancer, load_wine
from sklearn.datasets import load_boston, fetch_california_housing
from sklearn.model_selection import train_test_split, cross_validate, GridSearchCV
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import make_pipeline, Pipeline
```

### K 최근접 이웃 분류
- 입력 데이터 포인트와 가장 가까운 k개의 훈련 데이터 포인트가 출력
- k개의 데이터 포인트 중 가장 많은 클래스가 예측 결과

#### 붓꽃 데이터


```python
iris = load_iris()
```


```python
iris_df = pd.DataFrame(data=iris.data, columns = iris.feature_names)
iris_df['Target'] = iris.target
iris_df
```


```python
X, y = load_iris(return_X_y = True)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2)
```


```python
scaler = StandardScaler()
X_train_scale = scaler.fit_transform(X_train)
X_test_scale = scaler.transform(X_test) # train 데이터에 대해서 fit 한 다음에 test는 fit 된 데이터 결과에서 transform만 해준걸로 사용
```


```python
model = KNeighborsClassifier()
model.fit(X_train, y_train)
```


```python
print("학습 데이터 점수 : {}".format(model.score(X_train, y_train)))
print("평가 데이터 점수 : {}".format(model.score(X_test, y_test)))
```


```python
model = KNeighborsClassifier()
model.fit(X_train_scale, y_train)
```


```python
print("학습 데이터 점수 : {}".format(model.score(X_train, y_train)))
print("평가 데이터 점수 : {}".format(model.score(X_test, y_test)))
```


```python
cross_validate(
estimator=KNeighborsClassifier(),
X=X, y=y,
cv = 5,
n_jobs = multiprocessing.cpu_count(),
verbose=True)
```


```python
param_grid = [{'n_neighbors':[3,5,7],
              'weights':['uniform', 'distance'],
              'algorithm' : ['ball_tree', 'kd_tree', 'brute']}]
```


```python
gs = GridSearchCV(
estimator = KNeighborsClassifier(),
param_grid = param_grid,
n_jobs = multiprocessing.cpu_count(),
verbose=True)

gs.fit(X,y)
```


```python
gs.best_estimator_
```


```python
print("GridSearchCV best score : {}".format(gs.best_score_))
```


```python
def make_meshgrid(x, y, h=.02):
    x_min, x_max = x.min()-1, x.max()+1
    y_min, y_max = y.min()-1, y.max()+1
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
tsne = TSNE(n_components=2)
X_comp = tsne.fit_transform(X)
```


```python
iris_comp_df = pd.DataFrame(data=X_comp)
iris_comp_df['Target'] = y
iris_comp_df
```


```python
plt.scatter(X_comp[:,0], X_comp[:,1],
            c=y, cmap=plt.cm.coolwarm, s=20, edgecolors='k');
```


```python
model = KNeighborsClassifier()
model.fit(X_comp, y)
predict = model.predict(X_comp)
```


```python
xx, yy = make_meshgrid(X_comp[:,0], X_comp[:,1])
plot_contours(model, xx, yy, cmap=plt.cm.coolwarm, alpha=0.8)
plt.scatter(X_comp[:,0], X_comp[:,1], c=y, cmap=plt.cm.coolwarm, s=20, edgecolors='k');
```

#### 유방암 데이터


```python
cancer = load_breast_cancer()
```


```python
cancer_df = pd.DataFrame(data=cancer.data, columns = cancer.feature_names)
cancer_df['target'] = cancer.target
cancer_df
```


```python
X, y = cancer.data, cancer.target
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2)
```


```python
cancer_train_df = pd.DataFrame(data=X_train, columns = cancer.feature_names)
cancer_train_df['target'] = y_train
cancer_train_df
```


```python
cancer_test_df = pd.DataFrame(data=X_test, columns = cancer.feature_names)
cancer_test_df['target'] = y_test
cancer_test_df
```


```python
scaler = StandardScaler()
X_train_scale = scaler.fit_transform(X_train)
X_test_scale = scaler.transform(X_test)
```


```python
model = KNeighborsClassifier()
model.fit(X_train, y_train)
```


```python
print("학습 데이터 점수 : {}".format(model.score(X_train, y_train)))
print("평가 데이터 점수 : {}".format(model.score(X_test, y_test)))
```


```python
model = KNeighborsClassifier()
model.fit(X_train_scale, y_train)
```


```python
print("학습 데이터 점수 : {}".format(model.score(X_train_scale, y_train)))
print("평가 데이터 점수 : {}".format(model.score(X_test_scale, y_test)))
```


```python
estimator = make_pipeline(
StandardScaler(),
KNeighborsClassifier())
```


```python
cross_validate(
estimator = estimator,
X=X, y=y,
cv=5,
n_jobs = multiprocessing.cpu_count(),
verbose=True)
```


```python
pipe = Pipeline(
[('scaler', StandardScaler()),
('model', KNeighborsClassifier())])
```


```python
param_grid = [{'model__n_neighbors':[3,5,7],
              'model__weights' : ['uniform' ,'distance'],
              'model__algorithm' : ['ball_tree', 'kd_tree', 'brute']}]
```


```python
gs = GridSearchCV(
estimator = pipe,
param_grid = param_grid,
n_jobs = multiprocessing.cpu_count(),
verbose = True)

gs.fit(X,y)
```


```python
gs.best_estimator_
```


```python
print("GridSearchCV best score : {}".format(gs.best_score_))
```


```python
tsne = TSNE(n_components=2)
X_comp = tsne.fit_transform(X)
```


```python
cancer_comp_df = pd.DataFrame(data=X_comp)
cancer_comp_df['target'] = y
cancer_comp_df
```


```python
plt.scatter(X_comp[:,0], X_comp[:,1], c=y, cmap= plt.cm.coolwarm, s=20, edgecolors='k');
```


```python
model = KNeighborsClassifier()
model.fit(X_comp, y)
predict = model.predict(X_comp)
```


```python
xx, yy = make_meshgrid(X_comp[:,0], X_comp[:,1])
plot_contours(model, xx, yy, cmap=plt.cm.coolwarm, alpha=0.8)
plt.scatter(X_comp[:,0], X_comp[:,1], c=y, cmap=plt.cm.coolwarm, s=20, edgecolor='k');
```

### 와인 데이터


```python
wine = load_wine()

wine_df = pd.DataFrame(data=wine.data, columns = wine.feature_names)
wine_df['target'] = wine.target
wine_df

X, y = wine.data, wine.target
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2)

wine_train_df = pd.DataFrame(data=X_train, columns = wine.feature_names)
wine_train_df['target'] = y_train
wine_train_df

wine_test_df = pd.DataFrame(data=X_test, columns = wine.feature_names)
wine_test_df['target'] = y_test
wine_test_df

scaler = StandardScaler()
X_train_scale = scaler.fit_transform(X_train)
X_test_scale = scaler.transform(X_test)

model = KNeighborsClassifier()
model.fit(X_train, y_train)

print("학습 데이터 점수 : {}".format(model.score(X_train, y_train)))
print("평가 데이터 점수 : {}".format(model.score(X_test, y_test)))

model = KNeighborsClassifier()
model.fit(X_train_scale, y_train)

print("학습 데이터 점수 : {}".format(model.score(X_train_scale, y_train)))
print("평가 데이터 점수 : {}".format(model.score(X_test_scale, y_test)))

estimator = make_pipeline(
StandardScaler(),
KNeighborsClassifier())

cross_validate(
estimator = estimator,
X=X, y=y,
cv=5,
n_jobs = multiprocessing.cpu_count(),
verbose=True)

pipe = Pipeline(
[('scaler', StandardScaler()),
('model', KNeighborsClassifier())])

param_grid = [{'model__n_neighbors':[3,5,7],
              'model__weights' : ['uniform' ,'distance'],
              'model__algorithm' : ['ball_tree', 'kd_tree', 'brute']}]

gs = GridSearchCV(
estimator = pipe,
param_grid = param_grid,
n_jobs = multiprocessing.cpu_count(),
verbose = True)

gs.fit(X,y)

gs.best_estimator_

print("GridSearchCV best score : {}".format(gs.best_score_))

tsne = TSNE(n_components=2)
X_comp = tsne.fit_transform(X)

wine_comp_df = pd.DataFrame(data=X_comp)
wine_comp_df['target'] = y
wine_comp_df

plt.scatter(X_comp[:,0], X_comp[:,1], c=y, cmap= plt.cm.coolwarm, s=20, edgecolors='k');

model = KNeighborsClassifier()
model.fit(X_comp, y)
predict = model.predict(X_comp)

xx, yy = make_meshgrid(X_comp[:,0], X_comp[:,1])
plot_contours(model, xx, yy, cmap=plt.cm.coolwarm, alpha=0.8)
plt.scatter(X_comp[:,0], X_comp[:,1], c=y, cmap=plt.cm.coolwarm, s=20, edgecolor='k');
```

### k 최근접 이웃 회귀
- k 최근접 이웃 분류와 마찬가지로 예측에 이웃 데이터 포인트 사용
- 이웃 데이터 포인터의 평균이 예측 결과

#### 보스턴 주택 가격 데이터


```python
boston = load_boston()
```


```python
boston_df = pd.DataFrame(data=boston.data, columns = boston.feature_names)
boston_df['target'] = boston.target
boston_df
```


```python
X, y = boston.data, boston.target
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2)
```


```python
boston_train_df = pd.DataFrame(data=X_train, columns = boston.feature_names)
boston_train_df['target'] = y_train
boston_train_df
```


```python
boston_test_df = pd.DataFrame(data=X_test, columns = boston.feature_names)
boston_test_df['target'] = y_test
boston_test_df
```


```python
scaler = StandardScaler()
X_train_scale = scaler.fit_transform(X_train)
X_test_scale = scaler.transform(X_test)
```


```python
model = KNeighborsRegressor()
model.fit(X_train, y_train)
```


```python
print("학습 데이터 점수 : {}".format(model.score(X_train, y_train)))
print("평가 데이터 점수 : {}".format(model.score(X_test, y_test)))
```


```python
model = KNeighborsRegressor()
model.fit(X_train_scale, y_train)
```


```python
print("학습 데이터 점수 : {}".format(model.score(X_train_scale, y_train)))
print("평가 데이터 점수 : {}".format(model.score(X_test_scale, y_test)))
```


```python
estimator = make_pipeline(
StandardScaler(),
KNeighborsRegressor())
```


```python
cross_validate(
estimator = estimator,
X=X, y=y,
cv=5,
n_jobs=multiprocessing.cpu_count(),
verbose=True)
```


```python
pipe = Pipeline(
[('scaler', StandardScaler()),
('model', KNeighborsRegressor())])
```


```python
param_grid = [{'model__n_neighbors' : [3,5,7],
              'model__weights' : ['uniform' ,'distance'],
              'model__algorithm' : ['ball_tree' ,'kd_tree', 'brute']}]
```


```python
gs = GridSearchCV(
estimator = pipe,
param_grid = param_grid,
n_jobs = multiprocessing.cpu_count(),
verbose = True)
```


```python
gs.fit(X, y)
```


```python
gs.best_estimator_
```


```python
print('GridSearchCV best score : {}'.format(gs.best_score_))
```


```python
tsne = TSNE(n_components=1)
X_comp = tsne.fit_transform(X)
```


```python
boston_comp_df = pd.DataFrame(data=X_comp)
boston_comp_df['target'] = y
boston_comp_df
```


```python
plt.scatter(X_comp, y, c='b', cmap=plt.cm.coolwarm, s=20, edgecolors='k');
```


```python
model = KNeighborsRegressor()
model.fit(X_comp, y)
predict = model.predict(X_comp)
```


```python
plt.scatter(X_comp, y, c='b', cmap=plt.cm.coolwarm, s=20, edgecolors='k');
plt.scatter(X_comp, predict, c='r', cmap=plt.cm.coolwarm, s=20, edgecolors='k');
```

#### 캘리포니아 주택 가격


```python
california = fetch_california_housing()

california_df = pd.DataFrame(data=california.data, columns = california.feature_names)
california_df['target'] = california.target
california_df

X, y = california.data, california.target
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2)

california_train_df = pd.DataFrame(data=X_train, columns = california.feature_names)
california_train_df['target'] = y_train
california_train_df

california_test_df = pd.DataFrame(data=X_test, columns = california.feature_names)
california_test_df['target'] = y_test
california_test_df

scaler = StandardScaler()
X_train_scale = scaler.fit_transform(X_train)
X_test_scale = scaler.transform(X_test)

model = KNeighborsRegressor()
model.fit(X_train, y_train)

print("학습 데이터 점수 : {}".format(model.score(X_train, y_train)))
print("평가 데이터 점수 : {}".format(model.score(X_test, y_test)))

model = KNeighborsRegressor()
model.fit(X_train_scale, y_train)

print("학습 데이터 점수 : {}".format(model.score(X_train_scale, y_train)))
print("평가 데이터 점수 : {}".format(model.score(X_test_scale, y_test)))

estimator = make_pipeline(
StandardScaler(),
KNeighborsRegressor())

cross_validate(
estimator = estimator,
X=X, y=y,
cv=5,
n_jobs=multiprocessing.cpu_count(),
verbose=True)

pipe = Pipeline(
[('scaler', StandardScaler()),
('model', KNeighborsRegressor())])

param_grid = [{'model__n_neighbors' : [3,5,7],
              'model__weights' : ['uniform' ,'distance'],
              'model__algorithm' : ['ball_tree' ,'kd_tree', 'brute']}]

gs = GridSearchCV(
estimator = pipe,
param_grid = param_grid,
n_jobs = multiprocessing.cpu_count(),
verbose = True)

gs.fit(X, y)

gs.best_estimator_

print('GridSearchCV best score : {}'.format(gs.best_score_))

tsne = TSNE(n_components=1)
X_comp = tsne.fit_transform(X)

california_comp_df = pd.DataFrame(data=X_comp)
california_comp_df['target'] = y
california_comp_df

plt.scatter(X_comp, y, c='b', cmap=plt.cm.coolwarm, s=20, edgecolors='k');

model = KNeighborsRegressor()
model.fit(X_comp, y)
predict = model.predict(X_comp)

plt.scatter(X_comp, y, c='b', cmap=plt.cm.coolwarm, s=20, edgecolors='k');
plt.scatter(X_comp, predict, c='r', cmap=plt.cm.coolwarm, s=20, edgecolors='k');
```


```python

```
