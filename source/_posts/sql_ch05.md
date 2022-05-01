---
title: "5장. 그룹 쿼리와 집합 연산자 알아 보기"
author: "Hanjeongin"
date: 2022-04-28 18:00:00
tags: SQL
---

출처 : 오라클 SQL과 PL/SQL을 다루는 기술

# 그룹 쿼리와 집합 연산자 알아 보기


```python
%load_ext sql
```

    The sql extension is already loaded. To reload it, use:
      %reload_ext sql
    


```python
%sql oracle://ora_user:sirin@127.0.0.1:1521/myoracle
```

# 기본 집계 함수

## COUNT(expr)

- 쿼리 결과 건수, 즉 전체 로우 수를 반환 (행 갯수)
- NULL값은 계산이 되지 않는다


```python
%sql SELECT count(*) FROM employees
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>COUNT(*)</th>
    </tr>
    <tr>
        <td>107</td>
    </tr>
</table>




```python
%sql SELECT count(employee_id) FROM employees
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>COUNT(EMPLOYEE_ID)</th>
    </tr>
    <tr>
        <td>107</td>
    </tr>
</table>




```sql
%%sql

-- NULL이 들어있는 컬럼

SELECT count(department_id) FROM employees
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>COUNT(DEPARTMENT_ID)</th>
    </tr>
    <tr>
        <td>106</td>
    </tr>
</table>



- DISTICT를 붙이면 뒤따라 나오는 컬럼에 있는 유일한 값만 조회된다


```sql
%%sql

SELECT count(DISTINCT department_id)
FROM employees
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>COUNT(DISTINCTDEPARTMENT_ID)</th>
    </tr>
    <tr>
        <td>11</td>
    </tr>
</table>




```sql
%%sql

SELECT DISTINCT department_id
FROM employees
ORDER BY 1
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
    </tr>
    <tr>
        <td>10</td>
    </tr>
    <tr>
        <td>20</td>
    </tr>
    <tr>
        <td>30</td>
    </tr>
    <tr>
        <td>40</td>
    </tr>
    <tr>
        <td>50</td>
    </tr>
    <tr>
        <td>60</td>
    </tr>
    <tr>
        <td>70</td>
    </tr>
    <tr>
        <td>80</td>
    </tr>
    <tr>
        <td>90</td>
    </tr>
    <tr>
        <td>100</td>
    </tr>
    <tr>
        <td>110</td>
    </tr>
    <tr>
        <td>None</td>
    </tr>
</table>



## SUM(expr)

- 합계를 반환하는 함수


```sql
%%sql

SELECT
    SUM(salary)
    , SUM(DISTINCT salary)
FROM employees
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>SUM(SALARY)</th>
        <th>SUM(DISTINCTSALARY)</th>
    </tr>
    <tr>
        <td>691416</td>
        <td>409908</td>
    </tr>
</table>



## AVG(expr)

- 평균값을 반환한다


```sql
%%sql

SELECT
    AVG(salary)
    , AVG(DISTINCT salary)
FROM employees
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>AVG(SALARY)</th>
        <th>AVG(DISTINCTSALARY)</th>
    </tr>
    <tr>
        <td>6461.831775700934579439252336448598130841</td>
        <td>7067.379310344827586206896551724137931034</td>
    </tr>
</table>



## MIN(expr), MAX(expr)

- 최솟값, 최댓값을 반환한다


```sql
%%sql

SELECT
    MIN(salary)
    , MAX(salary)
FROM employees
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>MIN(SALARY)</th>
        <th>MAX(SALARY)</th>
    </tr>
    <tr>
        <td>2100</td>
        <td>24000</td>
    </tr>
</table>



## VARIANCE(expr), STDDEV(expr)

- 분산과 표준편차를 반환한다


```sql
%%sql

SELECT
    VARIANCE(salary)
    , STDDEV(salary)
FROM employees
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>VARIANCE(SALARY)</th>
        <th>STDDEV(SALARY)</th>
    </tr>
    <tr>
        <td>15284813.66954681713983424440134015164874</td>
        <td>3909.579730552481921059198878167256201202</td>
    </tr>
</table>



# GROUP BY 절과 HAVING 절

- 특정 그룹으로 묶어 데이터를 집계함


```sql
%%sql

SELECT
    department_id,
    SUM(salary)
