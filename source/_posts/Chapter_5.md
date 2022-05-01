---
title: "Chapter 5장"
author: "Hanjeongin"
date: 2022-03-30 20:12:00
tags: Python
---

출처 : 혼자 공부하는 머신러닝+딥러닝

# 데이터 불러오기

- 와인 데이터
  + alcohol(알코올 도수), sugar(당도), pH(산도)
  + 클래스 0 = 레드 와인
  + 클래스 1 = 화이트 와인


```python
import pandas as pd

wine = pd.read_csv('https://bit.ly/wine_csv_data')

wine.head()
```





  <div id="df-288300f0-c1b2-4028-8df7-21b3a2cfa4a6">
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
      <th>alcohol</th>
      <th>sugar</th>
      <th>pH</th>
      <th>class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>9.4</td>
      <td>1.9</td>
      <td>3.51</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>9.8</td>
      <td>2.6</td>
      <td>3.20</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9.8</td>
      <td>2.3</td>
      <td>3.26</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9.8</td>
      <td>1.9</td>
      <td>3.16</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9.4</td>
      <td>1.9</td>
      <td>3.51</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-288300f0-c1b2-4028-8df7-21b3a2cfa4a6')"
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
          document.querySelector('#df-288300f0-c1b2-4028-8df7-21b3a2cfa4a6 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-288300f0-c1b2-4028-8df7-21b3a2cfa4a6');
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




- info()
  + 결측치 확인 / 변수타입


```python
wine.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 6497 entries, 0 to 6496
    Data columns (total 4 columns):
     #   Column   Non-Null Count  Dtype  
    ---  ------   --------------  -----  
     0   alcohol  6497 non-null   float64
     1   sugar    6497 non-null   float64
     2   pH       6497 non-null   float64
     3   class    6497 non-null   float64
    dtypes: float64(4)
    memory usage: 203.2 KB
    

- describe()
  + 열에 대한 간략한 통계를 출력해 줌
  + 평균, 표준편차, 최소, 최대, 사분위수에 대해 알 수 있음


```python
wine.describe()
```





  <div id="df-1cdc8b8a-2553-4905-b2da-cc82a9d4416b">
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
      <th>alcohol</th>
      <th>sugar</th>
      <th>pH</th>
      <th>class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>6497.000000</td>
      <td>6497.000000</td>
      <td>6497.000000</td>
      <td>6497.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>10.491801</td>
      <td>5.443235</td>
      <td>3.218501</td>
      <td>0.753886</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.192712</td>
      <td>4.757804</td>
      <td>0.160787</td>
      <td>0.430779</td>
    </tr>
    <tr>
      <th>min</th>
      <td>8.000000</td>
      <td>0.600000</td>
      <td>2.720000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>9.500000</td>
      <td>1.800000</td>
      <td>3.110000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>10.300000</td>
      <td>3.000000</td>
      <td>3.210000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>11.300000</td>
      <td>8.100000</td>
      <td>3.320000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>14.900000</td>
      <td>65.800000</td>
      <td>4.010000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-1cdc8b8a-2553-4905-b2da-cc82a9d4416b')"
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
          document.querySelector('#df-1cdc8b8a-2553-4905-b2da-cc82a9d4416b button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-1cdc8b8a-2553-4905-b2da-cc82a9d4416b');
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




- 알코올 도수와 당도, pH 값으 스케일이 다름
- 표준화가 필요함

# 표준화 작업

- 판다스 데이터프레임을 넘파일 배열로 바꿔 줌


```python
data = wine[['alcohol', 'sugar', 'pH']].to_numpy()
target = wine['class'].to_numpy()
```

# 훈련 데이터와 테스트 데이터로 분리


```python
from sklearn.model_selection import train_test_split

train_input, test_input, train_target, test_target = train_test_split(
    data, target, test_size = 0.2, random_state = 42
) # test_size는 훈련데이터와 테스트데이터 비율을 8:2로 나눈 것

print(train_input.shape, test_input.shape)
```

    (5197, 3) (1300, 3)
    

- train_test_split() 함수는 설정값을 지정하지 않으면 25%를 테스트 세트로 지정 함


```python
from sklearn.preprocessing import StandardScaler

