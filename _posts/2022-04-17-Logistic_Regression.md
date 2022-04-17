### 로지스틱 회귀 예제


```python
plt.style.use(['seaborn-whitegrid'])
```


```python
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.linear_model import LogisticRegression
```


```python
samples = 1000
X, y = make_classification(n_samples=samples, n_features=2, 
                           n_informative=2, n_redundant=0,
                          n_clusters_per_class=1)
```


```python
fig, ax = plt.subplots(1, 1, figsize=(10,6))

ax.grid()
ax.set_xlabel('X')
ax.set_ylabel('Y')

for i in range(samples):
    if y[i] == 0:
        ax.scatter(X[i,0], X[i,1], edgecolors='k', alpha=0.5, marker='^', color='r')
    else:
        ax.scatter(X[i,0], X[i,1], edgecolors='k', alpha=0.5, marker='v', color='b')
        
plt.show()
```


```python
X_train, X_test, y_train, y_test = train_test_split(X, y , test_size = 0.2)
```


```python
model = LogisticRegression()
model.fit(X_train, y_train)
```


```python
print("학습 데이터 점수 : {}".format(model.score(X_train, y_train)))
print("평가 데이터 점수 : {}".format(model.score(X_test, y_test)))
```


```python
scores = cross_val_score(model, X, y, scoring='accuracy', cv=10)
print("CV 평균 점수 : {}".format(scores.mean()))
```


```python
model.intercept_, model.coef_
```


```python
x_min, x_max = X[:,0].min() - 0.5, X[:,0].max() + 0.5
y_min, y_max = X[:,1].min() - 0.5, X[:,1].max() + 0.5
xx, yy = np.meshgrid(np.arange(x_min, x_max, 0.02), np.arange(y_min, y_max, 0.02))
Z = model.predict(np.c_[xx.ravel(), yy.ravel()])

Z = Z.reshape(xx.shape)
plt.figure(1, figsize=(10,6))
plt.pcolormesh(xx, yy, Z, cmap = plt.cm.Pastel1)

plt.scatter(X[:,0], X[:,1], c=np.abs(y - 1), edgecolors='k', alpha=0.5, cmap=plt.cm.coolwarm)
plt.xlabel('X')
plt.ylabel('Y')

plt.xlim(xx.min(), xx.max())
plt.ylim(yy.min(), yy.max())
plt.xticks()
plt.yticks()

plt.show()
```

### 붓꽃 데이터


```python
from sklearn.datasets import load_iris

iris = load_iris()
print(iris.keys())
print(iris.DESCR)
```


```python
iris_df = pd.DataFrame(iris.data, columns=iris.feature_names)
species = pd.Series(iris.target, dtype='category')
species = species.cat.rename_categories(iris.target_names)
iris_df['species'] = species
```


```python
iris_df.describe()
```


```python
iris_df.boxplot();
```


```python
iris_df.plot();
```


```python
import seaborn as sns

sns.pairplot(iris_df, hue='species');
```

### 붓꽃 데이터에 대한 로지스틱 회귀


```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(iris.data[:, [2,3]], iris.target, 
                                                    test_size = 0.2, random_state=1, stratify=iris.target )
```


```python
from sklearn.linear_model import LinearRegression

model = LogisticRegression(solver='lbfgs', multi_class='auto', C=100.0, random_state=1)
model.fit(X_train, y_train)
```


```python
print("학습 데이터 점수 : {}".format(model.score(X_train, y_train)))
print("평가 데이터 점수 : {}".format(model.score(X_test, y_test)))
```


```python
X = np.vstack((X_train, X_test))
y = np.hstack((y_train, y_test))
```


```python
from matplotlib.colors import ListedColormap

x1_min, x1_max = X[:,0].min()-1, X[:,0].max()+1
x2_min, x2_max = X[:,1].min()-1, X[:,1].max()+1
xx1, xx2 = np.meshgrid(np.arange(x1_min, x1_max, 0.02),
                       np.arange(x2_min, x2_max, 0.02))
Z = model.predict(np.array([xx1.ravel(), xx2.ravel()]).T)
Z = Z.reshape(xx1.shape)

species = ('Setosa', 'Versicolour', 'Virginica')
markers = ('^', 'v', 's')
colors = ('blue', 'purple', 'red')
cmap = ListedColormap(colors[:len(np.unique(y))])
plt.contourf(xx1, xx2, Z, alpha=0.3, cmap = cmap)
plt.xlim(xx1.min(), xx1.max())
plt.ylim(xx2.min(), xx2.max())

for idx, cl in enumerate(np.unique(y)):
    plt.scatter(x=X[y == cl, 0], y=X[y == cl, 1],
               alpha=0.8, c=colors[idx],
               marker=markers[idx], label=species[cl],
               edgecolor='b')
    
X_comb_test, y_comb_test = X[range(105, 150),:], y[range(105, 150)]
plt.scatter(X_comb_test[:,0], X_comb_test[:,1],
            c='yellow', edgecolor='k', alpha=0.2,
            linewidth=1, marker='o', 
            s = 100, label='Test')

plt.xlabel('Petal Length (cm)')
plt.ylabel('Petal Width (cm)')
plt.legend(loc='upper left')
plt.tight_layout();
```


