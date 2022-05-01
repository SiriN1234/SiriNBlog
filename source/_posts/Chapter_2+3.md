---
title: "Chapter 2장과 3장"
author: "Hanjeongin"
date: 2022-03-28 23:00:00
tags: Python
---

출처 : 혼자 공부하는 머신러닝+딥러닝

# Chapter 2-1

# 지도학습과 비지도학습

- 지도학습 : 경진대회 유형
- 입력과 타깃 : 독립변수(입력), 종속변수(타깃)
- 정답이 있는 문제
  - 1 유형 : 타이타닉 생존자 분류 : Survived (타깃)
  - 2 유형 : 카페 예상매출액 : 숫자를 예측
- 특성(Feature) : 독립변수 (엑셀의 컬럼)
- 비지도 학습 : 뉴스기사 종류를 분류
  + 기사1 : 사회, 의학, ...
  + 기사2 : 사회, 경제, ...



```python
fish_length = [25.4, 26.3, 26.5, 29.0, 29.0, 29.7, 29.7, 30.0, 30.0, 30.7, 31.0, 31.0, 
                31.5, 32.0, 32.0, 32.0, 33.0, 33.0, 33.5, 33.5, 34.0, 34.0, 34.5, 35.0, 
                35.0, 35.0, 35.0, 36.0, 36.0, 37.0, 38.5, 38.5, 39.5, 41.0, 41.0, 9.8, 
                10.5, 10.6, 11.0, 11.2, 11.3, 11.8, 11.8, 12.0, 12.2, 12.4, 13.0, 14.3, 15.0]
                
fish_weight = [242.0, 290.0, 340.0, 363.0, 430.0, 450.0, 500.0, 390.0, 450.0, 500.0, 475.0, 500.0, 
                500.0, 340.0, 600.0, 600.0, 700.0, 700.0, 610.0, 650.0, 575.0, 685.0, 620.0, 680.0, 
                700.0, 725.0, 720.0, 714.0, 850.0, 1000.0, 920.0, 955.0, 925.0, 975.0, 950.0, 6.7, 
                7.5, 7.0, 9.7, 9.8, 8.7, 10.0, 9.9, 9.8, 12.2, 13.4, 12.2, 19.7, 19.9]
```


```python
fish_data = [[l, w] for l,w in zip(fish_length, fish_weight)]
fish_target = [1] * 35 + [0] * 14
```

- 머신러닝 모델


```python
from sklearn.neighbors import KNeighborsClassifier
kn = KNeighborsClassifier()
```

# 훈련세트와 세트 세트로 분리


```python
# 0부터 34 인덱스까지 사용
# 훈련데이터 독립변수
train_input = fish_data[:35]

# 훈련데이터 종속변수
train_target = fish_target[:35]

# 테스트데이터 독립변수
test_input = fish_data[35:]

# 테스트데이터 종속변수
test_target = fish_target[35:]

# train_input.shape, train_target.shape, test_input.shape, test_target.shape
```


```python
kn = kn.fit(train_input, train_target)
kn.score(test_input, test_target)
```




    0.0




```python
fish_data[34:]
```




    [[41.0, 950.0],
     [9.8, 6.7],
     [10.5, 7.5],
     [10.6, 7.0],
     [11.0, 9.7],
     [11.2, 9.8],
     [11.3, 8.7],
     [11.8, 10.0],
     [11.8, 9.9],
     [12.0, 9.8],
     [12.2, 12.2],
     [12.4, 13.4],
     [13.0, 12.2],
     [14.3, 19.7],
     [15.0, 19.9]]




```python
fish_target[34:]
```




    [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]



# 샘플링 편향

- 훈련 세트와 테스트 세트에 샘플이 골고루 섞이지 않은 현상

# 넘파이

- 리스트로 연산은 지원이 잘 안됨
- 리스트를 넘파이 배열로 변환


```python
import numpy as np

input_arr = np.array(fish_data)
target_arr = np.array(fish_target)

input_arr.shape, target_arr.shape
```




    ((49, 2), (49,))



- shuffle : 데이터를 섞어준다
- 실험 재현성


```python
np.random.seed(42)
index = np.arange(49)
index
```




    array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16,
           17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33,
           34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48])




```python
np.random.shuffle(index)
print(index)
```

    [13 45 47 44 17 27 26 25 31 19 12  4 34  8  3  6 40 41 46 15  9 16 24 33
     30  0 43 32  5 29 11 36  1 21  2 37 35 23 39 10 22 18 48 20  7 42 14 28
     38]
    

- 훈련 데이터 및 테스트 데이터 재 코딩


```python
train_input = input_arr[index[:35]]
train_target = target_arr[index[:35]]

print(input_arr[13], train_input[0])
```

    [ 32. 340.] [ 32. 340.]
    


```python
test_input = input_arr[index[35:]]
test_target = target_arr[index[35:]]
```


```python
train_input.shape, train_target.shape, test_input.shape, test_target.shape
```




    ((35, 2), (35,), (14, 2), (14,))



# 시각화 생략


```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots()

ax.scatter(train_input[:, 0], train_input[:, 1])
ax.scatter(test_input[:, 0], test_input[:, 1])
ax.set_xlabel('length')
ax.set_ylabel('weight')

plt.show()
```


    
![png](/images/Chapter_2+3/output_22_0.png)
    


# 두 번째 머신러닝 프로그램


```python
kn = kn.fit(train_input, train_target)

kn.score(test_input, test_target)
```




    1.0




```python
kn.predict(test_input)
```




    array([0, 0, 1, 0, 1, 1, 1, 0, 1, 1, 0, 1, 1, 0])




```python
test_target
```




    array([0, 0, 1, 0, 1, 1, 1, 0, 1, 1, 0, 1, 1, 0])



# Chapter 2-2

# 데이터 전처리

## 넘파이로 데이터 준비하기

- 도미와 빙어 데이터 준비