ss = StandardScaler()
ss.fit(train_input)
train_scaled = ss.transform(train_input)
test_scaled = ss.transform(test_input)
```

# 모델 만들기

## 로지스틱회귀


```python
from sklearn.linear_model import LogisticRegression
lr = LogisticRegression()
lr.fit(train_scaled, train_target)
print(lr.score(train_scaled, train_target))
print(lr.score(test_scaled, test_target))
print(lr.coef_, lr.intercept_)
```

    0.7808350971714451
    0.7776923076923077
    [[ 0.51270274  1.6733911  -0.68767781]] [1.81777902]
    

- 로지스틱 회귀 모델로는 다른 사람에게 살명하기 어려움
- 설명하기 쉬운 모델이 필요함

## 의사결정트리

- 질문을 하나씩 던져서 예 아니오로 분류를 함
- 계숙 질문을 추가해서 분류 정확도를 높이는 방식

의사결정트리는 1975년도 최초등장
- 다양한 산업에서 많이 쓰임

2001년 즈음 랜덤 포레스트
- 캐글 대회
- 2017~8년까지 정형데이터 대회는 랜덤포레스트가 메인 이론
- 치명적인 단점은 과적합이 잘 발생함
- 숫자에 민감하지 않음
- 표준화를 해줄 필요가 없음

로지스틱 회귀
- 수식 -> 숫자에 민감
- 표준화 필요

의사결정트리의 기본 알고리즘을 활용해서, MS, 구글 등 이런 회사들이 신규 알고리즘을 만듦
- XGBoost, LightGBM, CatBoost
- 캐글 정형데이터
- LightGBM(지금현재 실무에서 많이 쓰임)
  + 4월 말까지는 코드에 집중
  + PPT(알고리즘 소개)
  + LightGBM을 쓰려면 의사결정트리와 경사하강법에 대해서 알아야 됨
  


```python
from sklearn.tree import DecisionTreeClassifier
dt = DecisionTreeClassifier(random_state = 42)
dt.fit(train_scaled, train_target)

print(dt.score(train_scaled, train_target)) # 훈련 세트
print(dt.score(test_scaled, test_target)) # 테스트 세트
```

    0.996921300750433
    0.8592307692307692
    

- 성능보다 안정적인게 중요함
- 테스트스코어를 올리는 것 보다 훈련스코어를 내리는게 더 쉬움
- 그래서 훈련스코어를 낮추는 편

## 시각화


```python
import matplotlib.pyplot as plt
from sklearn.tree import plot_tree

