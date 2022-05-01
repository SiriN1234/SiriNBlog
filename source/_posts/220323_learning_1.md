---
title: "2022.03.23 수업내용과 실습1"
author: "Hanjeongin"
date: 2022-03-23 22:40:00
tags: Python
---

# 조언
- 머신러닝 / 딥러닝
- -> 수학을 잘 하는 사람 vs 수학을 처음 하는 사람
- -> 머신러닝 / 딥러닝 (인간이 만든 수식)
- -> 개념을 이해하고, 수식을 이해하고, 코드로 그 수식을 구현하면 잘 할 수 있음
- -> 머신러닝과 딥러닝을 쓰기 위해서는 수학자만 해야되냐!?
- -> 결론은 X
- -> 머신러닝 / 딥러닝의 주 목적이 인간 생활에 보편적인 문제 해결을 위해서 나온 것임
- -> 프레임워크 형태로 나옴 (개념은 이해하고 있어야함) --> 개념만 문자열 타입으로 매개변수를 잘 조정만 하면 모델이 만들어짐
- -> 성과를 내야하는데 (개발자는 배포를 잘해야 함)
  - 이미지 인식 모델을 만듬 / 쓸데가 없음 / 안드로이드 앱 / 웹앱에 탑재할줄만 알아도 충분
  - 기획 (어떤 문제를 풀까?) 
- -> AutoML
  - 코드를 4-5줄 치면 머신러닝 모델이 만들어짐

파이썬 라이브러리 설치 방법 (vs R)


```python
# R
# install.packages("패키지명")

# 파이썬 라이브러리 설치 코드에서 실행 X
# 터미널에서 설치
# 방법 1. conda 설치
# --> 아나콘다 설치 후, conda 설치 (주 사용목적 : 데이터 과학)
# conda에서 라이브러리 관리 (버전 업데이트가 조금 느림)

# 방법 2. pip 설치 (주 사용 목적 : 개발 + 데이터 과학 + 그외)
# --> 아나콘다 설치 안함 / 파이썬만 설치

# git bash 열고 하면 됨
# pip install numpy
```

# Numpy 라이브 불러오기


```python
import numpy as np
print(np.__version__)
```

    1.21.5
    

# 배열로 바꾸는 작업
- 1부터 10까지의 리스트를 만든다
- NumPy 배열로 변환해서 저장한다


```python
temp = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
arr = np.array(temp)
print(arr)
print(temp)
```

    [ 1  2  3  4  5  6  7  8  9 10]
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    


```python
print (type(temp))
print(type(arr))
```

    <class 'list'>
    <class 'numpy.ndarray'>
    

- arr 배열 숫자 5 출력


```python
print(arr[4:8])
```

    [5 6 7 8]
    

- NumPy를 이용하여 기초 통계 함수를 이용한다.


```python
print(np.mean(arr))
print(np.sum(arr))
print(np.median(arr))
print(np.std(arr)) # 표준편차
```

    5.5
    55
    5.5
    2.8722813232690143
    

## 사칙연산


```python
math_scores = [90, 80, 88]
english_scores = [80, 70, 90]

total_scores = math_scores + english_scores
print(total_scores)

math_arr = np.array(math_scores)
english_arr = np.array(english_scores)

total_scores = math_arr + english_arr
print(total_scores)
```

    [90, 80, 88, 80, 70, 90]
    [170 150 178]
    


```python
np.min(total_scores)
```




    150




```python
np.max(total_scores)
```




    178




```python
math_scores = [2, 3, 4]
english_scores = [1, 2, 3]

math_arr = np.array(math_scores)
english_arr = np.array(english_scores)

# 사칙연산
print("덧셈 : ", np.add(math_arr, english_arr))
print("뺄셈 : ", np.subtract(math_arr, english_arr))
print("곱셈 : ", np.multiply(math_arr, english_arr))
print("나눗셈 : ", np.divide(math_arr, english_arr))
print("거듭제곱 : ", np.power(math_arr, english_arr))
```

    덧셈 :  [3 5 7]
    뺄셈 :  [1 1 1]
    곱셈 :  [ 2  6 12]
    나눗셈 :  [2.         1.5        1.33333333]
    거듭제곱 :  [ 2  9 64]
    

## 배열의 생성
- 0차원부터 3차원까지 생성하는 방법


```python
temp_arr = np.array(20)
print(temp_arr)
print(type(temp_arr))
print(temp_arr.shape) # 배열의 크기를 보여주는 함수
```

    20
    <class 'numpy.ndarray'>
    ()
    


```python
# 1차원 배열
temp_arr = np.array([1, 2, 3])
print(temp_arr)
print(type(temp_arr))
print(temp_arr.shape) # 배열의 크기를 보여주는 함수
print(temp_arr.ndim) # 차원을 확인하는 함수
```

    [1 2 3]
    <class 'numpy.ndarray'>
    (3,)
    1
    


```python
# 2차원 배열
temp_arr = np.array([[1, 2, 3], [4, 5, 6]])
print(temp_arr)
print(type(temp_arr))
print(temp_arr.shape) # 배열의 크기를 보여주는 함수
print(temp_arr.ndim)
```

    [[1 2 3]
     [4 5 6]]
    <class 'numpy.ndarray'>
    (2, 3)
    2
    