FROM employees
GROUP BY department_id
ORDER BY department_id
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>SUM(SALARY)</th>
    </tr>
    <tr>
        <td>10</td>
        <td>4400</td>
    </tr>
    <tr>
        <td>20</td>
        <td>19000</td>
    </tr>
    <tr>
        <td>30</td>
        <td>24900</td>
    </tr>
    <tr>
        <td>40</td>
        <td>6500</td>
    </tr>
    <tr>
        <td>50</td>
        <td>156400</td>
    </tr>
    <tr>
        <td>60</td>
        <td>28800</td>
    </tr>
    <tr>
        <td>70</td>
        <td>10000</td>
    </tr>
    <tr>
        <td>80</td>
        <td>304500</td>
    </tr>
    <tr>
        <td>90</td>
        <td>58000</td>
    </tr>
    <tr>
        <td>100</td>
        <td>51608</td>
    </tr>
    <tr>
        <td>110</td>
        <td>20308</td>
    </tr>
    <tr>
        <td>None</td>
        <td>7000</td>
    </tr>
</table>




```sql
%%sql

-- 2013년 지역별 가계대출 총 잔액

SELECT
    period
    , region
    , SUM(loan_jan_amt) totl_jan
FROM kor_loan_status
WHERE period LIKE '2013%' AND rownum <= 10
GROUP BY period, region
ORDER BY period, region
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>period</th>
        <th>region</th>
        <th>totl_jan</th>
    </tr>
    <tr>
        <td>201310</td>
        <td>경남</td>
        <td>36254.3</td>
    </tr>
    <tr>
        <td>201310</td>
        <td>경북</td>
        <td>21706.6</td>
    </tr>
    <tr>
        <td>201310</td>
        <td>서울</td>
        <td>128066.1</td>
    </tr>
    <tr>
        <td>201310</td>
        <td>세종</td>
        <td>2583.7</td>
    </tr>
    <tr>
        <td>201310</td>
        <td>제주</td>
        <td>5142.7</td>
    </tr>
    <tr>
        <td>201311</td>
        <td>경남</td>
        <td>36746.5</td>
    </tr>
    <tr>
        <td>201311</td>
        <td>경북</td>
        <td>22060.9</td>
    </tr>
    <tr>
        <td>201311</td>
        <td>서울</td>
        <td>128418.4</td>
    </tr>
    <tr>
        <td>201311</td>
        <td>세종</td>
        <td>2653.4</td>
    </tr>
    <tr>
        <td>201311</td>
        <td>제주</td>
        <td>5209.2</td>
    </tr>
</table>



# ROLLUP절과 CUBE 절

## ROLLUP(expr1, expr2, ...)

- expr로 명시한 표현식을 기준으로 집계한 결과, 즉 추가적인 집계 정보를 보여준다
- 순차적으로 집계를 함


```sql
%%sql

SELECT
    period
    , gubun
    , SUM(loan_jan_amt) totl_nam
FROM kor_loan_status
WHERE period LIKE '2013%'
GROUP BY period, gubun
ORDER BY period
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>period</th>
        <th>gubun</th>
        <th>totl_nam</th>
    </tr>
    <tr>
        <td>201310</td>
        <td>기타대출</td>
        <td>676078</td>
    </tr>
    <tr>
        <td>201310</td>
        <td>주택담보대출</td>
        <td>411415.9</td>
    </tr>
    <tr>
        <td>201311</td>
        <td>기타대출</td>
        <td>681121.3</td>
    </tr>
    <tr>
        <td>201311</td>
        <td>주택담보대출</td>
        <td>414236.9</td>
    </tr>
</table>




```sql
%%sql

SELECT
    period
    , gubun
    , SUM(loan_jan_amt) totl_nam
FROM kor_loan_status
WHERE period LIKE '2013%'
GROUP BY ROLLUP(period, gubun)

-- 10월 합산, 11월 합산, 전체 합산도 같이 표시된다
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>period</th>
        <th>gubun</th>
        <th>totl_nam</th>
    </tr>
    <tr>
        <td>201310</td>
        <td>기타대출</td>
        <td>676078</td>
    </tr>
    <tr>
        <td>201310</td>
        <td>주택담보대출</td>
        <td>411415.9</td>
    </tr>
    <tr>
        <td>201310</td>
        <td>None</td>
        <td>1087493.9</td>
    </tr>
    <tr>
        <td>201311</td>
        <td>기타대출</td>
        <td>681121.3</td>
    </tr>
    <tr>
        <td>201311</td>
        <td>주택담보대출</td>
        <td>414236.9</td>
    </tr>
    <tr>
        <td>201311</td>
        <td>None</td>
        <td>1095358.2</td>
    </tr>
    <tr>
        <td>None</td>
        <td>None</td>
        <td>2182852.1</td>
    </tr>
</table>



## CUBE(expr1, expr2, ...)

- CUBE는 명시한 표현식의 개수에 따라 가능한 모든 조합별로 집계한 결과를 반환한다
- 2의 (expr 수)제곱 만큼 종류별로 집계 된다


```sql
%%sql