plt.figure(figsize = (10,7))
plot_tree(dt)
plt.show()
```


    
![png](/images/Chapter_5/output_23_0.png)
    


- 이대로 보기에는 너무 복잡함
- plot_tree() 함수에서 트리의 깊이를 제한해서 출력
- max_dapth를 통해 가능
- filled 매게변수에서 클래스에 맞게 노드의 색을 칠할 수 있음


```python
plt.figure(figsize = (16, 12))
plot_tree(dt, max_depth = 2, filled = True, feature_names = ['alcohol', 'sugar', 'pH'])
plt.show()
```


    
![png](/images/Chapter_5/output_25_0.png)
    


- gini는 불순도
- 질문을 통해서 걸러냄
- value에서 왼쪽이 조건 만족의 갯수, 오른쪽은 조건 불만족
- 지니 불순도 = 1 - (음성 클래스 비율 ^ 2 + 양성 클래스 비율 ^ 2)
- 결정 트리 모델은 부모 노드와 자식 노드의 불순도 차이가 가능한 크도록 트리를 성장시킴
- 부모와 자식 노드 사이의 불순도 차이를 **정보 이득(information gain)**이라고 부름

# 가지치기

- 과대적합을 방지하기 위한 것
- 결정 트리에서 가지치기를 하는 가장 간단한 방법은 자라날 수 있는 트리의 최대 깊이를 지정하는 것


```python
dt = DecisionTreeClassifier(max_depth = 3, random_state = 42)
dt.fit(train_scaled, train_target)
print(dt.score(train_scaled, train_target))
print(dt.score(test_scaled, test_target))
```

    0.8454877814123533
    0.8415384615384616
    

- 훈련 세트의 성능은 낮아졌지만 테스트 세트의 성능은 거의 그대로
- 안정성이 높아짐


```python
plt.figure(figsize = (20,15))
plot_tree(dt, filled = True, feature_names = ['alcohol', 'sugar', 'pH'])
plt.show()
```


    
![png](/images/Chapter_5/output_30_0.png)
    


## 시각화 해석

- 당도가 -0.802보다 크고 -0.239보다 작은 와인 중에 알코올 도수가 0.454와 같거나 작은 것이 레드와인
- 설명하기 이상함

## 의사결정트리의 특징

- 샘플을 어떤 클래스 비율로 나누는지 계산할 때 특성값의 스케일이 계산에 영향을 미치지 않음
- 따라서 표준화 전처리를 할 필요가 없음
- 전처리하기 전의 훈련 세트와 테스트 세트로 결정트리 모델을 훈련


```python
dt = DecisionTreeClassifier(max_depth = 3, random_state = 42)
dt.fit(train_input, train_target)
print(dt.score(train_input, train_target))
print(dt.score(test_input, test_target))
```

    0.8454877814123533
    0.8415384615384616
    

## 시각화


```python
plt.figure(figsize = (20,15))
plot_tree(dt, filled = True, feature_names = ['alcohol', 'sugar', 'pH'])
plt.show()
```


    
![png](/images/Chapter_5/output_35_0.png)
    


## 시각화 해석

- 특성값을 표준점수로 바꾸지 않아서 이해하기 훨씬 쉬움
- 당도가 1.625보다 크고 4.325보다 작은 와인 중에 알코올 도수가 11.025와 같거나 작은 것이 레드 와인, 그 외에는 화이트 와인으로 예측

## 특성 중요도

- 결정 트리는 어떤 특성이 가장 유용한지 나타내는 특성 중요도를 계산해 줌
- 이 트리의 루트 노드와 깊이 1에서 당도를 사용했기 때문에 아마도 당도(sugar)가 가장 유용한 특성 중 하나 일 것이라 예상
- 특성 중요도는 feature_importances_ 속성에 저장되어 있음


```python
print(dt.feature_importances_)
```

    [0.12345626 0.86862934 0.0079144 ]
    

- 알콜이 12%정도, 당도가 87%정도, 나머지는 산도
- 당도의 특성 중요도가 가장 높음

# 마무리

## 키워드로 끝내는 핵심 포인트

- **결정 트리**는 예 / 아니오에 대한 질문을 이어나가면서 정답을 찾아 학습하는 알고리즘입니다. 비교적 예측 과정을 이해하기 쉽고 성능도 뛰어납니다
- **불순도**는 결정 트리가 최적의 질문을 찾기 위한 기준입니다. 사이킷런은 지니 불순도와 엔트로피 불순도를 제공합니다.
- **정보 이득**은 부모 노드와 자식 노드의 불순도 차이입니다. 결정 트리 알고리즘은 정보 이득이 최대화되도록 학습합니다.
- ** 결정 트리는 제한 없이 성장하면 훈련 세트에 과대적합되기 쉽습니다. **가지치기**는 결정 트리의 성장을 제한하는 방법입니다. 사이킷런의 결정 트리 알고리즘은 여러가지 가지치기 매개변수를 제공합니다
- **특성 중요도**는 결정 트리에 사용된 특성이 불순도를 감수하는데 기여한 정도를 나타내는 값입니다. 특성 중요도를 계산할 수 있는 것이 결정 트리의 또다른 큰 장점입니다.

## 핵심 패키지와 함수

### pandas

- **info()**는 데이터프레임의 요약된 정보를 출력합니다. 인덱스와 컬럼 타입을 출력하고 널(null)이 아닌 값으 개수, 메모리 사용량을 제공합니다. verbose 매개변수의 기본값 True를 False로 바꾸면 각 열에 대한 정보를 출력하지 않습니다.
- **describe()**는 데이터프레임의 열의 통계 값을 제공합니다.
  + 수치형일 경우 최소, 최대, 평균, 표준편차와 사분위값 등이 출력됩니다.
  + 문자열 같은 객체 타입의 열은 가장 자주 등장하는 값과 횟수 등이 출력됩니다.
  + percentiles 매개변수에서 백분위수를 지정합니다. 기본값은 [0.25, .5, 0.75]입니다.

### scikit-learn

- **DecisionTreeClassifier**는 결정 트리 분류 클래스입니다.
  + criterion 매개변수는 불순도를 지정하며 기본값은 지니 불순도를 의미하는 'gini'이고 'entropy'를 선택하여 엔트로피 불순도를 사용할 수 있습니다.
  + splitter 매개변수는 노드를 분할하는 전략을 선택합니다. 기본값은 'best'로 정보 이득이 최대가 되도록 분할합니다. 'random'이면 임의로 노드를 분할합니다.
  + max_depth는 트리가 성장할 최대 깊이를 지정합니다. 기본값은 None으로 리프 노드가 순수하거나 min_samples_split보다 샘플 개수가 적을 때까지 성장합니다.
  + min_samples_split은 노드를 나누기 위한 최소 샘플의 개수입니다. 기본값은 2입니다.
  + max_features 매개변수는 최적의 분할을 위해 탐색할 특성의 개수를 지정합니다. 기본값은 None으로 모든 특성을 사용합니다.

- **plot_tree()**는 결정 트리 모델을 시각화합니다. 첫 번째 매개변수로 결정 트리 모델 객체를 전달합니다.
  + max_depth 매개변수로 나타낼 트리의 깊이를 지정합니다. 기본값은 None으로 모든 노드를 출력합니다.
  + feature_names 매개변수로 특성의 이름을 지정할 수 있습니다.
  + filled 매개변수를 True로 지정하면 타깃값에 따라 노드 안에 색을 채웁니다.

# 확인문제 3번 코드 테스트

- min_impurity_decrease를 사용해서 가지치기 해보기
- (노드의 정보 이득) * (노드의 샘플 수) / (전체 샘플 수) 값이 min_impurity_decrease보다 작으면 더 이상 분할하지 않음


```python
dt = DecisionTreeClassifier(min_impurity_decrease = 0.0005, random_state = 42)
dt.fit(train_input, train_target)

print(dt.score(train_input, train_target))
print(dt.score(test_input, test_target))

