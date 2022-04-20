### 결정 트리(Decision Tree)
- 분류와 회귀에 사용되는 지도 학습 방법
- 데이터 특성으로 부터 추론된 결정 규칙을 통해 값을 예측
- if-then-else 결정 규칙을 통해 데이터 학습
- 트리의 깊이가 깊을 수록 복잡한 모델
- 결정 트리 장점
    - 이해와 해석이 쉽다
    - 시각화가 용이하다
    - 많은 데이터 전처리가 필요하지 않다
    - 수치형과 범주형 데이터 모두를 다룰 수 있다


```python
import graphviz
import multiprocessing
plt.style.use(['seaborn-whitegrid'])
```


```python
from sklearn.datasets import load_iris, load_wine, load_breast_cancer
from sklearn.datasets import load_boston, load_diabetes
from sklearn import tree
from sklearn.tree import DecisionTreeClassifier, DecisionTreeRegressor
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import cross_val_score
from sklearn.pipeline import make_pipeline
```

### 분류를 위한 데이터

#### 붓꽃 데이터


```python
iris = load_iris()
```


```python
iris_df = pd.DataFrame(data=iris.data, columns = iris.feature_names)
iris_df['target'] = iris.target
iris_df
```


```python
wine = load_wine()
```


```python
wine_df = pd.DataFrame(data=wine.data, columns = wine.feature_names)
wine_df['target'] = wine.target
wine_df
```

#### 유방암 데이터


```python
cancer = load_breast_cancer()
```


```python
cancer_df = pd.DataFrame(data = cancer.data, columns=cancer.feature_names)
cancer_df['target'] = cancer.target
cancer_df
```

### 회귀를 위한 데이터

#### 보스턴 주택 가격 데이터


```python
boston = load_boston()
```


```python
boston_df = pd.DataFrame(data=boston.data, columns=boston.feature_names)
boston_df['target'] = boston.target
boston_df
```

#### 당뇨병 데이터


```python
diabetes = load_diabetes()
```


```python
diabetes_df = pd.DataFrame(data=diabetes.data, columns=diabetes.feature_names)
diabetes_df['target'] = diabetes.target
diabetes_df
```

### 분류 - DecisionTreeClassifier()
- DecisionTreeClassifier는 분류를 위한 결정트리 모델
- 두개의 배열 X,y를 입력 받음
    - X는 [n_samples, n_features] 크기의 데이터 특성 배열
    - y는 [n_samples] 크기의 정답 배열


```python
X = [[0,0], [1,1]]
y = [0,1]

model = tree.DecisionTreeClassifier()
model = model.fit(X,y)
```


```python
model.predict([[2., 2.]])
```


```python
model.predict_proba([[2., 2.]])
```

### 붓꽃 데이터 학습

#### 교차검증

#### 전처리 없이 학습


```python
model = DecisionTreeClassifier()
```


```python
cross_val_score(
estimator = model,
X = iris.data, y = iris.target,
cv=5,
n_jobs = multiprocessing.cpu_count())
```

#### 전처리 후 학습
- 결정 트리는 규칙을 학습하기 때문에 전처리에 큰 영향을 받지 않는다


```python
model = make_pipeline(
StandardScaler(),
DecisionTreeClassifier())
```


```python
cross_val_score(
estimator = model,
X=iris.data, y=iris.target,
cv=5,
n_jobs=multiprocessing.cpu_count())
```

#### 학습된 결정 트리 시각화


```python
model = DecisionTreeClassifier()
model.fit(iris.data, iris.target)
```

#### 텍스트를 통한 시각화


```python
r = tree.export_text(decision_tree=model,
                     feature_names=iris.feature_names)
print(r)
```

#### plot_tree를 이용한 시각화


```python
tree.plot_tree(model)
```

#### graphviz를 사용한 시각화


```python
dot_data = tree.export_graphviz(decision_tree = model,
                               feature_names=iris.feature_names,
                               class_names = iris.target_names,
                               filled=True, rounded=True,
                               special_characters=True)
graph = graphviz.Source(dot_data)
graph
```

