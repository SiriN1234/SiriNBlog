---
title: "7장. 복잡한 연산 결과를 추출해 내는 고급 쿼리 다루기"
author: "Hanjeongin"
date: 2022-04-29 17:00:00
tags: SQL
---

출처 : 오라클 SQL과 PL/SQL을 다루는 기술



```python
%load_ext sql
```


```python
%sql oracle://ora_user:sirin@127.0.0.1:1521/myoracle
```

# 분석 함수와 window 함수

- 분석 함수란 테이블에 있는 로우에 대해 특정 그룹별로 집계 값을 산출할 때 사용하는 함수이다

```
분석 함수(매개변수) OVER
    (PARTITION BY expr1, expr2, ...
         ORDER BY expr3, expr4, ...
     window 절)
```

- 분석 함수 : 분석 함수 역시 특정 그룹별 집계를 담당하므로 집계 함수에 속한다
- PARTITION BY 절 : 분석 함수로 계산될 대상 로우의 그룹(파티션)을 지정한다
- ORDER BY 절 : 파티션 안에서 순서를 지정한다
- WINDOW 절 : 파티션으로 분할된 그룹에 대해서 더 상세한 그룹으로 분할할 때 사용된다

## 분석함수

### ROW_NUMBER()

- 파티션으로 분할된 그룹별로 각 로우에 대한 순번을 반환하는 함수


```sql
%%sql

SELECT
    department_id
    , emp_name
    , ROW_NUMBER() OVER(PARTITION BY department_id
                        ORDER BY department_id, emp_name) dep_rows
FROM employees
WHERE rownum <= 10
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>emp_name</th>
        <th>dep_rows</th>
    </tr>
    <tr>
        <td>10</td>
        <td>Jennifer Whalen</td>
        <td>1</td>
    </tr>
    <tr>
        <td>20</td>
        <td>Michael Hartstein</td>
        <td>1</td>
    </tr>
    <tr>
        <td>20</td>
        <td>Pat Fay</td>
        <td>2</td>
    </tr>
    <tr>
        <td>40</td>
        <td>Susan Mavris</td>
        <td>1</td>
    </tr>
    <tr>
        <td>50</td>
        <td>Donald OConnell</td>
        <td>1</td>
    </tr>
    <tr>
        <td>50</td>
        <td>Douglas Grant</td>
        <td>2</td>
    </tr>
    <tr>
        <td>70</td>
        <td>Hermann Baer</td>
        <td>1</td>
    </tr>
    <tr>
        <td>90</td>
        <td>Steven King</td>
        <td>1</td>
    </tr>
    <tr>
        <td>110</td>
        <td>Shelley Higgins</td>
        <td>1</td>
    </tr>
    <tr>
        <td>110</td>
        <td>William Gietz</td>
        <td>2</td>
    </tr>
</table>



### RANK(), DENSE_RANK()

- RANK는 파티션별 순위를 반환하고, DENSE_RANK는 같은 순위가 나오면 다음 순위가 한 번 건너뛰지 않고 매겨진다


```sql
%%sql

SELECT
    department_id
    , emp_name
    , salary
    , RANK() OVER (PARTITION BY department_id
                   ORDER BY salary) dep_rank
    , DENSE_RANK() OVER (PARTITION BY department_id
                         ORDER BY salary) dep_denserank
FROM employees
WHERE department_id <= 40
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>emp_name</th>
        <th>salary</th>
        <th>dep_rank</th>
        <th>dep_denserank</th>
    </tr>
    <tr>
        <td>10</td>
        <td>Jennifer Whalen</td>
        <td>4400</td>
        <td>1</td>
        <td>1</td>
    </tr>
    <tr>
        <td>20</td>
        <td>Pat Fay</td>
        <td>6000</td>
        <td>1</td>
        <td>1</td>
    </tr>
    <tr>
        <td>20</td>
        <td>Michael Hartstein</td>
        <td>13000</td>
        <td>2</td>
        <td>2</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Karen Colmenares</td>
        <td>2500</td>
        <td>1</td>
        <td>1</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Guy Himuro</td>
        <td>2600</td>
        <td>2</td>
        <td>2</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Sigal Tobias</td>
        <td>2800</td>
        <td>3</td>
        <td>3</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Shelli Baida</td>
        <td>2900</td>
        <td>4</td>
        <td>4</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Alexander Khoo</td>
        <td>3100</td>
        <td>5</td>
        <td>5</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Den Raphaely</td>
        <td>11000</td>
        <td>6</td>
        <td>6</td>
    </tr>
    <tr>
        <td>40</td>
        <td>Susan Mavris</td>
        <td>6500</td>
        <td>1</td>
        <td>1</td>
    </tr>
</table>



### CUME_DIST(), PERCENT_RANK()

- CUME_DIST 함수는 주어진 그룹에 대한 상대전인 누적분포도 값을 반환한다
    + 값의 범위는 0초과 1이하
- PERCENT_RANK 함수는 해당 그룹 내의 백분위 순위를 반환한다
    + 값의 범위는 0이상 1미만이다


```sql
%%sql

SELECT
    department_id
    , emp_name
    , CUME_DIST() over(PARTITION BY department_id
                       ORDER BY salary) dep_dist
FROM employees
WHERE department_id <= 40
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>emp_name</th>
        <th>dep_dist</th>
    </tr>
    <tr>
        <td>10</td>
        <td>Jennifer Whalen</td>
        <td>1</td>
    </tr>
    <tr>
        <td>20</td>
        <td>Pat Fay</td>
        <td>0.5</td>
    </tr>
    <tr>
        <td>20</td>
        <td>Michael Hartstein</td>
        <td>1</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Karen Colmenares</td>
        <td>0.1666666666666666666666666666666666666667</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Guy Himuro</td>
        <td>0.3333333333333333333333333333333333333333</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Sigal Tobias</td>
        <td>0.5</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Shelli Baida</td>
        <td>0.6666666666666666666666666666666666666667</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Alexander Khoo</td>
        <td>0.8333333333333333333333333333333333333333</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Den Raphaely</td>
        <td>1</td>
    </tr>
    <tr>
        <td>40</td>
        <td>Susan Mavris</td>
        <td>1</td>
    </tr>
</table>




```sql
%%sql

SELECT
    department_id
    , emp_name
    , salary
    , rank() OVER (PARTITION BY department_id
                   ORDER BY salary) ranking
    , CUME_DIST() OVER (PARTITION BY department_id
                        ORDER BY salary) cume_dist_value
    , PERCENT_RANK() OVER (PARTITION BY department_id
                           ORDER BY salary) percentile
FROM employees
WHERE department_id = 60
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>emp_name</th>
        <th>salary</th>
        <th>ranking</th>
        <th>cume_dist_value</th>
        <th>percentile</th>
    </tr>
    <tr>
        <td>60</td>
        <td>Diana Lorentz</td>
        <td>4200</td>
        <td>1</td>
        <td>0.2</td>
        <td>0</td>
    </tr>
    <tr>
        <td>60</td>
        <td>David Austin</td>
        <td>4800</td>
        <td>2</td>
        <td>0.6</td>
        <td>0.25</td>
    </tr>
    <tr>
        <td>60</td>
        <td>Valli Pataballa</td>
        <td>4800</td>
        <td>2</td>
        <td>0.6</td>
        <td>0.25</td>
    </tr>
    <tr>
        <td>60</td>
        <td>Bruce Ernst</td>
        <td>6000</td>
        <td>4</td>
        <td>0.8</td>
        <td>0.75</td>
    </tr>
    <tr>
        <td>60</td>
        <td>Alexander Hunold</td>
        <td>9000</td>
        <td>5</td>
        <td>1</td>
        <td>1</td>
    </tr>