plt.figure(figsize = (20, 15))
plot_tree(dt, filled = True, feature_names = ['alcohol', 'sugar', 'pH'])
plt.show()
```

    0.8874350586877044
    0.8615384615384616
    


    
![png](/images/Chapter_5/output_44_1.png)
    


- 훈련 데이터 스코어와 테스트 데이터 스코어 값이 비슷함
- 안정성이 높음
- 전체적으로 성능도 좋은 편

# png 파일로 시각화


```python
import graphviz
from sklearn import tree

# DOT data
dot_data = tree.export_graphviz(dt, out_file=None, 
                                feature_names = ['alcohol', 'sugar', 'pH'],  
                                filled=True)

# Draw graph
graph = graphviz.Source(dot_data, format="png") 
graph
```




    
![svg](/images/Chapter_5/output_47_0.svg)
    




```python
graph.render("decision_tree_graphivz")
```




    'decision_tree_graphivz.png'



# 노드 색 바꾸기


```python
from matplotlib.colors import ListedColormap, to_rgb
import numpy as np

plt.figure(figsize=(20, 15))
artists = plot_tree(dt, filled = True, 
          feature_names = ['alcohol', 'sugar', 'pH'])

colors = ['blue', 'red']
for artist, impurity, value in zip(artists, dt.tree_.impurity, dt.tree_.value):
    r, g, b = to_rgb(colors[np.argmax(value)])
    f = impurity * 2
    artist.get_bbox_patch().set_facecolor((f + (1-f)*r, f + (1-f)*g, f + (1-f)*b))
    artist.get_bbox_patch().set_edgecolor('black')

plt.show()
```


    
![png](/images/Chapter_5/output_50_0.png)
    


# 교차 검증과 그리드 서치

- 키워드 : 하이퍼파라미터 (그리드서치 vs 랜덤서치)
- 데이터가 작을 때, 주로 사용
- 하이퍼파라미터
  + max_depth : 3, 정확도가 84%
- 결론
  + 모르면 디폴트만 쓰자
  + 가성비 (시간 대비 성능 보장 안됨)

# 검증 세트

- 테스트 세트 (1회성)
- 훈련 데이터를 훈련 데이터 + 검증 데이터로 재 분할

## 현실

- 테스트 데이터가 별도로 존재하지 않음
- 전체 데이터 = 훈련 (6) : 검증 (2) : 테스트 (2)
  + 테스트 데이터는 모르는 데이터로 생각

# 데이터 불러오기


```python
import pandas as pd

wine = pd.read_csv('https://bit.ly/wine_csv_data')

data = wine[['alcohol', 'sugar', 'pH']].to_numpy()
target = wine['class'].to_numpy()
```


```python
from sklearn.model_selection import train_test_split

train_input, test_input, train_target, test_target = train_test_split(
    data, target, test_size = 0.2, random_state = 42
)
```

- 검증 세트 만들기


```python
sub_input, val_input, sub_target, val_target = train_test_split(
    train_input, train_target, test_size = 0.2, random_state = 42
)
```


```python
print(sub_input.shape, val_input.shape, test_input.shape)
```

    (4157, 3) (1040, 3) (1300, 3)
    

# 모델 만든 후 평가

- 과대적합 발생


```python
from sklearn.tree import DecisionTreeClassifier
dt = DecisionTreeClassifier(random_state = 42)
dt.fit(sub_input, sub_target)
print(dt.score(sub_input, sub_target))
print(dt.score(val_input, val_target))
```

    0.9971133028626413
    0.864423076923077
    

# 교차 검증

- 교차 검증의 목적 : 좋은 모델이 만들어진다
  + 좋은 모델 != 성능 좋은 모델
  + 좋은 모델 = 과대적합이 아닌 모델 = 모형의 오차가 적은 모델 = 안정적인 모델
- 교재 245p
  + 모델 평가 1 : 90% (소요시간 : 1시간)
  + 모델 평가 2 : 85%
  + 모델 평가 3 : 80%
- 단점 : 시간이 오래 걸림

# 교차 검증 함수

- 사이킷런에 cross_validate()라는 교차 검증 함수 사용
- 먼저 평가할 모델 객체를 첫 번째 매개변수로 전달, 그 다음 훈련세트 전체를 cross_validate() 함수에 전달


```python
from sklearn.model_selection import cross_validate
scores = cross_validate(dt, train_input, train_target)

print(scores)
```

    {'fit_time': array([0.02719903, 0.01022577, 0.01074076, 0.01042938, 0.0099504 ]), 'score_time': array([0.00116682, 0.00098372, 0.00113153, 0.00108194, 0.0010345 ]), 'test_score': array([0.86923077, 0.84615385, 0.87680462, 0.84889317, 0.83541867])}
    

- 최종점수 평균 구하기
- 검증 폴드의 점수임


```python
import numpy as np

print(np.mean(scores['test_score']))
```

    0.8574181117533719
    

- 훈련 세트 섞은 후, 10-폴드 교차검증


```python
from sklearn.model_selection import StratifiedKFold
splitter = StratifiedKFold(n_splits = 10, shuffle = True, random_state = 42)
scores = cross_validate(dt, train_input, train_target, cv = splitter)

