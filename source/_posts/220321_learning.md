---
title: "2022.03.21 수업내용과 실습"
author: "Hanjeongin"
date: 2022-03-21 18:09:00
tags: Python
---

# Hello World


```python
print ("Hello, World!")
```

    Hello, World!
    

# 주석 처리
- 코드 작업 시, 특정 코드에 대해 설명
- 사용자 정의 함수 작성 시, 클래스 작성 시.. (도움말 작성..)


```python
# 한 줄 주석 처리
'''
여러줄 주석 처리 시 ("로도 가능 둘 중 하나로 위 아래를 통일)
'''

print("Hello, World!")
```

    Hello, World!
    

# 변수 (Scalar)
- 객체(object)로 구현이 됨
  + 하나의 자료형(Type)을 가진다 (이것만 기억하자)
  + 클래스로 정의가 됨
    - 다양한 함수들이 존재 함


## int
- int 정수를 표현하는 데 사용함


```python
# 데이터 전처리를 잘 해야 분석도 잘 함, 예측 모형도 잘 만듦
# 데이터 전처리를 잘 하기 위해서는, 기초문법이 중요함

num_int = 1
num_int2 = 3
print(num_int)
print(num_int2)
print(type(num_int))
```

    1
    3
    <class 'int'>
    

## float
- 실수를 표현하는데 사용한다


```python
num_float = 0.2
print(num_float)
print(type(num_float))
```

    0.2
    <class 'float'>
    

## bool
- True와 False로 나타내는 Boolean 값을 표현하는 데 사용한다


```python
bool_true = True
print(bool_true)
print(type(bool_true))
```

    True
    <class 'bool'>
    

## None
- Null을 나타내는 자료형으로 None이라는 한 가지 값만 가짐


```python
none_x = None
print(none_x)
print(type(none_x))
```

    None
    <class 'NoneType'>
    

## 정수형 사칙연산


```python
a = 11
b = 22
print('a + b = ', a + b)
print('a - b = ', a - b)
print('a * b = ', a * b)
print('a / b = ', a / b)
print('a // b = ', a // b)
print('a % b = ', a % b)
print('a ** b = ', a ** b)
```

    a + b =  33
    a - b =  -11
    a * b =  242
    a / b =  0.5
    a // b =  0
    a % b =  11
    a ** b =  81402749386839761113321
    

## 실수형 사칙연산


```python
a = 11.0
b = 22.0
print('a + b = ', a + b)
print('a - b = ', a - b)
print('a * b = ', a * b)
print('a / b = ', a / b)
print('a // b = ', a // b)
print('a % b = ', a % b)
print('a ** b = ', a ** b)
```

    a + b =  33.0
    a - b =  -11.0
    a * b =  242.0
    a / b =  0.5
    a // b =  0.0
    a % b =  11.0
    a ** b =  8.140274938683976e+22
    

# 논리형 연산자
- Bool 형은 True와 False 값으로 정의
- AND / OR


```python
x = 5 > 4
# print(x)
y = 3 > 4
# print(y)
print(x and x)
print(x and y)
print(x or y)
print(y or y)
```

    True
    False
    True
    False
    

# 비교 연산자
- 부등호를 의미
- 비교 연산자를 True와 False값으로 도출


## 논리 & 비교 연산자 응용


```python
var = input("입력하십시오 : ")
print(type(var))
```

    입력하십시오 : 123
    <class 'str'>
    

- 형변환을 해준다
- 문자열, 정수, 실수 등등


```python
var = int("1")
print(type(var))
```

    <class 'int'>
    


```python
var = int(input("숫자를 입력하세요 : "))
print(type(var))
```

    숫자를 입력하세요 : 456
    <class 'int'>
    


