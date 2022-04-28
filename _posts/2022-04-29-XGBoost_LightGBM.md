### XGBoost
- 트리 기반의 앙상블 기법
- 분류에 있어서 다른 알고리즘보다 좋은 예측 성능을 보여줌
- XGBoost는 GBM 기반이지만, GBM의 단점인 느린 수행 시간과 과적합 규제 부재 등의 문재를 해결
- 병렬 CPU 환경에서 빠르게 학습 가능


```python
from sklearn.datasets import load_iris, load_wine, load_breast_cancer
from sklearn.datasets import load_boston, load_diabetes
from sklearn.model_selection import train_test_split, cross_validate
from sklearn.metrics import accuracy_score, precision_score, recall_score

import xgboost as xgb
from xgboost import XGBClassifier, XGBRFRegressor
from xgboost import plot_importance, plot_tree

import graphviz 
plt.style.use(['seaborn-whitegrid'])

import warnings
warnings.filterwarnings('ignore')
```

### 파이썬 기반 XGBoost


```python
cancer = load_breast_cancer()
X_train, X_test, y_train, y_test = train_test_split(cancer.data, cancer.target, test_size=0.2, random_state=123)
dtrain = xgb.DMatrix(data=X_train, label=y_train)
dtest = xgb.DMatrix(data=X_test, label = y_test)
```


```python
params = {
    'max_depth':3,
    'eta':0.1,
    'objective':'binary:logistic',
    'eval_metric':'logloss',
    'early_stopping':100
}
num_rounds=400
```


```python
evals = [(dtrain, 'train'), (dtest, 'eval')]
xgb_model = xgb.train(params=params, dtrain=dtrain, num_boost_round=num_rounds, early_stopping_rounds=100, evals=evals)
```


```python
predicts = xgb_model.predict(dtest)
print(np.round(predicts[:10],3))
```


```python
preds = [1 if x > 0.5 else 0 for x in predicts]
print(preds[:10])
```


```python
print("정확도: {}".format(accuracy_score(y_test, preds)))
print("정밀도: {}".format(precision_score(y_test, preds)))
print("재현율: {}".format(recall_score(y_test, preds)))
```


```python
fig, ax = plt.subplots(figsize=(10,12))
plot_importance(xgb_model, ax=ax);
```


```python
dot_data = xgb.to_graphviz(xgb_model)
graph = graphviz.Source(dot_data)
graph
```

#### XGBClassifier

#### 붓꽃 데이터


```python
iris = load_iris()
X_train, X_test, y_train, y_test = train_test_split(iris.data, iris.target, test_size=0.2, random_state = 123)
```


```python
xgbc = XGBClassifier(n_estimators=400, learning_rate = 0.1, max_depth=3)
xgbc.fit(X_train, y_train)
preds = xgbc.predict(X_test)
preds_proba = xgbc.predict_proba(X_test)[:,1]
```


```python
cross_val = cross_validate(
estimator=xgbc,
X=iris.data, y=iris.target,
cv=5)
print('avg fit time: {} (+/- {})'.format(cross_val['fit_time'].mean(), cross_val['fit_time'].std()))
print('avg score time: {} (+/- {})'.format(cross_val['score_time'].mean(), cross_val['score_time'].std()))
print('avg test score: {} (+/- {})'.format(cross_val['test_score'].mean(), cross_val['test_score'].std()))
```


```python
fig,ax = plt.subplots(figsize=(10, 12))
plot_importance(xgbc, ax=ax);
```


```python
dot_data = xgb.to_graphviz(xgbc)
graph = graphviz.Source(dot_data)
graph
```

#### 와인 데이터


```python
wine = load_wine()
X_train, X_test, y_train, y_test = train_test_split(wine.data, wine.target, test_size=0.2, random_state = 123)
```


```python
xgbc = XGBClassifier(n_estimators=400, learning_rate = 0.1, max_depth=3)
xgbc.fit(X_train, y_train)
preds = xgbc.predict(X_test)
preds_proba = xgbc.predict_proba(X_test)[:,1]
```


```python
cross_val = cross_validate(
estimator=xgbc,
X=wine.data, y=wine.target,
cv=5)
print('avg fit time: {} (+/- {})'.format(cross_val['fit_time'].mean(), cross_val['fit_time'].std()))
print('avg score time: {} (+/- {})'.format(cross_val['score_time'].mean(), cross_val['score_time'].std()))
print('avg test score: {} (+/- {})'.format(cross_val['test_score'].mean(), cross_val['test_score'].std()))
```


```python
fig,ax = plt.subplots(figsize=(10, 12))
plot_importance(xgbc, ax=ax);
```


```python
dot_data = xgb.to_graphviz(xgbc)
graph = graphviz.Source(dot_data)
graph
```

