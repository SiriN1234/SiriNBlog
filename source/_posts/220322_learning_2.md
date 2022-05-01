---
title: "2022.03.22 수업내용과 실습2"
author: "Hanjeongin"
date: 2022-03-22 17:42:00
tags: Python
---

# 클래스를 만드는 목적
- 코드의 간결화
  + 코드를 재사용
- 여러 라이브러리 --> 클래스로 구현이 됨
  + list 클래스, str 클래스
  + 객체로 씀
  + 변수명으로 정의
- 여러 클래스들이 모여서 하나의 라이브러리가 됨
  + 장고/ 웹개발 / 머신러닝 / 시각화 / 데이터 전처리


```python
class Person : 

  # class attribute
  country = "korean"

  # instance attribute
  def __init__(self, name, age) :
    self.name = name
    self.age = age

if __name__ == "__main__" :
  kim = Person("Kim", 100)
  lee = Person("Lee", 100)

  # access class attribute
  print("kim은 {}" .format(kim.__class__.country))
  print("lee는 {}" .format(lee.__class__.country))
```

    kim은 korean
    lee는 korean
    

## instance 메서드 생성
- list.append(), list.extend()


```python
class Person : 

  # class attribute
  country = "korean"

  # instance attribute
  def __init__(self, name, age) :
    self.name = name
    self.age = age

  # instance method 정의
  def singing(self, songtitle, sales) :
    return "{} 판매량 {} 된 {}을 노래합니다" .format(self.name, sales, songtitle)

if __name__ == "__main__" :
  kim = Person("Kim", 100)
  lee = Person("Lee", 100)

  # access class attribute
  print("kim은 {}" .format(kim.__class__.country))
  print("lee는 {}" .format(lee.__class__.country))

  # call instance method
  print(kim.singing("A", 10))
  print(lee.singing("B", 100))
```

    kim은 korean
    lee는 korean
    Kim 판매량 10 된 A을 노래합니다
    Lee 판매량 100 된 B을 노래합니다
    


```python
name = "lee"
songtitle = "B"
print("{} {}을 노래합니다" .format(name, songtitle))
```

    lee B을 노래합니다
    

## 클래스 상속
- 부모님 집 # 부모 클래스 instance
  + 냉장고, 세탁기, tv, etc
  + 사용은 같이 함
- 직접 돈을 모음
  + 개인 노트북 구매 (개인 방에 비치) # 자식 클래스 instance method, attribute 추가확장
  + 노트북은 자신 것이지만, 추가 가전 제품을 구매해서 확장


```python
class Parent :
  
  # instance attribute
  def __init__(self, name, age) :
    self.name = name
    self.age = age

  # instance method 정의
  def whoAmI(self) :
     print("I am Parent")

  def singing(self, songtitle) :
     return "{} {}을 노래합니다" .format(self.name, songtitle)

  def dancing(self) :
    return "{} 현재 춤을 춥니다" .format(self.name)

class Child(Parent) :
  def __init__(self, name, age) : 
    # super() function 부모 클래스에서 받아옴
    super().__init__(name, age)
    print("Child Class is ON")

  def whoAmI(self) :
    print("I am Child")

  def studying(self) :
    print("I am Fast Runner")

if __name__ == "__main__" :
  child_kim = Child("kim", 15)
  parent_kim = Parent("kim", 45)
  print(child_kim.dancing())
  print(child_kim.singing("연애"))
  # print(parent_kim.studying())
  child_kim.whoAmI()
  parent_kim.whoAmI()
```

    Child Class is ON
    kim 현재 춤을 춥니다
    kim 연애을 노래합니다
    I am Child
    I am Parent
    


```python
class TV :
  def __init__(self) :
    self. __maxprice = 500

  def sell(self) :
    print("Selling price : {}" .format(self.__maxprice))

  # set method, get method
  def setMaxPrice(self, price) :
    self.__maxprice = price

if __name__ == "__main__" :
  tv = TV()
  tv.sell()

  # change price
  # 안 바뀌는 코드의 예시
  tv.__maxprice = 1000
  tv.sell()

  # setMaxPrice
  # 값을 바꿀 수 있다 외부의 입력값을 업데이트 할 수 있다
  tv.setMaxPrice(1000)
  tv.sell()
```

    Selling price : 500
    Selling price : 500
    Selling price : 1000
    

자식 클래스가 많이 있는 라이브러리는 사용자가 쓰기 까다로울 것 같은데, 많이 써주는 이유는 자식클래스 명 마다 의미를 주려고 그런 것인가?
- 특수 목적을 해결하기 위해서 라이브러리를 만듦
- 초기 버전은 3개 클래스 정도 만들면 해결이 되겠지?
  + 버그 나오고, 이슈 터기조 --> 개발자들이 해결
- 2개 더 만들고 다시 배포

## 클래스 내부 조건문
- init constructor에 조건문을 써보자


