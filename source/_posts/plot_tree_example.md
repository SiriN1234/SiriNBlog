---
title: "플롯 트리 연습"
author: "Hanjeongin"
date: 2022-03-31 17:39:00
tags: Python
---

# 목표
- sklear.tree.plot_tree의 색상을 변경해보자


```python
!pip install -U matplotlib
```

    Requirement already satisfied: matplotlib in /usr/local/lib/python3.7/dist-packages (3.2.2)
    Collecting matplotlib
      Downloading matplotlib-3.5.1-cp37-cp37m-manylinux_2_5_x86_64.manylinux1_x86_64.whl (11.2 MB)
    [K     |████████████████████████████████| 11.2 MB 5.0 MB/s 
    [?25hRequirement already satisfied: pyparsing>=2.2.1 in /usr/local/lib/python3.7/dist-packages (from matplotlib) (3.0.7)
    Requirement already satisfied: numpy>=1.17 in /usr/local/lib/python3.7/dist-packages (from matplotlib) (1.21.5)
    Requirement already satisfied: packaging>=20.0 in /usr/local/lib/python3.7/dist-packages (from matplotlib) (21.3)
    Requirement already satisfied: kiwisolver>=1.0.1 in /usr/local/lib/python3.7/dist-packages (from matplotlib) (1.4.0)
    Requirement already satisfied: pillow>=6.2.0 in /usr/local/lib/python3.7/dist-packages (from matplotlib) (7.1.2)
    Collecting fonttools>=4.22.0
      Downloading fonttools-4.31.2-py3-none-any.whl (899 kB)
    [K     |████████████████████████████████| 899 kB 49.1 MB/s 
    [?25hRequirement already satisfied: python-dateutil>=2.7 in /usr/local/lib/python3.7/dist-packages (from matplotlib) (2.8.2)
    Requirement already satisfied: cycler>=0.10 in /usr/local/lib/python3.7/dist-packages (from matplotlib) (0.11.0)
    Requirement already satisfied: typing-extensions in /usr/local/lib/python3.7/dist-packages (from kiwisolver>=1.0.1->matplotlib) (3.10.0.2)
    Requirement already satisfied: six>=1.5 in /usr/local/lib/python3.7/dist-packages (from python-dateutil>=2.7->matplotlib) (1.15.0)
    Installing collected packages: fonttools, matplotlib
      Attempting uninstall: matplotlib
        Found existing installation: matplotlib 3.2.2
        Uninstalling matplotlib-3.2.2:
          Successfully uninstalled matplotlib-3.2.2
    [31mERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
    albumentations 0.1.12 requires imgaug<0.2.7,>=0.2.5, but you have imgaug 0.2.9 which is incompatible.[0m
    Successfully installed fonttools-4.31.2 matplotlib-3.5.1
    




```python
%matplotlib inline 

import sklearn
print(sklearn.__version__)
import matplotlib
print(matplotlib.__version__)

# 필수 라이브러리 불러오기
from sklearn.datasets import load_iris
from sklearn import tree 
import matplotlib.pyplot as plt

# 데이터 불러오기
iris = load_iris()
print(iris.data.shape, iris.target.shape)
print("feature names", iris.feature_names)
print("class names", iris.target_names)

# 모형 학습 및 plot_tree 그래프 구현
dt = tree.DecisionTreeClassifier(random_state=0)
dt.fit(iris.data, iris.target)

fig, ax = plt.subplots(figsize=(10, 6))
ax = tree.plot_tree(dt, max_depth = 2, filled=True, feature_names = iris.feature_names, class_names = iris.target_names)
plt.show()
```

    1.0.2
    3.5.1
    (150, 4) (150,)
    feature names ['sepal length (cm)', 'sepal width (cm)', 'petal length (cm)', 'petal width (cm)']
    class names ['setosa' 'versicolor' 'virginica']
    


    
![png](/images/plot_tree_example/output_2_1.png)
    


# plot_tree 내부 구조 파악
- Annotation 클래스인 것 확인
  + 


```python
%matplotlib inline
fig, ax = plt.subplots(figsize=(10, 6))
ax = tree.plot_tree(dt, max_depth = 2, 
                    filled=True, 
                    feature_names = iris.feature_names, 
                    class_names = iris.target_names)

for i in range(0, len(ax)):
  print(type(ax[i]))
```

    <class 'matplotlib.text.Annotation'>
    <class 'matplotlib.text.Annotation'>
    <class 'matplotlib.text.Annotation'>
    <class 'matplotlib.text.Annotation'>
    <class 'matplotlib.text.Annotation'>
    <class 'matplotlib.text.Annotation'>
    <class 'matplotlib.text.Annotation'>
    <class 'matplotlib.text.Annotation'>
    <class 'matplotlib.text.Annotation'>
    


    
![png](/images/plot_tree_example/output_4_1.png)
    


# get_bbox_patch()


```python
print("impurity", dt.tree_.impurity[:3])
print("--")
print("value", dt.tree_.value[:3])
```

    impurity [0.66666667 0.         0.5       ]
    --
    value [[[50. 50. 50.]]
    
     [[50.  0.  0.]]
    
     [[ 0. 50. 50.]]]
    


```python
import numpy as np 

colors = ["indigo", "violet", "crimson"]
print(colors[np.argmax([[0., 0., 50.]])])
print(colors[np.argmax([[50., 0., 0.]])])
print(colors[np.argmax([[0., 50., 0.]])])
print(colors[np.argmax([[50., 50., 50.]])])
```

    crimson
    indigo
    violet
    indigo
    


```python
from matplotlib.colors import to_rgb
import numpy as np

%matplotlib inline
fig, ax = plt.subplots(figsize=(16, 10))
ax = tree.plot_tree(dt, max_depth = 3, 
                    filled=True, 
                    feature_names = iris.feature_names, 
                    class_names = iris.target_names)

i = 0
colors = ["yellow", "violet", "lavenderblush"]
for artist, impurity, value in zip(ax, dt.tree_.impurity, dt.tree_.value):
  r, g, b = to_rgb(colors[np.argmax(value)])
  # 코드가 길어서 i로 재 저장
  ip = impurity
  # print(ip + (1-ip)*r, ip + (1-ip)*g, ip + (1-ip)*b)
  if i % 2 == 0:
    # set_boxtyle 적용
    ax[i].get_bbox_patch().set_boxstyle("round", pad=0.3)
    ax[i].get_bbox_patch().set_facecolor((ip + (1-ip)*r, ip + (1-ip)*g, ip + (1-ip)*b))
    ax[i].get_bbox_patch().set_edgecolor('black')  
  else:
    ax[i].get_bbox_patch().set_boxstyle("circle", pad=0.3)
    ax[i].get_bbox_patch().set_facecolor((ip + (1-ip)*r, ip + (1-ip)*g, ip + (1-ip)*b))
    ax[i].get_bbox_patch().set_edgecolor('black')   
  i = i+1
```


    
![png](/images/plot_tree_example/output_8_0.png)
    

