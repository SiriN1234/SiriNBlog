---
title: "2022.03.25 수업내용과 실습2"
author: "Hanjeongin"
date: 2022-03-25 17:12:00
tags: Python
---

```python
# This Python 3 environment comes with many helpful analytics libraries installed
# It is defined by the kaggle/python Docker image: https://github.com/kaggle/docker-python
# For example, here's several helpful packages to load

import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)

# Input data files are available in the read-only "../input/" directory
# For example, running this (by clicking run or pressing Shift+Enter) will list all files under the input directory

import os
for dirname, _, filenames in os.walk('/kaggle/input'):
    for filename in filenames:
        print(os.path.join(dirname, filename))

# You can write up to 20GB to the current directory (/kaggle/working/) that gets preserved as output when you create a version using "Save & Run All" 
# You can also write temporary files to /kaggle/temp/, but they won't be saved outside of the current session
```

공통 : 데이터 전처리를 잘 해야 됨

머신러닝 / 딥러닝 (결과가 중요한 학문)
- 예측만 잘하면 됨
- A 모델, B 모델, C 모델
- 알고리즘의 갯수 무한정
- 자동차 기본옵션, 풀옵션


통계 (과정이 중요한 학문)
- 가설설정(평균의 차이)
- 정규분포, 등분산성 가정 중요함
- 통계해석이 중요함
- 변수들의 관계
- 독립변수들끼리의 관계, 독립변수와 종속변수들끼리의 관계

Scikit-Learn
- Python 머신러닝의 핵심 라이브러리
- 머신러닝 코드를 많이 치자

PyCaret
- AutoML 라이브러리 (옵션)
- 머신러닝 코드를 매우 덜 친다

# 라이브러리 불러오기


```python
import pandas as pd
import matplotlib
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sb
import os

print("Version Pandas", pd.__version__)
print("Version Matplotlib", matplotlib.__version__)
print("Version Numpy", np.__version__)
print("Version Seaborn", sb.__version__)

os.listdir('../input/tabular-playground-series-apr-2021/')
```


```python
BASE_DIR = "../input/tabular-playground-series-apr-2021/"

train = pd.read_csv(BASE_DIR + "train.csv")
test = pd.read_csv(BASE_DIR + "test.csv")
sample_submission = pd.read_csv(BASE_DIR + "sample_submission.csv")

train.shape, test.shape, sample_submission.shape
```


```python
train.head()
```


```python
test.head()
```


```python
sample_submission.head()
```

- 데이터를 합침


```python
# 데이터를 상하로 합침
all_df = pd.concat([train, test])
all_df.shape
```

- 결측치 합치기 & 처리하기 (이 부분이 제일 중요)


```python
# Start
print("Before Handling:", all_df.shape)

# Age
age_dict = all_df[['Age', 'Pclass']].dropna().groupby('Pclass').mean().round(0).to_dict()
print("Avg. Mean of Age by Pclass:", age_dict)
all_df['Age'] = all_df['Age'].fillna(all_df.Pclass.map(age_dict['Age']))

# Cabin
all_df["Cabin"].fillna("No Cabin", inplace = True)
print("Values from Cabin: ", all_df["Cabin"].unique())
all_df['Cabin_Code'] = all_df['Cabin'].fillna('X').map(lambda x: x[0].strip())
print("Values from Cabin Code: ", all_df["Cabin_Code"].unique())

# Fare
print("Avg. Mean:", np.round(all_df['Fare'].mean(), 2))
all_df['Fare'] = all_df['Fare'].fillna(round(all_df['Fare'].mean(), 2))

# Embarked
all_df["Embarked"].fillna("X", inplace = True)
print("Values from Embarked: ", all_df["Embarked"].unique())

# Delete Columns
all_df.drop(['Ticket', 'Cabin', 'Name', 'PassengerId'], axis=1, inplace=True)
print("After Handling:", all_df.shape)
```

- 피처 인코딩 : 문자열을 숫자로 바꾼다
- Feature Encoding
    + 목적 : 머신러닝 자체가 수식이니까, 문자를 받아들이지 않음. 문자를 수식으로 바꿔야 함.
    + Ordinal Encoding, Label Encoding, One-Hot Encoding


```python
cat_cols = ['Pclass', 'Sex', 'Cabin_Code', 'Embarked']
num_cols = ['Age', 'SibSp', 'Parch', 'Fare', 'Survived']

onehot_df = pd.get_dummies(all_df[cat_cols])
print("onehot_df Shape:", onehot_df.shape)

num_df = all_df[num_cols]
print("num_df Shape:", num_df.shape)

all_cleansed_df = pd.concat([num_df, onehot_df], axis=1)
print("all_cleansed_df Shape:", all_df.shape)
```


```python
all_cleansed_df.head()
```

# 머신러닝
- 데이터 셋 분리
    + x는 독립변수
    + y는 종속변수


```python
X = all_cleansed_df[:train.shape[0]]
print("X Shape is:", X.shape)
y = X['Survived']
X.drop(['Survived'], axis=1, inplace=True)
test_data = all_cleansed_df[train.shape[0]:].drop(columns=['Survived'])
test_data.info()
```