```python
fish_length = [25.4, 26.3, 26.5, 29.0, 29.0, 29.7, 29.7, 30.0, 30.0, 30.7, 31.0, 31.0, 
                31.5, 32.0, 32.0, 32.0, 33.0, 33.0, 33.5, 33.5, 34.0, 34.0, 34.5, 35.0, 
                35.0, 35.0, 35.0, 36.0, 36.0, 37.0, 38.5, 38.5, 39.5, 41.0, 41.0, 9.8, 
                10.5, 10.6, 11.0, 11.2, 11.3, 11.8, 11.8, 12.0, 12.2, 12.4, 13.0, 14.3, 15.0]
                
fish_weight = [242.0, 290.0, 340.0, 363.0, 430.0, 450.0, 500.0, 390.0, 450.0, 500.0, 475.0, 500.0, 
                500.0, 340.0, 600.0, 600.0, 700.0, 700.0, 610.0, 650.0, 575.0, 685.0, 620.0, 680.0, 
                700.0, 725.0, 720.0, 714.0, 850.0, 1000.0, 920.0, 955.0, 925.0, 975.0, 950.0, 6.7, 
                7.5, 7.0, 9.7, 9.8, 8.7, 10.0, 9.9, 9.8, 12.2, 13.4, 12.2, 19.7, 19.9]
```

- 넘파이 임포트


```python
import numpy as np
```

- 넘파이의 column_stack() 함수는 전달받은 리스트를 일렬로 세운 다음 차례대로 나란히 연결함
- 2개의 리스트를 붙여보자
- 연결할 리스트는 파이썬 튜플로 전달함


```python
np.column_stack(([1,2,3], [4,5,6,]))
```




    array([[1, 4],
           [2, 5],
           [3, 6]])



- 위와같이 fish_length와 fish_weight를 합침


```python
fish_data = np.column_stack((fish_length, fish_weight))

print(fish_data[:5])
```

    [[ 25.4 242. ]
     [ 26.3 290. ]
     [ 26.5 340. ]
     [ 29.  363. ]
     [ 29.  430. ]]
    

- np.ones()와 np.zeros() 함수를 이용해 타깃 데이터를 만들어보자


```python
fish_target = np.concatenate((np.ones(35), np.zeros(14)))

print(fish_target)
```

    [1. 1. 1. 1. 1. 1. 1. 1. 1. 1. 1. 1. 1. 1. 1. 1. 1. 1. 1. 1. 1. 1. 1. 1.
     1. 1. 1. 1. 1. 1. 1. 1. 1. 1. 1. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0.
     0.]
    

## 사이킷런으로 훈련 세트와 테스트 세트 나누기

- train_test_split()함수는 전달되는 리스트나 배열을 비율에 맞게 훈련세트와 테스트 세트로 나누어주고 섞어 줌
- 이 함수는 기본적으로 25%를 테스트 세트로 떼어 냄


```python
from sklearn.model_selection import train_test_split

train_input, test_input, train_target, test_target = train_test_split(fish_data, fish_target, random_state = 42)

print(train_input.shape, test_input.shape, train_target.shape, test_target.shape)
```

    (36, 2) (13, 2) (36,) (13,)
    

- 훈련 데이터와 테스트 데이터를 각각 36개와 13개로 나눔
- 이제 데이터 출력


```python
print(test_target)
```

    [1. 0. 0. 0. 1. 1. 1. 1. 1. 1. 1. 1. 1.]
    

- 샘플링 편향이 나타남
- stratify 매개변수에 타깃 데이터를 전달하면 클래스 비율에 맞게 데이터를 나눔


```python
train_input, test_input, train_target, test_target = train_test_split(
    fish_data, fish_target, stratify = fish_target, random_state = 42)

print(test_target)
```

    [0. 0. 1. 0. 1. 0. 1. 1. 1. 1. 1. 1. 1.]
    

## 수상한 도미 한 마리


```python
from sklearn.neighbors import KNeighborsClassifier
kn = KNeighborsClassifier()
kn.fit(train_input, train_target)
kn.score(test_input, test_target)
```




    1.0



- 테스트 데이터 넣기


```python
print(kn.predict([[25, 150]]))
```

    [0.]
    

- 시각화


```python
"""
import matplotlib.pyplot as plt
plt.scatter(train_input[:, 0], train_input[:, 1])
plt.scatter(25, 150, marker = '^') # marker 매개변수는 모양을 지정
plt.xlabel('length')
plt.ylabel('weight')
plt.show()
"""
```




    "\nimport matplotlib.pyplot as plt\nplt.scatter(train_input[:, 0], train_input[:, 1])\nplt.scatter(25, 150, marker = '^') # marker 매개변수는 모양을 지정\nplt.xlabel('length')\nplt.ylabel('weight')\nplt.show()\n"




```python
fig, ax = plt.subplots()
ax.scatter(train_input[:, 0], train_input[:, 1])
ax.scatter(25, 150, marker = '^') # marker 매개변수는 모양을 지정
ax.set_xlabel('length')
ax.set_ylabel('weight')
plt.show()
```


    
![png](/images/Chapter_2+3/output_50_0.png)
    



```python
distances, indexes = kn.kneighbors([[25, 150]])
```

- marker = 'D'로 지정하면 산점도를 마름모로 그림
- 삼각형 샘플에 가장 가까운 5개의 샘플을 초록 다이아몬드로 표시


```python
"""
plt.scatter(train_input[:, 0], train_input[:, 1])
plt.scatter(25, 150, marker = '^')
plt.scatter(train_input[indexes, 0], train_input[indexes, 1], marker = 'D')
plt.xlabel('length')
plt.ylabel('weight')
plt.show()
"""
```




    "\nplt.scatter(train_input[:, 0], train_input[:, 1])\nplt.scatter(25, 150, marker = '^')\nplt.scatter(train_input[indexes, 0], train_input[indexes, 1], marker = 'D')\nplt.xlabel('length')\nplt.ylabel('weight')\nplt.show()\n"




```python
fig, ax = plt.subplots()
ax.scatter(train_input[:, 0], train_input[:, 1])
ax.scatter(25, 150, marker = '^')
ax.scatter(train_input[indexes, 0], train_input[indexes, 1], marker = 'D')
ax.set_xlabel('length')
ax.set_ylabel('weight')
plt.show()
```


    
![png](/images/Chapter_2+3/output_54_0.png)
    



