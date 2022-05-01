---
title: "Chapter 6장"
author: "Hanjeongin"
date: 2022-03-31 17:40:00
tags: Python
---

출처 : 혼자 공부하는 머신러닝+딥러닝

# 비지도 학습

- vs 지도학습
  + 종속변수 = 타겟
- 비지도학습은 종속변수 및 타겟이 없음
- 분류
  + 다중분류
  + 전제조건이 (다양한 유형) 데이터가 많아야 함
  + 딥러닝과 연관이 됨

# 데이터 불러오기


```python
!wget https://bit.ly/fruits_300_data -O fruits_300.npy
```

    --2022-03-31 01:14:28--  https://bit.ly/fruits_300_data
    Resolving bit.ly (bit.ly)... 67.199.248.10, 67.199.248.11
    Connecting to bit.ly (bit.ly)|67.199.248.10|:443... connected.
    HTTP request sent, awaiting response... 301 Moved Permanently
    Location: https://github.com/rickiepark/hg-mldl/raw/master/fruits_300.npy [following]
    --2022-03-31 01:14:28--  https://github.com/rickiepark/hg-mldl/raw/master/fruits_300.npy
    Resolving github.com (github.com)... 140.82.113.4
    Connecting to github.com (github.com)|140.82.113.4|:443... connected.
    HTTP request sent, awaiting response... 302 Found
    Location: https://raw.githubusercontent.com/rickiepark/hg-mldl/master/fruits_300.npy [following]
    --2022-03-31 01:14:29--  https://raw.githubusercontent.com/rickiepark/hg-mldl/master/fruits_300.npy
    Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
    Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|185.199.108.133|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 3000128 (2.9M) [application/octet-stream]
    Saving to: ‘fruits_300.npy’
    
    fruits_300.npy      100%[===================>]   2.86M  --.-KB/s    in 0.07s   
    
    2022-03-31 01:14:29 (43.4 MB/s) - ‘fruits_300.npy’ saved [3000128/3000128]
    
    

- numpy 파일을 불러옴


```python
import numpy as np
import matplotlib.pyplot as plt

fruits = np.load('fruits_300.npy')
print(fruits.shape)
print(fruits.ndim)
```

    (300, 100, 100)
    3
    

- 첫번째 차원(300) = 샘플의 개수
- 두번째 차원(100) = 이미지 높이
- 세번째 차원(100) = 이미지 너비
- 이미지 크기 100 x 100


```python
fruits[0, 0, :]
```




    array([  1,   1,   1,   1,   1,   1,   1,   1,   1,   1,   1,   1,   1,
             1,   1,   1,   2,   1,   2,   2,   2,   2,   2,   2,   1,   1,
             1,   1,   1,   1,   1,   1,   2,   3,   2,   1,   2,   1,   1,
             1,   1,   2,   1,   3,   2,   1,   3,   1,   4,   1,   2,   5,
             5,   5,  19, 148, 192, 117,  28,   1,   1,   2,   1,   4,   1,
             1,   3,   1,   1,   1,   1,   1,   2,   2,   1,   1,   1,   1,
             1,   1,   1,   1,   1,   1,   1,   1,   1,   1,   1,   1,   1,
             1,   1,   1,   1,   1,   1,   1,   1,   1], dtype=uint8)



- 이미지 시각화
  + 흑백 사진을 담고있다
  + 0~255까지의 정숫값을 가진다
- 0에 가까우면 검게 나타남
- 255에 가까우면 밝게 나타남


```python
plt.imshow(fruits[0], cmap = 'gray')
plt.show()
```


    
![png](/images/Chapter_6/output_9_0.png)
    


cmap = 'gray_r'
- 0에 가까우면 밝게 나타남
- 255에 가까우면 검게 나타남


```python
plt.imshow(fruits[0], cmap = 'gray_r') # 반전
plt.show()
```


    
![png](/images/Chapter_6/output_11_0.png)
    


- 여러가지 이미지 시각화


```python
fig, axs = plt.subplots(1, 2)
axs[0].imshow(fruits[100], cmap = 'gray_r')
axs[1].imshow(fruits[200], cmap = 'gray_r')

plt.show()
```


    
![png](/images/Chapter_6/output_13_0.png)
    


# 픽셀값 분석