SELECT
    period
    , gubun
    , SUM(loan_jan_amt) totl_nam
FROM kor_loan_status
WHERE period LIKE '2013%'
GROUP BY CUBE(period, gubun)

-- 10월 합산, 11월 합산, 기타대출 합산, 주택담보대출 합산, 전체 합산이 표시된다
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>period</th>
        <th>gubun</th>
        <th>totl_nam</th>
    </tr>
    <tr>
        <td>None</td>
        <td>None</td>
        <td>2182852.1</td>
    </tr>
    <tr>
        <td>None</td>
        <td>기타대출</td>
        <td>1357199.3</td>
    </tr>
    <tr>
        <td>None</td>
        <td>주택담보대출</td>
        <td>825652.8</td>
    </tr>
    <tr>
        <td>201310</td>
        <td>None</td>
        <td>1087493.9</td>
    </tr>
    <tr>
        <td>201310</td>
        <td>기타대출</td>
        <td>676078</td>
    </tr>
    <tr>
        <td>201310</td>
        <td>주택담보대출</td>
        <td>411415.9</td>
    </tr>
    <tr>
        <td>201311</td>
        <td>None</td>
        <td>1095358.2</td>
    </tr>
    <tr>
        <td>201311</td>
        <td>기타대출</td>
        <td>681121.3</td>
    </tr>
    <tr>
        <td>201311</td>
        <td>주택담보대출</td>
        <td>414236.9</td>
    </tr>
</table>



## GROUPING SETS(expr1, expr2, ...)

- 특성 항목에 대한 소계를 계산한다


```sql
%%sql

SELECT
    period
    , gubun
    , SUM(loan_jan_amt) totl_nam
FROM kor_loan_status
WHERE period LIKE '2013%'
GROUP BY GROUPING SETS(period, gubun)

-- 기간별 합산과 구분에 따른 합산을 보여준다
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>period</th>
        <th>gubun</th>
        <th>totl_nam</th>
    </tr>
    <tr>
        <td>201311</td>
        <td>None</td>
        <td>1095358.2</td>
    </tr>
    <tr>
        <td>201310</td>
        <td>None</td>
        <td>1087493.9</td>
    </tr>
    <tr>
        <td>None</td>
        <td>주택담보대출</td>
        <td>825652.8</td>
    </tr>
    <tr>
        <td>None</td>
        <td>기타대출</td>
        <td>1357199.3</td>
    </tr>
</table>



# 집합 연산자

- 먼저 한국과 일본의 수출품을 출력한다


```sql
%%sql

SELECT goods
FROM exp_goods_asia
WHERE country = '한국'
ORDER BY seq
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>goods</th>
    </tr>
    <tr>
        <td>원유제외 석유류</td>
    </tr>
    <tr>
        <td>자동차</td>
    </tr>
    <tr>
        <td>전자집적회로</td>
    </tr>
    <tr>
        <td>선박</td>
    </tr>
    <tr>
        <td>LCD</td>
    </tr>
    <tr>
        <td>자동차부품</td>
    </tr>
    <tr>
        <td>휴대전화</td>
    </tr>
    <tr>
        <td>환식탄화수소</td>
    </tr>
    <tr>
        <td>무선송신기 디스플레이 부속품</td>
    </tr>
    <tr>
        <td>철 또는 비합금강</td>
    </tr>
</table>




```sql
%%sql

SELECT goods
FROM exp_goods_asia
WHERE country = '일본'
ORDER BY seq
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>goods</th>
    </tr>
    <tr>
        <td>자동차</td>
    </tr>
    <tr>
        <td>자동차부품</td>
    </tr>
    <tr>
        <td>전자집적회로</td>
    </tr>
    <tr>
        <td>선박</td>
    </tr>
    <tr>
        <td>반도체웨이퍼</td>
    </tr>
    <tr>
        <td>화물차</td>
    </tr>
    <tr>
        <td>원유제외 석유류</td>
    </tr>
    <tr>
        <td>건설기계</td>
    </tr>
    <tr>
        <td>다이오드, 트랜지스터</td>
    </tr>
    <tr>
        <td>기계류</td>
    </tr>
</table>



## UNION

- 합집합을 의미한다


```sql
%%sql