```python
X.shape, y.shape
```

- 라이브러리 설치


```python
!pip install scikit-learn==0.23.2
```


```python
import sklearn
print(sklearn.__version__)
```

- 층화추출(Stratfied Sampling)
- random_state : 실험 실행을 할 때마다 데이터셋이 계속 섞이는 것을 방지


```python
from sklearn.model_selection import train_test_split

# X_train -> 훈련데이터 X_val -> 검증데이터 test_size -> 훈련:검증을 7:3으로
X_train, X_val, y_train, y_val = train_test_split(X, y, test_size = 0.3, stratify = X[['Pclass']], random_state=42)
X_train.shape, X_val.shape, y_train.shape, y_val.shape
```

- Base Model
    + 가장 심플한 모델
- 평가지표
    + 분류모형 (정확도)


```python
from sklearn.metrics import accuracy_score
def acc_score(y_true, y_pred, **kwargs):
    return accuracy_score(y_true, (y_pred > 0.5).astype(int), **kwargs)
```

- 모형 학습, 예측, 예측모형의 평가


```python
from sklearn import tree
# DecisionTreeClassifier라는 모형
from sklearn.tree import DecisionTreeClassifier
# roc_curve, roc_auc_score -> 모형 평가에 관한 내용
from sklearn.metrics import roc_curve, roc_auc_score
from matplotlib import pyplot as plt

# 모형을 만드는건 위의 2줄이 끝임
tree_model = DecisionTreeClassifier(max_depth=3)
tree_model.fit(X_train, y_train)
# 예측
predictions = tree_model.predict_proba(X_val)
# 실제 값과 예측 값과 비교(평가)
AUC = roc_auc_score(y_val, predictions[:,1])
ACC = acc_score(y_val, predictions[:,1])
# AUC : 신뢰성, ACC 얼마나 잘 맞췄느냐
print("Model AUC:", AUC)
print("Model Accurarcy:", ACC)
print("\n")

fpr, tpr, _ = roc_curve(y_val, predictions[:,1])

fig, ax = plt.subplots(figsize=(10, 6))

ax.plot(fpr, tpr)
ax.text(x = 0.3, 
        y = 0.4, 
        s = "Model AUC is {}\n\nModel Accuracy is {}".format(np.round(AUC, 2), np.round(ACC, 2)), 
        fontsize=16, bbox=dict(facecolor='gray', alpha=0.3))
ax.set_xlabel('FPR')
ax.set_ylabel('TPR')
ax.set_title('ROC curve')

plt.show()
```

## Helper Class 생성


```python
from sklearn import tree
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import roc_curve, roc_auc_score, accuracy_score, confusion_matrix
from matplotlib import pyplot as plt

SEED = 0 # for Reproducibility

# class 
class sk_helper(object):
    def __init__(self, model, seed = 0, params={}):
        params['random_state'] = seed
        self.model = model(**params)
        self.model_name = str(model).split(".")[-1][:-2]
        
    # train
    def train(self, X_train, y_train):
        self.model.fit(X_train, y_train)
        
    # predict
    def predict(self, y_val):
        return self.model.predict(y_val)
    
    # inner fit
    def fit(self, x, y):
        return self.model.fit(x, y)
    
    # feature importance
    def feature_importances(self, X_train, y_train):
        return self.model.fit(X_train, y_train).feature_importances_
        
    # roc_curve
    def roc_curve_graph(self, X_train, y_train, X_val, y_val):
        self.model.fit(X_train, y_train)
        
        print("model_name:", self.model_name)
        model_name = self.model_name
        preds_proba = self.model.predict_proba(X_val)
        preds = (preds_proba[:, 1] > 0.5).astype(int)
        auc = roc_auc_score(y_val, preds_proba[:, 1])
        acc = accuracy_score(y_val, preds)
        confusion = confusion_matrix(y_val, preds)
        print('Confusion Matrix')
        print(confusion)
        print("Model AUC: {0:.3f}, Model Accuracy: {1:.3f}\n".format(auc, acc))
        fpr, tpr, _ = roc_curve(y_val, predictions[:,1])
        fig, ax = plt.subplots(figsize=(10, 6))

        ax.plot(fpr, tpr)
        ax.text(x = 0.3, 
                y = 0.4, 
                s = "Model AUC is {}\n\nModel Accuracy is {}".format(np.round(auc, 2), np.round(acc, 2)), 
                fontsize=16, bbox=dict(facecolor='gray', alpha=0.3))
        ax.set_xlabel('FPR')
        ax.set_ylabel('TPR')
        ax.set_title('ROC curve of {}'.format(model_name), fontsize=16)

        plt.show()
```


```python
%%time
tree_params = {'max_depth': 6} # 하이퍼 파라미터 개념

tree_model = sk_helper(model=DecisionTreeClassifier, seed=SEED, params=tree_params)
tree_model.roc_curve_graph(X_train, y_train, X_val, y_val)

```