```python
# 3차원 배열
temp_arr = np.array([[[1, 2, 3], [4, 5, 6]], [[1, 2, 3], [4, 5, 6]]])
print(temp_arr)
print(type(temp_arr))
print(temp_arr.shape) # 배열의 크기를 보여주는 함수
print(temp_arr.ndim)
```

    [[[1 2 3]
      [4 5 6]]
    
     [[1 2 3]
      [4 5 6]]]
    <class 'numpy.ndarray'>
    (2, 2, 3)
    3
    


```python
temp_arr = np.array([1, 2, 3, 4], ndmin = 2)
print(temp_arr)
print(type(temp_arr))
print(temp_arr.shape) # 배열의 크기를 보여주는 함수
print(temp_arr.ndim)
```

    [[1 2 3 4]]
    <class 'numpy.ndarray'>
    (1, 4)
    2
    

## 소숫점 정렬


```python
temp_arr = np.trunc([-1.23, 1.23])
temp_arr
```




    array([-1.,  1.])




```python
temp_arr = np.fix([-1.23, 1.23])
temp_arr
```




    array([-1.,  1.])




```python
temp_arr = np.around([-1.23456, 1.23456], 4)
temp_arr
```




    array([-1.2346,  1.2346])




```python
temp_arr = np.round([-1.23456, 1.23456], 4)
temp_arr
```




    array([-1.2346,  1.2346])




```python
temp_arr = np.floor([-1.23456, 1.23456]) # 소숫점을 내림
temp_arr
```




    array([-2.,  1.])




```python
temp_arr = np.ceil([-1.23456, 1.23456]) # 소숫점을 올림
temp_arr
```




    array([-1.,  2.])



- shape는 높이 * 세로 * 가로 순인가?
- axis 축 설정

# 배열을 생성하는 다양한 방법들


```python
temp_arr = np.arange(5)
temp_arr
```




    array([0, 1, 2, 3, 4])




```python
temp_arr = np.arange(1, 11, 3)
temp_arr
```




    array([ 1,  4,  7, 10])




```python
zero_arr = np.zeros((2, 3))
print(zero_arr)
print(type(zero_arr))
print(zero_arr.shape) # 배열의 크기를 보여주는 함수
print(zero_arr.ndim)
print(zero_arr.dtype)
```

    [[0. 0. 0.]
     [0. 0. 0.]]
    <class 'numpy.ndarray'>
    (2, 3)
    2
    float64
    


```python
temp_arr = np.ones((4,5), dtype = "int32")
print(temp_arr)
print(type(temp_arr))
print(temp_arr.shape) # 배열의 크기를 보여주는 함수
print(temp_arr.ndim)
print(temp_arr.dtype)
```

    [[1 1 1 1 1]
     [1 1 1 1 1]
     [1 1 1 1 1]
     [1 1 1 1 1]]
    <class 'numpy.ndarray'>
    (4, 5)
    2
    int32
    


```python
temp_arr = np.ones((12,12), dtype = "int32")
temp_res_arr = temp_arr.reshape(8, -1)
print(temp_res_arr)
print(type(temp_res_arr))
print(temp_res_arr.shape) # 배열의 크기를 보여주는 함수
print(temp_res_arr.ndim)
print(temp_res_arr.dtype)
```

    [[1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1]
     [1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1]
     [1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1]
     [1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1]
     [1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1]
     [1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1]
     [1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1]
     [1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1]]
    <class 'numpy.ndarray'>
    (8, 18)
    2
    int32
    

# numpy 조건식



```python
temp_arr = np.arange(10)
temp_arr
```




    array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])




```python
# 5보다 작은 값은 원래 값으로 반환
# 5보다 큰 값은 원래 값 * 10으로 반환
np.where(temp_arr < 5, temp_arr, temp_arr *10)
```




    array([ 0,  1,  2,  3,  4, 50, 60, 70, 80, 90])




```python
# 0~100까지의 배열을 만들고, 50보다 작은 값은 곱하기 10, 나머지는 원래 값으로 반환
temp_arr = np.arange(101)
np.where(temp_arr < 50, temp_arr * 10, temp_arr)
```




    array([  0,  10,  20,  30,  40,  50,  60,  70,  80,  90, 100, 110, 120,
           130, 140, 150, 160, 170, 180, 190, 200, 210, 220, 230, 240, 250,
           260, 270, 280, 290, 300, 310, 320, 330, 340, 350, 360, 370, 380,
           390, 400, 410, 420, 430, 440, 450, 460, 470, 480, 490,  50,  51,
            52,  53,  54,  55,  56,  57,  58,  59,  60,  61,  62,  63,  64,
            65,  66,  67,  68,  69,  70,  71,  72,  73,  74,  75,  76,  77,
            78,  79,  80,  81,  82,  83,  84,  85,  86,  87,  88,  89,  90,
            91,  92,  93,  94,  95,  96,  97,  98,  99, 100])



- np.select


```python
temp_arr = np.arange(10)
temp_arr

# 5보다 큰 곱하기 2, 2보다 작은 값은 더하기 100
```




    array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])




```python
condlist = [temp_arr > 5, temp_arr <2]
choielist = [temp_arr *2, temp_arr + 100]
np.select(condlist, choielist, default = temp_arr)
```




    array([100, 101,   2,   3,   4,   5,  12,  14,  16,  18])