```python
apple = fruits[0:100].reshape(-1, 100 * 100)
pineapple = fruits[100:200].reshape(-1, 100 * 100)
banana = fruits[200:300].reshape(-1, 100 * 100)

apple.shape, pineapple.shape, banana.shape
```




    ((100, 10000), (100, 10000), (100, 10000))



- axis = 0 vs axis = 1 차이 확인 (p. 293)

- 각 이미지에 대한 픽셀 평균값 비교


```python
print(apple.mean(axis = 1))
```

    [ 88.3346  97.9249  87.3709  98.3703  92.8705  82.6439  94.4244  95.5999
      90.681   81.6226  87.0578  95.0745  93.8416  87.017   97.5078  87.2019
      88.9827 100.9158  92.7823 100.9184 104.9854  88.674   99.5643  97.2495
      94.1179  92.1935  95.1671  93.3322 102.8967  94.6695  90.5285  89.0744
      97.7641  97.2938 100.7564  90.5236 100.2542  85.8452  96.4615  97.1492
      90.711  102.3193  87.1629  89.8751  86.7327  86.3991  95.2865  89.1709
      96.8163  91.6604  96.1065  99.6829  94.9718  87.4812  89.2596  89.5268
      93.799   97.3983  87.151   97.825  103.22    94.4239  83.6657  83.5159
     102.8453  87.0379  91.2742 100.4848  93.8388  90.8568  97.4616  97.5022
      82.446   87.1789  96.9206  90.3135  90.565   97.6538  98.0919  93.6252
      87.3867  84.7073  89.1135  86.7646  88.7301  86.643   96.7323  97.2604
      81.9424  87.1687  97.2066  83.4712  95.9781  91.8096  98.4086 100.7823
     101.556  100.7027  91.6098  88.8976]
    

- 각 과일에 대한 히스토그램 작성


```python
plt.hist(np.mean(apple, axis = 1), alpha = 0.8)
plt.hist(np.mean(pineapple, axis = 1), alpha = 0.8)
plt.hist(np.mean(banana, axis = 1), alpha = 0.8)
plt.legend(['apple', 'pineapple', 'banana'])
plt.show()
```


    
![png](/images/Chapter_6/output_20_0.png)
    



```python
fig, axs = plt.subplots(1, 3, figsize = (20, 5))
axs[0].bar(range(10000), np.mean(apple, axis = 0))
axs[1].bar(range(10000), np.mean(pineapple, axis = 0))
axs[2].bar(range(10000), np.mean(banana, axis = 0))
plt.show()
```


    
![png](/images/Chapter_6/output_21_0.png)
    



```python
apple_mean = np.mean(apple, axis = 0).reshape(100, 100)
pineapple_mean = np.mean(pineapple, axis = 0).reshape(100, 100)
banana_mean = np.mean(banana, axis = 0).reshape(100, 100)

fig, axs = plt.subplots(1, 3, figsize = (20,5))
axs[0].imshow(apple_mean, cmap = 'gray_r')
axs[1].imshow(pineapple_mean, cmap = 'gray_r')
axs[2].imshow(banana_mean, cmap = 'gray_r')

plt.show()
```


    
![png](/images/Chapter_6/output_22_0.png)
    


# 평균값과 가까운 사진 고르기


```python
abs_diff = np.abs(fruits - apple_mean)
abs_mean = np.mean(abs_diff, axis = (1, 2))
print(abs_mean.shape)
```

    (300,)
    

- 오차의 값이 가장 작은 순서대로 100개를 골라본다


```python
apple_index = np.argsort(abs_mean)[:100]
fig, axs = plt.subplots(10, 10, figsize = (10, 10))
for i in range(10) :
  for j in range(10) :
    axs[i, j]. imshow(fruits[apple_index[i * 10 + j]], cmap = 'gray_r')
    axs[i, j].axis('off')

plt.show()
```


    
![png](/images/Chapter_6/output_26_0.png)
    


# K-평균 알고리즘

- 각각의 픽셀값 (3차원 -> 1차원배열) 평균을 구함
  + 픽셀의 평균값을 활용해서 사과, 파인애플, 바나나에 근사한 이미지를 추출하는 것
- 어떻게 평균값을 구할 수 있을까?
  + K-평균 알고리즘 (K-Means) 알고리즘
  + 평균값 = Cluster Center = Centroid

# 데이터 불러오기