print(np.mean(scores['test_score']))
```

    0.8574181117533719
    

# 하이퍼파라미터 튜닝을 꼭 하고싶다면

- 랜덤 서치를 사용하자
- 자동으로 잡아주는 라이브러리들이 등장하기 시작함
  + hyperopt

## 그리드 서치

- 하이퍼파라미터 탐색과 교차 검증을 한 번에 수행함
- 별도로 cross_validate() 함수를 호출할 필요 없음


```python
from sklearn.model_selection import GridSearchCV
params = {'min_impurity_decrease' : [0.0001, 0.0002, 0.0003, 0.0004, 0.0005]}

gs = GridSearchCV(DecisionTreeClassifier(random_state = 42), params, n_jobs = -1)
gs.fit(train_input, train_target)
```




    GridSearchCV(estimator=DecisionTreeClassifier(random_state=42), n_jobs=-1,
                 param_grid={'min_impurity_decrease': [0.0001, 0.0002, 0.0003,
                                                       0.0004, 0.0005]})



- 그리드 서치는 훈련이 끝나면 모델 중에서 검증 점수가 가장 높은 모델의 매개변수 조합으로 전체 훈련 세트에서 자도응로 다시 모델을 훈련 함
- 이 모델은 gs 객체의 best_estimator_ 속성에 저장되어 있음
- 그리드 서치로 찾은 최적의 매개변수는 best_params_ 속성에 저장되어 있음


```python
dt = gs.best_estimator_
print(dt)
print(dt.score(train_input, train_target))
print(gs.best_params_)
```

    DecisionTreeClassifier(min_impurity_decrease=0.0001, random_state=42)
    0.9615162593804117
    {'min_impurity_decrease': 0.0001}
    

- 교차 검증으로 얻은 점수 출력


```python
print(gs.cv_results_['mean_test_score'])
```

    [0.86819297 0.86453617 0.86492226 0.86780891 0.86761605]
    

## 랜덤 서치

- 252p
- 매개변수 값의 목록을 전달하는 것이 아니라 매개변수를 샘플링 할 수 있도록 확률 분포 객체를 전달
- scipy는 파이썬의 핵심 과학 라이브러리 중 하나. 적분, 보간, 선형 대수, 확률 등을 포함한 수치 계산 전용 라이브러리.
- uniform과 randint 클래스는 모두 주어진 범위에서 고르게 값을 뽑음
- 이를 '균등 군포에서 샘플링한다'라고 말 함
- randint는 정수를 뽑고 uniform은 실수를 뽑음


```python
from scipy.stats import uniform, randint

rgen = randint(0, 10)
rgen.rvs(10)
```




    array([2, 7, 0, 3, 9, 2, 3, 7, 4, 4])




```python
np.unique(rgen.rvs(1000), return_counts = True)
```




    (array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9]),
     array([ 96,  93, 105,  93, 105,  97,  87, 105, 113, 106]))




```python
from sklearn.model_selection import RandomizedSearchCV

# p.254

params = {
    'min_impurity_decrease' : uniform(0.0001, 0.001), # 0.0001에서 0.001 사이의 실숫값을 샘플링
    'max_depth' : randint(20, 50), # 20에서 50 사이의 정수를 샘플링
    'min_samples_split' : randint(2, 25), # 2에서 25 사이의 정수를 샘플링
    'min_samples_leaf' : randint(1, 25), # 1에서 25 사이의 정수를 샘플링
}

# params에 정의된 매개변수 범위에서 총 100번(n_iter 매개변수)을 샘플링하여 교차 검증을 수행하고, 최적의 매개변수 조합을 찾음
gs = RandomizedSearchCV(DecisionTreeClassifier(random_state = 42), params,
                        n_iter = 100, n_jobs = -1, random_state = 42)

