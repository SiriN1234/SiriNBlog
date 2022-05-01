---
title: "2022.03.25 수업내용과 실습1"
author: "Hanjeongin"
date: 2022-03-25 17:11:00
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

# 데이터 불러오기


```python
import pandas as pd
df = pd.read_csv("/kaggle/input/kaggle-survey-2021/kaggle_survey_2021_responses.csv")
df.head()
```


```python
questions = df.iloc[0, :]
questions
```


```python
df2 = df.iloc[1:, :].reset_index(drop = True)
df2.head()
```


```python
df2['Q1'].value_counts()
```

## Ploty Express


```python
temp = pd.DataFrame({
  "Fruit": ["Apples", "Oranges", "Bananas", "Apples", "Oranges", "Bananas"],
  "Contestant": ["Alex", "Alex", "Alex", "Jordan", "Jordan", "Jordan"],
  "Number Eaten": [2, 1, 3, 1, 3, 2],
})

temp
```


```python
from plotly.offline import init_notebook_mode
init_notebook_mode(connected = True)

import plotly.express as px # Seaborn

fig = px.bar(temp, x = 'Fruit', y = "Number Eaten", color = "Contestant", barmode = "group")
fig.show()
```


```python
import plotly.graph_objects as go # Matplotlib

q1_df = df2['Q1'].value_counts()
# print(q1_df)

fig = go.Figure()
fig.add_trace(go.Bar(x = q1_df.index, y = q1_df.values))

fig.show()
```


```python
CATEGORY_ORDER = ["18-21", "22-24", "25-29", "30-34", "35-39", "40-44", "45-49", "50-54", "55-59", "60-69", "70+"]

q1_df = df2['Q1'].value_counts()
# print(q1_df)

# basic graph
fig = go.Figure()
fig.add_trace(go.Bar(x = q1_df.index, y = q1_df.values))

# styling changes
fig.update_layout(plot_bgcolor = "white", 
                 font  = dict(color = "#909999"), 
                 title = dict(text = "your TITLE text"), 
                 xaxis = dict(title = "your X-AXIS TITLE", linecolor = "#21DBAA", categoryorder = "array", categoryarray = CATEGORY_ORDER), 
                 yaxis = dict(title = "your Y-AXIS TITLE", linecolor = "#DB9021"))

```

## Group Bar Graph


```python
# df2['Q2'].value_counts()
q1_q2_df = df2.loc[:, ["Q1", "Q2"]].replace({'Prefer not to say':'etc', 'Nonbinary':"etc", "Prefer to self-describe": "etc"})
q1_q2_df['Q2'].value_counts() # 3개 데이터를 etc로 통합, 전처리

```


```python
q1_q2_df = q1_q2_df.groupby(['Q2','Q1']).size().reset_index().rename(columns = {0:"Count"})
q1_q2_df.head()
```


```python
fig = go.Figure()
for gender, group in q1_q2_df.groupby("Q2"):
   fig.add_trace(go.Bar(x = group['Q1'], y = group['Count'], name = gender))
fig.update_layout(barmode="group", 
                 plot_bgcolor = "white")
fig.show()
```

# 나라별 임금 격차
- Q3, Q25


```python
df2['Q3'].value_counts()
```


```python
df2['Q25'].value_counts()
```


```python
q3_df = df['Q3'].value_counts()
fig = go.Figure()
fig.add_trace(go.Bar(x = q3_df.index, y = q3_df.values))
fig.show()
```


