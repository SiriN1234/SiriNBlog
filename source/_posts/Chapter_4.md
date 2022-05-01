---
title: "Chapter 4장"
author: "Hanjeongin"
date: 2022-03-29 16:58:00
tags: Python
---

출처 : 혼자 공부하는 머신러닝+딥러닝

# 데이터 불러오기

- 컬럼 설명 177p 그림


```python
import pandas as pd

fish = pd.read_csv('https://bit.ly/fish_csv_data')
fish.head()
```





  <div id="df-e573fc57-aa15-46d6-a1a5-744320d4011a">
    <div class="colab-df-container">
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
      <th>Species</th>
      <th>Weight</th>
      <th>Length</th>
      <th>Diagonal</th>
      <th>Height</th>
      <th>Width</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bream</td>
      <td>242.0</td>
      <td>25.4</td>
      <td>30.0</td>
      <td>11.5200</td>
      <td>4.0200</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Bream</td>
      <td>290.0</td>
      <td>26.3</td>
      <td>31.2</td>
      <td>12.4800</td>
      <td>4.3056</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bream</td>
      <td>340.0</td>
      <td>26.5</td>
      <td>31.1</td>
      <td>12.3778</td>
      <td>4.6961</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bream</td>
      <td>363.0</td>
      <td>29.0</td>
      <td>33.5</td>
      <td>12.7300</td>
      <td>4.4555</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bream</td>
      <td>430.0</td>
      <td>29.0</td>
      <td>34.0</td>
      <td>12.4440</td>
      <td>5.1340</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-e573fc57-aa15-46d6-a1a5-744320d4011a')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

      <script>
        const buttonEl =
          document.querySelector('#df-e573fc57-aa15-46d6-a1a5-744320d4011a button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-e573fc57-aa15-46d6-a1a5-744320d4011a');
          const dataTable =
            await google.colab.kernel.invokeFunction('convertToInteractive',
                                                     [key], {});
          if (!dataTable) return;

          const docLinkHtml = 'Like what you see? Visit the ' +
            '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
            + ' to learn more about interactive tables.';
          element.innerHTML = '';
          dataTable['output_type'] = 'display_data';
          await google.colab.output.renderOutput(dataTable, element);
          const docLink = document.createElement('div');
          docLink.innerHTML = docLinkHtml;
          element.appendChild(docLink);
        }
      </script>
    </div>
  </div>




# 데이터 변환

- 배열로 변환
- 독립변수


```python
fish_input = fish[['Weight', 'Length', 'Diagonal', 'Height', 'Width']].to_numpy()

fish_input.shape
```




    (159, 5)



- target 배열로 변환
- 종속변수


```python
fish_target = fish['Species'].to_numpy()
```

## 훈련데이터와 테스트데이터


```python
from sklearn.model_selection import train_test_split

train_input, test_input, train_target, test_target = train_test_split(
    fish_input, fish_target, random_state = 42
)
```

- 표준화 전처리


```python
from sklearn.preprocessing import StandardScaler

ss = StandardScaler()
ss.fit(train_input)

train_scaled = ss.transform(train_input)
test_scaled = ss.transform(test_input)

print(train_input[:5])
print(train_scaled[:5])
print(test_scaled[:5])
```

    [[720.      35.      40.6     16.3618   6.09  ]
     [500.      45.      48.       6.96     4.896 ]
     [  7.5     10.5     11.6      1.972    1.16  ]
     [110.      22.      23.5      5.5225   3.995 ]
     [140.      20.7     23.2      8.5376   3.2944]]
    [[ 0.91965782  0.60943175  0.81041221  1.85194896  1.00075672]
     [ 0.30041219  1.54653445  1.45316551 -0.46981663  0.27291745]
     [-1.0858536  -1.68646987 -1.70848587 -1.70159849 -2.0044758 ]
     [-0.79734143 -0.60880176 -0.67486907 -0.82480589 -0.27631471]
     [-0.71289885 -0.73062511 -0.70092664 -0.0802298  -0.7033869 ]]
    [[-0.88741352 -0.91804565 -1.03098914 -0.90464451 -0.80762518]
     [-1.06924656 -1.50842035 -1.54345461 -1.58849582 -1.93803151]
     [-0.54401367  0.35641402  0.30663259 -0.8135697  -0.65388895]
     [-0.34698097 -0.23396068 -0.22320459 -0.11905019 -0.12233464]
     [-0.68475132 -0.51509149 -0.58801052 -0.8998784  -0.50124996]]
    