```python
print(train_input[indexes])
```

    [[[ 25.4 242. ]
      [ 15.   19.9]
      [ 14.3  19.7]
      [ 13.   12.2]
      [ 12.2  12.2]]]
    


```python
print(train_target[indexes])
```

    [[1. 0. 0. 0. 0.]]
    

- distances 배열에는 이웃 샘플까지의 거리가 담겨있음


```python
print(distances)
```

    [[ 92.00086956 130.48375378 130.73859415 138.32150953 138.39320793]]
    

## 기준을 맞춰라

- x축은 범위가 좁고, y축은 범위가 넓음
- 범위를 동일하게 맞춰줌
- xlim() 사용


```python
"""
plt.scatter(train_input[:, 0], train_input[:, 1])
plt.scatter(25, 150, marker = '^')
plt.scatter(train_input[indexes, 0], train_input[indexes, 1], marker = 'D')
plt.xlim((0, 1000))
plt.xlabel('length')
plt.ylabel('weight')
plt.show()
"""
```




    "\nplt.scatter(train_input[:, 0], train_input[:, 1])\nplt.scatter(25, 150, marker = '^')\nplt.scatter(train_input[indexes, 0], train_input[indexes, 1], marker = 'D')\nplt.xlim((0, 1000))\nplt.xlabel('length')\nplt.ylabel('weight')\nplt.show()\n"




```python
fig, ax = plt.subplots()
ax.scatter(train_input[:, 0], train_input[:, 1])
ax.scatter(25, 150, marker = '^')
ax.scatter(train_input[indexes, 0], train_input[indexes, 1], marker = 'D')
ax.set_xlim((0, 1000))
ax.set_xlabel('length')
ax.set_ylabel('weight')
plt.show()
```


    
![png](/images/Chapter_2+3/output_61_0.png)
    


- 두 특성의 값이 놓인 범위가 매우 다름
- 이를 두 특성의 스케일이 다르다고 말함
- 특성값을 일정한 기준으로 맞춰 주어야 예측이 가능
- 이것을 데이터 전처리라 부름
- 가장 널리 사용하는 전처리 방법 중 하나는 표준점수


```python
mean = np.mean(train_input, axis = 0)
std = np.std(train_input, axis = 0)

print(mean, std)
```

    [ 27.29722222 454.09722222] [  9.98244253 323.29893931]
    

- 원본 데이터에서 평균을 빼고 표준편차로 나누어 표준점수로 변환
- 이런 넘파이 기능을 브로드캐스팅이라 부름


```python
train_scaled = (train_input - mean) / std

print(train_scaled)
```

    [[ 0.24070039  0.14198246]
     [-1.51237757 -1.36683783]
     [ 0.5712808   0.76060496]
     [-1.60253587 -1.37766373]
     [ 1.22242404  1.45655528]
     [ 0.17057727 -0.07453542]
     [ 0.87180845  0.80390854]
     [ 0.87180845  1.22457184]
     [ 0.37092904  0.06465464]
     [ 0.77163257  0.82246721]
     [ 0.97198434  1.68853872]
     [-1.61255346 -1.3742613 ]
     [ 0.72154463  0.51315596]
     [-1.53241275 -1.3742613 ]
     [ 0.17057727 -0.28177396]
     [ 0.5712808   0.76060496]
     [ 0.34087627  0.14198246]
     [ 1.12224816  1.54934866]
     [ 0.62136874  0.60594934]
     [-1.30200822 -1.34363949]
     [ 0.42101698  0.14198246]
     [-0.19005591 -0.65604058]
     [-1.75279969 -1.38384995]
     [ 0.47110492  0.45129371]
     [-1.68267658 -1.38137546]
     [ 0.62136874  0.48222484]
     [-1.67265899 -1.38292202]
     [ 0.77163257  0.76060496]
     [ 0.47110492  0.45129371]
     [ 0.77163257  0.83793278]
     [-1.43223687 -1.36683783]
     [ 0.27075315 -0.01267317]
     [ 0.47110492 -0.35291555]
     [-1.2318851  -1.34302087]
     [ 0.27075315 -0.19825992]
     [ 1.37268787  1.61121091]]
    

## 전처리 데이터로 모델 훈련하기


```python
"""
plt.scatter(train_scaled[:, 0], train_scaled[:, 1])
plt.scatter(25, 150, marker = '^')
plt.xlabel('length')
plt.ylabel('weight')
plt.show()
"""
```




    "\nplt.scatter(train_scaled[:, 0], train_scaled[:, 1])\nplt.scatter(25, 150, marker = '^')\nplt.xlabel('length')\nplt.ylabel('weight')\nplt.show()\n"




```python
fig, ax = plt.subplots()
ax.scatter(train_scaled[:, 0], train_scaled[:, 1])
ax.scatter(25, 150, marker = '^')
ax.set_xlabel('length')
ax.set_ylabel('weight')
plt.show()
```


    
![png](/images/Chapter_2+3/output_68_0.png)
    


- 기준을 맞춰주고 다시 그리기


```python
"""
new = ([25, 150] - mean) / std
plt.scatter(train_scaled[:, 0], train_scaled[:, 1])
plt.scatter(new[0], new[1], marker = '^')
plt.xlabel('length')
plt.ylabel('weight')
plt.show()
"""
```




    "\nnew = ([25, 150] - mean) / std\nplt.scatter(train_scaled[:, 0], train_scaled[:, 1])\nplt.scatter(new[0], new[1], marker = '^')\nplt.xlabel('length')\nplt.ylabel('weight')\nplt.show()\n"




```python
new = ([25, 150] - mean) / std

fig, ax = plt.subplots()
ax.scatter(train_scaled[:, 0], train_scaled[:, 1])
ax.scatter(new[0], new[1], marker = '^')
ax.set_xlabel('length')
ax.set_ylabel('weight')
plt.show()
```


    
![png](/images/Chapter_2+3/output_71_0.png)
    