gs.fit(train_input, train_target)
```




    RandomizedSearchCV(estimator=DecisionTreeClassifier(random_state=42),
                       n_iter=100, n_jobs=-1,
                       param_distributions={'max_depth': <scipy.stats._distn_infrastructure.rv_frozen object at 0x7fed0d11b210>,
                                            'min_impurity_decrease': <scipy.stats._distn_infrastructure.rv_frozen object at 0x7fed0d11b2d0>,
                                            'min_samples_leaf': <scipy.stats._distn_infrastructure.rv_frozen object at 0x7fed0d14fe90>,
                                            'min_samples_split': <scipy.stats._distn_infrastructure.rv_frozen object at 0x7fed0d0f33d0>},
                       random_state=42)



- 최적의 매개변수 조합 출력


```python
gs.best_params_
```




    {'max_depth': 39,
     'min_impurity_decrease': 0.00034102546602601173,
     'min_samples_leaf': 7,
     'min_samples_split': 13}



# 마무리

## 키워드로 끝내는 핵심 포인트

- **검증 세트**는 하이퍼파라미터 튜닝을 위해 모델을 평가할 때, 테스트 세트를 사용하지 않기 위해 훈련 세트에서 다시 떼어 낸 데이터 세트입니다.
- **교차 검증**은 훈련 세트를 여러 폴드로 나눈 다음 한 폴드가 검증 세트의 역할을 하고 나머지 폴드에서는 모델을 훈련합니다. 교차 검증은 이런 식으로 모든 폴드에 대해 검증 점수를 얻어 평균하는 방법입니다.
- **그리드 서치**는 하이퍼파라미터 탐색을 자동화해 주는 도구입니다. 탐색할 매개변수를 나열하면 교차 검증을 수행하여 가장 좋은 검증 점수의 매개변수 조합을 선택합니다. 마지막으로 이 매개변수 조합으로 최종 모델을 훈련합니다.
- *랜덤 서치**는 연속된 매개변수 값을 탐색할 때 유용합니다. 탐색할 값을 직접 나열하는 것이 아니고 탐색 값을 샘플링할 수 있는 확률 분포 객체를 전달합니다. 지정된 횟수만큼 샘플링하여 교차 검증을 수행하기 때문에 시스템 자원이 허락하는 만큼 탐색량을 조절할 수 있습니다.

## 핵심 패키지와 함수

### scikit-learn

- **cross_validate()**는 교차 검증을 수행하는 함수힙니다
  + 첫 번째 매개변수에 교차 검증을 수행할 무델 객체를 전달합니다. 두 번째와 세 번째 매개변수에 특성과 타깃 데이터를 전달합니다.
  + scoring 매개변수에 검증에 사용할 평가 지표를 지정할 수 있습니다. 기본적으로 분류 모델은 정확도를 의미하는 'accuracy', 회귀 무델은 결정계수를 의미하는 'r2'가 됩니다.
  + cv 매개변수에 교차 검증 폴드 수나 스플리터 객체를 지정할 수 있습니다. 기본값은 5입니다. 회귀할 때는 KFold 클래스를 사용하고 분류일 때는 StratifiedKFold 클래스를 사용하여 5-폴드 교차 검증을 수행합니다
  + n_jobs 매개변수는 교차 검증을 수행할 때 사용할 CPU 코어 수를 지정합니다. 기본값은 1로 하나의 코어를 사용합니다. -1로 지정하면 시스템에 있는 모든 코어를 사용합니다.
  + return_train_score 매개변수를 True로 지정하면 훈련 세트의 점수도 반환합니다. 기본값은 False입니다.

- **GridSearchCV**는 교차 검증으로 하이퍼파라미터 탐색을 수행합니다. 최상의 무델을 찾은 후 훈련 세트 전체를 사용해 최종 모델을 훈련합니다.
  + 첫 번째 매개변수로 그리드 서치를 수행할 모델 객체를 전달합니다. 두 번째 매개변수에는 탐색할 모델의 매개변수와 값을 전달합니다.
  + scoring, cv, n_jobs, return_train_score 매개변수는 cross_validate() 함수와 동일합니다.

- **RandomizedSearchCV**는 교차 검증으로 랜덤한 하이퍼파라미터 탐색을 수행합니다. 최상의 무델을 찾은 후 훈련 세트 전체를 사용해 최종 모델을 훈련합니다.
  + 첫 번째 매개변수로 그리드 서치를 수행할 모델 객체를 전달합니다. 두 번째 매개변수에는 탐색할 모델의 매개변수와 확률 분포 객체를 전달합니다.
  + scoring, cv, n_jobs, return_train_score 매개변수는 cross_validate() 함수와 동일합니다.

# 트리의 앙상블

- LightGBM 기억
  + GBM --> XGBoost --> LightGBM
  + 참고 1. 모델 개발 속도가 빨라졌나
  + 참고 2. 모델의 성능이 좋아졌나
- TabNet (2019)
  + 딥러닝 컨셉 이해 필요

## 랜덤 포레스트

- 결정 트리 나무를 500개 심기
- 최종적인 결정은 투표 방식
  + 나무-1 : 양성
  + 나무-2 : 음성
  + 나무-3 : 양성
  + ...
  + 나무-500 : 양성
  + 합산해서 결과 도출

# 데이터 불러오기, 데이터 셋 분리


```python
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split

wine = pd.read_csv('https://bit.ly/wine_csv_data')

data = wine[['alcohol', 'sugar', 'pH']].to_numpy()
target = wine['class'].to_numpy()

train_input, test_input, train_target, test_target = train_test_split(data, 
                                                                      target, 
                                                                      test_size=0.2, 
                                                                      random_state=42)
```

- p. 267
  + cross_validate() 교차 검증 수행
  + RandomForestClassifier는 기본적으로 100개의 결정 트리를 사용하므로 n_jobs 매개변수를 -1로 지정하여 모든 CPU 코어를 사용하는 것이 좋음
  + return_train_score 매개변수를 True로 지정하면 검증 점수뿐만 아니라 훈련 세트에 대한 점수도 같이 반환
  + 훈련 세트와 검증 세트의 점수를 비교하면 과대적합을 파악하는데 용이함


```python
from sklearn.model_selection import cross_validate
from sklearn.ensemble import RandomForestClassifier