```python
!wget https://bit.ly/fruits_300_data -O fruits_300.npy
```

    --2022-03-31 02:14:23--  https://bit.ly/fruits_300_data
    Resolving bit.ly (bit.ly)... 67.199.248.11, 67.199.248.10
    Connecting to bit.ly (bit.ly)|67.199.248.11|:443... connected.
    HTTP request sent, awaiting response... 301 Moved Permanently
    Location: https://github.com/rickiepark/hg-mldl/raw/master/fruits_300.npy [following]
    --2022-03-31 02:14:23--  https://github.com/rickiepark/hg-mldl/raw/master/fruits_300.npy
    Resolving github.com (github.com)... 140.82.112.3
    Connecting to github.com (github.com)|140.82.112.3|:443... connected.
    HTTP request sent, awaiting response... 302 Found
    Location: https://raw.githubusercontent.com/rickiepark/hg-mldl/master/fruits_300.npy [following]
    --2022-03-31 02:14:23--  https://raw.githubusercontent.com/rickiepark/hg-mldl/master/fruits_300.npy
    Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
    Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|185.199.108.133|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 3000128 (2.9M) [application/octet-stream]
    Saving to: ‘fruits_300.npy’
    
    fruits_300.npy      100%[===================>]   2.86M  --.-KB/s    in 0.02s   
    
    2022-03-31 02:14:23 (153 MB/s) - ‘fruits_300.npy’ saved [3000128/3000128]
    
    


```python
import numpy as np
import matplotlib.pyplot as plt

fruits = np.load('fruits_300.npy')
print(fruits.shape)
print(fruits.ndim)
```

    (300, 100, 100)
    3
    

- 3차원 ( 샘플개수, 너비, 높이)
- 2차원 (샘플개수, 너비 x 높이)


```python
fruits_2d = fruits.reshape(-1, 100 * 100)
fruits_2d.shape
```




    (300, 10000)



- k-평균 알고리즘 활용


```python
from sklearn.cluster import KMeans
km = KMeans(n_clusters = 3, random_state = 42)
km.fit(fruits_2d)
```




    KMeans(n_clusters=3, random_state=42)



- 모형학습 후, labels


```python
print(km.labels_)
```

    [2 2 2 2 2 0 2 2 2 2 2 2 2 2 2 2 2 2 0 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
     2 2 2 2 2 0 2 0 2 2 2 2 2 2 2 0 2 2 2 2 2 2 2 2 2 0 0 2 2 2 2 2 2 2 2 0 2
     2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 0 2 2 2 2 2 2 2 2 0 0 0 0 0 0 0 0 0 0 0
     0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
     0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
     0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
     1 1 1 1 1 1 1 1 1 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
     1 1 1 1 1 1 1 1 1 1 1 1 1 1 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
     1 1 1 1]
    

- 직접 샘플의 개수 확인


```python
print(np.unique(km.labels_, return_counts = True))
```

    (array([0, 1, 2], dtype=int32), array([111,  98,  91]))
    

- 그래프로 시각화


```python
import matplotlib.pyplot as plt

def draw_fruits(arr, ratio=1):
    n = len(arr)    # n은 샘플 개수입니다
    # 한 줄에 10개씩 이미지를 그립니다. 샘플 개수를 10으로 나누어 전체 행 개수를 계산합니다. 
    rows = int(np.ceil(n/10))
    # 행이 1개 이면 열 개수는 샘플 개수입니다. 그렇지 않으면 10개입니다.
    cols = n if rows < 2 else 10
    fig, axs = plt.subplots(rows, cols, 
                            figsize=(cols*ratio, rows*ratio), squeeze=False)
    for i in range(rows):
        for j in range(cols):
            if i*10 + j < n:    # n 개까지만 그립니다.
                axs[i, j].imshow(arr[i*10 + j], cmap='gray_r')
            axs[i, j].axis('off')
    plt.show()
```


```python
draw_fruits(fruits[km.labels_ == 0])
```


    
![png](/images/Chapter_6/output_41_0.png)
    



```python
draw_fruits(fruits[km.labels_ == 1])
```


    
![png](/images/Chapter_6/output_42_0.png)
    



```python
draw_fruits(fruits[km.labels_ == 2])
```


    
![png](/images/Chapter_6/output_43_0.png)
    