- k-최근접 이웃 모델을 다시 훈련


```python
kn.fit(train_scaled, train_target)
test_scaled = (test_input - mean) / std
kn.score(test_scaled, test_target)
```




    1.0



- 새로운 샘플을 이용헤 모델의 예측 출력


```python
print(kn.predict([new]))
```

    [1.]
    

- 산점도 그리기


```python
"""
distances, indexes = kn.kneighbors([new])
plt.scatter(train_scaled[:, 0], train_scaled[:, 1])
plt.scatter(new[0], new[1], marker = '^')
plt.scatter(train_scaled[indexes, 0], train_scaled[indexes, 1], marker = 'D')
plt.xlabel('length')
plt.ylabel('weight')
plt.show()
"""
```




    "\ndistances, indexes = kn.kneighbors([new])\nplt.scatter(train_scaled[:, 0], train_scaled[:, 1])\nplt.scatter(new[0], new[1], marker = '^')\nplt.scatter(train_scaled[indexes, 0], train_scaled[indexes, 1], marker = 'D')\nplt.xlabel('length')\nplt.ylabel('weight')\nplt.show()\n"




```python
distances, indexes = kn.kneighbors([new])

fig, ax = plt.subplots()
ax.scatter(train_scaled[:, 0], train_scaled[:, 1])
ax.scatter(new[0], new[1], marker = '^')
ax.scatter(train_scaled[indexes, 0], train_scaled[indexes, 1], marker = 'D')
ax.set_xlabel('length')
ax.set_ylabel('weight')
plt.show()
```


    
![png](/images/Chapter_2+3/output_78_0.png)
    


# 마무리

## 키워드로 끝나는 핵심 포인트

- **데이터 전처리**는 머신러닝 모델에 훈련 데이터를 주입하기 전에 가공하는 단계를 말함. 때로는 데이터 전처리에 많은 시간이 소모되기도 함.
- **표준점수**는 훈련 세트의 스케일을 바꾸는 대표적인 방법 중 하나. 표준점수를 얻으려면 특성의 평균을 빼고 표준편차로 나눔. 반드시 훈련 세트와 평균과 표준편차로 테스트 세트를 바꿔야 함.
- **브로드캐스팅**은 크기가 다른 넘파이 배열에서 자동으로 사칙 연산을 모든 행이나 열로 확장하여 수행하는 기능.

## 핵심 패키지와 함수

### scikit-learn

- **train_test_split()**은 훈련 데이터를 훈련 세트와 테스트 세트로 나누는 함수. 여러개의 배열을 전달할 수 있음. 테스트 세트로 나눌 비율은 test_size 매개변수에서 지정할 수 있으며 기본값은 0.25(25%)임.
- **kneighbors()**는 k-최근접 이웃 객체의 메서드. 이 메서드는 입력한 데이터에 가장 가까운 이웃을 찾아 거리와 이웃 샘플의 인덱스를 반환. 기본적으로 이웃의 개수는 KNeighborsClassifier 클래스의 객체를 생성할 때 지정한 개수를 사용함. 하지만 n_neighbors 매개변수에서 다르게 지정할 수도 있음
- return_distance 매개변수를 False로 지정하면 이웃 샘플의 인덱스만 반환하고 거리는 반환하지 않음. 이 매개변수의 기본값은 True

# 확인문제

## 1. 이 방식은 스케일 조정방식의 하나로 특성값을 0에서 표준편차의 몇 배수만큼 떨어져 있는지로 변환한 값입니다. 이 값을 무엇이라 부르나요?

- 1. 기본점수
- 2. 원점수
- 3. 표준점수
- 4. 사분위수

### 답 : 3번

## 2. 테스트 세트의 스케일을 조정하려고 합니다. 다음 중 어떤 데이터의 통계 값을 사용해야 하나요?

- 1. 훈련세트
- 2. 테스트 세트
- 3. 전체 데이터

### 답 : 1번

# Chapter 3-1

# 데이터 준비


```python
import numpy as np

perch_length = np.array(
    [8.4, 13.7, 15.0, 16.2, 17.4, 18.0, 18.7, 19.0, 19.6, 20.0, 
     21.0, 21.0, 21.0, 21.3, 22.0, 22.0, 22.0, 22.0, 22.0, 22.5, 
     22.5, 22.7, 23.0, 23.5, 24.0, 24.0, 24.6, 25.0, 25.6, 26.5, 
     27.3, 27.5, 27.5, 27.5, 28.0, 28.7, 30.0, 32.8, 34.5, 35.0, 
     36.5, 36.0, 37.0, 37.0, 39.0, 39.0, 39.0, 40.0, 40.0, 40.0, 
     40.0, 42.0, 43.0, 43.0, 43.5, 44.0]
     )
perch_weight = np.array(
    [5.9, 32.0, 40.0, 51.5, 70.0, 100.0, 78.0, 80.0, 85.0, 85.0, 
     110.0, 115.0, 125.0, 130.0, 120.0, 120.0, 130.0, 135.0, 110.0, 
     130.0, 150.0, 145.0, 150.0, 170.0, 225.0, 145.0, 188.0, 180.0, 
     197.0, 218.0, 300.0, 260.0, 265.0, 250.0, 250.0, 300.0, 320.0, 
     514.0, 556.0, 840.0, 685.0, 700.0, 700.0, 690.0, 900.0, 650.0, 
     820.0, 850.0, 900.0, 1015.0, 820.0, 1100.0, 1000.0, 1100.0, 
     1000.0, 1000.0]
     )
```

# K-최근접 이웃 회귀(Regression)
- 중요도 : 하 (그냥 넘어가도 됨)

# 시각화


```python
import matplotlib.pyplot as plt

# 객체 지향으로 변경
fig, ax = plt.subplots()
ax.scatter(perch_length, perch_weight)
ax.set_xlabel("length")
ax.set_ylabel("weight")
plt.show()
```


    
![png](/images/Chapter_2+3/output_86_0.png)
    


# 훈련데이터와 테스트데이터셋 분리