SELECT goods
FROM exp_goods_asia
WHERE country = '한국'
UNION -- 합집합 개념 적용
SELECT goods
FROM exp_goods_asia
WHERE country = '일본'
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>goods</th>
    </tr>
    <tr>
        <td>LCD</td>
    </tr>
    <tr>
        <td>건설기계</td>
    </tr>
    <tr>
        <td>기계류</td>
    </tr>
    <tr>
        <td>다이오드, 트랜지스터</td>
    </tr>
    <tr>
        <td>무선송신기 디스플레이 부속품</td>
    </tr>
    <tr>
        <td>반도체웨이퍼</td>
    </tr>
    <tr>
        <td>선박</td>
    </tr>
    <tr>
        <td>원유제외 석유류</td>
    </tr>
    <tr>
        <td>자동차</td>
    </tr>
    <tr>
        <td>자동차부품</td>
    </tr>
    <tr>
        <td>전자집적회로</td>
    </tr>
    <tr>
        <td>철 또는 비합금강</td>
    </tr>
    <tr>
        <td>화물차</td>
    </tr>
    <tr>
        <td>환식탄화수소</td>
    </tr>
    <tr>
        <td>휴대전화</td>
    </tr>
</table>



## UNION ALL

- UNION과 비슷하지만 중복된 항목도 모두 조회한다


```sql
%%sql

SELECT goods
FROM exp_goods_asia
WHERE country = '한국'
UNION ALL -- 합집합 개념 적용
SELECT goods
FROM exp_goods_asia
WHERE country = '일본'
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>goods</th>
    </tr>
    <tr>
        <td>원유제외 석유류</td>
    </tr>
    <tr>
        <td>자동차</td>
    </tr>
    <tr>
        <td>전자집적회로</td>
    </tr>
    <tr>
        <td>선박</td>
    </tr>
    <tr>
        <td>LCD</td>
    </tr>
    <tr>
        <td>자동차부품</td>
    </tr>
    <tr>
        <td>휴대전화</td>
    </tr>
    <tr>
        <td>환식탄화수소</td>
    </tr>
    <tr>
        <td>무선송신기 디스플레이 부속품</td>
    </tr>
    <tr>
        <td>철 또는 비합금강</td>
    </tr>
    <tr>
        <td>자동차</td>
    </tr>
    <tr>
        <td>자동차부품</td>
    </tr>
    <tr>
        <td>전자집적회로</td>
    </tr>
    <tr>
        <td>선박</td>
    </tr>
    <tr>
        <td>반도체웨이퍼</td>
    </tr>
    <tr>
        <td>화물차</td>
    </tr>
    <tr>
        <td>원유제외 석유류</td>
    </tr>
    <tr>
        <td>건설기계</td>
    </tr>
    <tr>
        <td>다이오드, 트랜지스터</td>
    </tr>
    <tr>
        <td>기계류</td>
    </tr>
</table>



## INTERSECT

- 교집합을 의미한다


```sql
%%sql

SELECT goods
FROM exp_goods_asia
WHERE country = '한국'
INTERSECT -- 교집합 개념 적용
SELECT goods
FROM exp_goods_asia
WHERE country = '일본'
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>goods</th>
    </tr>
    <tr>
        <td>선박</td>
    </tr>
    <tr>
        <td>원유제외 석유류</td>
    </tr>
    <tr>
        <td>자동차</td>
    </tr>
    <tr>
        <td>자동차부품</td>
    </tr>
    <tr>
        <td>전자집적회로</td>
    </tr>
</table>



## MINUS

- 차집합을 의미한다


```sql
%%sql

SELECT goods
FROM exp_goods_asia
WHERE country = '한국'
MINUS -- 차집합 개념 적용
SELECT goods
FROM exp_goods_asia
WHERE country = '일본'
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>goods</th>
    </tr>
    <tr>
        <td>LCD</td>
    </tr>
    <tr>
        <td>무선송신기 디스플레이 부속품</td>
    </tr>
    <tr>
        <td>철 또는 비합금강</td>
    </tr>
    <tr>
        <td>환식탄화수소</td>
    </tr>
    <tr>
        <td>휴대전화</td>
    </tr>
</table>



## 집합 연산자의 제한사항

1. 집합 연산자로 연결되는 각 SELECT문의 SELECT 리스트의 개수와 데이터 타입은 일치해야 한다
2. 집합 연산자로 SELECT 문을 연결할 때 ORDER BY절은 맨 마지막 문장에서만 사용할 수 있다
3. BLOB, CLOB, BFILE 타입의 컬럼에 대해서는 집합 연산자를 사용할 수 없다
4. UNION, INTERSECT, MINUS 연산자는 LONG형 컬럼에는 사용할 수 없다
