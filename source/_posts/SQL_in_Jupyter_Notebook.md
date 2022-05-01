---
title: "Jupyter Notebook에서 SQL 실행하기"
author: "Hanjeongin"
date: 2022-04-27 18:00:00
tags: SQL
---

## 라이브러리 설치

- 공통적으로 다음 라이브러리를 설치한다

```jsx
pip install ipython-sql
```

![Untitled](/images/SQL_in_Jupyter_Notebook/Untitled.png)

- 다음으로 접속하려는 DB에 맞춰서 라이브러리를 설치한다

```jsx
# sql server
pip install pyodbc

# PostgreSQL 
pip install pyscopg2

# MySQL
pip install PyMySQL

# Oracle
pip install cx_Oracle
```

![Untitled](/images/SQL_in_Jupyter_Notebook/Untitled%201.png)

## Jupyter Notebook에서 설정하기

- Jupyter Notebook에서 매직명령어로 익스텐션을 로드

```jsx
%load_ext sql
```

- 다음과 같은 창이 떠서 Install을 했다
    
    ![Untitled](/images/SQL_in_Jupyter_Notebook/Untitled%202.png)
    
- 설치하면 정상적으로 실행이 된다
    
    ![Untitled](/images/SQL_in_Jupyter_Notebook/Untitled%203.png)
    
- 접속하려는 DB에 맞는 코드를 입력 후 실행

```jsx
# SQL Server
%sql mssql+pyodbc://user_name:password@host:port_number/db

# PostgreSQL
%sql postgresql://user_name:password@host:port_number/db
            
# MySQL
%sql mysql://user_name:password@host:port_number/db

# Oracle
%sql oracle://user_name:password@127.0.0.1:port_number/db
```

![Untitled](/images/SQL_in_Jupyter_Notebook/Untitled%204.png)

- 연결이 되었으면 코드 앞에 %%sql을 붙이고 sql문을 실행하면 된다 (세미콜론을 빼야된다)

![Untitled](/images/SQL_in_Jupyter_Notebook/Untitled%205.png)

- Jupyterlab에서 실행을 했을 때도 잘 실행이 된다

![Untitled](/images/SQL_in_Jupyter_Notebook/Untitled%206.png)

- markdown으로 변환해서 블로그에 올려도 잘 나오는 것을 확인할 수 있다
    
    ![Untitled](/images/SQL_in_Jupyter_Notebook/Untitled%207.png)
    
- 참고1 : [https://95pbj.tistory.com/47](https://95pbj.tistory.com/47)
- 참고2 : [https://towardsdatascience.com/heres-how-to-run-sql-in-jupyter-notebooks-f26eb90f3259](https://towardsdatascience.com/heres-how-to-run-sql-in-jupyter-notebooks-f26eb90f3259)