#### 유방암 데이터


```python
cancer = load_breast_cancer()
X_train, X_test, y_train, y_test = train_test_split(cancer.data, cancer.target, test_size=0.2, random_state = 123)
```


```python
xgbc = XGBClassifier(n_estimators=400, learning_rate = 0.1, max_depth=3)
xgbc.fit(X_train, y_train)
preds = xgbc.predict(X_test)
preds_proba = xgbc.predict_proba(X_test)[:,1]
```


```python
cross_val = cross_validate(
estimator=xgbc,
X=cancer.data, y=cancer.target,
cv=5)
print('avg fit time: {} (+/- {})'.format(cross_val['fit_time'].mean(), cross_val['fit_time'].std()))
print('avg score time: {} (+/- {})'.format(cross_val['score_time'].mean(), cross_val['score_time'].std()))
print('avg test score: {} (+/- {})'.format(cross_val['test_score'].mean(), cross_val['test_score'].std()))
```


```python
fig,ax = plt.subplots(figsize=(10, 12))
plot_importance(xgbc, ax=ax);
```


```python
dot_data = xgb.to_graphviz(xgbc)
graph = graphviz.Source(dot_data)
graph
```

#### XGBRegressor


```python
boston = load_boston()
X_train, X_test, y_train, y_test = train_test_split(boston.data, boston.target, test_size=0.2, random_state=123)
```


```python
xgbr = XGBRFRegressor(n_estimators=400, learning_rate = 0.1, max_depth=3, objective='reg:squarederror')
xgbr.fit(X_train, y_train)
preds = xgbr.predict(X_test)
```


```python
cross_val = cross_validate(
estimator=xgbr,
X=boston.data, y=boston.target,
cv=5)
print('avg fit time: {} (+/-{})'.format(cross_val['fit_time'].mean(), cross_val['fit_time'].std()))
print('avg score time: {} (+/-{})'.format(cross_val['score_time'].mean(), cross_val['score_time'].std()))
print('avg test score: {} (+/-{})'.format(cross_val['test_score'].mean(), cross_val['test_score'].std()))
```


```python
fig,ax = plt.subplots(figsize=(10, 12))
plot_importance(xgbr, ax=ax);
```


```python
dot_data = xgb.to_graphviz(xgbr)
graph = graphviz.Source(dot_data)
graph
```


```python
diabetes = load_diabetes()
X_train, X_test, y_train, y_test = train_test_split(boston.data, boston.target, test_size=0.2, random_state=123)
```


```python
xgbr = XGBRFRegressor(n_estimators=400, learning_rate = 0.1, max_depth=3, objective='reg:squarederror')
xgbr.fit(X_train, y_train)
preds = xgbr.predict(X_test)
```


```python
cross_val = cross_validate(
estimator=xgbr,
X=diabetes.data, y=diabetes.target,
cv=5)
print('avg fit time: {} (+/-{})'.format(cross_val['fit_time'].mean(), cross_val['fit_time'].std()))
print('avg score time: {} (+/-{})'.format(cross_val['score_time'].mean(), cross_val['score_time'].std()))
print('avg test score: {} (+/-{})'.format(cross_val['test_score'].mean(), cross_val['test_score'].std()))
```


```python
fig,ax = plt.subplots(figsize=(10, 12))
plot_importance(xgbr, ax=ax);
```


```python
dot_data = xgb.to_graphviz(xgbr)
graph = graphviz.Source(dot_data)
graph
```

### LightGBM
- 빠른 학습과 예측 시간
- 더 적은 메모리 사용
- 범주형 특징의 자동 변환과 최적 분할


```python
from lightgbm import LGBMClassifier, LGBMRegressor
from lightgbm import plot_importance, plot_metric, plot_tree
```

#### LGBMClassifier

#### 붓꽃 데이터


```python
iris = load_iris()
X_train, X_test, y_train, y_test = train_test_split(iris.data, iris.target, test_size=0.2, random_state=123)
```


```python
lgbmc = LGBMClassifier(n_estimators=400)
evals = [(X_test, y_test)]
lgbmc.fit(X_train, y_train, early_stopping_rounds=100, eval_metric="logloss", eval_set = evals, verbose=True)
preds = lgbmc.predict(X_test)
```


```python
cross_val = cross_validate(
estimator=lgbmc,
X=iris.data, y=iris.target,
cv=5)
print('avg fit time: {} (+/-{})'.format(cross_val['fit_time'].mean(), cross_val['fit_time'].std()))
print('avg score time: {} (+/-{})'.format(cross_val['score_time'].mean(), cross_val['score_time'].std()))
print('avg test score: {} (+/-{})'.format(cross_val['test_score'].mean(), cross_val['test_score'].std()))
```


```python
plot_metric(lgbmc);
```