#### 시각화


```python
n_classes = 3
plot_colors = 'ryb'
plot_step = 0.02
```

#### 결정 경계 시각화


```python
plt.figure(figsize=(16,8))

for pairidx, pair in enumerate([[0,1], [0,2], [0,3],
                               [1,2], [1,3], [2,3]]):
    X = iris.data[:,pair]
    y = iris.target
    
    model = DecisionTreeClassifier()
    model = model.fit(X,y)
    
    plt.subplot(2,3, pairidx + 1)
    
    x_min, x_max = X[:,0].min()-1, X[:,0].max()+1
    y_min, y_max = X[:,1].min()-1, X[:,1].max()+1
    xx, yy = np.meshgrid(np.arange(x_min, x_max, plot_step),
                         np.arange(y_min, y_max, plot_step))
    plt.tight_layout(h_pad = -0.5, w_pad=0.5, pad=2.5)
    
    Z = model.predict(np.c_[xx.ravel(), yy.ravel()])
    Z = Z.reshape(xx.shape)
    cs = plt.contourf(xx, yy, Z, cmap = plt.cm.RdYlBu)
    
    plt.xlabel(iris.feature_names[pair[0]])
    plt.ylabel(iris.feature_names[pair[1]])
    
    for i, color in zip(range(n_classes), plot_colors):
        idx = np.where(y == i)
        plt.scatter(X[idx, 0], X[idx,1], c=color, label=iris.target_names[i],
                   cmap=plt.cm.RdYlBu, edgecolor='b', s=15)
        
plt.suptitle("Decision surface")
plt.legend(loc='lower right', borderpad=0, handletextpad=0)
plt.axis('tight')
```

#### 하이퍼파라미터를 변경해 보면서 결정 계기의 변화 확인


```python
plt.figure(figsize=(16,8))

for pairidx, pair in enumerate([[0,1], [0,2], [0,3],
                               [1,2], [1,3], [2,3]]):
    X = iris.data[:,pair]
    y = iris.target
    
    model = DecisionTreeClassifier(max_depth = 2)
    model = model.fit(X,y)
    
    plt.subplot(2,3, pairidx + 1)
    
    x_min, x_max = X[:,0].min()-1, X[:,0].max()+1
    y_min, y_max = X[:,1].min()-1, X[:,1].max()+1
    xx, yy = np.meshgrid(np.arange(x_min, x_max, plot_step),
                         np.arange(y_min, y_max, plot_step))
    plt.tight_layout(h_pad = -0.5, w_pad=0.5, pad=2.5)
    
    Z = model.predict(np.c_[xx.ravel(), yy.ravel()])
    Z = Z.reshape(xx.shape)
    cs = plt.contourf(xx, yy, Z, cmap = plt.cm.RdYlBu)
    
    plt.xlabel(iris.feature_names[pair[0]])
    plt.ylabel(iris.feature_names[pair[1]])
    
    for i, color in zip(range(n_classes), plot_colors):
        idx = np.where(y == i)
        plt.scatter(X[idx, 0], X[idx,1], c=color, label=iris.target_names[i],
                   cmap=plt.cm.RdYlBu, edgecolor='b', s=15)
        
plt.suptitle("Decision surface")
plt.legend(loc='lower right', borderpad=0, handletextpad=0)
plt.axis('tight')
```

### 와인 데이터 학습

#### 교차 검증

#### 전처리 없이 학습


```python
model = DecisionTreeClassifier()
```


```python
cross_val_score(
estimator = model,
X=wine.data, y=wine.target,
cv=5, 
n_jobs = multiprocessing.cpu_count())
```

#### 전처리 후 학습


```python
model = make_pipeline(
StandardScaler(),
DecisionTreeClassifier())
```


```python
cross_val_score(
estimator=model,
X=wine.data, y=wine.target,
cv=5,
n_jobs = multiprocessing.cpu_count())
```

#### 학습된 결정 트리 시각화


```python
model = DecisionTreeClassifier()
model.fit(wine.data, wine.target)
```

#### 텍스트를 통한 시각화