# 클러스터 중심


```python
draw_fruits(km.cluster_centers_.reshape(-1, 100, 100), ratio = 3)
```


    
![png](/images/Chapter_6/output_45_0.png)
    



```python
print(km.transform(fruits_2d[100:101]))
```

    [[3393.8136117  8837.37750892 5267.70439881]]
    


```python
print(km.predict(fruits_2d[100:101]))
```

    [0]
    


```python
draw_fruits(fruits[100:101])
```


    
![png](/images/Chapter_6/output_48_0.png)
    


# 최적의 K-평균 찾기


```python
inertia = []
for k in range(2, 7) :
  km = KMeans(n_clusters = k, random_state = 42)
  km.fit(fruits_2d)
  inertia.append(km.inertia_)

plt.plot(range(2, 7), inertia)
plt.show()
```


    
![png](/images/Chapter_6/output_50_0.png)
    


# PCA

- 차원축소의 개념
- PCA 개념
- 과일 사진의 경우, 10,000개의 픽셀 (높이 x 너비)
- 10,000개의 특성이 있는 셈 (차원)
- 정형데이터에서도 활용 가능
  + 문자열 데이터, 수치형 데이터 (연속형 데이터, 비연속형 데이터)
  + 캐글 대회 : 수치형 컬럼 304개
    + 연산은 RAM에서 처리
    + 라면을 5개 끓여야 함 / 냄비 크기는 3개가 max

### 차원축소 = 일부 특성을 선택하여 데이터 크기를 줄임

- 머신러닝 측면 : 과대적합 방지 & 성능 향상

- 양적 데이터 사이의 분산 - 공분산 관계를 애용해서 선형결합으로 표시되는 주성분을 찾음
- 2-3개의 주성분으로 전체 변동을 찾는 것이 PCA

- p. 326
- 그래프를 보면, 처음 10개의 주성분이 (10,000개의 픽셀)

- 알고리즘을 구성 할 때, 필요한 데이터 픽셀 수, 300 * 10,000개 픽셀
- 원래는 300 x PCA 10개의 주성분으로 줄임
- 기존 1시간 걸릴 것이 10분 걸림
- 그럼에도 불구하고, 분류가 더 잘 되더라

# PCA 클래스

## 데이터 불러오기


```python
!wget https://bit.ly/fruits_300_data -O fruits_300.npy
```

    --2022-03-31 06:15:56--  https://bit.ly/fruits_300_data
    Resolving bit.ly (bit.ly)... 67.199.248.10, 67.199.248.11
    Connecting to bit.ly (bit.ly)|67.199.248.10|:443... connected.
    HTTP request sent, awaiting response... 301 Moved Permanently
    Location: https://github.com/rickiepark/hg-mldl/raw/master/fruits_300.npy [following]
    --2022-03-31 06:15:56--  https://github.com/rickiepark/hg-mldl/raw/master/fruits_300.npy
    Resolving github.com (github.com)... 140.82.113.4
    Connecting to github.com (github.com)|140.82.113.4|:443... connected.
    HTTP request sent, awaiting response... 302 Found
    Location: https://raw.githubusercontent.com/rickiepark/hg-mldl/master/fruits_300.npy [following]
    --2022-03-31 06:15:57--  https://raw.githubusercontent.com/rickiepark/hg-mldl/master/fruits_300.npy
    Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 185.199.109.133, 185.199.111.133, 185.199.110.133, ...
    Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|185.199.109.133|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 3000128 (2.9M) [application/octet-stream]
    Saving to: ‘fruits_300.npy’
    
    fruits_300.npy      100%[===================>]   2.86M  --.-KB/s    in 0.07s   
    
    2022-03-31 06:15:57 (42.9 MB/s) - ‘fruits_300.npy’ saved [3000128/3000128]
    
    

- 배열로 업로드


```python
import numpy as np

fruits = np.load('fruits_300.npy')
fruits_2d = fruits.reshape(-1, 100 * 100)
fruits_2d.shape
```




    (300, 10000)



- sklearn.decomposition 모듈


```python
from sklearn.decomposition import PCA
pca = PCA(n_components = 50)
pca.fit(fruits_2d)
```




    PCA(n_components=50)




```python
print(pca.components_.shape)
```

    (50, 10000)
    

- 그래프 그리기