# k-최근접 이웃 분류기의 확률 예측


```python
from sklearn.neighbors import KNeighborsClassifier

kn = KNeighborsClassifier(n_neighbors=3)
kn.fit(train_scaled, train_target)

print(kn.score(train_scaled, train_target))
print(kn.score(test_scaled, test_target))
```

    0.8907563025210085
    0.85
    

- 182p
- 다중분류


```python
import numpy as np

proba = kn.predict_proba(test_scaled[:5])
print(np.round(proba, decimals = 4))
print(kn.classes_)
```

    [[0.     0.     1.     0.     0.     0.     0.    ]
     [0.     0.     0.     0.     0.     1.     0.    ]
     [0.     0.     0.     1.     0.     0.     0.    ]
     [0.     0.     0.6667 0.     0.3333 0.     0.    ]
     [0.     0.     0.6667 0.     0.3333 0.     0.    ]]
    ['Bream' 'Parkki' 'Perch' 'Pike' 'Roach' 'Smelt' 'Whitefish']
    

# 로지스틱 회귀
- 중요도 : 최상
- 오늘 유튜브 영상 반드시 신청
  + 개념 재복습 필요
- Why?
  + 로지스틱 회귀
    + 통계로도 활용 (의학통계)
    + 머신러닝 분류모형의 기초 모형인데, 성능이 생각보다 괜찮음
      - 데이터셋, 수치 데이터 기반
    + 딥러닝 : 초기모형에 해당됨


```python
import numpy as np
import matplotlib.pyplot as plt

z = np.arange(-5, 5, 0.1)
phi = 1 / (1 + np.exp(-z))

# print(z)
# print(phi)

fig, ax = plt.subplots()

ax.plot(z, phi)
ax.set_xlabel('z')
ax.set_ylabel('phi')

plt.show()
```


    
![png](/images/Chapter_4/output_15_0.png)
    


- 개발자 취업을 원함
  + 공부를 별도로 하지 않는다
- 컨셉만 이해하면 됨
- 데이터분석 관련 일하고 싶다
  + 반드시 공부를 해야 함

# 로지스틱 회귀로 이진 분류 수행하기


```python
char_arr = np.array(['A', 'B', 'C', 'D', 'E'])

print(char_arr[[True, False, True, False, False]])
```

    ['A' 'C']
    


```python
bream_smelt_indexes = (train_target == 'Bream') | (train_target == 'Smelt')
train_bream_smelt = train_scaled[bream_smelt_indexes]
target_bream_smelt = train_target[bream_smelt_indexes]
```

- p.186
- 모형 만들고 예측하기


```python
from sklearn.linear_model import LogisticRegression
lr = LogisticRegression()

# 왼쪽은 독립변수 오른쪽은 독립변수
lr.fit(train_bream_smelt, target_bream_smelt)
```




    LogisticRegression()




```python
# 예측하기
# 클래스로 분류
# 확률값 -> 0.5
print(lr.predict(train_bream_smelt[:5]))
```

    ['Bream' 'Smelt' 'Bream' 'Bream' 'Bream']
    


```python
print(lr.predict_proba(train_bream_smelt[:5]))
print(lr.classes_)
```

    [[0.99759855 0.00240145]
     [0.02735183 0.97264817]
     [0.99486072 0.00513928]
     [0.98584202 0.01415798]
     [0.99767269 0.00232731]]
    ['Bream' 'Smelt']
    

- 방정식의 각 기울기와 상수를 구하는 코드


```python
print(lr.coef_, lr.intercept_)
```

    [[-0.4037798  -0.57620209 -0.66280298 -1.01290277 -0.73168947]] [-2.16155132]
    

- z식
- z값을 출력하자


```python
decisions = lr.decision_function(train_bream_smelt[:5])

print(decisions)
```

    [-6.02927744  3.57123907 -5.26568906 -4.24321775 -6.0607117 ]
    