```python
from sklearn.model_selection import train_test_split

train_input, test_input, train_target, test_target = train_test_split(
    perch_length, perch_weight, random_state = 42
)

train_input.shape, test_input.shape, train_target.shape, test_target.shape
```




    ((42,), (14,), (42,), (14,))



- reshape() 사용하여 2차원 배열로 변환


```python
train_input = train_input.reshape(-1, 1)
test_input = test_input.reshape(-1, 1)

print(train_input.shape, test_input.shape)
```

    (42, 1) (14, 1)
    

# 결정계수

- 모델이 얼마만큼 정확하냐?
  + 99.2%
- 절댓값은 아님 / 상대적인 값


```python
from sklearn.neighbors import KNeighborsRegressor

# knr 클래스 불러오기
knr = KNeighborsRegressor()

# 모형학습
knr.fit(train_input, train_target)

# 테스트 점수 확인하기
knr.score(test_input, test_target)
```




    0.992809406101064



# MAE
- 타깃과 예측의 절댓값 오차를 평균하여 반환


```python
from sklearn.metrics import mean_absolute_error

# 예측 데이터 만들기
test_prediction = knr.predict(test_input)

test_prediction
```




    array([  60. ,   79.6,  248. ,  122. ,  136. ,  847. ,  311.4,  183.4,
            847. ,  113. , 1010. ,   60. ,  248. ,  248. ])



- mae를 구한다


```python
mae = mean_absolute_error(test_target, test_prediction)

print(mae)
```

    19.157142857142862
    

- 평균적으로 19g정도 다르다

# 과대적합 vs 과소적합
- 공통점은 머신러닝 모형이 실제 테스트 시 잘 예측을 못함
- 과대적합 : 훈련데이터에는 예측 잘함 / 테스트데이터에서는 예측을 잘 못함
  + 처리하기 곤란
- 과소적합 : 훈련데이터에서는 예측을 못하고, 테스트데이터에서는 예측을 잘 함 or 둘 다 예측을 잘 못함
  + 데이터의 양이 적거나, 모델을 너무 간단하게 만듦



```python
# 훈련 데이터 점수 확인하자

print(knr.score(train_input, train_target))
```

    0.9698823289099254
    

- 0.97정도 나옴


```python
# Default 5를 3으로
# 머신러닝 모형을 살짝 변형
knr.n_neighbors = 3

# 모델을 다시 훈련
knr.fit(train_input, train_target)
print(knr.score(train_input, train_target))
```

    0.9804899950518966
    

- 훈련데이터로 검증 0.98


```python
print(knr.score(test_input, test_target))
```

    0.9746459963987609
    

- MAE 구하기


```python
# 예측 데이터 만들기
test_prediction = knr.predict(test_input)
mae = mean_absolute_error(test_target, test_prediction)
print(mae)
```

    35.42380952380951
    

- 평균적으로 35.4g 다름

# 결론

- k 그룹을 5로 했을 때, R2 점수는 0.98, MAE는 19였음
- k 그룹을 3으로 했을 때, R2 점수는 0.97, MAE는 35였음
- k 그룹을 7으로 했을 때, R2 점수는 0.97, MAE는 32였음

# Chapter 3-2

# 데이터 불러오기


```python
import numpy as np

perch_length = np.array(
    [8.4, 13.7, 15.0, 16.2, 17.4, 18.0, 18.7, 19.0, 19.6, 20.0, 
     21.0, 21.0, 21.0, 21.3, 22.0, 22.0, 22.0, 22.0, 22.0, 22.5, 
     22.5, 22.7, 23.0, 23.5, 24.0, 24.0, 24.6, 25.0, 25.6, 26.5, 
     27.3, 27.5, 27.5, 27.5, 28.0, 28.7, 30.0, 32.8, 34.5, 35.0, 
     36.5, 36.0, 37.0, 37.0, 39.0, 39.0, 39.0, 40.0, 40.0, 40.0, 
     40.0, 42.0, 43.0, 43.0, 43.5, 44.0]
     )
perch_weight = np.array(
    [5.9, 32.0, 40.0, 51.5, 70.0, 100.0, 78.0, 80.0, 85.0, 85.0, 
     110.0, 115.0, 125.0, 130.0, 120.0, 120.0, 130.0, 135.0, 110.0, 
     130.0, 150.0, 145.0, 150.0, 170.0, 225.0, 145.0, 188.0, 180.0, 
     197.0, 218.0, 300.0, 260.0, 265.0, 250.0, 250.0, 300.0, 320.0, 
     514.0, 556.0, 840.0, 685.0, 700.0, 700.0, 690.0, 900.0, 650.0, 
     820.0, 850.0, 900.0, 1015.0, 820.0, 1100.0, 1000.0, 1100.0, 
     1000.0, 1000.0]
     )
```

# 훈련 세트와 테스트 세트 분리


```python
from sklearn.model_selection import train_test_split

train_input, test_input, train_target, test_target = train_test_split(
    perch_length, perch_weight, random_state = 42
)

train_input.shape, test_input.shape, train_target.shape, test_target.shape
```




    ((42,), (14,), (42,), (14,))




```python
train_input = train_input.reshape(-1, 1)
test_input = test_input.reshape(-1, 1)

print(train_input.shape, test_input.shape)
```

    (42, 1) (14, 1)
    

# 모델 만들기


```python
from sklearn.neighbors import KNeighborsRegressor

# knr 클래스 불러오기
knr = KNeighborsRegressor(n_neighbors = 3)

# 모형학습
knr.fit(train_input, train_target)
```




    KNeighborsRegressor(n_neighbors=3)



# 예측
- p132


```python
print(knr.predict([[50]]))
```

    [1033.33333333]
    

# 시각화


```python
import matplotlib.pyplot as plt

# 50cm 농어의 이웃을 구함
distances, indexes = knr.kneighbors([[50]])

# 훈련 세트의 산점도를 그린다
fig, ax = plt.subplots()
ax.scatter(train_input, train_target)

# 훈련 세트 중에서 이웃 샘플만 다시 그림
ax.scatter(train_input[indexes], train_target[indexes], marker = 'D')

# 50cm 농어 데이터
ax.scatter(50, 1033, marker = '^')
ax.set_xlabel('length')
ax.set_ylabel('weight')
plt.show()
```


    
![png](/images/Chapter_2+3/output_119_0.png)
    