```python
q3_q25 = df2[['Q3', 'Q25']]
q3_q25['Q25'].replace(['$0-999', '1,000-1,999'], '$0-1,999', inplace = True)
q3_q25['Q25'].replace(['2,000-2,999', '3,000-3,999'], '$2,000-3,999', inplace = True)
q3_q25['Q25'].replace(['2,000-2,999', '3,000-3,999'], '$2,000-3,999', inplace = True)
q3_q25['Q25'].replace(['4,000-4,999', '5,000-7,499'], '$4,000-7,499', inplace = True)
q3_q25['Q25'].replace(['25,000-29,999', '60,000-69,999',  
                       '30,000-39,999','15,000-19,999', '70,000-79,999', 
                       '10,000-14,999', '20,000-24,999', '7,500-9,999', 
                       '100,000-124,999', '40,000-49,999', '50,000-59,999', 
                       '300,000-499,999', '200,000-249,999', '125,000-149,999', 
                       '250,000-299,999', '80,000-89,999', '90,000-99,999', 
                       '150,000-199,999', '>$1,000,000', '$500,000-999,999'], '$7,500+', inplace = True)

q3_q25['Q25'].value_counts()
```


```python
q3_q25.dropna(subset = ["Q25"], inplace=True)
```


```python
q3_q25 = q3_q25.groupby(['Q3','Q25']).size().reset_index().rename(columns = {0:"Count"})

# India
india_df = q3_q25[q3_q25['Q3'] == "India"].reset_index(drop = True)
india_df['percentage'] = india_df["Count"] / india_df["Count"].sum()
india_df.head()
```


```python
# USA
usa_df = q3_q25[q3_q25['Q3'] == "United States of America"].reset_index(drop = True)
usa_df['percentage'] = usa_df["Count"] / usa_df["Count"].sum()
usa_df.head()
```


```python
india_df['%'] = np.round(india_df['percentage'] * 100, 1)
usa_df['%'] = np.round(usa_df['percentage'] * 100, 1)

india_usa_df = pd.concat([india_df, usa_df]).reset_index()
india_usa_df
```


```python
fig = go.Figure()
for country, group in india_usa_df.groupby("Q3"):
   fig.add_trace(go.Bar(x = group['Q25'], 
                        y = group['%'], 
                        name = country, 
                        text = group['%'].astype(str) + "%", 
                        textposition='auto'))
fig.update_layout(barmode="group", 
                  plot_bgcolor = "white")
fig.show()
```

# 연도별 주 사용 언어의 비율 차이
- 막대 그래프
- Python, R, SQL


```python
df_2021 = pd.read_csv("../input/kaggle-survey-2021/kaggle_survey_2021_responses.csv")
df_2020 = pd.read_csv("../input/kaggle-survey-2020/kaggle_survey_2020_responses.csv")
df_2019 = pd.read_csv("../input/kaggle-survey-2019/multiple_choice_responses.csv")
```


```python
df_2019.shape, df_2020.shape, df_2021.shape
```


```python
print("2019:", df_2019['Q19'].unique().tolist())
print("2020:", df_2020['Q8'].unique().tolist())
print("2021:", df_2021['Q8'].unique().tolist())
```


```python
# 프로그래밍 언어만 뽑아옴

programming_list = ["Python", "R", "SQL", "Java", "C", "Bash", "Javascript", "C++"]
programming_df = pd.Series(programming_list)

df_2019 = df_2019[df_2019['Q19'].isin(programming_df)]
df_2020 = df_2020[df_2020['Q8'].isin(programming_df)]
df_2021 = df_2021[df_2021['Q8'].isin(programming_df)]

print("2019:", df_2019['Q19'].unique().tolist())
print("2020:", df_2020['Q8'].unique().tolist())
print("2021:", df_2021['Q8'].unique().tolist())
```


```python
q3_q5_q19_2019 = df_2019.loc[:, ['Q3', 'Q5', 'Q19']]
q3_q5_q19_2019 = q3_q5_q19_2019.rename(columns = {'Q19': 'Q8'}, inplace = False) # To match with other datasets
q3_q5_q8_2020 = df_2020.loc[:, ['Q3', 'Q5', 'Q8']]
q3_q5_q8_2021 = df_2021.loc[:, ['Q3', 'Q5', 'Q8']]

q3_q5_q19_2019.shape, q3_q5_q8_2020.shape, q3_q5_q8_2021.shape
```


```python
# 구분을 위해 연도추가

q3_q5_q19_2019['year'] = '2019'
q3_q5_q8_2020['year'] = '2020'
q3_q5_q8_2021['year'] = '2021'

q3_q5_q19_2019.shape, q3_q5_q8_2020.shape, q3_q5_q8_2021.shape
```