```python
r = tree.export_text(decision_tree = model,
                    feature_names = wine.feature_names)

print(r)
```

#### plot_tree를 사용한 시각화


```python
tree.plot_tree(model);
```

#### graphviz를 사용한 시각화


```python
dot_data = tree.export_graphviz(decision_tree = model,
                               feature_names = wine.feature_names,
                               class_names = wine.target_names,
                               filled=True, rounded=True,
                               special_characters = True)
graph = graphviz.Source(dot_data)
graph
```

#### 시각화


```python
n_classes = 3
plot_colors = 'ryb'
plot_step = 0.02
```

#### 결정 경계 시각화


```python
plt.figure(figsize=(16,8))

for pairidx, pair in enumerate([[0,1], [0,2], [0,3],
                               [1,2], [1,3], [2,3]]):
    X = wine.data[:,pair]
    y = wine.target
    
    model = DecisionTreeClassifier(max_depth = 2)
    model = model.fit(X,y)
    
    plt.subplot(2,3, pairidx + 1)
    
    x_min, x_max = X[:,0].min()-1, X[:,0].max()+1
    y_min, y_max = X[:,1].min()-1, X[:,1].max()+1
    xx, yy = np.meshgrid(np.arange(x_min, x_max, plot_step),
                         np.arange(y_min, y_max, plot_step))
    plt.tight_layout(h_pad = -0.5, w_pad=0.5, pad=2.5)
    
    Z = model.predict(np.c_[xx.ravel(), yy.ravel()])
    Z = Z.reshape(xx.shape)
    cs = plt.contourf(xx, yy, Z, cmap = plt.cm.RdYlBu)
    
    plt.xlabel(wine.feature_names[pair[0]])
    plt.ylabel(wine.feature_names[pair[1]])
    
    for i, color in zip(range(n_classes), plot_colors):
        idx = np.where(y == i)
        plt.scatter(X[idx, 0], X[idx,1], c=color, label=wine.target_names[i],
                   cmap=plt.cm.RdYlBu, edgecolor='b', s=15)
        
plt.suptitle("Decision surface")
plt.legend(loc='lower right', borderpad=0, handletextpad=0)
plt.axis('tight')
```

### 유방암 데이터 학습

#### 교차 검증

#### 전처리 없이 학습


```python
model = DecisionTreeClassifier()
```


```python
cross_val_score(
estimator = model,
X=cancer.data, y=cancer.target,
cv=5,
n_jobs = multiprocessing.cpu_count())
```

#### 전처리 후 학습


```python
model = make_pipeline(
StandardScaler(),
DecisionTreeClassifier())
```


```python
cross_val_score(
estimator=model,
X=cancer.data, y=cancer.target,
cv=5,
n_jobs = multiprocessing.cpu_count())
```

#### 학습된 결정 트리 시각화


```python
model = DecisionTreeClassifier()
model.fit(cancer.data, cancer.target)
```

#### 텍스트를 통한 시각화


```python
r = tree.export_text(decision_tree=model)
print(r)
```

#### plot_tree를 사용한 시각화


```python
tree.plot_tree(model)
```

#### graphviz를 사용한 시각화


```python
dot_data = tree.export_graphviz(decision_tree = model,
                               feature_names = cancer.feature_names,
                               class_names = cancer.target_names,
                               filled=True, rounded=True,
                               special_characters = True)

graph = graphviz.Source(dot_data)
graph
```

### 시각화


```python
n_classes = 2
plot_colors = 'ryb'
plot_step = 0.02
```

#### 결정경계 시각화