- 머신러닝 모델은 주기적으로 훈련해야 함
  + MLOps (Machine Learning & Operations)
  + 최근에 각광받는 데이터 관련 직업 필수 스킬
  + 입사와 함께 공부 시작 (데이터 분석가, 머신러닝 엔지니어, 데이터 싸이언티스트 희망자)

# 선형회귀 (통계)

- 5가지 가정들...
- 잔차의 정규성
- 등분산성, 다중공선성 등등...
- 종속변수 ~ 독립변수간의 **인과관계**를 찾는 과정

# 선형회귀 (머신러닝)

- 평가지표 확인이 더 중요
- R2 점수, MAE, MSE 등등...


```python
from sklearn.linear_model import LinearRegression

lr = LinearRegression()

# 선형 회귀 모델 훈련
lr.fit(train_input, train_target)

# 50cm 농어 예측
print(lr.predict([[50]]))
```

    [1241.83860323]
    


```python
fig, ax = plt.subplots()

ax.scatter(train_input, train_target)
ax.scatter(train_input[indexes], train_target[indexes], marker = 'D')

ax.scatter(50, 1241, marker = '^')
ax.set_xlabel('length')
ax.set_ylabel('weight')
plt.show()
```


    
![png](/images/Chapter_2+3/output_124_0.png)
    


# 회귀식을 찾기


```python
# 기울기 + 상수
print(lr.coef_, lr.intercept_)
```

    [39.01714496] -709.0186449535477
    

- **기울기** : 계수 = 가중치(딥러닝에서 많이 씀)


```python
fig, ax = plt.subplots()
ax.scatter(train_input, train_target)

# 15~50까지의 1차 방정식 그래프를 그린다
ax.plot([15, 50], [15 * lr.coef_ + lr.intercept_,
                   50 * lr.coef_ + lr.intercept_])

# 50cm 농어 데이터
ax.scatter(50, 1241.8, marker = '^')

plt.show()
```


    
![png](/images/Chapter_2+3/output_128_0.png)
    


- 모형 평가 (p. 138)
  + 과소적합

# 다항회귀의 필요성
- 치어를 생각해보자
- 치어가 1cm
- 무게가 음수가 나오기 때문에 말이 안됨
- 모형이 불안정함


```python
print(lr.predict([[1]]))
```

    [-670.00149999]
    

- (p. 140) 1차방정식을 2차방정식으로 만드는 과정이 나옴
- 넘파이 브로드캐스팅
  + 배열의 크기가 동일하면 상관 없음
  + 배열의 크기가 다른데, 연산을 할 때, 브로드캐스팅 원리가 적용
  + 브로드캐스팅 튜토리얼, 뭘 찾아서, 추가적으로 공부를 해야함(분석가)


```python
train_poly = np.column_stack((train_input ** 2, train_input))
test_poly = np.column_stack((test_input ** 2, test_input))

print(train_poly.shape, test_poly.shape)
```

    (42, 2) (14, 2)
    


```python
lr = LinearRegression()

lr.fit(train_poly, train_target)

print(lr.predict([[50 ** 2, 50]]))
```

    [1573.98423528]
    


```python
print(lr.coef_, lr.intercept_)
```

    [  1.01433211 -21.55792498] 116.0502107827827
    


```python
# 구간별 직선을 그리기 위해 15에서 49까지 정수 배열을 만듦
point = np.arange(15, 50)

# 훈련 세트의 산점도를 그림
fig, ax = plt.subplots()
ax.scatter(train_input, train_target)

# 15에서 49까지 2차 방정식 그래프를 그림
ax.plot(point, 1.01 * point ** 2 - 21.6 * point + 116.05)

# 50cm 농어 데이터
ax.scatter(50, 1574, marker = '^')
ax.set_xlabel('length')
ax.set_ylabel('weight')
plt.show()
```


    
![png](/images/Chapter_2+3/output_136_0.png)
    


- KNN의 문제점
  + 농어의 길이가 커져도 무게는 동일함 (현실성 제로)
- 단순 선형회귀(1차방정식)의 문제점
  + 치어를 1cm로 놓고 보면, 무게가 음수로 나옴 (현실성 제로)
- 다항 회귀(2차방정식)로 변경
  + 현실성 있음

# Chapter 3-3

# 특성 공학과 규제

## 다중 회귀

- 여러 개의 특성을 사용한 회귀
- 기존의 특성을 사용해 새로운 특성을 뽑아내는 작업을 특성 공학이라 부름

## 데이터 준비

- 특성이 3개로 늘어났기 때문에 데이터를 북사해 붙여넣는 것도 번거로움
- 넘파이는 이런 작업을 잘 지원하지 않음
- 판다스를 이용하면 간단함
- 데이터프레임은 판다스의 핵심 데이터 구조
- csv 파일을 일거와서 데이터프레임을 만든 다음 to_numpy() 메서드를 사용