</table>



### NTITLE(expr)

- 파티션별로 expr에 명시된 값 만큼 분할한 결과를 반환한다
- 예를들어 expr이 4일경우 4등분 한다


```sql
%%sql

SELECT
    department_id
    , emp_name
    , salary
    , NTILE(5) OVER (PARTITION BY department_id
                      ORDER BY salary) NTILES
FROM employees
WHERE department_id IN (30, 60)
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>emp_name</th>
        <th>salary</th>
        <th>ntiles</th>
    </tr>
    <tr>
        <td>30</td>
        <td>Karen Colmenares</td>
        <td>2500</td>
        <td>1</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Guy Himuro</td>
        <td>2600</td>
        <td>1</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Sigal Tobias</td>
        <td>2800</td>
        <td>2</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Shelli Baida</td>
        <td>2900</td>
        <td>3</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Alexander Khoo</td>
        <td>3100</td>
        <td>4</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Den Raphaely</td>
        <td>11000</td>
        <td>5</td>
    </tr>
    <tr>
        <td>60</td>
        <td>Diana Lorentz</td>
        <td>4200</td>
        <td>1</td>
    </tr>
    <tr>
        <td>60</td>
        <td>Valli Pataballa</td>
        <td>4800</td>
        <td>2</td>
    </tr>
    <tr>
        <td>60</td>
        <td>David Austin</td>
        <td>4800</td>
        <td>3</td>
    </tr>
    <tr>
        <td>60</td>
        <td>Bruce Ernst</td>
        <td>6000</td>
        <td>4</td>
    </tr>
    <tr>
        <td>60</td>
        <td>Alexander Hunold</td>
        <td>9000</td>
        <td>5</td>
    </tr>
</table>



### LAG(expr, offset, default_value), LEAD(expr, offset, default_value)

- LAG와 LEAD 함수는 주어진 그룹과 순서에 따라 다른 로우에 있는 값을 참조할 때 사용한다
- LAG는 선행 로우의 값을 참조하고 LEAD는 후행 로우의 값을 참조한다


```sql
%%sql

SELECT
    emp_name
    , hire_date
    , salary
    , LAG(salary, 1, 0) OVER (ORDER BY hire_date) AS prev_sal
    , LEAD(salary, 1, 0) OVER (ORDER BY hire_date) AS next_sal
FROM employees
WHERE department_id = 30
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>emp_name</th>
        <th>hire_date</th>
        <th>salary</th>
        <th>prev_sal</th>
        <th>next_sal</th>
    </tr>
    <tr>
        <td>Den Raphaely</td>
        <td>2002-12-07 00:00:00</td>
        <td>11000</td>
        <td>0</td>
        <td>3100</td>
    </tr>
    <tr>
        <td>Alexander Khoo</td>
        <td>2003-05-18 00:00:00</td>
        <td>3100</td>
        <td>11000</td>
        <td>2800</td>
    </tr>
    <tr>
        <td>Sigal Tobias</td>
        <td>2005-07-24 00:00:00</td>
        <td>2800</td>
        <td>3100</td>
        <td>2900</td>
    </tr>
    <tr>
        <td>Shelli Baida</td>
        <td>2005-12-24 00:00:00</td>
        <td>2900</td>
        <td>2800</td>
        <td>2600</td>
    </tr>
    <tr>
        <td>Guy Himuro</td>
        <td>2006-11-15 00:00:00</td>
        <td>2600</td>
        <td>2900</td>
        <td>2500</td>
    </tr>
    <tr>
        <td>Karen Colmenares</td>
        <td>2007-08-10 00:00:00</td>
        <td>2500</td>
        <td>2600</td>
        <td>0</td>
    </tr>
</table>



## window 절

- 파티션으로 분할된 그룹에 대해 별도로 다시 그룹, 즉 부분 집합을 만드는 역할을 한다

```
(ROWS | RANGE)
    (BETWEEN (UNBOUNDED PRECEDING
              | CURRENT ROW
              | value_expr (PRECEDING | FOLLOWING))
         AND (UNBOUNDED PRECEDING
              | CURRENT ROW
              | value_expr (PRECEDING | FOLLOWING))
| (UNBOUNDED PRECEDING
   | CURRENT ROW
   | value_expr (PRECEDING | FOLLOWING))
```

- ROWS : 로우 단위로 window 절을 지정한다
- RANGE : 로우가 아닌 논리적인 범위로 window 절을 지정한다
- BETWEEN~AND : window 절의 시작과 끝 지점을 명시한다
    + BETWEEN을 명시하지 않고 두 번째 옵션만 지정하면 이 지점이 시작 지점이 되고 끝 지점은 현재 로우가 된다
- UNBOUNDED PRECENDING : 파티션으로 구분된 첫 번째 로우가 시작 지점이 된다
- ?UNBOUNDED FOLLOWING : 파티션으로 구분된 마지막 로우가 끝 지점이 된다
- CURRENT ROW : 시작 및 끝 지점이 현재 로우가 된다
- value_expr PRECENDING : 끝 지점일 경우, 시작 지점은 value_expr PRECENDING
- value_expr FOLLOWING : 시작 지점일 경우, 끝 지점은 value_expr FOLLOWING


```sql
%%sql

/*
SUM 3개 모두 hire_date를 기준으로 정렬되엇다
SUM1은 파티션의 처음부터 끝까지 합한 값을 출력
SUM2는 파티션의 처음부터 현재 로우까지 합한 값을 출력
SUM3은 현재 로우부터 파티션의 끝까지 합한 값을 출력한다
*/

SELECT
    department_id
    , emp_name
    , hire_date
    , salary
    , SUM(salary) OVER (PARTITION BY department_id ORDER BY hire_date
                        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
                        ) AS all_salary
    , SUM(salary) OVER (PARTITION BY department_id ORDER BY hire_date
                        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
                        ) AS first_current_sal
    , SUM(salary) OVER (PARTITION BY department_id ORDER BY hire_date
                        ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING
                        ) AS current_end_sal
FROM employees
WHERE department_id IN (30, 90)
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>emp_name</th>
        <th>hire_date</th>
        <th>salary</th>
        <th>all_salary</th>
        <th>first_current_sal</th>
        <th>current_end_sal</th>
    </tr>
    <tr>
        <td>30</td>
        <td>Den Raphaely</td>
        <td>2002-12-07 00:00:00</td>
        <td>11000</td>
        <td>24900</td>
        <td>11000</td>
        <td>24900</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Alexander Khoo</td>
        <td>2003-05-18 00:00:00</td>
        <td>3100</td>
        <td>24900</td>
        <td>14100</td>
        <td>13900</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Sigal Tobias</td>
        <td>2005-07-24 00:00:00</td>
        <td>2800</td>
        <td>24900</td>
        <td>16900</td>
        <td>10800</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Shelli Baida</td>
        <td>2005-12-24 00:00:00</td>
        <td>2900</td>
        <td>24900</td>
        <td>19800</td>
        <td>8000</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Guy Himuro</td>
        <td>2006-11-15 00:00:00</td>
        <td>2600</td>
        <td>24900</td>
        <td>22400</td>
        <td>5100</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Karen Colmenares</td>
        <td>2007-08-10 00:00:00</td>
        <td>2500</td>
        <td>24900</td>
        <td>24900</td>
        <td>2500</td>
    </tr>
    <tr>
        <td>90</td>
        <td>Lex De Haan</td>
        <td>2001-01-13 00:00:00</td>
        <td>17000</td>
        <td>58000</td>
        <td>17000</td>
        <td>58000</td>
    </tr>
    <tr>
        <td>90</td>
        <td>Steven King</td>
        <td>2003-06-17 00:00:00</td>
        <td>24000</td>
        <td>58000</td>
        <td>41000</td>
        <td>41000</td>
    </tr>
    <tr>
        <td>90</td>
        <td>Neena Kochhar</td>
        <td>2005-09-21 00:00:00</td>
        <td>17000</td>
        <td>58000</td>
        <td>58000</td>
        <td>17000</td>
    </tr>
</table>



### FIRST_VALUE(expr), LAST(expr)

- 주어진 그룹 상에서 첫 번째 값과 마지막 값을 반환한다


```sql
%%sql