```python
num1 = int(input("숫자를 입력하세요 : "))
num2 = int(input("숫자를 입력하세요 : "))
if (num1 > num2) :
    print("첫 번째 숫자가 두 번째 숫자보다 큽니다")
elif (num1 < num2) :
    print("첫 번째 숫자가 두 번째 숫자보다 작습니다")
else :
  print("첫 번째 숫자와 두 번째 숫자가 같습니다")

```

    숫자를 입력하세요 : 3
    숫자를 입력하세요 : 3
    첫 번째 숫자와 두 번째 숫자가 같습니다
    

# 변수 (Non Scalar)
- 문자열을 입력


```python
print("'Hello, world'")
print('"Hello, World"')
```

    'Hello, world'
    "Hello, World"
    

## String 연산자
- 덧셈 연산자를 써보자


```python
str1 = "Hello "
str2 = "World! "

print(str1 + str2)
```

    Hello World! 
    

- 곱셈 연산자를 써보자


```python
greeting = str1 + str2
print(greeting * 3)
```

    Hello World! Hello World! Hello World! 
    

# Indexing
- 문자열 인덱싱은 각각의 문자열 안에서 범위를 지정하여 특정 문자를 추출한다


```python
greeting = "Hello Kaggle!"
print(greeting[7])
```

    a
    

# 슬라이싱
- 범위를 지정하고 데이터를 가져온다


```python
greeting

print(greeting[:])
print(greeting[:6])
print(greeting[6:])
print(greeting[1:7])
print(greeting[0:9:2])
```

    Hello Kaggle!
    Hello 
    Kaggle!
    ello K
    HloKg
    


```python
print('i\'m SiriN \nNice to meet you')
```

    i'm SiriN 
    Nice to meet you
    

# 문자열 길이 구하기
- len() 함수를 이용한다


```python
len(greeting)
```




    13



# 문자열 포매팅
- %d는 정수를 대입
- %s는 문자열을 대입


```python
print("num1은 %d이고, greeting에 저장된 문자열은 %s이다" % (num1, greeting))
```

    num1은 3이고, greeting에 저장된 문자열은 Hello Kaggle!이다
    

# 포맷 코드
- % 뒤에 숫자를 넣어 사용
- 숫자 뒤에 .x를 붙이는 것으로 원하는 소숫점 자리까지 출력할 수 있음


```python
print("%-3d test" % num1)
print("%10.4f test" % 1.234567)
```

    3   test
        1.2346 test
    

# format 함수를 이용한 포매팅
- {0} {1}... 의 자리에 format 함수를 이용해 값을 넣을 수 있다


```python
print("I {0} SiriN {1}" .format("am", 123))
```

    I am SiriN 123
    

# 문자열 나누기
- replace 함수를 사용


```python
a = "Life is too short"
a.replace("Life", "Your leg")
```




    'Your leg is too short'



# 대소문자 바꾸기
- 대문자로 바꾸는 함수는 upper
- 소문자로 바꾸는 함수는 lower


```python
print(a.upper())
print(a.lower())
```

    LIFE IS TOO SHORT
    life is too short
    

# 리스트
- 시퀀스 데이터 타입
- 데이터에 순서가 존재하냐? 슬라이싱이 가능해야 함
대괄호("값1, 값2, 값3]")


```python
a = [] # 빈 리스트
a_func = list() # 빈 리스트 생성
b = [1] # 숫자가 요소가 될 수 있다
c = ['apple'] # 문자열도 요소가 될 수 있다
d = [1, 2, ['apple']] # 리스트 안에 또 다른 리스트를 요소로 넣을 수 있다

print(a)
print(a_func)
print(b)
print(c)
print(d)
```

    []
    []
    [1]
    ['apple']
    [1, 2, ['apple']]
    

## 리스트 슬라이싱