```python
import pandas as pd
df = pd.read_csv('https://bit.ly/perch_csv_data')
perch_full = df.to_numpy()
print(perch_full)
```

    [[ 8.4   2.11  1.41]
     [13.7   3.53  2.  ]
     [15.    3.82  2.43]
     [16.2   4.59  2.63]
     [17.4   4.59  2.94]
     [18.    5.22  3.32]
     [18.7   5.2   3.12]
     [19.    5.64  3.05]
     [19.6   5.14  3.04]
     [20.    5.08  2.77]
     [21.    5.69  3.56]
     [21.    5.92  3.31]
     [21.    5.69  3.67]
     [21.3   6.38  3.53]
     [22.    6.11  3.41]
     [22.    5.64  3.52]
     [22.    6.11  3.52]
     [22.    5.88  3.52]
     [22.    5.52  4.  ]
     [22.5   5.86  3.62]
     [22.5   6.79  3.62]
     [22.7   5.95  3.63]
     [23.    5.22  3.63]
     [23.5   6.28  3.72]
     [24.    7.29  3.72]
     [24.    6.38  3.82]
     [24.6   6.73  4.17]
     [25.    6.44  3.68]
     [25.6   6.56  4.24]
     [26.5   7.17  4.14]
     [27.3   8.32  5.14]
     [27.5   7.17  4.34]
     [27.5   7.05  4.34]
     [27.5   7.28  4.57]
     [28.    7.82  4.2 ]
     [28.7   7.59  4.64]
     [30.    7.62  4.77]
     [32.8  10.03  6.02]
     [34.5  10.26  6.39]
     [35.   11.49  7.8 ]
     [36.5  10.88  6.86]
     [36.   10.61  6.74]
     [37.   10.84  6.26]
     [37.   10.57  6.37]
     [39.   11.14  7.49]
     [39.   11.14  6.  ]
     [39.   12.43  7.35]
     [40.   11.93  7.11]
     [40.   11.73  7.22]
     [40.   12.38  7.46]
     [40.   11.14  6.63]
     [42.   12.8   6.87]
     [43.   11.93  7.28]
     [43.   12.51  7.42]
     [43.5  12.6   8.14]
     [44.   12.49  7.6 ]]
    

- 타깃 데이터


```python
import numpy as np

perch_length = np.array([8.4, 13.7, 15.0, 16.2, 17.4, 18.0, 18.7, 19.0, 19.6, 20.0, 21.0,
       21.0, 21.0, 21.3, 22.0, 22.0, 22.0, 22.0, 22.0, 22.5, 22.5, 22.7,
       23.0, 23.5, 24.0, 24.0, 24.6, 25.0, 25.6, 26.5, 27.3, 27.5, 27.5,
       27.5, 28.0, 28.7, 30.0, 32.8, 34.5, 35.0, 36.5, 36.0, 37.0, 37.0,
       39.0, 39.0, 39.0, 40.0, 40.0, 40.0, 40.0, 42.0, 43.0, 43.0, 43.5,
       44.0])

perch_weight = np.array([5.9, 32.0, 40.0, 51.5, 70.0, 100.0, 78.0, 80.0, 85.0, 85.0, 110.0,
       115.0, 125.0, 130.0, 120.0, 120.0, 130.0, 135.0, 110.0, 130.0,
       150.0, 145.0, 150.0, 170.0, 225.0, 145.0, 188.0, 180.0, 197.0,
       218.0, 300.0, 260.0, 265.0, 250.0, 250.0, 300.0, 320.0, 514.0,
       556.0, 840.0, 685.0, 700.0, 700.0, 690.0, 900.0, 650.0, 820.0,
       850.0, 900.0, 1015.0, 820.0, 1100.0, 1000.0, 1100.0, 1000.0,
       1000.0])
```

- perch_full과 perch_weight를 훈련 세트와 테스트 세트로 나눔


```python
from sklearn.model_selection import train_test_split

train_input, test_input, train_target, test_target = train_test_split(
    perch_full, perch_weight, random_state = 42)
```

## 사이킷런의 변환기

- 사이킷런은 특성을 만들거나 전처리하기 위한 클래스를 제공하는데 이를 변환기라 부름
- fit(), transform() 메서드 제공
- 사용할 변환기는 PolynomialFeatures 클래스
- sklearn.preprocessing 패키지에 포함되어 있음


```python
from sklearn.preprocessing import PolynomialFeatures

poly = PolynomialFeatures()

poly.fit([[2, 3]])
print(poly.transform([[2, 3]]))
```

    [[1. 2. 3. 4. 6. 9.]]
    

- fit() 메서드는 새롭게 만들 특성 조합을 찾고 transform() 메서드는 실제로 데이터를 변환 함
- 2와 3을 각기 제곱한 4와 9가 추가되었고, 2와 3을 곱한 6이 추가 됨
- 1은 필요 없기 때문에 include_bias = False로 지정하여 다시 변환


```python
poly = PolynomialFeatures(include_bias = False)

poly.fit([[2, 3]])
print(poly.transform([[2, 3]]))
```

    [[2. 3. 4. 6. 9.]]
    


```python
# 훈련 데이터 적용

poly = PolynomialFeatures(include_bias = False)

poly.fit(train_input)
train_poly = poly.transform(train_input)

print(train_poly.shape)
```

    (42, 9)
    

- PolynomialFeatures 클래스는 9개의 특성이 어떻게 만들어 졌는지 확인할 수 있게 해줌
- get_feature_names_out() 메서드 호출


```python
poly.get_feature_names_out()
```




    array(['x0', 'x1', 'x2', 'x0^2', 'x0 x1', 'x0 x2', 'x1^2', 'x1 x2',
           'x2^2'], dtype=object)



- 'x0'은 첫 번째 특성을 의미
- 'x0 ^2'는 첫 번째 특성의 제곱
- 'x0 x1'은 첫 번째 특성과 두 번째 특성의 곱을 나타냄


```python
# 테스트 데이터 변환

test_poly = poly.transform(test_input)
```

## 다중 회귀 모델 훈련하기

- train_poly를 사용해서 훈련


```python
from sklearn.linear_model import LinearRegression

lr = LinearRegression()

lr.fit(train_poly, train_target)
print(lr.score(train_poly, train_target))
```

    0.9903183436982124
    

- 테스트 세트 점수 확인


```python
print(lr.score(test_poly, test_target))
```

    0.9714559911594134
    

- PolynomialFeatures 클래스의 degree 매개변수를 이용하여 필요한 고차항의 최대 차수를 지정할 수 있음
- 5제곱까지 특성을 만들어 출력


```python
poly = PolynomialFeatures(degree = 5, include_bias = False)

poly.fit(train_input)
train_poly = poly.transform(train_input)
test_poly = poly.transform(test_input)

print(train_poly.shape)
```

    (42, 55)
    

- 특성의 개수는 55개
- 이를 이용해 점수 확인