```python
import matplotlib.pyplot as plt

def draw_fruits(arr, ratio=1):
    n = len(arr)    # n은 샘플 개수입니다
    # 한 줄에 10개씩 이미지를 그립니다. 샘플 개수를 10으로 나누어 전체 행 개수를 계산합니다. 
    rows = int(np.ceil(n/10))
    # 행이 1개 이면 열 개수는 샘플 개수입니다. 그렇지 않으면 10개입니다.
    cols = n if rows < 2 else 10
    fig, axs = plt.subplots(rows, cols, 
                            figsize=(cols*ratio, rows*ratio), squeeze=False)
    for i in range(rows):
        for j in range(cols):
            if i*10 + j < n:    # n 개까지만 그립니다.
                axs[i, j].imshow(arr[i*10 + j], cmap='gray_r')
            axs[i, j].axis('off')
    plt.show()

draw_fruits(pca.components_.reshape(-1, 100, 100))
```


    
![png](/images/Chapter_6/output_65_0.png)
    



```python
fruits_pca = pca.transform(fruits_2d)
print(fruits_pca.shape)
```

    (300, 50)
    

- 데이터의 원래 크기 대비해서 1/200로 줄임
- 용량이 줄었다는 것과 똑같음

# 원본 데이터 재구성

- 10,000개의 특성을 50개로 줄임
- 100% 재구성은 어렵지만, 그래도 쓸만하다


```python
fruits_inverse = pca.inverse_transform(fruits_pca)
print(fruits_inverse.shape)
```

    (300, 10000)
    


```python
fruits_reconstruct = fruits_inverse.reshape(-1, 100, 100)
print(fruits_reconstruct.shape)
```

    (300, 100, 100)
    

## 그래프 작성


```python
for start in [0, 100, 200] :
  draw_fruits(fruits_reconstruct[start:start + 100])
  print("\n")
```


    
![png](/images/Chapter_6/output_72_0.png)
    


    
    
    


    
![png](/images/Chapter_6/output_72_2.png)
    


    
    
    


    
![png](/images/Chapter_6/output_72_4.png)
    


    
    
    

# 설명된 분산


```python
plt.plot(pca.explained_variance_ratio_)
plt.show()
```


    
![png](/images/Chapter_6/output_74_0.png)
    


- 처음 10개의 주성분이 대부분의 분산을 표현한다
- 11개 주성분부터 ~ 50개까지는 잘 설명이 안됨


```python
print(np.sum(pca.explained_variance_ratio_))
```

    0.9215509156198924
    

# 다름 알고리즘과 함꼐 사용하기

- 3개의 과일 사진 분류 위해 로지스틱 회귀 사용


```python
from sklearn.linear_model import LogisticRegression

lr = LogisticRegression()

# 타깃값 생성
target = np.array([0] * 100 + [1] * 100 + [2] * 100)
print(target)
```

    [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
     0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
     0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 1
     1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
     1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
     1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
     2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
     2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
     2 2 2 2]
    

- 교차검증 진행
- PCA 수행 전, RAW DATA


```python
from sklearn.model_selection import cross_validate
scores = cross_validate(lr, fruits_2d, target)
print(np.mean(scores['test_score']))
print(np.mean(scores['fit_time']))
```

    0.9966666666666667
    1.433236312866211
    

- PCA 수행 후, 학습 시간 비교


```python
scores = cross_validate(lr, fruits_pca, target)
print(np.mean(scores['test_score']))
print(np.mean(scores['fit_time']))
```

    1.0
    0.06142778396606445
    

- 주 성분의 매개변수 개수 지정, 분산비율 지정


```python
pca = PCA(n_components = 0.5)
pca.fit(fruits_2d)
print(pca.n_components_)
```

    2
    

- 주 성분을 2개로 압축시킴


```python
fruits_pca = pca.transform(fruits_2d)
print(fruits_pca.shape)
```

    (300, 2)
    