```python
import multiprocessing
from sklearn.model_selection import GridSearchCV

param_grid = [{'penalty': ['l1', 'l2'],
              'C': [2.0, 2.2, 2.4, 2.6, 2.8]}]

gs = GridSearchCV(estimator = LogisticRegression(), param_grid = param_grid,
                 scoring='accuracy', cv=10, n_jobs=multiprocessing.cpu_count())
gs
```


```python
result = gs.fit(iris.data, iris.target)
```


```python
print(gs.best_estimator_)
print("최적 점수 : {}".format(gs.best_score_))
print("최적 파라미터 : {}".format(gs.best_params_))
pd.DataFrame(result.cv_results_)
```

### 유방암 데이터


```python
from sklearn.datasets import load_breast_cancer
```


```python
cancer = load_breast_cancer()
print(cancer.keys())
print(cancer.DESCR)
```


```python
cancer_df = pd.DataFrame(cancer.data, columns=cancer.feature_names)
cancer_df['Target'] = cancer.target
cancer_df.head()
```


```python
cancer_df.describe()
```


```python
fig = plt.figure(figsize=[10,6])
plt.title('Breast Cancer', fontsize=15)
plt.boxplot(cancer.data)
plt.xticks(np.arange(30)+1, cancer.feature_names, rotation=90)
plt.xlabel('Features')
plt.ylabel('Scale')
plt.tight_layout()
```

### 유방암 데이터에 대한 로지스틱 회귀


```python
from sklearn.linear_model import LogisticRegression
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
```


```python
X, y = load_breast_cancer(return_X_y = True)
X_train, X_test, y_train, y_test = train_test_split(X, y)
```


```python
model = LogisticRegression(max_iter = 3000)
model.fit(X_train, y_train)
```


```python
print("학습 데이터 점수 : {}".format(model.score(X_train, y_train)))
print("평가 데이터 점수 : {}".format(model.score(X_test, y_test)))
```

### SGD를 사용한 선형회귀 분석


```python
from sklearn.linear_model import SGDRegressor
from sklearn.datasets import load_boston
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import make_pipeline
```


```python
X, y = load_boston(return_X_y = True)
X_train, X_test, y_train, y_test = train_test_split(X,y)
```


```python
model = make_pipeline(StandardScaler(), SGDRegressor(loss='squared_loss'))
model.fit(X_train, y_train)
```


```python
print("학습 데이터 점수 : {}".format(model.score(X_train, y_train)))
print("평가 데이터 점수 : {}".format(model.score(X_test, y_test)))
```

### 붓꽃 데이터에 대한 SGD 분류


```python
from sklearn.linear_model import SGDClassifier
from sklearn.datasets import load_iris, load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import make_pipeline
```


```python
X, y = load_iris(return_X_y = True)
X_train, X_test, y_train, y_test = train_test_split(X,y)
```


```python
model = make_pipeline(StandardScaler(), SGDClassifier(loss='log'))
model.fit(X_train, y_train)
```


```python
print("학습 데이터 점수 : {}".format(model.score(X_train, y_train)))
print("평가 데이터 점수 : {}".format(model.score(X_test, y_test)))
```

### 유방암 데이터에 대한 SGD 분류


```python
X,y = load_breast_cancer(return_X_y = True)
X_train, X_test, y_train, y_test = train_test_split(X,y)
```


```python
model = make_pipeline(StandardScaler(), SGDClassifier(loss = 'log'))
model.fit(X_train, y_train)
```


```python
print("학습 데이터 점수 : {}".format(model.score(X_train, y_train)))
print("평가 데이터 점수 : {}".format(model.score(X_test, y_test)))
```