rf = RandomForestClassifier(n_jobs = -1, random_state = 42)
scores = cross_validate(rf, train_input, train_target,
                        return_train_score = True, n_jobs = -1)

print(np.mean(scores['train_score']), np.mean(scores['test_score']))
```

    0.9973541965122431 0.8905151032797809
    

- 과대적합
- 특성 중요도 출력


```python
rf.fit(train_input, train_target)
print(rf.feature_importances_)
```

    [0.23167441 0.50039841 0.26792718]
    

- 자체적으로 모델을 평가하는 점수를 얻을 수 있음
- 부트스트랩 샘플에 포함되지 않고 남는 샘플을 OOB 샘플이라 함
- OOB 샘플을 사용하여 부트스트랩 샘플로 훈련한 결정 트리를 평가할 수 있음
- oob_score 매개변수를 True로 지정하면 사용 가능


```python
rf = RandomForestClassifier(oob_score = True, n_jobs = -1, random_state = 42)
rf.fit(train_input, train_target)
print(rf.oob_score_)
```

    0.8934000384837406
    

# 그레이디언트 부스팅

- 이전 트리의 오차를 보완하는 방식으로 사용
- 깊이가 얕은 트리를 사용
- 학습을 매개변수로 속도를 조정
- 단점 : 속도가 느림


```python
from sklearn.ensemble import GradientBoostingClassifier
gb = GradientBoostingClassifier(random_state = 42)
scores = cross_validate(gb, train_input, train_target,
                        return_train_score = True, n_jobs = -1)

print(np.mean(scores['train_score']), np.mean(scores['test_score']))
```

    0.8881086892152563 0.8720430147331015
    


```python
gb = GradientBoostingClassifier(n_estimators = 500, learning_rate = 0.2, random_state = 42)
scores = cross_validate(gb, train_input, train_target,
                        return_train_score = True, n_jobs = -1)

print(np.mean(scores['train_score']), np.mean(scores['test_score']))
```

    0.9464595437171814 0.8780082549788999
    

- 흐름
  + 0. 데이터 전처리 / 시각화
  + 1. 기본 모형으로 전체 흐름을 설계
  + 2. 여러 모형으로 비교 대조
  + 3. 교차 검증, 하피어파라미터 성능 비교
  + ...
  + 1등하는 그날까지

# 히스토그램 기반 그레이디언트 부스팅

- 정형 데이터를 다루는 머신러닝 알고리즘 중 가장 인기가 높은 알고리즘
- 입력 특성을 256개 구간으로 나눔
  + 노드를 분할할 때 최적의 분할을 매우 빠르게 찾을 수 있음
- 256개의 구간 중에서 하나를 떼어 놓고 누락된 값을 위해서 사용
  + 입력에 누락된 특성이 있더라도 이를 따로 전처리할 필요가 없음



```python
from sklearn.experimental import enable_hist_gradient_boosting
from sklearn.ensemble import HistGradientBoostingClassifier
hgb = HistGradientBoostingClassifier(random_state = 42)
scores = cross_validate(hgb, train_input, train_target,
                        return_train_score = True)

print(np.mean(scores['train_score']), np.mean(scores['test_score']))
```

    0.9321723946453317 0.8801241948619236
    

- 특성 중요도 확인


```python
from sklearn.inspection import permutation_importance

hgb.fit(train_input, train_target)
result = permutation_importance(hgb, train_input, train_target,
                                n_repeats = 10, random_state = 42, n_jobs = -1)

print(result.importances_mean)
```

    [0.08876275 0.23438522 0.08027708]
    

- 테스트 세트의 특성 중요도 확인


```python
result = permutation_importance(hgb, test_input, test_target,
                                n_repeats = 10, random_state = 42, n_jobs = -1)

print(result.importances_mean)
```

    [0.05969231 0.20238462 0.049     ]
    

- 테스트 세트의 성능 확인


```python
hgb.score(test_input, test_target)
```




    0.8723076923076923



# XGBoost
- 그레디언트 부스팅 알고리즘을 구현한 라이브러리


```python
from xgboost import XGBClassifier
xgb = XGBClassifier(tree_method = 'hist', random_state = 42)
scores = cross_validate(xgb, train_input, train_target,
                        return_train_score = True)

print(np.mean(scores['train_score']), np.mean(scores['test_score']))
```

    0.8824322471423747 0.8726214185237284
    

# LightGBM


```python
from lightgbm import LGBMClassifier
lgb = LGBMClassifier(random_state = 42)
scores = cross_validate(lgb, train_input, train_target,
                        return_train_score = True, n_jobs = -1)