```python
plt.figure(figsize=(16,8))

for pairidx, pair in enumerate([[0,1], [0,2], [0,3]]):
                              
    X = cancer.data[:,pair]
    y = cancer.target
    
    model = DecisionTreeClassifier()
    model = model.fit(X,y)
    
    plt.subplot(2,3, pairidx + 1)
    
    x_min, x_max = X[:,0].min()-1, X[:,0].max()+1
    y_min, y_max = X[:,1].min()-1, X[:,1].max()+1
    xx, yy = np.meshgrid(np.arange(x_min, x_max, plot_step),
                         np.arange(y_min, y_max, plot_step))
    plt.tight_layout(h_pad = -0.5, w_pad=0.5, pad=2.5)
    
    Z = model.predict(np.c_[xx.ravel(), yy.ravel()])
    Z = Z.reshape(xx.shape)
    cs = plt.contourf(xx, yy, Z, cmap = plt.cm.RdYlBu)
    
    plt.xlabel(cancer.feature_names[pair[0]])
    plt.ylabel(cancer.feature_names[pair[1]])
    
    for i, color in zip(range(n_classes), plot_colors):
        idx = np.where(y == i)
        plt.scatter(X[idx, 0], X[idx,1], c=color, label=cancer.target_names[i],
                   cmap=plt.cm.RdYlBu, edgecolor='b', s=15)
        
plt.suptitle("Decision surface")
plt.legend(loc='lower right', borderpad=0, handletextpad=0)
plt.axis('tight')
```

#### 하이퍼파라미터를 변경해 보면서 결정 경계의 변화 확인


```python
plt.figure(figsize=(16,8))

for pairidx, pair in enumerate([[0,1], [0,2], [0,3]]):
                             
    X = cancer.data[:,pair]
    y = cancer.target
    
    model = DecisionTreeClassifier(max_depth = 2)
    model = model.fit(X,y)
    
    plt.subplot(2,3, pairidx + 1)
    
    x_min, x_max = X[:,0].min()-1, X[:,0].max()+1
    y_min, y_max = X[:,1].min()-1, X[:,1].max()+1
    xx, yy = np.meshgrid(np.arange(x_min, x_max, plot_step),
                         np.arange(y_min, y_max, plot_step))
    plt.tight_layout(h_pad = -0.5, w_pad=0.5, pad=2.5)
    
    Z = model.predict(np.c_[xx.ravel(), yy.ravel()])
    Z = Z.reshape(xx.shape)
    cs = plt.contourf(xx, yy, Z, cmap = plt.cm.RdYlBu)
    
    plt.xlabel(cancer.feature_names[pair[0]])
    plt.ylabel(cancer.feature_names[pair[1]])
    
    for i, color in zip(range(n_classes), plot_colors):
        idx = np.where(y == i)
        plt.scatter(X[idx, 0], X[idx,1], c=color, label=cancer.target_names[i],
                   cmap=plt.cm.RdYlBu, edgecolor='b', s=15)
        
plt.suptitle("Decision surface")
plt.legend(loc='lower right', borderpad=0, handletextpad=0)
plt.axis('tight')
```

### 회귀 - DecisionTreeRegressor()

### 보스턴 주택 가격 데이터 학습

#### 교차 검증

#### 전처리 없이 학습


```python
model = DecisionTreeRegressor()
```


```python
cross_val_score(
estimator=model,
X=boston.data, y=boston.target,
cv=5,
n_jobs = multiprocessing.cpu_count())
```

#### 전처리 후 학습


```python
model = make_pipeline(
StandardScaler(),
DecisionTreeRegressor())
```


```python
cross_val_score(
estimator=model,
X=boston.data, y = boston.target,
cv=5,
n_jobs = multiprocessing.cpu_count())
```

#### 학습된 결정 트리 시각화


```python
model = DecisionTreeRegressor()
```


```python
model.fit(boston.data, boston.target)
```

#### 텍스트를 통한 시각화


```python
print(tree.export_text(model))
```

#### plot_tree를 사용한 시각화


```python
tree.plot_tree(model);
```

#### graphviz를 사용한 시각화


```python
dot_data = tree.export_graphviz(decision_tree=model,
                               feature_names = boston.feature_names,
                               filled=True, rounded=True,
                               special_characters= True)
graph = graphviz.Source(dot_data)
graph
```

### 시각화

#### 회귀식 시각화