/*
FIRST_VALUE 3개 모두 hire_date를 기준으로 정렬되엇다
FIRST_VALUE1은 파티션의 처음부터 끝까지를 기준으로 첫 번째 값을 출력
FIRST_VALUE2는 파티션의 처음부터 현재 로우를 기준으로 첫 번째 값을 출력
FIRST_VALUE3은 현재 로우부터 파티션의 끝을 기준으로 첫 번째 값을 출력
*/

SELECT
    department_id
    , emp_name
    , hire_date
    , salary
    , FIRST_VALUE(salary) OVER (PARTITION BY department_id ORDER BY hire_date
                        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
                        ) AS all_salary
    , FIRST_VALUE(salary) OVER (PARTITION BY department_id ORDER BY hire_date
                        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
                        ) AS first_current_sal
    , FIRST_VALUE(salary) OVER (PARTITION BY department_id ORDER BY hire_date
                        ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING
                        ) AS current_end_sal
FROM employees
WHERE department_id IN (30, 90)
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>emp_name</th>
        <th>hire_date</th>
        <th>salary</th>
        <th>all_salary</th>
        <th>first_current_sal</th>
        <th>current_end_sal</th>
    </tr>
    <tr>
        <td>30</td>
        <td>Den Raphaely</td>
        <td>2002-12-07 00:00:00</td>
        <td>11000</td>
        <td>11000</td>
        <td>11000</td>
        <td>11000</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Alexander Khoo</td>
        <td>2003-05-18 00:00:00</td>
        <td>3100</td>
        <td>11000</td>
        <td>11000</td>
        <td>3100</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Sigal Tobias</td>
        <td>2005-07-24 00:00:00</td>
        <td>2800</td>
        <td>11000</td>
        <td>11000</td>
        <td>2800</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Shelli Baida</td>
        <td>2005-12-24 00:00:00</td>
        <td>2900</td>
        <td>11000</td>
        <td>11000</td>
        <td>2900</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Guy Himuro</td>
        <td>2006-11-15 00:00:00</td>
        <td>2600</td>
        <td>11000</td>
        <td>11000</td>
        <td>2600</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Karen Colmenares</td>
        <td>2007-08-10 00:00:00</td>
        <td>2500</td>
        <td>11000</td>
        <td>11000</td>
        <td>2500</td>
    </tr>
    <tr>
        <td>90</td>
        <td>Lex De Haan</td>
        <td>2001-01-13 00:00:00</td>
        <td>17000</td>
        <td>17000</td>
        <td>17000</td>
        <td>17000</td>
    </tr>
    <tr>
        <td>90</td>
        <td>Steven King</td>
        <td>2003-06-17 00:00:00</td>
        <td>24000</td>
        <td>17000</td>
        <td>17000</td>
        <td>24000</td>
    </tr>
    <tr>
        <td>90</td>
        <td>Neena Kochhar</td>
        <td>2005-09-21 00:00:00</td>
        <td>17000</td>
        <td>17000</td>
        <td>17000</td>
        <td>17000</td>
    </tr>
</table>




```sql
%%sql

/*
LAST_VALUE 3개 모두 hire_date를 기준으로 정렬되엇다
LAST_VALUE1은 파티션의 처음부터 끝까지를 기준으로 마지막 값을 출력
LAST_VALUE2는 파티션의 처음부터 현재 로우를 기준으로 마지막 값을 출력
LAST_VALUE3은 현재 로우부터 파티션의 끝을 기준으로 마지막 값을 출력
*/

SELECT
    department_id
    , emp_name
    , hire_date
    , salary
    , LAST_VALUE(salary) OVER (PARTITION BY department_id ORDER BY hire_date
                        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
                        ) AS all_salary
    , LAST_VALUE(salary) OVER (PARTITION BY department_id ORDER BY hire_date
                        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
                        ) AS first_current_sal
    , LAST_VALUE(salary) OVER (PARTITION BY department_id ORDER BY hire_date
                        ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING
                        ) AS current_end_sal
FROM employees
WHERE department_id IN (30, 90)
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>emp_name</th>
        <th>hire_date</th>
        <th>salary</th>
        <th>all_salary</th>
        <th>first_current_sal</th>
        <th>current_end_sal</th>
    </tr>
    <tr>
        <td>30</td>
        <td>Den Raphaely</td>
        <td>2002-12-07 00:00:00</td>
        <td>11000</td>
        <td>2500</td>
        <td>11000</td>
        <td>2500</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Alexander Khoo</td>
        <td>2003-05-18 00:00:00</td>
        <td>3100</td>
        <td>2500</td>
        <td>3100</td>
        <td>2500</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Sigal Tobias</td>
        <td>2005-07-24 00:00:00</td>
        <td>2800</td>
        <td>2500</td>
        <td>2800</td>
        <td>2500</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Shelli Baida</td>
        <td>2005-12-24 00:00:00</td>
        <td>2900</td>
        <td>2500</td>
        <td>2900</td>
        <td>2500</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Guy Himuro</td>
        <td>2006-11-15 00:00:00</td>
        <td>2600</td>
        <td>2500</td>
        <td>2600</td>
        <td>2500</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Karen Colmenares</td>
        <td>2007-08-10 00:00:00</td>
        <td>2500</td>
        <td>2500</td>
        <td>2500</td>
        <td>2500</td>
    </tr>
    <tr>
        <td>90</td>
        <td>Lex De Haan</td>
        <td>2001-01-13 00:00:00</td>
        <td>17000</td>
        <td>17000</td>
        <td>17000</td>
        <td>17000</td>
    </tr>
    <tr>
        <td>90</td>
        <td>Steven King</td>
        <td>2003-06-17 00:00:00</td>
        <td>24000</td>
        <td>17000</td>
        <td>24000</td>
        <td>17000</td>
    </tr>
    <tr>
        <td>90</td>
        <td>Neena Kochhar</td>
        <td>2005-09-21 00:00:00</td>
        <td>17000</td>
        <td>17000</td>
        <td>17000</td>
        <td>17000</td>
    </tr>
</table>



### NTH_VALUE(measure_expr, n)

- 주어진 그룹에서 n번째 로우에 해당하는 measure_expr 값을 반환한다


```sql
%%sql