```python
a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(a)
print(a[0])
print(a[3:])
print(a[3:5])
print(a[::-1]) # 역순
print(a[::2])
```

    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    1
    [4, 5, 6, 7, 8, 9, 10]
    [4, 5]
    [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
    [1, 3, 5, 7, 9]
    


```python
a = [["apple", "banana", "cherry"], 1]
print(a[0])
print(a[0][1])
print(a[0][0][4])
print(a[0][2][2])
```

    ['apple', 'banana', 'cherry']
    banana
    e
    e
    [1, ['apple', 'banana', 'cherry']]
    [['apple', 'banana', 'cherry']]
    

## 리스트 연산자


```python
a = ["john", "evan"]
b = ["alice", "eva"]
c = a + b
print(c)
```

    ['john', 'evan', 'alice', 'eva']
    


```python
c = a * 3
d = b * 0
print("a * 3 = ", c)
print("b * 0 = ", d)
```

    a * 3 =  ['john', 'evan', 'john', 'evan', 'john', 'evan']
    b * 0 =  []
    

# 리스트 수정 및 삭제


```python
a = [0, 1, 2]
a[1] = "b"
print(a)
```

    [0, 'b', 2]
    

## 리스트 값 추가하기


```python
a = [100, 200, 300]
a.append(400)
print(a)

a.extend([500, 600])
print(a)
```

    [100, 200, 300, 400]
    [100, 200, 300, 400, 500, 600]
    


```python
a = [0, 1, 2]
# a.insert(인덱스 번호, 넣고자하는 값)
a.insert(1, 100)
print(a)
```

    [0, 100, 1, 2]
    

# 리스트 값 삭제하기


```python
a = [4, 3, 2, 1, "A"]
a.remove(3) # 리스트 안에 있는 값을 삭제
print(a)
a.remove("A")
print(a)
```

    [4, 2, 1, 'A']
    [4, 2, 1]
    


```python
a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

del a[1] # 인덱스 번호
print(a)

del a[1:5]
print(a)
```

    [1, 3, 4, 5, 6, 7, 8, 9, 10]
    [1, 7, 8, 9, 10]
    


```python
b = ["a", "b", "c", "d"]
x = b.pop() # 맨 마지막 값을 추출
print(x)
print(b)
```

    d
    ['a', 'b', 'c']
    

## 그 외 메서드


```python
a = [0, 1, 2, 3]
print(a)

a. clear()
print(a)
```

    [0, 1, 2, 3]
    []
    


```python
a = ["a", "a", "b", "b"]
print(a.index("a"))
print(a.index("b"))
```

    0
    2
    


```python
a = [1, 4, 5, 2, 3]
b = [1, 4, 5, 2, 3]

a.sort()
print("sort() : ", a)

# 내림차순 sort()
b.sort(reverse = True)
print("sort(reerse = True) : ", b)
```

    sort() :  [1, 2, 3, 4, 5]
    sort(reerse = True) :  [5, 4, 3, 2, 1]
    


```python
c = [4, 3, 2, 'a']
# c.sort()
```

# 튜플
- List와 비슷함
- 슬라이싱, 인덱싱 등등
- 리스트와 차이점은 튜플은 수정 삭제가 안된다


```python
tuple1 = (0) # 끝에 콤마(,)를 붙이지 않을 때 --> int
tuple2 = (0, ) # 끝에 콤마를 붙일 때 --> tuple
tuple3 = 0, 1, 2
print(tuple1)
print(tuple2)
print(tuple3)
```

    0
    (0,)
    (0, 1, 2)
    


```python
a = (0, 1, 2, 3, 'a')
print(type(a))

b = list(a)
print(b)
b[1] = "b"

a = tuple(b)
print(a)

# a[1] = "b"
# del a[4]
```

    <class 'tuple'>
    [0, 1, 2, 3, 'a']
    (0, 'b', 2, 3, 'a')
    

## 튜플 인덱싱 및 슬라이싱 하기


```python
a = (0, 1, 2, 3, 'a')
print(a[1])
print(a[3])
print(a[4])
```

    1
    3
    a
    

## 더하기 곱셈 연산자 사용


```python
a = ('a', 'b')
b = ('c', 'd')

print(a + b)
print(a * 3)
print(b * 0)
```

    ('a', 'b', 'c', 'd')
    ('a', 'b', 'a', 'b', 'a', 'b')
    ()
    

# 딕셔너리
- key-value값으로 나뉨


```python
dict_01 = {'teacher' : 'evan',
           'class' : 601,
           'students' : 24,
           '학생이름' : ['A', 'Z']}

print(dict_01['teacher'])
print(dict_01['class'])
print(dict_01['students'])
print(dict_01['학생이름'])

```

    evan
    601
    24
    ['A', 'Z']
    


```python
print(list(dict_01.keys()))
```

    ['teacher', 'class', 'students', '학생이름']
    


```python
print(dict_01.values())
```

    dict_values(['evan', 601, 24, ['A', 'Z']])
    


```python
dict_01.items()
```




    dict_items([('teacher', 'evan'), ('class', 601), ('students', 24), ('학생이름', ['A', 'Z'])])




```python
print(dict_01.get("teacher", "값 없음"))
print(dict_01.get("선생님", "값 없음"))
print(dict_01.get("class"))
# print(dict_01['선생님'])
print(dict_01.get("students"))
```

    evan
    값 없음
    601
    24
    

# 조건문 & 반복문


```python
weather = "맑음"
if (weather == "비") : # 조건식에서 False가 나오면
  print("우산을 가져간다")
else :
  print("우산을 가져가지 않는다")
```

    우산을 가져가지 않는다
    

- 등급표 만들기
- 60점 이상 합격 / 불합격
- 숫자는 아무거나 써도 상관없음


```python
score = int(input("점수를 입력하세요 : "))

if (score >= 60) :
  print("합격")
else :
  print("불합격")
```

    점수를 입력하세요 : 59
    불합격
    


```python
# 90점 이상은 A등급
# 80점 이상은 B등급
# 나머지는 F등급

score = int(input("점수를 입력하세요 : "))

if (score >= 90) :
  print("A등급")
elif (score >= 80 and score < 90) :
  print("B등급")
else :
  print("F등급")
```

    점수를 입력하세요 : 79
    F등급
    

## 반복문
- 안녕하세요! 10번 반복


```python
for i in range(10):
  print(i + 1, "안녕하세요! ")
```

    1 안녕하세요! 
    2 안녕하세요! 
    3 안녕하세요! 
    4 안녕하세요! 
    5 안녕하세요! 
    6 안녕하세요! 
    7 안녕하세요! 
    8 안녕하세요! 
    9 안녕하세요! 
    10 안녕하세요! 
    


```python
count = range(50)
print(count)

for n in count:
  print(str(n + 1) + "번째")
  if (n + 1) == 5:
    print("Break")
    break
  print("up")
```

    range(0, 50)
    1번째
    up
    2번째
    up
    3번째
    up
    4번째
    up
    5번째
    Break
    


```python
a = "hello"
for x in a : 
  print(x)
  if x == "l" :
    break
```

    h
    e
    l
    

- 반복문 작성 방법은 여러가지 : zip, range, enumerate, len, etc


```python
alphabets = ['A', 'B', 'C']
for index, value in enumerate(alphabets) :
  print(index, value)
```

    0 A
    1 B
    2 C
    


```python
i = 0

for i in range(999) :
  print("반복")
  i + 1
  if (i == 9) :
    break

```

    반복
    반복
    반복
    반복
    반복
    반복
    반복
    반복
    반복
    반복
    


```python
i = 0
print("1은 반복, 그 외의 숫자는 탈출입니다. 숫자를 입력하세요 : ")

for i in range(9999) :
  num = int(input())
  if(num != 1) :
    print("탈출!")
    break
  else :
    print("다시 숫자를 입력하세요 : ")
```

    1은 반복, 그 외의 숫자는 탈출입니다. 숫자를 입력하세요 : 
    1
    다시 숫자를 입력하세요 : 
    1
    다시 숫자를 입력하세요 : 
    1
    다시 숫자를 입력하세요 : 
    3
    탈출!
    

# 연습문제 1번
## 다음 코드의 결과값은 무엇일까?

![image.png](data:image/png;base64,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)

- if "wife" in a: print("wife") --> wife가 없으므로 출력 X
- elif "python" in a and "you" not in a: print("python") --> python이 있고 you도 있기 때문에 출력 X
- elif "shirt" not in a: print("shirt") --> shirt가 없기 때문에 shirt 출력

## 답 : shirt

# 연습문제 2번
## while문을 사용해 1부터 1000까지의 자연수 중 3의 배수의 합을 구해 보자.


```python
a = 0
sum = 0

while a < 1001 :
  if (a % 3 == 0) :
    sum += a
  a += 1

print(sum)
```

    166833
    

# 연습문제 3번
## while문을 사용하여 다음과 같이 별(*)을 표시하는 프로그램을 작성해 보자.

![image.png](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADwAAAB6CAYAAADj/TADAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAHzSURBVHhe7Z0xTsMwGIV/WCrlAGTjCEyMvQETKyMn6NoTsOYEXIGJnqBje4luZMkWKeoAtokQS/5YaVzHz+9bmvgNzaffbeW39ObbIBlx279mQ3bCf1v6fD5L27bSdZ0LUHHCVrZpmn4JG7el7WRzwQmjb+P/8FsaHQqjQ2F0KIwOhQc5VvL6Ufc36TIubETLspKDuXy4P0lVllIdf6MUcaeluh6b3MGIPsmbbOXzayOP/WqKeE54L+vdVra7tezNhFPe2p4TNtjP8OlF3p/v+oU08RcGgT9L6FAYHQqjQ2F0KIwOhdGhMDoUHkQr8RIq+MaFtRJPyxbKDCVeWgWf54QHSjwtWyjzlHhatjBY4qFDYXQojA6F0aEwOhRGh8LoUBgdf2F7yJ9S4mlZBMaFzQNPKvG0LCKBS7zlFXyeE55Q4mlZRMKXeFoWAZZ46FAYHQqjQ2F0KIwOhdGhMDoURsdf2B7kpxR1U7NAjAubh5q9xNOywEQs8eIUfJ4TnrnE07LAxC3xtCwQLPHQoTA6FEaHwuhQGB0Ko0NhdCiMDoUHse3EUA1z7ewCxoXNG1+1tdSyGVhoaxmu0fSc8BVbSy2bgeW2llp2AWwt0aEwOhRGh8LoUBgdCqOTp/BqtXI3OeCEi6JwNzngjof2Io9/xRP5AbPjIhbjtFR2AAAAAElFTkSuQmCC)


```python
i = 1

while i < 6 :
  print("*" * i)
  i += 1
```

    *
    **
    ***
    ****
    *****
    

# 연습문제 4번
## for문을 사용해 1부터 100까지의 숫자를 출력해 보자.


```python
for i in range(100) :
  print(i + 1)
```

    1
    2
    3
    4
    5
    6
    7
    8
    9
    10
    11
    12
    13
    14
    15
    16
    17
    18
    19
    20
    21
    22
    23
    24
    25
    26
    27
    28
    29
    30
    31
    32
    33
    34
    35
    36
    37
    38
    39
    40
    41
    42
    43
    44
    45
    46
    47
    48
    49
    50
    51
    52
    53
    54
    55
    56
    57
    58
    59
    60
    61
    62
    63
    64
    65
    66
    67
    68
    69
    70
    71
    72
    73
    74
    75
    76
    77
    78
    79
    80
    81
    82
    83
    84
    85
    86
    87
    88
    89
    90
    91
    92
    93
    94
    95
    96
    97
    98
    99
    100
    

# 연습문제 5번
## A 학급에 총 10명의 학생이 있다. 이 학생들의 중간고사 점수는 다음과 같다.
## [70, 60, 55, 75, 95, 90, 80, 80, 85, 100]
## for문을 사용하여 A 학급의 평균 점수를 구해 보자.


```python
a = [70, 60, 55, 75, 95, 90, 80, 80, 85, 100]
sum = 0
count = 0

for score in a :
  sum += score
  count += 1

print(sum / count)
```

    79.0
    