```python
class Employee :

  # init constructor
  # name, salary
  def __init__(self, name, salary = 0) : 
    self.name = name

    # 조건문 추가
    if (salary >= 0) :
      self.salary = salary
    else :
      self.salary = 0
      print("급여는 0원이 될 수 없다. 다시 입력하세요 : ")

  def update_salary(self, amount) :
    # self.salary = self.salary + amount
    self.salary += amount

  def weekly_salary(self) :
    return self.salary / 7

if __name__ == "__main__" :
  emp01 = Employee("Han", -50000)
  print(emp01.name)
  print(emp01.salary)
  emp01.salary = emp01.salary + 1500
  print(emp01.salary)
  emp01.update_salary(3000)
  print(emp01.salary)
  week_salary = emp01.weekly_salary()
  print(week_salary)

```

    급여는 0원이 될 수 없다. 다시 입력하세요 : 
    Han
    0
    1500
    4500
    642.8571428571429
    

## 클래스 Docstring


```python
class Person :
  """
  사람을 표현하는 클래스

  ...

  Attributes
  ----------
  name : str
    name of the person
  
  age : int
    age of the person

  Methods
  ----------

  info(additional = "")
    Prints the person's name and age

  """

  def __init__(self, name, age) :
    """
    Constructs all the neccessary attributes for the person object

    Parameters
    ----------
    name : str
      name of the person
  
    age : int
      age of the person

    """

    self.name = name
    self.age = age

  def info(self, additional = None) :
    """
    Person's info

    Parameters
    ----------
    additional : str, optional
      more info to be displayed (Default is None)

    Returns
    ----------
    None

    """

    print(f'My name is {self.name}. I am {self.age} years old. ' + additional)


  if __name__ == "__main__" :
    person = Person("Han", age = 20)
    person.info("나의 직장은 00이다")
    # help(Person)
```

    My name is Han. I am 20 years old. 나의 직장은 00이다
    

# 4장 연습문제

# 연습문제 1번
## 주어진 자연수가 홀수인지 짝수인지 판별해 주는 함수(is_odd)를 작성해 보자.



```python
print("자연수를 입력하세요 : ")
num = int(input())

def is_odd(num) :
  if (num % 2 == 0) :
    print("짝수입니다")
  else :
    print("홀수입니다")

is_odd(num)
```

    자연수를 입력하세요 : 
    3
    홀수입니다
    

# 연습문제 2번
## 입력으로 들어오는 모든 수의 평균 값을 계산해 주는 함수를 작성해 보자. (단 입력으로 들어오는 수의 개수는 정해져 있지 않다.)


```python
input_list = []

for i in range(999) :
  print("%d번째 숫자를 입력하세요(0을 입력하면 중단됨) : " % (i + 1))
  num = int(input())
  if(num == 0) :
    print("0을 입력해서 합산을 중단합니다")
    break
  else :
    input_list.append(num)

def avg() :
  print("평균값은 %f" % (sum(input_list) / len(input_list)))

print(avg())
```

    1번째 숫자를 입력하세요(0을 입력하면 중단됨) : 
    3
    2번째 숫자를 입력하세요(0을 입력하면 중단됨) : 
    4
    3번째 숫자를 입력하세요(0을 입력하면 중단됨) : 
    5
    4번째 숫자를 입력하세요(0을 입력하면 중단됨) : 
    0
    0을 입력해서 합산을 중단합니다
    평균값은 4.000000
    None
    

# 연습문제 3번
## 다음은 두 개의 숫자를 입력받아 더하여 돌려주는 프로그램이다.


```
input1 = input("첫번째 숫자를 입력하세요:")
input2 = input("두번째 숫자를 입력하세요:")

total = input1 + input2
print("두 수의 합은 %s 입니다" % total)
```

## 이 프로그램을 수행해 보자.


```
첫번째 숫자를 입력하세요:3
두번째 숫자를 입력하세요:6
두 수의 합은 36 입니다
```
## 3과 6을 입력했을 때 9가 아닌 36이라는 결괏값을 돌려주었다. 이 프로그램의 오류를 수정해 보자.




```python
input1 = int(input("첫번째 숫자를 입력하세요:"))
input2 = int(input("두번째 숫자를 입력하세요:"))

total = input1 + input2
print("두 수의 합은 %s 입니다" % total)
```

    첫번째 숫자를 입력하세요:3
    두번째 숫자를 입력하세요:6
    두 수의 합은 9 입니다
    

# 연습문제 4번
## 다음 중 출력 결과가 다른 것 한 개를 골라 보자.


```
print("you" "need" "python")
print("you"+"need"+"python")
print("you", "need", "python")
print("".join(["you", "need", "python"]))
```

- 세 번째 출력에서 ,가 있기 때문에 띄어쓰기가 되서 출력이 된다


## 답 : 3번