print(np.mean(scores['train_score']), np.mean(scores['test_score']))
```

    0.9338079582727165 0.8789710890649293
    

# 마무리

## 키워드로 끝내는 핵심 포인트

- **앙상블 학습**은 더 좋은 예측 결과를 만들기 위해 여러 개의 모델을 훈련하는 머신러닝 알고리즘을 말합니다.
- **랜덤 포레스트**는 대표적인 결정 트리 기반의 앙상블 학습 방법입니다. 부트스트랩 샘플을 사용하고 랜덤하게 일부 특성을 선택하여 트리를 만드는 것이 특징입니다.
- **엑스트라 트리**는 랜덤 포레스트와 비슷하게 결정 트리를 사용하여 앙상블 모델을 만들지만 부트스트랩 샘플을 사용하지 않습니다. 대신 랜덤하게 노드를 분할해 과대적합을 감소시킵니다.
- **그레이디언트 부스팅**은 랜덤 포레스트나 엑스트라 트리와 달리 결정 트리를 연속적으로 추가하여 손실 함수를 최소화하는 앙상블 방법입니다. 이런 이유로 훈련 속도가 조금 느리지만 더 좋은 성능을 기대할 수 있습니다. 그레이디언트 부스팅의 속도를 개선한 것이 **히스토그램 기반 그레이디언트 부스팅**이며 안정적인 결과와 높은 성능으로 매우 인기가 높습니다

## 핵심 패키지와 함수

### scikit-learn

- **RandomForestClassifier**는 랜덤 포레스트 분류 클래스입니다.
  + n_estimators 매개변수는 앙상블을 구성할 트리의 개수를 지정합니다. 기본값은 100입니다.
  + criterion 매개변수는 불순도를 지정하며 기본값은 지니 불순도를 의미하는 'gini'이고 'entropy'를 선택하여 엔트로피 불순도를 사용할 수 있습니다.
  + max_depth는 트리가 성장할 최대 깊이를 지정합니다. 기본값은 None으로 지정하면 리프노드가 순수하거나 min_samples_split보다 샘플 개수가 적을 때까지 성장합니다.
  + min_samples_split은 노드를 나누기 위한 최소 샘플의 개수입니다. 기본값은 2입니다.
  + max_features 매개변수는 최적의 분할을 위해 탐색할 특성의 개수를 지정합니다. 기본값은 auto로 특성 개수의 제곱근입니다.
  + bootstrap 매개변수는 부트스트랩 샘플을 사용할지 지정합니다. 기본값은 True입니다.
  + oob_score는 OOB 샘플을 사용하여 훈련한 모델을 평가할지 지정합니다. 기본값은 False입니다.
  + n_jobs 매개변수는 병렬 실행에 사용할 CPU 코어 수를 지정합니다. 기본값은 1로 하나의 코어를 사용합니다. -1로 지정하면 시스템에 있는 모든 코어를 사용합니다.

- **ExtraTreesClassifier**는 엑스트라 트리 분류 클래스입니다.
  + n_estimators, criterion, max_depth, min_samples_split, max_features 매개변수는 랜덤 포레스트와 동일합니다.
  + bootstrap 매개변수는 부트스트랩 샘플을 사용할지 지정합니다. 기본값은 False입니다.
  + oob_score는 OOB 샘플을 사용하여 훈련한 모델을 평가할지 지정합니다. 기본값은 False입니다.
  + n_jobs 매개변수는 병렬 실행에 사용할 CPU 코어 수를 지정합니다. 기본값은 1로 하나의 코어를 사용합니다. -1로 지정하면 시스템에 있는 모든 코어를 사용합니다.

- **GradientBoostingClassifier**는 그레이디언트 부스팅 분류 클래스입니다.
  + loss 매개변수는 손실 함수를 지정합니다. 기본값은 로지스틱 손실 함수를 의미하는'deviance'입니다.
  + learning_rate 매개변수는 트리가 앙상블에 기여하는 정도를 조절합니다. 기본값은 0.1입니다.
  + n_estimators 매개변수는 부스팅 단계를 수행하는 트리의 개수입니다. 기본값은 100입니다.
  + subsample 매개변수는 사용할 훈련 세트의 샘플 비율을 지정합니다. 기본값은 1.0입니다.
  + max_depth 매개변수는 개별 회귀 트리의 최대 깊이입니다. 기본값은 3입니다.

- **HistGradientBoostingClassifier**는 히스토그램 기반 그레이디언트 부스팅 분류 클래스입니다.
  + learning_rate 매개변수는 학습률 또는 감쇠율이라고 합니다. 기본값은 0.1이며 1.0이면 감쇠가 전혀 없습니다.
  + max_iter는 부스팅 단계를 수행하는 트리의 개수입니다. 기본값은 100입니다.
  + max_bins는 입력 데이터를 나눌 구간의 개수입니다. 기본값은 255이며 이보다 크게 지정할 수 없습니다. 여기에 1개의 구간이 누락된 값을 위해 추가됩니다.