```python
plt.figure(figsize=(116,8))

for pairidx, pair in enumerate([0, 1, 2]):
    X = boston.data[:, pair].reshape(-1,1)
    y = boston.target
    
    model = DecisionTreeRegressor()
    model.fit(X,y)
    
    X_test = np.arange(min(X), max(X), 0.1)[:, np.newaxis]
    predict = model.predict(X_test)
    
    plt.subplot(1,3, pairidx + 1)
    plt.scatter(X, y, s=20, edgecolors='k',
               c= 'darkorange', label='data')
    plt.plot(X_test, predict, color='royalblue', linewidth=2)
    plt.xlabel(boston.feature_names[pair])
    plt.ylabel('Target')
```

#### 하이퍼파라미터를 변경해 보면서 결정경계의 변화 확인


```python
plt.figure(figsize=(16,8))

for pairidx, pair in enumerate([0, 1, 2]):
    X = boston.data[:, pair].reshape(-1,1)
    y = boston.target
    
    model = DecisionTreeRegressor(max_depth = 2)
    model.fit(X,y)
    
    X_test = np.arange(min(X), max(X), 0.1)[:, np.newaxis]
    predict = model.predict(X_test)
    
    plt.subplot(1,3, pairidx + 1)
    plt.scatter(X, y, s=20, edgecolors='k',
               c= 'darkorange', label='data')
    plt.plot(X_test, predict, color='royalblue', linewidth=2)
    plt.xlabel(boston.feature_names[pair])
    plt.ylabel('Target')
```

### 당뇨병 데이터 학습

#### 교차 검증

#### 전처리 없이 학습


```python
cross_val_score(
estimator=model,
X=diabetes.data, y=diabetes.target,
cv=5,
n_jobs = multiprocessing.cpu_count())
```

#### 전처리 후 학습


```python
model = make_pipeline(
StandardScaler(),
DecisionTreeRegressor())
```


```python
cross_val_score(
estimator=model,
X=diabetes.data, y=diabetes.target,
cv=5,
n_jobs = multiprocessing.cpu_count())
```

#### 학습된 결정 트리 시각화


```python
model = DecisionTreeRegressor()
model.fit(diabetes.data, diabetes.target)
```

#### 텍스트를 통한 시각화


```python
print(tree.export_text(model, feature_names = diabetes.feature_names))
```

#### plot_tree를 사용한 시각화


```python
tree.plot_tree(model)
```

#### graphviz를 사용한 시각화


```python
dot_data = tree.export_graphviz(decision_tree = model,
                                feature_names = diabetes.feature_names,
                                filled=True, rounded=True,
                                special_characters =  True)
graph = graphviz.Source(dot_data)
graph
```

### 시각화

#### 회귀식 시각화


```python
plt.figure(figsize=(116,8))

for pairidx, pair in enumerate([0, 2, 3]):
    X = diabetes.data[:, pair].reshape(-1,1)
    y = diabetes.target
    
    model = DecisionTreeRegressor()
    model.fit(X,y)
    
    X_test = np.arange(min(X), max(X), 0.1)[:, np.newaixs]
    predict = model.predict(X_test)
    
    plt.subplot(1,3, pairidx + 1)
    plt.scatter(X, y, s=20, edgecolors='k',
               c= 'darkorange', label='data')
    plt.plot(X_test, predict, color='royalblue', linewidth=2)
    plt.xlabel(diabetes.feature_names[pair])
    plt.ylabel('Target')
```

#### 하이퍼파라미터를 변경해 보면서 회귀식 시각화


```python
plt.figure(figsize=(116,8))

for pairidx, pair in enumerate([0, 2, 3]):
    X = diabetes.data[:, pair].reshape(-1,1)
    y = diabetes.target
    
    model = DecisionTreeRegressor(max_depth = 3)
    model.fit(X,y)
    
    X_test = np.arange(min(X), max(X), 0.1)[:, np.newaixs]
    predict = model.predict(X_test)
    
    plt.subplot(1,3, pairidx + 1)
    plt.scatter(X, y, s=20, edgecolors='k',
               c= 'darkorange', label='data')
    plt.plot(X_test, predict, color='royalblue', linewidth=2)
    plt.xlabel(diabetes.feature_names[pair])
    plt.ylabel('Target')
```