/*
NTH_VALUE 3개 모두 hire_date를 기준으로 정렬되엇다
NTH_VALUE1은 파티션의 처음부터 끝까지를 기준으로 2번째 값을 출력
NTH_VALUE2는 파티션의 처음부터 현재 로우를 기준으로 3번째 값을 출력 (앞 2개는 3번째 값이 없으므로 None으로 나왔다)
NTH_VALUE3은 현재 로우부터 파티션의 끝을 기준으로 2번째 값을 출력 (끝의 값들은 다음값이 없으므로 None이 나왔다)
*/

SELECT
    department_id
    , emp_name
    , hire_date
    , salary
    , NTH_VALUE(salary, 2) OVER (PARTITION BY department_id ORDER BY hire_date
                        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
                        ) AS all_salary
    , NTH_VALUE(salary, 3) OVER (PARTITION BY department_id ORDER BY hire_date
                        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
                        ) AS first_current_sal
    , NTH_VALUE(salary, 2) OVER (PARTITION BY department_id ORDER BY hire_date
                        ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING
                        ) AS current_end_sal
FROM employees
WHERE department_id IN (30, 90)
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>emp_name</th>
        <th>hire_date</th>
        <th>salary</th>
        <th>all_salary</th>
        <th>first_current_sal</th>
        <th>current_end_sal</th>
    </tr>
    <tr>
        <td>30</td>
        <td>Den Raphaely</td>
        <td>2002-12-07 00:00:00</td>
        <td>11000</td>
        <td>3100</td>
        <td>None</td>
        <td>3100</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Alexander Khoo</td>
        <td>2003-05-18 00:00:00</td>
        <td>3100</td>
        <td>3100</td>
        <td>None</td>
        <td>2800</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Sigal Tobias</td>
        <td>2005-07-24 00:00:00</td>
        <td>2800</td>
        <td>3100</td>
        <td>2800</td>
        <td>2900</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Shelli Baida</td>
        <td>2005-12-24 00:00:00</td>
        <td>2900</td>
        <td>3100</td>
        <td>2800</td>
        <td>2600</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Guy Himuro</td>
        <td>2006-11-15 00:00:00</td>
        <td>2600</td>
        <td>3100</td>
        <td>2800</td>
        <td>2500</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Karen Colmenares</td>
        <td>2007-08-10 00:00:00</td>
        <td>2500</td>
        <td>3100</td>
        <td>2800</td>
        <td>None</td>
    </tr>
    <tr>
        <td>90</td>
        <td>Lex De Haan</td>
        <td>2001-01-13 00:00:00</td>
        <td>17000</td>
        <td>24000</td>
        <td>None</td>
        <td>24000</td>
    </tr>
    <tr>
        <td>90</td>
        <td>Steven King</td>
        <td>2003-06-17 00:00:00</td>
        <td>24000</td>
        <td>24000</td>
        <td>None</td>
        <td>17000</td>
    </tr>
    <tr>
        <td>90</td>
        <td>Neena Kochhar</td>
        <td>2005-09-21 00:00:00</td>
        <td>17000</td>
        <td>24000</td>
        <td>17000</td>
        <td>None</td>
    </tr>
</table>



# 계층형 쿼리

- 2차원 형태의 테이블에 저장된 데이터를 계층형 구조로 결과를 반환하는 쿼리를 말한다

## 계층형 쿼리

```
SELECT expr1, expr2, ...
FROM 테이블
WHERE 조건
START WITH[최상위 조건]
CONNECT BY [NOCYCLE][PRIOR 계층형 구조 조건];
```

```
SELECT
    department_id
    , LPAD(' ', 3 * (LEVEL - 1)) || department_name, LEVEL
FROM departments
START WITH parent_id IS NULL
CONNECT BY PRIOR department_id = parent_id;
```

![image.png](data:image/png;base64,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)

### 테이블 만들기

- manager_id는 해당 사원의 매니저 사번이 있다
- 각 매니저는 사원 ID가 있다
- 사원별 계층 구조를 만들고, 부서 테이블과 조인해서 부서명까지 조회한다

```
SELECT
    a.employee_id
    , LPAD(' ', 3 * (LEVEL - 1)) || a.emp_name
    , LEVEL
    , b.department_name
FROM
    employees a
    , departments b
WHERE a.department_id = b.department_id
START WITH a.manager_id IS NULL
CONNECT BY PRIOR a.employee_id = a.manager_id;
```

![image.png](data:image/png;base64,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)

- WHERE절에 부서 번호 30번을 조회하라는 조건을 걸어준다

```
SELECT
    a.employee_id
    , LPAD(' ', 3 * (LEVEL - 1)) || a.emp_name
    , LEVEL
    , b.department_name
FROM
    employees a
    , departments b
WHERE a.department_id = b.department_id
  AND a.department_id = 30
START WITH a.manager_id IS NULL
CONNECT BY NOCYCLE PRIOR a.employee_id = a.manager_id;
```

![image.png](data:image/png;base64,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)

- CONNECT BY절에 부서 번호가 30이라는 조건을 걸어준다

```
SELECT
    a.employee_id
    , LPAD(' ', 3 * (LEVEL - 1)) || a.emp_name
    , LEVEL
    , b.department_name
FROM
    employees a
    , departments b
WHERE a.department_id = b.department_id
START WITH a.manager_id IS NULL
CONNECT BY NOCYCLE PRIOR a.employee_id = a.manager_id
AND a.department_id = 30;
```

![image.png](data:image/png;base64,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)

- CONNECT BY PRIOR 자식컬럼 = 부모컬럼 : 부모에서 자식으로 트리 구성(TOP DOWN)
- CONNECT BY PRIOR 부모컬럼 = 자식컬럼 : 자식에서 부모로 트리 구성 (BOTTOM UP)
- CONNECT BY NOCYCLE PRIOR = 무한루프 방지


```sql
%%sql

SELECT
    a.employee_id
    , LPAD(' ', 3 * (LEVEL - 1)) || a.emp_name
    , LEVEL
    , b.department_name
FROM
    employees a
    , departments b
WHERE a.department_id = b.department_id
START WITH a.manager_id IS NULL
CONNECT BY NOCYCLE PRIOR a.manager_id = a.employee_id
AND a.department_id = 30
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>employee_id</th>
        <th>LPAD(&#x27;&#x27;,3*(LEVEL-1))||A.EMP_NAME</th>
        <th>LEVEL</th>
        <th>department_name</th>
    </tr>
    <tr>
        <td>100</td>
        <td>Steven King</td>
        <td>1</td>
        <td>기획부</td>
    </tr>
</table>



## 계층형 쿼리 심화학습

### 계층형 쿼리 정렬

- 계층형 쿼리는 계층형 구조에 맞게 순서대로 출력되는데 ORDER BY 절로 그 순서를 변경할 수 있다
- 정렬과 동시에 계층형 구조까지 보존하려면 ORDER SIBLINGS BY 절을 사용해야 한다
- 레벨이 같은 형제 로우에 한해서 정렬을 수행한다


```sql
%%sql