```python
%%time
from sklearn.ensemble import RandomForestClassifier

rf_params = {
    'n_jobs': -1,
    'n_estimators': 500,
     'warm_start': True, 
     #'max_features': 0.2,
    'max_depth': 6,
    'min_samples_leaf': 2,
    'max_features' : 'sqrt',
    'verbose': 1
}

rf_model = sk_helper(model=RandomForestClassifier, seed=SEED, params=rf_params)
rf_model.roc_curve_graph(X_train, y_train, X_val, y_val)
```


```python
%%time

import lightgbm
from lightgbm import LGBMClassifier
print(lightgbm.__version__)
lgb_params = {
    'metric': 'auc',
    'n_estimators': 10000,
    'objective': 'binary',
}

lgb_model = sk_helper(model=LGBMClassifier, seed=SEED, params=lgb_params)
lgb_model.roc_curve_graph(X_train, y_train, X_val, y_val)
```


```python
tree_features = tree_model.feature_importances(X_train, y_train)
rf_features = rf_model.feature_importances(X_train, y_train)
lgb_features = lgb_model.feature_importances(X_train, y_train)
```


```python
cols = X.columns.values
feature_df = pd.DataFrame({'features': cols, 
                          'Decision Tree': tree_features, 
                          'RandomForest': rf_features, 
                          'LightGBM': lgb_features})

feature_df
```


```python
%matplotlib inline

import seaborn as sb
import matplotlib.pyplot as plt

width = 0.3
x = np.arange(0, len(feature_df.index))

## ax[0] graph
fig, ax = plt.subplots(nrows = 2, ncols = 1, figsize = (16, 16)) # Option sharex=True
ax[0].bar(x - width/2, feature_df['Decision Tree'], color = "#0095FF", width = width)
ax[0].bar(x + width/2, feature_df['RandomForest'], color = "#E6C0B1", width = width)
ax[0].set_xticks(x)
ax[0].set_xticklabels(feature_df['features'], rotation=90)

## ax[0] legend
colors = {'Decision Tree':'#0095FF', 'RandomForest':'#E6C0B1'} 
labels = list(colors.keys())
handles = [plt.Rectangle((0,0),1,1, color=colors[label]) for label in labels]

ax[0].legend(handles, labels, bbox_to_anchor = (0.95, 0.95))
ax[0].set_title("Feature Importance between Decision Tree and RandomForest", fontsize=20)

## ax[1] graph
ax[1].bar(x, feature_df['LightGBM'], color = "#60F09E")
ax[1].set_xticks(x)
ax[1].set_xticklabels(feature_df['features'], rotation=90)
ax[1].set_title("Feature Importance of LightGBM", fontsize=20)

## plt manage
## plt.xticks(x, feature_df['features'], rotation=90)
plt.tight_layout()
plt.show()
```


```python
import numpy as np
from datetime import datetime

version = datetime.now().strftime("%d-%m-%Y %H-%M-%S")

def final_submission(model, data, version):
    final_preds = model.predict(data)
    binarizer = np.vectorize(lambda x: 1 if x >= .5 else 0)
    prediction_binarized = binarizer(final_preds)
    submission = pd.concat([sample_submission,pd.DataFrame(prediction_binarized)], axis=1).drop(columns=['Survived'])
    submission.columns = ['PassengerId', 'Survived']
    submission.to_csv('Sklearn of Submit Date {} Submission.csv'.format(version), index=False)
    
final_submission(lgb_model, test_data, version)
```

## PyCaret


```python
# !pip uninstall scikit-learn -y
!pip install pycaret==2.2.3
```

# 라이브러리 버전 확인


```python
# check version
from pycaret.utils import version
import sklearn
print("pycaret version:", version())
print("sklearn version:", sklearn.__version__)
```

# 데이터셋 재정리


```python
all_df_pycaret = pd.concat([X, y], axis=1)
all_df_pycaret['Survived'] = all_df_pycaret['Survived'].astype('int64')
all_df_pycaret.info()
```

- ML 만들기 세


```python
from pycaret.classification import *

setup(data = all_df_pycaret, 
      target = 'Survived', 
      fold = 3,
      silent = True
     )

set_config('seed', 123)
```

# 클래스
- 각 개별적으로 모형 다 만들어야함
- 평가 수치도 개별적으로 뽑은 후, 하나로 합치는 작업 (코드로 구현)


```python
%%time

best_model = compare_models(sort = 'Accuracy', n_select = 3)
```


```python
%%time
gbc_model = create_model('gbc')
```


```python
%%time
tuned_gbc = tune_model(gbc_model, n_iter = 50)
```

# Feature Importance


```python
plot_model(tuned_gbc, plot = 'feature_all')
```

# 제출


```python
predictions = predict_model(tuned_gbc, data = test_data)
predictions.info()
```


```python
submission = pd.read_csv(BASE_DIR + 'sample_submission.csv')
submission['Survived'] = predictions['Label']
submission.to_csv('PyCaret Submission.csv', index=False)
submission.head()
```
