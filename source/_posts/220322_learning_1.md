---
title: "2022.03.22 수업내용과 실습1"
author: "Hanjeongin"
date: 2022-03-22 17:41:00
tags: Python
---

# 기초 문법 리뷰


```python
# 리스트
book_list = ["A", "B", "C"]
# appen, extend, insert, remove, pop, etc

# tuple
book_tuple = ("A", "B", "C")
# 수정 삭제가 불가능하다

# 딕셔너리
book_dictionary = {"책 제목" : ["A", "B"],
                   "출판년도" : [2011, 2002,]}
# keys(), values(), items(), get()
```

## 조건문 & 반복문


```python
if True :
  print("코드 실행")
elif True :
  print("코드 실행")
else :
  print("코드 실행")
```


```python
for i in range(3) :
  print(i + 1, "안녕하세요")
```

    1 안녕하세요
    2 안녕하세요
    3 안녕하세요
    


```python
book_list = ["R", "Python"]
for book in book_list :
  print(book)
```

    R
    Python
    


```python
strings01 = "Hello World"
for char in strings01 :
  print(char)
```

    H
    e
    l
    l
    o
     
    W
    o
    r
    l
    d
    


```python
num_tuple = (1, 2, 3, 4)
for num in num_tuple :
  print(num)
```

    1
    2
    3
    4
    


```python
num_dict = {"A" : 1, "B" : 2}
for num in num_dict :
  # print(num)
  print(num_dict[num])
```

    1
    2
    

## 반복문의 필요성


```python
product_name = ["요구르트", "우유"]
prices = [2000, 3000]
quantities = [5, 3]

name = product_name[0]
sales = prices[0] * quantities[0]
print(name + "의 매출액은 " + str(sales) + "원이다")

name = product_name[1]
sales = prices[1] * quantities[1]
print(name + "의 매출액은 " + str(sales) + "원이다")
```

    요구르트의 매출액은 10000원이다
    우유의 매출액은 9000원이다
    


```python
product_name = ["요구르트", "우유", "빵", "과자"]
prices = [2000, 3000, 1500, 1000]
quantities = [5, 3, 7, 6]

for i in range(4) :
  print(product_name[i] + "의 매출액은 " + str(prices[i] * quantities[i]) + "원이다")
```

    요구르트의 매출액은 10000원이다
    우유의 매출액은 9000원이다
    빵의 매출액은 10500원이다
    과자의 매출액은 6000원이다
    


```python
product_name = ["요구르트", "우유", "빵", "과자"]
prices = [2000, 3000, 1500, 1000]
quantities = [5, 3, 7, 6]

for i in [0, 1, 2, 3] :
  name = product_name[i]
  sales = prices[i] * quantities[i]
  print(name + "의 매출액은 " + str(sales) + "원이다")
```

    요구르트의 매출액은 10000원이다
    우유의 매출액은 9000원이다
    빵의 매출액은 10500원이다
    과자의 매출액은 6000원이다
    

## while
- 조건식이 들어간 반복문 (vs . for-loop : 범위가 중요함)


```python
count = 1
while count < 5 :
  print(count)
  count += 1

print("count가 5 이상이 되었습니다")
```

    1
    2
    3
    4
    count가 5 이상이 되었습니다
    


```python
count = 3
while count > 0 :
  print(count)
  count -= 1

print("count가 0 미만이 되었습니다")
```

    3
    2
    1
    count가 0 미만이 되었습니다
    

- 개발자를 지향한다!
  + while을 좀 더 비중있게 다루는 걸 추천
- 데이터 분석
  + for-loop 공부를 좀 더 비중 있게 하는 것 추천

# 사용자 정의 함수 (User-Defined Function)
- why?

# 클래스(Class)를  왜 쓸까?
- 코드의 반복성을 줄이기 위해서 사용

# len() --> 누군가가 만들었고, 우리는 그냥 그걸 쓰는 것
- 리스트의 길이를 구할 때 씀
- 리스트의 전체 길이를 구하겠다? --> 1회성? 나만 쓰는가? X


```python
def 함수명() :
  # 코드 실행
  return 값
```


```python
def add(a, b) :
  c = a + b
  return c

def minus(a, b) :
  c = a - b
  return c

def mul(a, b) :
  c = a * b
  return c

def div(a, b) :
  c = a / b
  return c

print(add(1, 2))
print(minus(1, 2))
print(mul(3, 4))
print(div(10, 2))
```

    3
    -1
    12
    5.0
    

### jupyter notebook, .ipynb 파일명
### .py로 저장 (PyCharm..)


```python
!which python
```

    /usr/local/bin/python
    

- basic.py로 저장할 때, 예시


```python
# /usr/local/bin/python
# -*- coding : utf-8 -*-

def add(a,b) :
  c = a + b
  return c

if __name__ == "__main__" :
  a = 1
  b = 2
  print(add(a,b))
```

    3
    

## 파이썬 함수 주석 처리



```python
# /usr/local/bin/python
# -*- coding : utf-8 -*-

def temp(content, letter) :
  """
  content안에 있는 문자를 세는 함수입니다.

  Args :
    content(str) : 탐색 문자열
    letter(str) : 찾을 문자열

  Return :
    int
  """
  print("함수 테스트")

  cnt = len([char for char in content if char == letter])
  return cnt

if __name__ == "__main__" :
  # help(temp)
  docstring = temp.__doc__
  print(docstring)
```

    
      content안에 있는 문자를 세는 함수입니다.
    
      Args :
        content(str) : 탐색 문자열
        letter(str) : 찾을 문자열
    
      Return :
        int
      
    

## 리스트 컴프리헨션
- for-loop 반복문을 한줄로 처리


```python
my_list = [[10], [20, 30]]
# print(my_list)

flattend_list = []

for value_list in my_list :
  # print(value_list)
  for value in value_list :
    print(value)
    flattend_list.append(value)

print(flattend_list)

# 결괏값 : [10, 20, 30]
```

    10
    20
    30
    [10, 20, 30]
    


```python
my_list = [[10], [20, 30]]
flattned_list = [value for value_list in my_list for value in value_list]
print(flattend_list)
```

    [10, 20, 30]
    


```python
letters = []
for char in "helloworld" :
  letters.append(char)
print(letters)
```

    ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']
    


```python
letters2 = [char for char in "helloworld"]
print(letters2)
```

    ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']
    


```python
value_list = [1, 2, 3, 4, 5]
print("avg : ", sum(value_list) / len(value_list))

# 중간값
midpoint = int(len(value_list) / 2)
# len(value_list) %2 == 0 :
(value_list[midpoint -1] + value_list[midpoint]) / 2
print(value_list[midpoint])

def mean_and_median(value_list) :
  """
  숫자 리스트의 요소들의 평균과 중간값을 구하는 코드를 작성해라
  Args :
    value_list(iterable of int / float) : A list of int numbers

  Returns :
    tuple(float, float)
  """
  
  # 평균
  mean = sum(value_list) / len(value_list)

  # 중간값
  midpoint = int(len(value_list) / 2)
  if (len(value_list) % 2 == 0) :
    median = (value_list[midpoint-1] + value_list[midpoint]) / 2
  else :
    median = value_list[midpoint]
  
  return mean, median

if __name__ == "__main__" :
  value_list = [1, 1, 2, 2, 3, 4, 5]
  avg, median = mean_and_median(value_list)
  print("avg : ", avg)
  print("median : ", median)

```

    avg :  3.0
    3
    avg :  2.5714285714285716
    median :  2
    

- 데코레이터, 변수명 immutable or mutable, context manager