SELECT
    department_id
    , LPAD(' ', 3 * (LEVEL - 1)) || department_name
    , LEVEL
FROM departments
START WITH parent_id IS NULL
CONNECT BY PRIOR department_id = parent_id
ORDER SIBLINGS BY department_name
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>LPAD(&#x27;&#x27;,3*(LEVEL-1))||DEPARTMENT_NAME</th>
        <th>LEVEL</th>
    </tr>
    <tr>
        <td>10</td>
        <td>총무기획부</td>
        <td>1</td>
    </tr>
    <tr>
        <td>30</td>
        <td>&nbsp;&nbsp;&nbsp;구매/생산부</td>
        <td>2</td>
    </tr>
    <tr>
        <td>210</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IT 지원</td>
        <td>3</td>
    </tr>
    <tr>
        <td>220</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;NOC</td>
        <td>3</td>
    </tr>
    <tr>
        <td>180</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;건설팀</td>
        <td>3</td>
    </tr>
    <tr>
        <td>170</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;생산팀</td>
        <td>3</td>
    </tr>
    <tr>
        <td>200</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;운영팀</td>
        <td>3</td>
    </tr>
    <tr>
        <td>90</td>
        <td>&nbsp;&nbsp;&nbsp;기획부</td>
        <td>2</td>
    </tr>
    <tr>
        <td>60</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IT</td>
        <td>3</td>
    </tr>
    <tr>
        <td>230</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IT 헬프데스크</td>
        <td>4</td>
    </tr>
    <tr>
        <td>110</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;경리부</td>
        <td>3</td>
    </tr>
    <tr>
        <td>270</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;급여팀</td>
        <td>4</td>
    </tr>
    <tr>
        <td>120</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;재무팀</td>
        <td>4</td>
    </tr>
    <tr>
        <td>100</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;자금부</td>
        <td>3</td>
    </tr>
    <tr>
        <td>130</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;세무팀</td>
        <td>4</td>
    </tr>
    <tr>
        <td>160</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;수익관리팀</td>
        <td>4</td>
    </tr>
    <tr>
        <td>140</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;신용관리팀</td>
        <td>4</td>
    </tr>
    <tr>
        <td>150</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;주식관리팀</td>
        <td>4</td>
    </tr>
    <tr>
        <td>70</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;홍보부</td>
        <td>3</td>
    </tr>
    <tr>
        <td>20</td>
        <td>&nbsp;&nbsp;&nbsp;마케팅</td>
        <td>2</td>
    </tr>
    <tr>
        <td>50</td>
        <td>&nbsp;&nbsp;&nbsp;배송부</td>
        <td>2</td>
    </tr>
    <tr>
        <td>80</td>
        <td>&nbsp;&nbsp;&nbsp;영업부</td>
        <td>2</td>
    </tr>
    <tr>
        <td>190</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;계약팀</td>
        <td>3</td>
    </tr>
    <tr>
        <td>240</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;공공 판매사업팀</td>
        <td>3</td>
    </tr>
    <tr>
        <td>250</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;판매팀</td>
        <td>3</td>
    </tr>
    <tr>
        <td>40</td>
        <td>&nbsp;&nbsp;&nbsp;인사부</td>
        <td>2</td>
    </tr>
    <tr>
        <td>260</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;채용팀</td>
        <td>3</td>
    </tr>
</table>



### CONNECT_BY_ROOT

- 계층형 쿼리에서 최상위 로우를 반환하는 연산자다


```sql
%%sql

SELECT
    department_id
    , LPAD(' ', 3 * (LEVEL - 1)) || department_name
    , LEVEL
    , CONNECT_BY_ROOT department_name AS root_name
FROM departments
START WITH parent_id IS NULL
CONNECT BY PRIOR department_id = parent_id
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>LPAD(&#x27;&#x27;,3*(LEVEL-1))||DEPARTMENT_NAME</th>
        <th>LEVEL</th>
        <th>root_name</th>
    </tr>
    <tr>
        <td>10</td>
        <td>총무기획부</td>
        <td>1</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>20</td>
        <td>&nbsp;&nbsp;&nbsp;마케팅</td>
        <td>2</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>30</td>
        <td>&nbsp;&nbsp;&nbsp;구매/생산부</td>
        <td>2</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>170</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;생산팀</td>
        <td>3</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>180</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;건설팀</td>
        <td>3</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>200</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;운영팀</td>
        <td>3</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>210</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IT 지원</td>
        <td>3</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>220</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;NOC</td>
        <td>3</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>40</td>
        <td>&nbsp;&nbsp;&nbsp;인사부</td>
        <td>2</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>260</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;채용팀</td>
        <td>3</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>&nbsp;&nbsp;&nbsp;배송부</td>
        <td>2</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>&nbsp;&nbsp;&nbsp;영업부</td>
        <td>2</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>190</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;계약팀</td>
        <td>3</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>240</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;공공 판매사업팀</td>
        <td>3</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>250</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;판매팀</td>
        <td>3</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>90</td>
        <td>&nbsp;&nbsp;&nbsp;기획부</td>
        <td>2</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>60</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IT</td>
        <td>3</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>230</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IT 헬프데스크</td>
        <td>4</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>70</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;홍보부</td>
        <td>3</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>100</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;자금부</td>
        <td>3</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>130</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;세무팀</td>
        <td>4</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>140</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;신용관리팀</td>
        <td>4</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>150</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;주식관리팀</td>
        <td>4</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>160</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;수익관리팀</td>
        <td>4</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>110</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;경리부</td>
        <td>3</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>120</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;재무팀</td>
        <td>4</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>270</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;급여팀</td>
        <td>4</td>
        <td>총무기획부</td>
    </tr>
</table>



### CONNECT_BY_ISLEAF

- CONNECT BY 조건에 정의된 관계에 따라 해당 로우가 최하위 자식 로우이면 1을, 그렇지 않으면 0을 반환하는 의사 컬럼이다


```sql
%%sql

SELECT
    department_id
    , LPAD(' ', 3 * (LEVEL - 1)) || department_name
    , LEVEL
    , CONNECT_BY_ISLEAF
FROM departments
START WITH parent_id IS NULL
CONNECT BY PRIOR department_id = parent_id
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>LPAD(&#x27;&#x27;,3*(LEVEL-1))||DEPARTMENT_NAME</th>
        <th>LEVEL</th>
        <th>connect_by_isleaf</th>
    </tr>
    <tr>
        <td>10</td>
        <td>총무기획부</td>
        <td>1</td>
        <td>0</td>
    </tr>
    <tr>
        <td>20</td>
        <td>&nbsp;&nbsp;&nbsp;마케팅</td>
        <td>2</td>
        <td>1</td>
    </tr>
    <tr>
        <td>30</td>
        <td>&nbsp;&nbsp;&nbsp;구매/생산부</td>
        <td>2</td>
        <td>0</td>
    </tr>
    <tr>
        <td>170</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;생산팀</td>
        <td>3</td>
        <td>1</td>
    </tr>
    <tr>
        <td>180</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;건설팀</td>
        <td>3</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;운영팀</td>
        <td>3</td>
        <td>1</td>
    </tr>
    <tr>
        <td>210</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IT 지원</td>
        <td>3</td>
        <td>1</td>
    </tr>
    <tr>
        <td>220</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;NOC</td>
        <td>3</td>
        <td>1</td>
    </tr>
    <tr>
        <td>40</td>
        <td>&nbsp;&nbsp;&nbsp;인사부</td>
        <td>2</td>
        <td>0</td>
    </tr>
    <tr>
        <td>260</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;채용팀</td>
        <td>3</td>
        <td>1</td>
    </tr>
    <tr>
        <td>50</td>
        <td>&nbsp;&nbsp;&nbsp;배송부</td>
        <td>2</td>
        <td>1</td>
    </tr>
    <tr>
        <td>80</td>
        <td>&nbsp;&nbsp;&nbsp;영업부</td>
        <td>2</td>
        <td>0</td>
    </tr>
    <tr>
        <td>190</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;계약팀</td>
        <td>3</td>
        <td>1</td>
    </tr>
    <tr>
        <td>240</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;공공 판매사업팀</td>
        <td>3</td>
        <td>1</td>
    </tr>
    <tr>
        <td>250</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;판매팀</td>
        <td>3</td>
        <td>1</td>
    </tr>
    <tr>
        <td>90</td>
        <td>&nbsp;&nbsp;&nbsp;기획부</td>
        <td>2</td>
        <td>0</td>
    </tr>
    <tr>
        <td>60</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IT</td>
        <td>3</td>
        <td>0</td>
    </tr>
    <tr>
        <td>230</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IT 헬프데스크</td>
        <td>4</td>
        <td>1</td>
    </tr>
    <tr>
        <td>70</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;홍보부</td>
        <td>3</td>
        <td>1</td>
    </tr>
    <tr>
        <td>100</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;자금부</td>
        <td>3</td>
        <td>0</td>
    </tr>
    <tr>
        <td>130</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;세무팀</td>
        <td>4</td>
        <td>1</td>
    </tr>
    <tr>
        <td>140</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;신용관리팀</td>
        <td>4</td>
        <td>1</td>
    </tr>
    <tr>
        <td>150</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;주식관리팀</td>
        <td>4</td>
        <td>1</td>
    </tr>
    <tr>
        <td>160</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;수익관리팀</td>
        <td>4</td>
        <td>1</td>
    </tr>
    <tr>
        <td>110</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;경리부</td>
        <td>3</td>
        <td>0</td>
    </tr>
    <tr>
        <td>120</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;재무팀</td>
        <td>4</td>
        <td>1</td>
    </tr>
    <tr>
        <td>270</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;급여팀</td>
        <td>4</td>
        <td>1</td>
    </tr>
</table>



### SYS_CONNECT_BY_PATH(colm, char)

- 계층형 쿼리에서만 사용할 수 있다
- 루트 노드에서 시작해 자신의 행까지 연결된 경로 정보를 반환한다
- 이 함수의 첫 번째 파라미터로는 컬럼이, 두 번째 파라미터인 char은 컬럼 간 구분자를 의미한다
- 두 번째 매개변수인 구분자로 해당 컬럼 값에 포함된 문자는 사용할 수 없다


```sql
%%sql

SELECT
    department_id
    , LPAD(' ', 3 * (LEVEL - 1)) || department_name
    , LEVEL
    , SYS_CONNECT_BY_PATH(department_name, '|')
FROM departments
START WITH parent_id IS NULL
CONNECT BY PRIOR department_id = parent_id
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>LPAD(&#x27;&#x27;,3*(LEVEL-1))||DEPARTMENT_NAME</th>
        <th>LEVEL</th>
        <th>SYS_CONNECT_BY_PATH(DEPARTMENT_NAME,&#x27;|&#x27;)</th>
    </tr>
    <tr>
        <td>10</td>
        <td>총무기획부</td>
        <td>1</td>
        <td>|총무기획부</td>
    </tr>
    <tr>
        <td>20</td>
        <td>&nbsp;&nbsp;&nbsp;마케팅</td>
        <td>2</td>
        <td>|총무기획부|마케팅</td>
    </tr>
    <tr>
        <td>30</td>
        <td>&nbsp;&nbsp;&nbsp;구매/생산부</td>
        <td>2</td>
        <td>|총무기획부|구매/생산부</td>
    </tr>
    <tr>
        <td>170</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;생산팀</td>
        <td>3</td>
        <td>|총무기획부|구매/생산부|생산팀</td>
    </tr>
    <tr>
        <td>180</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;건설팀</td>
        <td>3</td>
        <td>|총무기획부|구매/생산부|건설팀</td>
    </tr>
    <tr>
        <td>200</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;운영팀</td>
        <td>3</td>
        <td>|총무기획부|구매/생산부|운영팀</td>
    </tr>
    <tr>
        <td>210</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IT 지원</td>
        <td>3</td>
        <td>|총무기획부|구매/생산부|IT 지원</td>
    </tr>
    <tr>
        <td>220</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;NOC</td>
        <td>3</td>
        <td>|총무기획부|구매/생산부|NOC</td>
    </tr>
    <tr>
        <td>40</td>
        <td>&nbsp;&nbsp;&nbsp;인사부</td>
        <td>2</td>
        <td>|총무기획부|인사부</td>
    </tr>
    <tr>
        <td>260</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;채용팀</td>
        <td>3</td>
        <td>|총무기획부|인사부|채용팀</td>
    </tr>
    <tr>
        <td>50</td>
        <td>&nbsp;&nbsp;&nbsp;배송부</td>
        <td>2</td>
        <td>|총무기획부|배송부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>&nbsp;&nbsp;&nbsp;영업부</td>
        <td>2</td>
        <td>|총무기획부|영업부</td>
    </tr>
    <tr>
        <td>190</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;계약팀</td>
        <td>3</td>
        <td>|총무기획부|영업부|계약팀</td>
    </tr>
    <tr>
        <td>240</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;공공 판매사업팀</td>
        <td>3</td>
        <td>|총무기획부|영업부|공공 판매사업팀</td>
    </tr>
    <tr>
        <td>250</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;판매팀</td>
        <td>3</td>
        <td>|총무기획부|영업부|판매팀</td>
    </tr>
    <tr>
        <td>90</td>
        <td>&nbsp;&nbsp;&nbsp;기획부</td>
        <td>2</td>
        <td>|총무기획부|기획부</td>
    </tr>
    <tr>
        <td>60</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IT</td>
        <td>3</td>
        <td>|총무기획부|기획부|IT</td>
    </tr>
    <tr>
        <td>230</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IT 헬프데스크</td>
        <td>4</td>
        <td>|총무기획부|기획부|IT|IT 헬프데스크</td>
    </tr>
    <tr>
        <td>70</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;홍보부</td>
        <td>3</td>
        <td>|총무기획부|기획부|홍보부</td>
    </tr>
    <tr>
        <td>100</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;자금부</td>
        <td>3</td>
        <td>|총무기획부|기획부|자금부</td>
    </tr>
    <tr>
        <td>130</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;세무팀</td>
        <td>4</td>
        <td>|총무기획부|기획부|자금부|세무팀</td>
    </tr>
    <tr>
        <td>140</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;신용관리팀</td>
        <td>4</td>
        <td>|총무기획부|기획부|자금부|신용관리팀</td>
    </tr>
    <tr>
        <td>150</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;주식관리팀</td>
        <td>4</td>
        <td>|총무기획부|기획부|자금부|주식관리팀</td>
    </tr>
    <tr>
        <td>160</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;수익관리팀</td>
        <td>4</td>
        <td>|총무기획부|기획부|자금부|수익관리팀</td>
    </tr>
    <tr>
        <td>110</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;경리부</td>
        <td>3</td>
        <td>|총무기획부|기획부|경리부</td>
    </tr>
    <tr>
        <td>120</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;재무팀</td>
        <td>4</td>
        <td>|총무기획부|기획부|경리부|재무팀</td>
    </tr>
    <tr>
        <td>270</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;급여팀</td>
        <td>4</td>
        <td>|총무기획부|기획부|경리부|급여팀</td>
    </tr>
</table>



### CONNECT_BY_ISCYCLE

- 현재 로우가 자식을 갖고 있는데 동시에 그 자식 로우가 부모 로우이면 1을, 그렇지 않으면 0을 반환한다
- 이를 이용해 오류의 원인이 되는 데이터를 찾아 내어 수정하면 된다


```sql
%%sql

SELECT
    department_id
    , LPAD(' ', 3 * (LEVEL - 1)) || department_name
    , LEVEL
    , CONNECT_BY_ISCYCLE ISLoop
    , parent_id
FROM departments
START WITH department_id = 30
CONNECT BY NOCYCLE PRIOR department_id = parent_id
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>LPAD(&#x27;&#x27;,3*(LEVEL-1))||DEPARTMENT_NAME</th>
        <th>LEVEL</th>
        <th>isloop</th>
        <th>parent_id</th>
    </tr>
    <tr>
        <td>30</td>
        <td>구매/생산부</td>
        <td>1</td>
        <td>0</td>
        <td>10</td>
    </tr>
    <tr>
        <td>170</td>
        <td>&nbsp;&nbsp;&nbsp;생산팀</td>
        <td>2</td>
        <td>0</td>
        <td>30</td>
    </tr>
    <tr>
        <td>180</td>
        <td>&nbsp;&nbsp;&nbsp;건설팀</td>
        <td>2</td>
        <td>0</td>
        <td>30</td>
    </tr>
    <tr>
        <td>200</td>
        <td>&nbsp;&nbsp;&nbsp;운영팀</td>
        <td>2</td>
        <td>0</td>
        <td>30</td>
    </tr>
    <tr>
        <td>210</td>
        <td>&nbsp;&nbsp;&nbsp;IT 지원</td>
        <td>2</td>
        <td>0</td>
        <td>30</td>
    </tr>
    <tr>
        <td>220</td>
        <td>&nbsp;&nbsp;&nbsp;NOC</td>
        <td>2</td>
        <td>0</td>
        <td>30</td>
    </tr>
</table>



# WITH 절

## 개선된 서브 쿼리

- 별칭으로 사용한 SELECT 문의 FROM 절에 다른 SELECT 구문의 별칭 참조가 가능하다
- 별칭으로 사용한 SELECT 문의 FROM 절에 다른 SELECT 구문의 별칭 참조가 가능하다

```
WITH 별칭1 AS (SELECT 문),
     별칭2 AS (SELECT 문)
...
SELECT
FROM 별칭1, 별칭2 ...
```


```sql
%%sql

/*WITH절 없는 쿼리*/

SELECT b2.*
FROM ( SELECT period, region, sum(loan_jan_amt) jan_amt
         FROM kor_loan_status 
         GROUP BY period, region
      ) b2,      
      ( SELECT b.period,  MAX(b.jan_amt) max_jan_amt
         FROM ( SELECT period, region, sum(loan_jan_amt) jan_amt
                  FROM kor_loan_status 
                 GROUP BY period, region
              ) b,
              ( SELECT MAX(PERIOD) max_month
                  FROM kor_loan_status
                 GROUP BY SUBSTR(PERIOD, 1, 4)
              ) a
         WHERE b.period = a.max_month
         GROUP BY b.period
      ) c   
 WHERE b2.period = c.period
   AND b2.jan_amt = c.max_jan_amt
 ORDER BY 1
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>period</th>
        <th>region</th>
        <th>jan_amt</th>
    </tr>
    <tr>
        <td>201112</td>
        <td>서울</td>
        <td>334728.3</td>
    </tr>
    <tr>
        <td>201212</td>
        <td>서울</td>
        <td>331572.3</td>
    </tr>
    <tr>
        <td>201311</td>
        <td>서울</td>
        <td>334062.7</td>
    </tr>
</table>




```sql
%%sql

/*WITH절 사용*/

WITH b2 AS (SELECT
                period
                , region
                , sum(loan_jan_amt) jan_amt
            FROM kor_loan_status
            GROUP BY period, region) -- AS SELECT
     , c AS (SELECT
                b.period
                , MAX(b.jan_amt) max_jan_amt
             FROM (SELECT
                        period
                        , region
                        , sum(loan_jan_amt) jan_amt
                   FROM kor_loan_status
                   GROUP BY period, region) b
                   , (SELECT MAX(PERIOD) max_month
                      FROM kor_loan_status
                      GROUP BY SUBSTR(PERIOD, 1, 4)) a -- FROM
             WHERE b.period = a.max_month
             GROUP BY b.period) -- AS SELECT
SELECT b2.*
FROM
    b2, c
WHERE b2.period = c.period
  AND b2.jan_amt = c.max_jan_amt
ORDER BY 1
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>period</th>
        <th>region</th>
        <th>jan_amt</th>
    </tr>
    <tr>
        <td>201112</td>
        <td>서울</td>
        <td>334728.3</td>
    </tr>
    <tr>
        <td>201212</td>
        <td>서울</td>
        <td>331572.3</td>
    </tr>
    <tr>
        <td>201311</td>
        <td>서울</td>
        <td>334062.7</td>
    </tr>
</table>



## 순환 서브 쿼리

- WITH절은 표준 SQL에 포함된 내용으로 다른 종류의 DBMS에서도 사용할 수 있다
- 순환 서브 쿼리란 계층형 쿼리와 개념이 같다


```sql
%%sql

/*기존 개층형 쿼리*/

SELECT
    department_id
    , LPAD(' ', 3 * (LEVEL - 1)) || department_name
    , LEVEL
FROM departments
START WITH parent_id IS NULL
CONNECT BY PRIOR department_id = parent_id
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>LPAD(&#x27;&#x27;,3*(LEVEL-1))||DEPARTMENT_NAME</th>
        <th>LEVEL</th>
    </tr>
    <tr>
        <td>10</td>
        <td>총무기획부</td>
        <td>1</td>
    </tr>
    <tr>
        <td>20</td>
        <td>&nbsp;&nbsp;&nbsp;마케팅</td>
        <td>2</td>
    </tr>
    <tr>
        <td>30</td>
        <td>&nbsp;&nbsp;&nbsp;구매/생산부</td>
        <td>2</td>
    </tr>
    <tr>
        <td>170</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;생산팀</td>
        <td>3</td>
    </tr>
    <tr>
        <td>180</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;건설팀</td>
        <td>3</td>
    </tr>
    <tr>
        <td>200</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;운영팀</td>
        <td>3</td>
    </tr>
    <tr>
        <td>210</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IT 지원</td>
        <td>3</td>
    </tr>
    <tr>
        <td>220</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;NOC</td>
        <td>3</td>
    </tr>
    <tr>
        <td>40</td>
        <td>&nbsp;&nbsp;&nbsp;인사부</td>
        <td>2</td>
    </tr>
    <tr>
        <td>260</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;채용팀</td>
        <td>3</td>
    </tr>
    <tr>
        <td>50</td>
        <td>&nbsp;&nbsp;&nbsp;배송부</td>
        <td>2</td>
    </tr>
    <tr>
        <td>80</td>
        <td>&nbsp;&nbsp;&nbsp;영업부</td>
        <td>2</td>
    </tr>
    <tr>
        <td>190</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;계약팀</td>
        <td>3</td>
    </tr>
    <tr>
        <td>240</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;공공 판매사업팀</td>
        <td>3</td>
    </tr>
    <tr>
        <td>250</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;판매팀</td>
        <td>3</td>
    </tr>
    <tr>
        <td>90</td>
        <td>&nbsp;&nbsp;&nbsp;기획부</td>
        <td>2</td>
    </tr>
    <tr>
        <td>60</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IT</td>
        <td>3</td>
    </tr>
    <tr>
        <td>230</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IT 헬프데스크</td>
        <td>4</td>
    </tr>
    <tr>
        <td>70</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;홍보부</td>
        <td>3</td>
    </tr>
    <tr>
        <td>100</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;자금부</td>
        <td>3</td>
    </tr>
    <tr>
        <td>130</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;세무팀</td>
        <td>4</td>
    </tr>
    <tr>
        <td>140</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;신용관리팀</td>
        <td>4</td>
    </tr>
    <tr>
        <td>150</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;주식관리팀</td>
        <td>4</td>
    </tr>
    <tr>
        <td>160</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;수익관리팀</td>
        <td>4</td>
    </tr>
    <tr>
        <td>110</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;경리부</td>
        <td>3</td>
    </tr>
    <tr>
        <td>120</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;재무팀</td>
        <td>4</td>
    </tr>
    <tr>
        <td>270</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;급여팀</td>
        <td>4</td>
    </tr>
</table>



- WITH절로 표현할 때는 ORDER SIBLINGS BY절과 같은 기능이 필요함
- 이를 위해서 SEARCH 구문을 사용함
    + DEPTH FIRST BY : 같은 노드에 있는 로우, 즉 형제(sibling)로우 보다 자식 로우가 먼저 조회된다
    + BREADTH FIRST BY : 자식 로우보다 형제 로우가 먼저 조회된다
    + 같은 레벨에 있는 형제 로우일 때는 BY 다음에 명시된 컬럼 순으로 조회된다
    + SET 다음에는 가상 컬럼 형태로 최종 SELECT 절에서 사용할 수 있다


```sql
%%sql

/*WITH절 사용*/

WITH recur (department_id
            , parent_id
            , department_name
            , lvl) -- recur
     AS (SELECT
            department_id
            , parent_id
            , department_name
            , 1 AS lvl
         FROM departments
         WHERE parent_id IS NULL
         UNION ALL
         SELECT
            a.department_id
            , a.parent_id
            , a.department_name
            , b.lvl + 1
        FROM
            departments a
            , recur b
        WHERE a.parent_id = b.department_id) -- AS SELECT
SEARCH DEPTH FIRST BY department_id SET order_seq
SELECT
    department_id
    , LPAD(' ', 3 * (lvl - 1)) || department_name
    , lvl
    , order_seq
FROM recur
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>LPAD(&#x27;&#x27;,3*(LVL-1))||DEPARTMENT_NAME</th>
        <th>lvl</th>
        <th>order_seq</th>
    </tr>
    <tr>
        <td>10</td>
        <td>총무기획부</td>
        <td>1</td>
        <td>1</td>
    </tr>
    <tr>
        <td>20</td>
        <td>&nbsp;&nbsp;&nbsp;마케팅</td>
        <td>2</td>
        <td>2</td>
    </tr>
    <tr>
        <td>30</td>
        <td>&nbsp;&nbsp;&nbsp;구매/생산부</td>
        <td>2</td>
        <td>3</td>
    </tr>
    <tr>
        <td>170</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;생산팀</td>
        <td>3</td>
        <td>4</td>
    </tr>
    <tr>
        <td>180</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;건설팀</td>
        <td>3</td>
        <td>5</td>
    </tr>
    <tr>
        <td>200</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;운영팀</td>
        <td>3</td>
        <td>6</td>
    </tr>
    <tr>
        <td>210</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IT 지원</td>
        <td>3</td>
        <td>7</td>
    </tr>
    <tr>
        <td>220</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;NOC</td>
        <td>3</td>
        <td>8</td>
    </tr>
    <tr>
        <td>40</td>
        <td>&nbsp;&nbsp;&nbsp;인사부</td>
        <td>2</td>
        <td>9</td>
    </tr>
    <tr>
        <td>260</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;채용팀</td>
        <td>3</td>
        <td>10</td>
    </tr>
    <tr>
        <td>50</td>
        <td>&nbsp;&nbsp;&nbsp;배송부</td>
        <td>2</td>
        <td>11</td>
    </tr>
    <tr>
        <td>80</td>
        <td>&nbsp;&nbsp;&nbsp;영업부</td>
        <td>2</td>
        <td>12</td>
    </tr>
    <tr>
        <td>190</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;계약팀</td>
        <td>3</td>
        <td>13</td>
    </tr>
    <tr>
        <td>240</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;공공 판매사업팀</td>
        <td>3</td>
        <td>14</td>
    </tr>
    <tr>
        <td>250</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;판매팀</td>
        <td>3</td>
        <td>15</td>
    </tr>
    <tr>
        <td>90</td>
        <td>&nbsp;&nbsp;&nbsp;기획부</td>
        <td>2</td>
        <td>16</td>
    </tr>
    <tr>
        <td>60</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IT</td>
        <td>3</td>
        <td>17</td>
    </tr>
    <tr>
        <td>230</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IT 헬프데스크</td>
        <td>4</td>
        <td>18</td>
    </tr>
    <tr>
        <td>70</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;홍보부</td>
        <td>3</td>
        <td>19</td>
    </tr>
    <tr>
        <td>100</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;자금부</td>
        <td>3</td>
        <td>20</td>
    </tr>
    <tr>
        <td>130</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;세무팀</td>
        <td>4</td>
        <td>21</td>
    </tr>
    <tr>
        <td>140</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;신용관리팀</td>
        <td>4</td>
        <td>22</td>
    </tr>
    <tr>
        <td>150</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;주식관리팀</td>
        <td>4</td>
        <td>23</td>
    </tr>
    <tr>
        <td>160</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;수익관리팀</td>
        <td>4</td>
        <td>24</td>
    </tr>
    <tr>
        <td>110</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;경리부</td>
        <td>3</td>
        <td>25</td>
    </tr>
    <tr>
        <td>120</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;재무팀</td>
        <td>4</td>
        <td>26</td>
    </tr>
    <tr>
        <td>270</td>
        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;급여팀</td>
        <td>4</td>
        <td>27</td>
    </tr>
</table>