```python
scores = cross_validate(lr, fruits_pca, target)
print(np.mean(scores['test_score']))
print(np.mean(scores['fit_time']))
```

    /usr/local/lib/python3.7/dist-packages/sklearn/linear_model/_logistic.py:818: ConvergenceWarning: lbfgs failed to converge (status=1):
    STOP: TOTAL NO. of ITERATIONS REACHED LIMIT.
    
    Increase the number of iterations (max_iter) or scale the data as shown in:
        https://scikit-learn.org/stable/modules/preprocessing.html
    Please also refer to the documentation for alternative solver options:
        https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression
      extra_warning_msg=_LOGISTIC_SOLVER_CONVERGENCE_MSG,
    

    0.9933333333333334
    0.08337922096252441
    

    /usr/local/lib/python3.7/dist-packages/sklearn/linear_model/_logistic.py:818: ConvergenceWarning: lbfgs failed to converge (status=1):
    STOP: TOTAL NO. of ITERATIONS REACHED LIMIT.
    
    Increase the number of iterations (max_iter) or scale the data as shown in:
        https://scikit-learn.org/stable/modules/preprocessing.html
    Please also refer to the documentation for alternative solver options:
        https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression
      extra_warning_msg=_LOGISTIC_SOLVER_CONVERGENCE_MSG,
    /usr/local/lib/python3.7/dist-packages/sklearn/linear_model/_logistic.py:818: ConvergenceWarning: lbfgs failed to converge (status=1):
    STOP: TOTAL NO. of ITERATIONS REACHED LIMIT.
    
    Increase the number of iterations (max_iter) or scale the data as shown in:
        https://scikit-learn.org/stable/modules/preprocessing.html
    Please also refer to the documentation for alternative solver options:
        https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression
      extra_warning_msg=_LOGISTIC_SOLVER_CONVERGENCE_MSG,
    

- 차원 축소된 데이터를 K-평균 알고리즘에 추가


```python
from sklearn.cluster import KMeans
km = KMeans(n_clusters = 3, random_state = 42)
km.fit(fruits_pca)
print(np.unique(km.labels_, return_counts = True))
```

    (array([0, 1, 2], dtype=int32), array([110,  99,  91]))
    


```python
for label in range(0, 3) :
  draw_fruits(fruits[km.labels_ == label])
  print("\n")
```


    
![png](/images/Chapter_6/output_90_0.png)
    


    
    
    


    
![png](/images/Chapter_6/output_90_2.png)
    


    
    
    


    
![png](/images/Chapter_6/output_90_4.png)
    


    
    
    

- 시각화로 뿌려주기


```python
for label in range(0, 3) :
  data = fruits_pca[km.labels_ == label]
  plt.scatter(data[:, 0], data[:, 1])

plt.legend(['apple', 'banana', 'pineapple'])
plt.show()
```


    
![png](/images/Chapter_6/output_92_0.png)
    


# 마무리

## 키워드로 끝내는 핵심 포인트

- **차원 축소**는 원본 데이터의 특성을 적은 수의 새로운 특성으로 변환하는 비지도 학습의 한 종류입니다. 차원 숙소는 저장 공간을 줄이고 시각화하기 쉽습니다. 또한 다른 알고리즘의 성능을 높일 수 있습니다.
- **주성분 분석**은 차원 축소 알고리즘의 하나로 데이터에서 가장 분산이 큰 방향을 찾는 방법입니다. 이런 방향을 주성분이라고 부릅니다. 원본 데이터를 주성분에 투영하여 새로운 특성을 만들 수 있습니다. 일반적으로 주성분은 원본 데이터에 있는 특성 개수보다 작습니다.
- **설명된 분산**은 주성분 분석에서 주성분이 얼마나 원본 데이터의 분산을 잘 나타내는지 기록한 것입니다. 사이킷런의 PCA 클래스는 주성분 개수나 설명된 분산의 비율을 지정하여 주성분 분석을 수행할 수 있습니다.

## 핵심 패키지와 함수

### scikit-learn

- **PCA**는 주성분 분석을 수행하는 클래스입니다.
  + n_components는 주성분의 개수를 지정합니다. 기본값은 None으로 샘플 개수와 특성 개수 중에 작은 것의 값을 사용합니다.
  + random_state에는 넘파이 난수 시드 값을 지정할 수 있습니다.
  + components_ 속성에는 훈련 세트에서 찾은 주성분이 저장됩니다.
  + explained_variance_ 속성에는 설명된 분산이 저장되고, explained_variance_ratio_에는 설명된 분산의 비율이 저장됩니다.
  + inverse_transform() 메서드는 transform() 메서드로 차원을 축소시킨 데이터를 다시 원본 차원으로 복원합니다.