```python
plot_importance(lgbmc, figsize=(10, 12));
```


```python
plot_tree(lgbmc, figsize=(28, 24));
```

#### 와인 데이터


```python
wine = load_wine()
X_train, X_test, y_train, y_test = train_test_split(wine.data, wine.target, test_size=0.2, random_state=123)
```


```python
lgbmc = LGBMClassifier(n_estimators=400)
evals = [(X_test, y_test)]
lgbmc.fit(X_train, y_train, early_stopping_rounds=100, eval_metric="logloss", eval_set = evals, verbose=True)
preds = lgbmc.predict(X_test)
```


```python
cross_val = cross_validate(
estimator=lgbmc,
X=wine.data, y=wine.target,
cv=5)
print('avg fit time: {} (+/-{})'.format(cross_val['fit_time'].mean(), cross_val['fit_time'].std()))
print('avg score time: {} (+/-{})'.format(cross_val['score_time'].mean(), cross_val['score_time'].std()))
print('avg test score: {} (+/-{})'.format(cross_val['test_score'].mean(), cross_val['test_score'].std()))
```


```python
plot_metric(lgbmc);
```


```python
plot_importance(lgbmc, figsize=(10, 12));
```


```python
plot_tree(lgbmc, figsize=(28,24));
```

#### 유방암 데이터


```python
cancer = load_breast_cancer()
X_train, X_test, y_train, y_test = train_test_split(cancer.data, cancer.target, test_size=0.2, random_state=123)
```


```python
lgbmc = LGBMClassifier(n_estimators=400)
evals = [(X_test, y_test)]
lgbmc.fit(X_train, y_train, early_stopping_rounds=100, eval_metric="logloss", eval_set = evals, verbose=True)
preds = lgbmc.predict(X_test)
```


```python
cross_val = cross_validate(
estimator=lgbmc,
X=cancer.data, y=cancer.target,
cv=5)
print('avg fit time: {} (+/-{})'.format(cross_val['fit_time'].mean(), cross_val['fit_time'].std()))
print('avg score time: {} (+/-{})'.format(cross_val['score_time'].mean(), cross_val['score_time'].std()))
print('avg test score: {} (+/-{})'.format(cross_val['test_score'].mean(), cross_val['test_score'].std()))
```


```python
plot_metric(lgbmc);
```


```python
plot_importance(lgbmc, figsize=(10, 12));
```


```python
plot_tree(lgbmc, figsize=(28,24));
```

#### LGBMRegressor

#### 보스턴 데이터


```python
boston = load_boston()
X_train, X_test, y_train, y_test = train_test_split(boston.data, boston.target, test_size=0.2, random_state=123)
```


```python
lgbmr = LGBMRegressor(n_estimators=400)
evals = [(X_test, y_test)]
lgbmr.fit(X_train, y_train, early_stopping_rounds=100, eval_metric="logloss", eval_set = evals, verbose=True)
preds = lgbmc.predict(X_test)
```


```python
cross_val = cross_validate(
estimator=lgbmr,
X=boston.data, y=boston.target,
cv=5)
print('avg fit time: {} (+/-{})'.format(cross_val['fit_time'].mean(), cross_val['fit_time'].std()))
print('avg score time: {} (+/-{})'.format(cross_val['score_time'].mean(), cross_val['score_time'].std()))
print('avg test score: {} (+/-{})'.format(cross_val['test_score'].mean(), cross_val['test_score'].std()))
```


```python
plot_metric(lgbmr);
```


```python
plot_importance(lgbmr, figsize=(10, 12));
```


```python
plot_tree(lgbmr, figsize=(28,24));
```

#### 당뇨병 데이터


```python
diabetes = load_diabetes()
X_train, X_test, y_train, y_test = train_test_split(diabetes.data, diabetes.target, test_size=0.2, random_state=123)
```


```python
lgbmr = LGBMRegressor(n_estimators=400)
evals = [(X_test, y_test)]
lgbmr.fit(X_train, y_train, early_stopping_rounds=100, eval_metric="logloss", eval_set = evals, verbose=True)
preds = lgbmr.predict(X_test)
```


```python
cross_val = cross_validate(
estimator=lgbmr,
X=diabetes.data, y=diabetes.target,
cv=5)
print('avg fit time: {} (+/-{})'.format(cross_val['fit_time'].mean(), cross_val['fit_time'].std()))
print('avg score time: {} (+/-{})'.format(cross_val['score_time'].mean(), cross_val['score_time'].std()))
print('avg test score: {} (+/-{})'.format(cross_val['test_score'].mean(), cross_val['test_score'].std()))
```


```python
plot_metric(lgbmr);
```


```python
plot_importance(lgbmr, figsize=(10, 12));
```


```python
plot_tree(lgbmr, figsize=(28,24));
```