```python
final_df = pd.concat([q3_q5_q19_2019, q3_q5_q8_2020, q3_q5_q8_2021])
print(final_df.head())


year_q5_q8 = final_df.groupby(['year', 'Q8']).size().reset_index().rename(columns = {0:"Count"})

# 2019
q8_2019 = year_q5_q8[year_q5_q8['year'] == "2019"].reset_index(drop = True)
q8_2019['percentage'] = q8_2019["Count"] / q8_2019["Count"].sum()
q8_2019['%'] = np.round(q8_2019['percentage'] * 100, 1)

# 2020
q8_2020 = year_q5_q8[year_q5_q8['year'] == "2020"].reset_index(drop = True)
q8_2020['percentage'] = q8_2020["Count"] / q8_2020["Count"].sum()
q8_2020['%'] = np.round(q8_2020['percentage'] * 100, 1)

# 2021
q8_2021 = year_q5_q8[year_q5_q8['year'] == "2021"].reset_index(drop = True)
q8_2021['percentage'] = q8_2021["Count"] / q8_2021["Count"].sum()
q8_2021['%'] = np.round(q8_2021['percentage'] * 100, 1)

```


```python
fig = go.Figure()

fig.add_trace(go.Bar(x = q8_2019['Q8'], 
                     y = q8_2019['%'], 
                     name = "2019", 
                     text = q8_2019['%'].astype(str) + "%", 
                     textposition='auto'))

fig.add_trace(go.Bar(x = q8_2020['Q8'], 
                     y = q8_2020['%'], 
                     name = "2020", 
                     text = q8_2020['%'].astype(str) + "%", 
                     textposition='auto'))

fig.add_trace(go.Bar(x = q8_2021['Q8'], 
                     y = q8_2021['%'], 
                     name = "2021", 
                     text = q8_2021['%'].astype(str) + "%", 
                     textposition='auto'))

fig.show()
```

# 히트맵


```python
import plotly.figure_factory as ff

z=[[1, 90, 30, 50, 1], [20, 1, 60, 80, 30], [30, 60, 1, 50, 20]]
x=['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
y=['Morning', 'Afternoon', 'Evening']

fig = ff.create_annotated_heatmap(z, x = x, y = y, colorscale = "Viridis")
fig.show()
```


```python
import plotly.graph_objects as go
from functools import reduce
from itertools import product

z=[[1, 90, 30, 50, 1], [20, 1, 60, 80, 30], [30, 60, 1, 50, 20]]
x=['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
y=['Morning', 'Afternoon', 'Evening']

def get_anno_text(z_value):
    annotations=[]
    a, b = len(z_value), len(z_value[0])
    flat_z = reduce(lambda x,y: x+y, z_value) # z_value.flat if you deal with numpy
    coords = product(range(a), range(b))
    for pos, elem in zip(coords, flat_z):
        annotations.append({'font': {'color': '#FFFFFF'},
                    'showarrow': False,
                    'text': str(elem),
                    'x': pos[1],
                    'y': pos[0]})
    return annotations

fig = go.Figure(data=go.Heatmap(
                   z=z,
                   x=x,
                   y=y,
                   hoverongaps = True))

fig.update_layout(annotations = get_anno_text(z))
fig.show()
```


```python
df2.head()
```


```python
df2.groupby(['Q4', 'Q1']).size().unstack().fillna(0).astype("int16")
```


```python
import plotly.graph_objects as go
import plotly.figure_factory as ff

z = df2.groupby(['Q4', 'Q1']).size().unstack().fillna(0).astype('int64')
z_data = z.apply(lambda x:np.round(x/x.sum(), 2), axis = 1).to_numpy() # convert to correlation matrix
x = z.columns.tolist()
y = z.index.tolist()

fig = ff.create_annotated_heatmap(z_data, x = x, y = y, colorscale = "Viridis")
fig.show()
```