```python
lr.fit(train_poly, train_target)

# 훈련 데이터 점수 확인
print(lr.score(train_poly,train_target))

# 테스트 데이터 점수 확인
print(lr.score(test_poly, test_target))
```

    0.9999999999991097
    -144.40579242684848
    

- 과대적합 발생
- 특성을 다시 줄여야 함

## 규제

- 규제는 머신러닝 모델이 훈련 세트를 너무 과도하게 학습하지 못하도록 훼방하는 것을 말함
- 규제를 하기 전에 정규화
- StandardScaler 클래스 사용해서 표준점수를 구함


```python
from sklearn.preprocessing import StandardScaler

ss = StandardScaler()

ss.fit(train_poly)
train_scaled = ss.transform(train_poly)
test_scaled = ss.transform(test_poly)
```

- 선형 회귀 모델에 규제를 추가한 모델을 릿지와 라쏘라고 부름
- 릿지는 계수를 제곱한 값을 기준으로 규제를 적용
- 라쏘는 계수의 절댓값을 기준으로 규제를 적용

## 릿지 회귀


```python
from sklearn.linear_model import Ridge

ridge = Ridge()

ridge.fit(train_scaled, train_target)

# 훈련 데이터 점수 확인
print(ridge.score(train_scaled, train_target))

# 테스트 데이터 점수 확인
print(ridge.score(test_scaled, test_target))
```

    0.9896101671037343
    0.9790693977615397
    

- alpha 매개변수로 규제의 강도를 조정할 수 있음


```python
import matplotlib.pyplot as plt

train_score = []
test_score = []

alpha_list = [0.001, 0.01, 0.1, 1, 10, 100]

for alpha in alpha_list :
  # 릿지 모델을 만듦
  ridge = Ridge(alpha = alpha)

  # 릿지 모델을 훈련
  ridge.fit(train_scaled, train_target)

  # 훈련 점수와 테스트 점수를 저장
  train_score.append(ridge.score(train_scaled, train_target))
  test_score.append(ridge.score(test_scaled, test_target))
```

- 시각화


```python
fig, ax = plt.subplots()

ax.plot(np.log10(alpha_list), train_score)
ax.plot(np.log10(alpha_list), test_score)
ax.set_xlabel('alpha')
ax.set_ylabel('R^2')

plt.show()
```


    
![png](/images/Chapter_2+3/output_172_0.png)
    


- 위는 훈련 세트 그래프, 아래는 테스트 세트 그래프
- 적절한 alpha 값은 두 그래프가 가장 가깝고 테스트 세트의 점수가 가장 높은 -1, 즉 10^-1 = 0.1임
- alpha 값을 0.1로 하여 최종 모델 훈련


```python
ridge = Ridge(alpha = 0.1)
ridge.fit(train_scaled, train_target)

# 훈련 데이터 점수 확인
print(ridge.score(train_scaled, train_target))

# 테스트 데이터 점수 확인
print(ridge.score(test_scaled, test_target))
```

    0.9903815817570366
    0.9827976465386926
    

## 라쏘 회귀


```python
from sklearn.linear_model import Lasso

lasso = Lasso()

lasso.fit(train_scaled, train_target)

# 훈련 데이터 점수 확인
print(lasso.score(train_scaled, train_target))

# 테스트 데이터 점수 확인
print(lasso.score(test_scaled, test_target))
```

    0.989789897208096
    0.9800593698421883
    

- alpha값을 바꾸어가며 점수 계산


```python
train_score = []
test_score = []

alpha_list = [0.001, 0.01, 0.1, 1, 10, 100]

for alpha in alpha_list :
  # 라쏘 모델을 만듦
  lasso = Lasso(alpha = alpha, max_iter = 10000)

  # 라쏘 모델을 훈련
  lasso.fit(train_scaled, train_target)

  # 훈련 점수와 테스트 점수를 저장
  train_score.append(lasso.score(train_scaled, train_target))
  test_score.append(lasso.score(test_scaled, test_target))
```

    /usr/local/lib/python3.7/dist-packages/sklearn/linear_model/_coordinate_descent.py:648: ConvergenceWarning: Objective did not converge. You might want to increase the number of iterations, check the scale of the features or consider increasing regularisation. Duality gap: 1.878e+04, tolerance: 5.183e+02
      coef_, l1_reg, l2_reg, X, y, max_iter, tol, rng, random, positive
    /usr/local/lib/python3.7/dist-packages/sklearn/linear_model/_coordinate_descent.py:648: ConvergenceWarning: Objective did not converge. You might want to increase the number of iterations, check the scale of the features or consider increasing regularisation. Duality gap: 1.297e+04, tolerance: 5.183e+02
      coef_, l1_reg, l2_reg, X, y, max_iter, tol, rng, random, positive
    

- 경고는 무시해도 됨
- 시각화


```python
fig, ax = plt.subplots()

ax.plot(np.log10(alpha_list), train_score)
ax.plot(np.log10(alpha_list), test_score)
ax.set_xlabel('alpha')
ax.set_ylabel('R^2')

plt.show()
```


    
![png](/images/Chapter_2+3/output_180_0.png)
    


- 위가 훈련세트 그래프, 아래는 테스트 세트 그래프
- 이 그래프에서 최적의 alpha 값은 1, 즉 10^1 = 10


```python
lasso = Lasso(alpha = 10)

lasso.fit(train_scaled, train_target)

# 훈련 데이터 점수 확인
print(lasso.score(train_scaled, train_target))

# 테스트 데이터 점수 확인
print(lasso.score(test_scaled, test_target))
```

    0.9888067471131867
    0.9824470598706695
    

- 라쏘 모델은 계수 값을 아예 0으로 만들 수 있음
- 라쏘 모델의 계수는 coef_ 속성에 저장되어 있음


```python
print(np.sum(lasso.coef_ == 0))
```

    40
    

- 55개 특성 모델을 주입했지만 라쏘 모델이 사용한 특성은 15개
- 이런 특징 때문에 라쏘 모델을 유용한 특성을 골라내는 용도로도 사용할 수 있음