```python
from scipy.special import expit

print(expit(decisions))
```

    [0.00240145 0.97264817 0.00513928 0.01415798 0.00232731]
    

## 경사하강법이 쓰인 여러 알고리즘

- (이미지, 텍스트) 딥러닝 기초 알고리즘
- 트리 알고리즘 + 경사하강법 융합 = 부스팅 계열
  + 대표 알고리즘 : LightGBM, Xgboost, Catboost
  + kaggle 1등으로 자주 쓰인 알고리즘 = LightGBM, Xgboost
  + 하이퍼 파라미터의 갯수가 80개가 넘음

# SGDClassifier

- 확률적 경사하강법 분류기


```python
import pandas as pd

fish = pd.read_csv("https://bit.ly/fish_csv_data")
fish.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 159 entries, 0 to 158
    Data columns (total 6 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   Species   159 non-null    object 
     1   Weight    159 non-null    float64
     2   Length    159 non-null    float64
     3   Diagonal  159 non-null    float64
     4   Height    159 non-null    float64
     5   Width     159 non-null    float64
    dtypes: float64(5), object(1)
    memory usage: 7.6+ KB
    

- 배열로 변환하는 코드
  + 독립변수 = fish_input
  + 종속변수 = fish_target


```python
fish_input = fish[['Weight', 'Length', 'Diagonal', 'Height', 'Width']].to_numpy()

fish_input.shape

fish_target = fish['Species'].to_numpy()
```

- 훈련 세트와 테스트 세트로 분리


```python
from sklearn.model_selection import train_test_split

train_input, test_input, train_target, test_target = train_test_split(
    fish_input, fish_target, random_state = 42
)

train_input.shape, test_input.shape, train_target.shape, test_target.shape
```




    ((119, 5), (40, 5), (119,), (40,))



- 표준화 처리
  + 다시 한번 강조하지만 꼭 훈련 세트에서 학습한 통계값으로 테스트 세트도 변환한다
  + 키워드 : Data Leakage
  + 데이터 분석 희망자 필수 공부


```python
from sklearn.preprocessing import StandardScaler

ss = StandardScaler()
ss.fit(train_input)

# ss 훈련데이터만 활용해서 학습이 끝난 상태
# 표준화 처리를 훈련데이터와 테스트데이터에 동시 적용
train_scaled = ss.transform(train_input)
test_scaled = ss.transform(test_input)
```

# 모델 학습

- 2개의 매개 변수 지정
- loss = "log" = 로지스틱 손실 함수로 지정
- max_iter = 에포크 횟수 지정


```python
from sklearn.linear_model import SGDClassifier

# 매개변수 지정
# 하이퍼 파라미터 설정
## 매개변수 값을 dictionary 형태로 추가하는 코드 작성 가능
## 입문자들에게는 비추천
sc = SGDClassifier(loss = "log", max_iter = 100, random_state = 42)

# 모형 학습
sc.fit(train_scaled, train_target)

# 스코어 확인(정확도)
print(sc.score(train_scaled, train_target))
print(sc.score(test_scaled, test_target))
```

    0.8403361344537815
    0.8
    

- 적절한 에포크 숫자를 찾자


```python
import numpy as np
sc = SGDClassifier(loss = "log", max_iter = 100, tol = None, random_state = 42)
train_score = []
test_score = []
classes = np.unique(train_target)

for _ in range(0, 300) :
  sc.partial_fit(train_scaled, train_target, classes = classes)
  train_score.append(sc.score(train_scaled, train_target))
  test_score.append(sc.score(test_scaled, test_target))

# 정확도 상위 5개 출력
print(train_score[:5])
print(test_score[:5])
```

    [0.5294117647058824, 0.6218487394957983, 0.6386554621848739, 0.7310924369747899, 0.7226890756302521]
    [0.65, 0.55, 0.575, 0.7, 0.7]
    

- 모형 학습 시각화


```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots()

ax.plot(train_score)
ax.plot(test_score)
ax.set_xlabel("Epoch")
ax.set_ylabel("Accuracy")
plt.show()
```


    
![png](/images/Chapter_4/output_43_0.png)
    


- 위가 훈련데이터, 밑이 테스트데이터
