---
title: "Self-Check 6장 7장"
author: "Hanjeongin"
date: 2022-04-29 18:00:00
tags: SQL
---

출처 : 오라클 SQL과 PL/SQL을 다루는 기술


```python
%load_ext sql
```


```python
%sql oracle://ora_user:sirin@127.0.0.1:1521/myoracle
```

# 6장 연습문제

## 1번

101번 사원에 대해 아래의 결과를 산출하는 쿼리를 작성해 보자.

---------------------------------------------------------------------------------------
사번   사원명   job명칭 job시작일자  job종료일자   job수행부서명
---------------------------------------------------------------------------------------

### 문제해결과정

사번, 사원명은 employees 테이블에 있다
job 시작일자, job 종료일자는 job_history 테이블에 있다
job 명칭은 jobs 테이블에 있다
수행부서명은 departments 테이블에 있다

employees 테이블과 departments 테이블은 department_id를 이용해 조인
departments 테이블과 job_history 테이블은 department_id를 이용해 조인
job_history 테이블과 jobs 테이블은 job_id를 이용해 조인


```sql
%%sql

SELECT
    a.employee_id
    , a.emp_name
    , b.start_date
    , b.end_date
    , c.job_title
    , d.department_name
FROM
    employees a
    , job_history b
    , jobs c
    , departments d
WHERE a.department_id = d.department_id
  AND d.department_id = b.department_id
  AND b.job_id = c.job_id
  AND a.employee_id = 101
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>employee_id</th>
        <th>emp_name</th>
        <th>start_date</th>
        <th>end_date</th>
        <th>job_title</th>
        <th>department_name</th>
    </tr>
    <tr>
        <td>101</td>
        <td>Neena Kochhar</td>
        <td>1995-09-17 00:00:00</td>
        <td>2001-06-17 00:00:00</td>
        <td>Administration Assistant</td>
        <td>기획부</td>
    </tr>
    <tr>
        <td>101</td>
        <td>Neena Kochhar</td>
        <td>2002-07-01 00:00:00</td>
        <td>2006-12-31 00:00:00</td>
        <td>Public Accountant</td>
        <td>기획부</td>
    </tr>
</table>



## 2번

아래의 쿼리를 수행하면 오류가 발생한다. 오류의 원인은 무엇인지 설명해 보자.


```sql
%%sql

SELECT
    a.employee_id
    , a.emp_name
    , b.job_id
    , b.department_id
FROM
    employees a
    , job_history b
WHERE a.employee_id = b.employee_id(+)
  AND a.department_id(+) = b.department_id
```

### 답

AND a.department_id(+) = b.department_id;에서 외부조인이 앞에 붙어서

## 3번

외부 조인을 할 때 (+)연산자를 같이사용할 수 없는데, IN 절에 사용하는 값이 한 개이면 사용할 수 있다 그 이유는 무엇인지 설명해 보자.

### 답

오라클은 IN 절에 포함된 값을 기준으로 OR로 변환한다.
예를 들어, 
   department_id IN (10, 20, 30) 은
   department_id = 10
   OR department_id = 20
   OR department_id = 30) 로 바꿔 쓸 수 있다.
   
그런데 IN절에 값이 1개인 경우, 즉 department_id IN (10)일 경우 department_id = 10 로 변환할 수 있으므로,
외부조인을 하더라도 값이 1개인 경우는 사용할 수 있다.

## 4번

다음의 쿼리를 ANSI 문법으로 변경해보자.


```sql
%%sql

SELECT a.department_id, a.department_name
FROM departments a, employees b
WHERE a.department_id = b.department_id
  AND b.salary > 3000
ORDER BY a.department_name
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>department_name</th>
    </tr>
    <tr>
        <td>60</td>
        <td>IT</td>
    </tr>
    <tr>
        <td>60</td>
        <td>IT</td>
    </tr>
    <tr>
        <td>60</td>
        <td>IT</td>
    </tr>
    <tr>
        <td>60</td>
        <td>IT</td>
    </tr>
    <tr>
        <td>60</td>
        <td>IT</td>
    </tr>
    <tr>
        <td>110</td>
        <td>경리부</td>
    </tr>
    <tr>
        <td>110</td>
        <td>경리부</td>
    </tr>
    <tr>
        <td>30</td>
        <td>구매/생산부</td>
    </tr>
    <tr>
        <td>30</td>
        <td>구매/생산부</td>
    </tr>
    <tr>
        <td>90</td>
        <td>기획부</td>
    </tr>
    <tr>
        <td>90</td>
        <td>기획부</td>
    </tr>
    <tr>
        <td>90</td>
        <td>기획부</td>
    </tr>
    <tr>
        <td>20</td>
        <td>마케팅</td>
    </tr>
    <tr>
        <td>20</td>
        <td>마케팅</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>40</td>
        <td>인사부</td>
    </tr>
    <tr>
        <td>100</td>
        <td>자금부</td>
    </tr>
    <tr>
        <td>100</td>
        <td>자금부</td>
    </tr>
    <tr>
        <td>100</td>
        <td>자금부</td>
    </tr>
    <tr>
        <td>100</td>
        <td>자금부</td>
    </tr>
    <tr>
        <td>100</td>
        <td>자금부</td>
    </tr>
    <tr>
        <td>100</td>
        <td>자금부</td>
    </tr>
    <tr>
        <td>10</td>
        <td>총무기획부</td>
    </tr>
    <tr>
        <td>70</td>
        <td>홍보부</td>
    </tr>
</table>



### 문제해결과정

2번째 테이블을 INNER JOIN 뒤에 쓰고 WHERE절의 첫 번째 조건을 ON 뒤로 옮겨준다


```sql
%%sql

SELECT
    a.department_id
    , a.department_name
FROM
    departments a
INNER JOIN employees b
        ON (a.department_id = b.department_id)
WHERE b.salary > 3000
ORDER BY a.department_name
```

## 5번

다음은 연관성 있는 서브 쿼리다. 이를 연관성 없는 서브 쿼리로 변환해 보자.


```sql
%%sql

SELECT a.department_id, a.department_name
 FROM departments a
WHERE EXISTS ( SELECT 1 
                 FROM job_history b
                WHERE a.department_id = b.department_id )
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>department_name</th>
    </tr>
    <tr>
        <td>20</td>
        <td>마케팅</td>
    </tr>
    <tr>
        <td>50</td>
        <td>배송부</td>
    </tr>
    <tr>
        <td>60</td>
        <td>IT</td>
    </tr>
    <tr>
        <td>80</td>
        <td>영업부</td>
    </tr>
    <tr>
        <td>90</td>
        <td>기획부</td>
    </tr>
    <tr>
        <td>110</td>
        <td>경리부</td>
    </tr>
</table>



### 문제해결과정

메인 쿼리에 쓰이는 테이블의 값이 서브 쿼리에 들어가지 않도록 바꿔준다


```sql
%%sql

SELECT
    department_id
    , department_name
FROM departments
WHERE department_id IN (SELECT department_id
                        FROM job_history)
```

## 6번

도별 이탈리아 최대매출액과 사원을 작성하는 쿼리를 학습했다. 이 쿼리를 기준으로 최대매출액 뿐만 아니라 최소매출액과 해당 사원을 조회하는 쿼리를 작성해 보자.

### 문제해결과정

필요한 것
이탈리아 연도 매출액 사원

이탈리아는 countries 테이블에 있다
연도와 매출액은 sales 테이블의 sales_month를 이용해 만들어야 할 것 같다
사원은 employees 테이블에 있다

countries 테이블에서 sales 테이블로 바로 조인을 할 수 없기 때문에 중간에 있는 customers 테이블을 거쳐간다
countries 테이블과 customers 테이블은 country_id를 이용해 조인한다
customers 테이블과 sales 테이블은 cust_id를 이용해 조인한다
sales 테이블과 employees 테이블은 employee_id를 이용해 조인한다

#### (1)

연도, 사원별 이탈리아 매출액 구하기
연도는 sales 테이블의 sales_month를 이용하고 사원은 customers 테이블의 employee_id,
연간 매출액은 sales 테이블의 amount_sold을 이용하고 이탈리아는 countries 테이블의 country_name을 이용해 구한다


```sql
%%sql

SELECT
    SUBSTR(a.sales_month, 1, 4) AS years
    , a.employee_id
    , SUM(a.amount_sold) AS amount_sold
FROM
    sales a
    , customers b
    , countries c
WHERE a.cust_id = b.cust_id
  AND b.country_id = c.country_id
  AND c.country_name = 'Italy'
GROUP BY SUBSTR(a.sales_month, 1, 4), a.employee_id
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>years</th>
        <th>employee_id</th>
        <th>amount_sold</th>
    </tr>
    <tr>
        <td>1998</td>
        <td>1</td>
        <td>5404.82</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>170</td>
        <td>10822.62</td>
    </tr>
    <tr>
        <td>1999</td>
        <td>150</td>
        <td>132543.37</td>
    </tr>
    <tr>
        <td>1999</td>
        <td>175</td>
        <td>126982.7</td>
    </tr>
    <tr>
        <td>1999</td>
        <td>160</td>
        <td>72663.95</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>171</td>
        <td>62765.84</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>176</td>
        <td>69330.91</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>153</td>
        <td>142987.82</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>167</td>
        <td>171591.21</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>153</td>
        <td>2443.72</td>
    </tr>
    <tr>
        <td>1998</td>
        <td>156</td>
        <td>121910.05</td>
    </tr>
    <tr>
        <td>1999</td>
        <td>168</td>
        <td>100289.54</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>172</td>
        <td>96872.2</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>169</td>
        <td>102573.99</td>
    </tr>
    <tr>
        <td>1999</td>
        <td>145</td>
        <td>4646.74</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>163</td>
        <td>92604.53</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>179</td>
        <td>123200.62</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>149</td>
        <td>138663.65</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>154</td>
        <td>83216.54</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>176</td>
        <td>9718.24</td>
    </tr>
    <tr>
        <td>1998</td>
        <td>157</td>
        <td>203236.79</td>
    </tr>
    <tr>
        <td>1999</td>
        <td>162</td>
        <td>94310.98</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>175</td>
        <td>24371.95</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>177</td>
        <td>90685.56</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>173</td>
        <td>426018.7</td>
    </tr>
    <tr>
        <td>1998</td>
        <td>174</td>
        <td>258841.2</td>
    </tr>
    <tr>
        <td>1998</td>
        <td>158</td>
        <td>224240.07</td>
    </tr>
    <tr>
        <td>1999</td>
        <td>159</td>
        <td>163403.76</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>170</td>
        <td>79950.04</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>150</td>
        <td>76638.75</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>148</td>
        <td>64039.34</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>155</td>
        <td>32005</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>161</td>
        <td>53927.44</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>166</td>
        <td>141683.55</td>
    </tr>
    <tr>
        <td>1998</td>
        <td>145</td>
        <td>311761.02</td>
    </tr>
    <tr>
        <td>1999</td>
        <td>147</td>
        <td>193319.44</td>
    </tr>
    <tr>
        <td>1999</td>
        <td>146</td>
        <td>89197.23</td>
    </tr>
    <tr>
        <td>1999</td>
        <td>151</td>
        <td>39957.53</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>162</td>
        <td>16259.5</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>154</td>
        <td>116769.25</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>171</td>
        <td>14145.59</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>148</td>
        <td>1557.54</td>
    </tr>
    <tr>
        <td>1999</td>
        <td>152</td>
        <td>97859.24</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>160</td>
        <td>77893.94</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>165</td>
        <td>145408.5</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>164</td>
        <td>145790.31</td>
    </tr>
</table>



#### (2)

(1)을 이용해서 연도별 최대 매출액, 최소 매출액을 구한다


```sql
%%sql

SELECT
    years
    , MAX(amount_sold) AS max_sold
    , MIN(amount_sold) AS min_sold
FROM
    (SELECT
        SUBSTR(a.sales_month, 1, 4) AS years
        , a.employee_id
        , SUM(a.amount_sold) AS amount_sold
    FROM
        sales a
        , customers b
        , countries c
    WHERE a.cust_id = b.cust_id
    AND b.country_id = c.country_id
    AND c.country_name = 'Italy'
    GROUP BY SUBSTR(a.sales_month, 1, 4), a.employee_id) K
GROUP BY years
ORDER BY years
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>years</th>
        <th>max_sold</th>
        <th>min_sold</th>
    </tr>
    <tr>
        <td>1998</td>
        <td>311761.02</td>
        <td>5404.82</td>
    </tr>
    <tr>
        <td>1999</td>
        <td>193319.44</td>
        <td>4646.74</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>142987.82</td>
        <td>16259.5</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>426018.7</td>
        <td>1557.54</td>
    </tr>
</table>



#### (3)

(1)의 결과와 (2)의 결과를 조인해서 최대매출, 최소매출액을 일으킨 사원을 찾아야 하므로, (1)과 (2) 결과를 인라인 뷰로 만든다


```sql
%%sql

SELECT
    emp.years
    , emp.amount_sold
FROM
    (SELECT
        SUBSTR(a.sales_month, 1, 4) AS years
        , a.employee_id
        , SUM(a.amount_sold) AS amount_sold
     FROM
         sales a
         , customers b
         , countries c
     WHERE a.cust_id = b.cust_id
     AND b.country_id = c.country_id
     AND c.country_name = 'Italy'
     GROUP BY SUBSTR(a.sales_month, 1, 4), a.employee_id) emp
     , (SELECT
           years
           , MAX(amount_sold) AS max_sold
           , MIN(amount_sold) AS min_sold
        FROM
            (SELECT
                SUBSTR(a.sales_month, 1, 4) AS years
                , a.employee_id
                , SUM(a.amount_sold) AS amount_sold
            FROM
                sales a
                , customers b
                , countries c
            WHERE a.cust_id = b.cust_id
            AND b.country_id = c.country_id
            AND c.country_name = 'Italy'
            GROUP BY SUBSTR(a.sales_month, 1, 4), a.employee_id) K
        GROUP BY years) sale
WHERE emp.years = sale.years
  AND (emp.amount_sold = sale.max_sold OR emp.amount_sold = sale.min_sold)
ORDER BY years
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>years</th>
        <th>amount_sold</th>
    </tr>
    <tr>
        <td>1998</td>
        <td>311761.02</td>
    </tr>
    <tr>
        <td>1998</td>
        <td>5404.82</td>
    </tr>
    <tr>
        <td>1999</td>
        <td>4646.74</td>
    </tr>
    <tr>
        <td>1999</td>
        <td>193319.44</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>142987.82</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>16259.5</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>426018.7</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>1557.54</td>
    </tr>
</table>



#### (4)

마지막으로 (3)의 결과와 사원 테이블을 조인해서 사원 이름을 가져온다


```sql
%%sql

SELECT
    emp.years
    , emp2.emp_name
    , emp.amount_sold
FROM
    (SELECT
        SUBSTR(a.sales_month, 1, 4) as years
        , a.employee_id
        , SUM(a.amount_sold) AS amount_sold
     FROM
        sales a
        , customers b
        , countries c
     WHERE a.cust_id = b.cust_id
       AND b.country_id = c.country_id
       AND c.country_name = 'Italy'
    GROUP BY SUBSTR(a.sales_month, 1, 4), a.employee_id) emp
    , (SELECT
          years
         , MAX(amount_sold) AS max_sold
         , MIN(amount_sold) AS min_sold
       FROM
          (SELECT
              SUBSTR(a.sales_month, 1, 4) as years
              , a.employee_id
              , SUM(a.amount_sold) AS amount_sold
           FROM
              sales a
              , customers b
              , countries c
           WHERE a.cust_id = b.cust_id
             AND b.country_id = c.country_id
             AND c.country_name = 'Italy'
           GROUP BY SUBSTR(a.sales_month, 1, 4), a.employee_id) K
        GROUP BY years) sale
    , employees emp2
WHERE emp.years = sale.years
  AND (emp.amount_sold = sale.max_sold OR emp.amount_sold = sale.min_sold)
  AND emp.employee_id = emp2.employee_id
ORDER BY years
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>years</th>
        <th>emp_name</th>
        <th>amount_sold</th>
    </tr>
    <tr>
        <td>1998</td>
        <td>John Russell</td>
        <td>311761.02</td>
    </tr>
    <tr>
        <td>1999</td>
        <td>John Russell</td>
        <td>4646.74</td>
    </tr>
    <tr>
        <td>1999</td>
        <td>Alberto Errazuriz</td>
        <td>193319.44</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>Christopher Olsen</td>
        <td>142987.82</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>Clara Vishney</td>
        <td>16259.5</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>Sundita Kumar</td>
        <td>426018.7</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>Gerald Cambrault</td>
        <td>1557.54</td>
    </tr>
</table>



# 7장 연습문제

## 1번

계층형 쿼리 응용편에서 LISTAGG 함수를 사용해 다음과 같이 로우를 컬럼으로 분리했다

```
SELECT department_id,
         LISTAGG(emp_name, ',') WITHIN GROUP (ORDER BY emp_name) as empnames
    FROM employees
   WHERE department_id IS NOT NULL
   GROUP BY department_id;
```

LISTAGG 함수 대신 계층형 쿼리, 분석함수를 사용해서 위 쿼리와 동일한 결과를 산출하는 쿼리를 작성해 보자

### 문제해결과정


```sql
%%sql

SELECT
    department_id
    , SUBSTR(SYS_CONNECT_BY_PATH(emp_name, ','), 2) empnames
FROM
    (SELECT emp_name
            , department_id
            , COUNT(*) OVER (PARTITION BY department_id) cnt
            , ROW_NUMBER () OVER (PARTITION BY department_id ORDER BY emp_name) rowseq
     FROM employees
     WHERE department_id IS NOT NULL)
WHERE rowseq = cnt
START WITH rowseq = 1
CONNECT BY PRIOR rowseq + 1 = rowseq
AND PRIOR department_id = department_id
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>department_id</th>
        <th>empnames</th>
    </tr>
    <tr>
        <td>10</td>
        <td>Jennifer Whalen</td>
    </tr>
    <tr>
        <td>20</td>
        <td>Michael Hartstein,Pat Fay</td>
    </tr>
    <tr>
        <td>30</td>
        <td>Alexander Khoo,Den Raphaely,Guy Himuro,Karen Colmenares,Shelli Baida,Sigal Tobias</td>
    </tr>
    <tr>
        <td>40</td>
        <td>Susan Mavris</td>
    </tr>
    <tr>
        <td>50</td>
        <td>Adam Fripp,Alana Walsh,Alexis Bull,Anthony Cabrio,Britney Everett,Curtis Davies,Donald OConnell,Douglas Grant,Girard Geoni,Hazel Philtanker,Irene Mikkilineni,James Landry,James Marlow,Jason Mallin,Jean Fleaur,Jennifer Dilly,John Seo,Joshua Patel,Julia Dellinger,Julia Nayer,Kelly Chung,Kevin Feeney,Kevin Mourgos,Ki Gee,Laura Bissot,Martha Sullivan,Matthew Weiss,Michael Rogers,Mozhe Atkinson,Nandita Sarchand,Payam Kaufling,Peter Vargas,Randall Matos,Randall Perkins,Renske Ladwig,Samuel McCain,Sarah Bell,Shanta Vollman,Stephen Stiles,Steven Markle,TJ Olson,Timothy Gates,Trenna Rajs,Vance Jones,Winston Taylor</td>
    </tr>
    <tr>
        <td>60</td>
        <td>Alexander Hunold,Bruce Ernst,David Austin,Diana Lorentz,Valli Pataballa</td>
    </tr>
    <tr>
        <td>70</td>
        <td>Hermann Baer</td>
    </tr>
    <tr>
        <td>80</td>
        <td>Alberto Errazuriz,Allan McEwen,Alyssa Hutton,Amit Banda,Charles Johnson,Christopher Olsen,Clara Vishney,Danielle Greene,David Bernstein,David Lee,Eleni Zlotkey,Elizabeth Bates,Ellen Abel,Gerald Cambrault,Harrison Bloom,Jack Livingston,Janette King,John Russell,Jonathon Taylor,Karen Partners,Lindsey Smith,Lisa Ozer,Louise Doran,Mattea Marvins,Nanette Cambrault,Oliver Tuvault,Patrick Sully,Peter Hall,Peter Tucker,Sarath Sewall,Sundar Ande,Sundita Kumar,Tayler Fox,William Smith</td>
    </tr>
    <tr>
        <td>90</td>
        <td>Lex De Haan,Neena Kochhar,Steven King</td>
    </tr>
    <tr>
        <td>100</td>
        <td>Daniel Faviet,Ismael Sciarra,John Chen,Jose Manuel Urman,Luis Popp,Nancy Greenberg</td>
    </tr>
    <tr>
        <td>110</td>
        <td>Shelley Higgins,William Gietz</td>
    </tr>
</table>



## 2번

아래의 쿼리는 사원테이블에서 JOB_ID가 'SH_CLERK'인 사원을 조회하는 쿼리이다

```
SELECT employee_id, emp_name, hire_date
FROM employees
WHERE job_id = 'SH_CLERK'
ORDER By hire_date; 
```

사원테이블에서 퇴사일자(retire_date)는 모두 비어있는데, 위 결과에서 사원번호가 184인 사원의 퇴사일자는 다음으로 입사일자가 빠른 192번 사원의 입사일자라고 가정해서 다음과 같은 형태로 결과를 추출해낼 수 있도록 쿼리를 작성해 보자. (입사일자가 가장 최근인 183번 사원의 퇴사일자는 NULL이다)

EMPLOYEE_ID EMP_NAME             HIRE_DATE             RETIRE_DATE
----------- -------------------- -------------------  ---------------------------
        184 Nandita Sarchand     2004/01/27 00:00:00  2004/02/04 00:00:00
        192 Sarah Bell           2004/02/04 00:00:00  2005/02/20 00:00:00
        185 Alexis Bull          2005/02/20 00:00:00  2005/03/03 00:00:00
        193 Britney Everett      2005/03/03 00:00:00  2005/06/14 00:00:00
        188 Kelly Chung          2005/06/14 00:00:00  2005/08/13 00:00:00
....        
....
        199 Douglas Grant        2008/01/13 00:00:00  2008/02/03 00:00:00
        183 Girard Geoni         2008/02/03 00:00:00

### 문제해결과정

2번째 hire_date를 1번째 retire_date로 3번째 hire_date는 2번째 retire_date로 이런식으로 반복하면 된다
후행 로우의 값을 참조할 때는 LEAD를 쓴다


```sql
%%sql

SELECT
    EMPLOYEE_ID
    , EMP_NAME
    , HIRE_DATE
    , LEAD(hire_date) OVER (PARTITION BY job_id ORDER BY hire_date) AS retire_date
FROM employees
WHERE job_id = 'SH_CLERK'
ORDER By hire_date
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>employee_id</th>
        <th>emp_name</th>
        <th>hire_date</th>
        <th>retire_date</th>
    </tr>
    <tr>
        <td>184</td>
        <td>Nandita Sarchand</td>
        <td>2004-01-27 00:00:00</td>
        <td>2004-02-04 00:00:00</td>
    </tr>
    <tr>
        <td>192</td>
        <td>Sarah Bell</td>
        <td>2004-02-04 00:00:00</td>
        <td>2005-02-20 00:00:00</td>
    </tr>
    <tr>
        <td>185</td>
        <td>Alexis Bull</td>
        <td>2005-02-20 00:00:00</td>
        <td>2005-03-03 00:00:00</td>
    </tr>
    <tr>
        <td>193</td>
        <td>Britney Everett</td>
        <td>2005-03-03 00:00:00</td>
        <td>2005-06-14 00:00:00</td>
    </tr>
    <tr>
        <td>188</td>
        <td>Kelly Chung</td>
        <td>2005-06-14 00:00:00</td>
        <td>2005-08-13 00:00:00</td>
    </tr>
    <tr>
        <td>189</td>
        <td>Jennifer Dilly</td>
        <td>2005-08-13 00:00:00</td>
        <td>2006-01-24 00:00:00</td>
    </tr>
    <tr>
        <td>180</td>
        <td>Winston Taylor</td>
        <td>2006-01-24 00:00:00</td>
        <td>2006-02-23 00:00:00</td>
    </tr>
    <tr>
        <td>181</td>
        <td>Jean Fleaur</td>
        <td>2006-02-23 00:00:00</td>
        <td>2006-04-24 00:00:00</td>
    </tr>
    <tr>
        <td>196</td>
        <td>Alana Walsh</td>
        <td>2006-04-24 00:00:00</td>
        <td>2006-05-23 00:00:00</td>
    </tr>
    <tr>
        <td>197</td>
        <td>Kevin Feeney</td>
        <td>2006-05-23 00:00:00</td>
        <td>2006-06-24 00:00:00</td>
    </tr>
    <tr>
        <td>186</td>
        <td>Julia Dellinger</td>
        <td>2006-06-24 00:00:00</td>
        <td>2006-07-01 00:00:00</td>
    </tr>
    <tr>
        <td>194</td>
        <td>Samuel McCain</td>
        <td>2006-07-01 00:00:00</td>
        <td>2006-07-11 00:00:00</td>
    </tr>
    <tr>
        <td>190</td>
        <td>Timothy Gates</td>
        <td>2006-07-11 00:00:00</td>
        <td>2007-02-07 00:00:00</td>
    </tr>
    <tr>
        <td>187</td>
        <td>Anthony Cabrio</td>
        <td>2007-02-07 00:00:00</td>
        <td>2007-03-17 00:00:00</td>
    </tr>
    <tr>
        <td>195</td>
        <td>Vance Jones</td>
        <td>2007-03-17 00:00:00</td>
        <td>2007-06-21 00:00:00</td>
    </tr>
    <tr>
        <td>182</td>
        <td>Martha Sullivan</td>
        <td>2007-06-21 00:00:00</td>
        <td>2007-06-21 00:00:00</td>
    </tr>
    <tr>
        <td>198</td>
        <td>Donald OConnell</td>
        <td>2007-06-21 00:00:00</td>
        <td>2007-12-19 00:00:00</td>
    </tr>
    <tr>
        <td>191</td>
        <td>Randall Perkins</td>
        <td>2007-12-19 00:00:00</td>
        <td>2008-01-13 00:00:00</td>
    </tr>
    <tr>
        <td>199</td>
        <td>Douglas Grant</td>
        <td>2008-01-13 00:00:00</td>
        <td>2008-02-03 00:00:00</td>
    </tr>
    <tr>
        <td>183</td>
        <td>Girard Geoni</td>
        <td>2008-02-03 00:00:00</td>
        <td>None</td>
    </tr>
</table>



## 3번

sales 테이블에는 판매데이터, customers 테이블에는 고객정보가 있다. 2001년 12월(SALES_MONTH = '200112') 판매데이터 중 현재일자를 기준으로 고객의 나이(customers.cust_year_of_birth)를 계산해서 다음과 같이 연령대별 매출금액을 보여주는 쿼리를 작성해 보자.

-------------------------   
연령대    매출금액
-------------------------
10대      xxxxxx
20대      ....
30대      .... 
40대      ....
-------------------------   

### 문제해결과정

연령대를 구하려면 sysdate의 연도를 가져와서 cust_year_of_birth를 빼주면 된다
매출 금액은 sales 테이블의 amount_sold를 쓰면 된다
customers 테이블과 sales 테이블은 cust_id를 이용해 조인 해준다


```sql
%%sql

WITH basis AS (SELECT
                    WIDTH_bUCKET(to_char(sysdate, 'yyyy') - b.cust_year_of_birth, 10, 90, 8) AS old_seg
                    , TO_CHAR(SYSDATE, 'yyyy') - b.cust_year_of_birth AS olds
                    , s.amount_sold
               FROM
                    sales s
                    , customers b
               WHERE s.sales_month = '200112'
                 AND s.cust_id = b.cust_id)
    , real_data AS (SELECT
                        old_seg * 10 || '대' AS old_segment
                        , SUM(amount_sold) AS sold_seg_amt
                    FROM basis
                    GROUP BY old_seg)
SELECT *
FROM real_data
ORDER BY old_segment
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>old_segment</th>
        <th>sold_seg_amt</th>
    </tr>
    <tr>
        <td>30대</td>
        <td>97073.96</td>
    </tr>
    <tr>
        <td>40대</td>
        <td>316936.49</td>
    </tr>
    <tr>
        <td>50대</td>
        <td>348209.7</td>
    </tr>
    <tr>
        <td>60대</td>
        <td>603012.69</td>
    </tr>
    <tr>
        <td>70대</td>
        <td>621160.02</td>
    </tr>
    <tr>
        <td>80대</td>
        <td>404331.35</td>
    </tr>
    <tr>
        <td>90대</td>
        <td>156317.9</td>
    </tr>
</table>



## 4번

3번 문제를 이용해 월별로 판매금액이 가장 하위에 속하는 대륙 목록을 뽑아보자 (대륙목록은 countries 테이블의 country_region에 있으며, country_id 컬럼으로 customers 테이블과 조인을 해서 구한다)

---------------------------------   
매출월    지역(대륙)  매출금액 
---------------------------------
199801    Oceania      xxxxxx
199803    Oceania      xxxxxx
...
---------------------------------

### 문제해결과정

각 월은 sales 테이블의 sales_month에 있고 판매금액은 sales 테이블의 amount_sold에 있다
sales 테이블과 customers 테이블은 cust_id를 이용해 조인해준다
순위는 rank()를 이용해 구한다


```sql
%%sql

WITH basis AS (SELECT
                    c.country_region
                    , s.sales_month
                    , SUM(s.amount_sold) AS amt
               FROM
                    sales s
                    , customers b
                    , countries c
               WHERE s.cust_id = b.cust_id
                 AND b.country_id = c.country_id
               GROUP BY c.country_region, s.sales_month)
    , real_data AS (SELECT
                        sales_month
                        , country_region
                        , amt
                        , RANK() OVER (PARTITION BY sales_month ORDER BY amt) ranks
                    FROM basis)
SELECT *
FROM real_data
WHERE ranks = 1
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>sales_month</th>
        <th>country_region</th>
        <th>amt</th>
        <th>ranks</th>
    </tr>
    <tr>
        <td>199801</td>
        <td>Oceania</td>
        <td>96005.64</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199803</td>
        <td>Oceania</td>
        <td>80391.31</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199804</td>
        <td>Oceania</td>
        <td>72746.98</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199805</td>
        <td>Oceania</td>
        <td>99230.96</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199806</td>
        <td>Oceania</td>
        <td>67077.62</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199807</td>
        <td>Oceania</td>
        <td>85096.46</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199808</td>
        <td>Oceania</td>
        <td>78091.58</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199809</td>
        <td>Oceania</td>
        <td>118901.26</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199810</td>
        <td>Oceania</td>
        <td>85674.25</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199811</td>
        <td>Oceania</td>
        <td>79296.06</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199812</td>
        <td>Oceania</td>
        <td>77523.49</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199901</td>
        <td>Oceania</td>
        <td>80821.91</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199902</td>
        <td>Oceania</td>
        <td>72027.75</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199903</td>
        <td>Oceania</td>
        <td>68351.69</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199904</td>
        <td>Oceania</td>
        <td>55428</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199905</td>
        <td>Oceania</td>
        <td>61505.12</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199906</td>
        <td>Oceania</td>
        <td>62447.22</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199907</td>
        <td>Oceania</td>
        <td>79778.39</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199908</td>
        <td>Oceania</td>
        <td>74464.55</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199909</td>
        <td>Oceania</td>
        <td>78706.32</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199910</td>
        <td>Oceania</td>
        <td>70338.84</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199911</td>
        <td>Oceania</td>
        <td>73675.66</td>
        <td>1</td>
    </tr>
    <tr>
        <td>199912</td>
        <td>Oceania</td>
        <td>79383.9</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200001</td>
        <td>Middle East</td>
        <td>61.98</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200002</td>
        <td>Middle East</td>
        <td>212.96</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200003</td>
        <td>Oceania</td>
        <td>67237.91</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200004</td>
        <td>Oceania</td>
        <td>57534.46</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200005</td>
        <td>Oceania</td>
        <td>78823.15</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200006</td>
        <td>Oceania</td>
        <td>62379.39</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200007</td>
        <td>Oceania</td>
        <td>91608.17</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200008</td>
        <td>Oceania</td>
        <td>82134.07</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200009</td>
        <td>Oceania</td>
        <td>76403.85</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200010</td>
        <td>Oceania</td>
        <td>96232.62</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200011</td>
        <td>Oceania</td>
        <td>75968.46</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200012</td>
        <td>Oceania</td>
        <td>84306.21</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200101</td>
        <td>Middle East</td>
        <td>628.89</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200102</td>
        <td>Oceania</td>
        <td>108562.93</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200103</td>
        <td>Oceania</td>
        <td>124198.57</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200104</td>
        <td>Oceania</td>
        <td>82461.22</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200105</td>
        <td>Oceania</td>
        <td>107033.54</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200106</td>
        <td>Oceania</td>
        <td>81323.28</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200107</td>
        <td>Oceania</td>
        <td>89864.35</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200108</td>
        <td>Oceania</td>
        <td>97856.05</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200109</td>
        <td>Oceania</td>
        <td>111858.77</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200110</td>
        <td>Oceania</td>
        <td>94510.46</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200111</td>
        <td>Oceania</td>
        <td>78982.09</td>
        <td>1</td>
    </tr>
    <tr>
        <td>200112</td>
        <td>Oceania</td>
        <td>89601.27</td>
        <td>1</td>
    </tr>
</table>



## 5번

5장 연습문제 5번의 정답 결과를 이용해 다음과 같이 지역별, 대출종류별, 월별 대출잔액과 지역별 파티션을 만들어 대출종류별 대출잔액의 %를 구하는 쿼리를 작성해보자

------------------------------------------------------------------------------------------------
지역    대출종류        201111         201112    201210    201211   201212   203110    201311
------------------------------------------------------------------------------------------------
서울    기타대출       73996.9( 36% )
서울    주택담보대출   130105.9( 64% ) 
부산
...
...
-------------------------------------------------------------------------------------------------

### 문제해결과정

일단 5장의 테이블 형태와 그 코드

---------------------------------------------------------------------------------------
지역   201111   201112    201210    201211   201212   203110    201311
---------------------------------------------------------------------------------------
서울   
부산
...
...
---------------------------------------------------------------------------------------

```
SELECT REGION, 
       SUM(AMT1) AS "201111", 
       SUM(AMT2) AS "201112", 
       SUM(AMT3) AS "201210", 
       SUM(AMT4) AS "201211", 
       SUM(AMT5) AS "201312", 
       SUM(AMT6) AS "201310",
       SUM(AMT6) AS "201311"
  FROM ( 
         SELECT REGION,
                CASE WHEN PERIOD = '201111' THEN LOAN_JAN_AMT ELSE 0 END AMT1,
                CASE WHEN PERIOD = '201112' THEN LOAN_JAN_AMT ELSE 0 END AMT2,
                CASE WHEN PERIOD = '201210' THEN LOAN_JAN_AMT ELSE 0 END AMT3, 
                CASE WHEN PERIOD = '201211' THEN LOAN_JAN_AMT ELSE 0 END AMT4, 
                CASE WHEN PERIOD = '201212' THEN LOAN_JAN_AMT ELSE 0 END AMT5, 
                CASE WHEN PERIOD = '201310' THEN LOAN_JAN_AMT ELSE 0 END AMT6,
                CASE WHEN PERIOD = '201311' THEN LOAN_JAN_AMT ELSE 0 END AMT7
         FROM KOR_LOAN_STATUS
       )
GROUP BY REGION
ORDER BY REGION;
```

기존 코드에 대출 종류와 대출잔액의 %를 추가하면 된다
백분율을 구하는 것은 RATIO_TO_REPORT를 사용하면 된다


```sql
%%sql

WITH basis AS (SELECT
                    region
                    , gubun
                    , SUM(AMT1) AS AMT1
                    , SUM(AMT2) AS AMT2
                    , SUM(AMT3) AS AMT3
                    , SUM(AMT4) AS AMT4
                    , SUM(AMT5) AS AMT5
                    , SUM(AMT6) AS AMT6
                    , SUM(AMT7) AS AMT7
               FROM (SELECT
                        region
                        , gubun
                        , CASE WHEN period = '201111' THEN loan_jan_amt ELSE 0 END AMT1
                        , CASE WHEN period = '201112' THEN loan_jan_amt ELSE 0 END AMT2
                        , CASE WHEN period = '201210' THEN loan_jan_amt ELSE 0 END AMT3
                        , CASE WHEN period = '201211' THEN loan_jan_amt ELSE 0 END AMT4
                        , CASE WHEN period = '201212' THEN loan_jan_amt ELSE 0 END AMT5
                        , CASE WHEN period = '201310' THEN loan_jan_amt ELSE 0 END AMT6
                        , CASE WHEN period = '201311' THEN loan_jan_amt ELSE 0 END AMT7
                     FROM kor_loan_status)
               GROUP BY region, gubun)
SELECT
    region
    , gubun
    , AMT1 || '(' || ROUND(RATIO_TO_REPORT(amt1) OVER (PARTITION BY REGION), 2) * 100 || '%)' AS "201111"
    , AMT2 || '(' || ROUND(RATIO_TO_REPORT(amt2) OVER (PARTITION BY REGION), 2) * 100 || '%)' AS "201112"
    , AMT3 || '(' || ROUND(RATIO_TO_REPORT(amt3) OVER (PARTITION BY REGION), 2) * 100 || '%)' AS "201210"
    , AMT4 || '(' || ROUND(RATIO_TO_REPORT(amt4) OVER (PARTITION BY REGION), 2) * 100 || '%)' AS "201211"
    , AMT5 || '(' || ROUND(RATIO_TO_REPORT(amt5) OVER (PARTITION BY REGION), 2) * 100 || '%)' AS "201212"
    , AMT6 || '(' || ROUND(RATIO_TO_REPORT(amt6) OVER (PARTITION BY REGION), 2) * 100 || '%)' AS "201310"
    , AMT7 || '(' || ROUND(RATIO_TO_REPORT(amt7) OVER (PARTITION BY REGION), 2) * 100 || '%)' AS "201311"
FROM basis
ORDER BY region
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>region</th>
        <th>gubun</th>
        <th>201111</th>
        <th>201112</th>
        <th>201210</th>
        <th>201211</th>
        <th>201212</th>
        <th>201310</th>
        <th>201311</th>
    </tr>
    <tr>
        <td>강원</td>
        <td>기타대출</td>
        <td>11502.4(71%)</td>
        <td>11677.5(71%)</td>
        <td>12025.6(71%)</td>
        <td>12093.6(71%)</td>
        <td>12366.1(71%)</td>
        <td>12778.9(70%)</td>
        <td>12893.6(70%)</td>
    </tr>
    <tr>
        <td>강원</td>
        <td>주택담보대출</td>
        <td>4627.4(29%)</td>
        <td>4683.1(29%)</td>
        <td>4945.2(29%)</td>
        <td>4951.5(29%)</td>
        <td>5083.6(29%)</td>
        <td>5411.6(30%)</td>
        <td>5459.2(30%)</td>
    </tr>
    <tr>
        <td>경기</td>
        <td>기타대출</td>
        <td>167394.3(61%)</td>
        <td>168444.1(61%)</td>
        <td>170357.3(61%)</td>
        <td>170191.9(61%)</td>
        <td>171441.8(61%)</td>
        <td>172518.1(61%)</td>
        <td>173416.4(61%)</td>
    </tr>
    <tr>
        <td>경기</td>
        <td>주택담보대출</td>
        <td>108188.5(39%)</td>
        <td>109028.8(39%)</td>
        <td>109371.5(39%)</td>
        <td>108928(39%)</td>
        <td>110059.4(39%)</td>
        <td>108957.4(39%)</td>
        <td>109400(39%)</td>
    </tr>
    <tr>
        <td>경남</td>
        <td>기타대출</td>
        <td>29389.4(67%)</td>
        <td>29904.2(67%)</td>
        <td>32289.4(66%)</td>
        <td>32680(66%)</td>
        <td>33579.4(65%)</td>
        <td>36254.3(65%)</td>
        <td>36746.5(65%)</td>
    </tr>
    <tr>
        <td>경남</td>
        <td>주택담보대출</td>
        <td>14695.4(33%)</td>
        <td>15062.4(33%)</td>
        <td>16985.4(34%)</td>
        <td>17168.8(34%)</td>
        <td>17789.1(35%)</td>
        <td>19560.1(35%)</td>
        <td>19809.6(35%)</td>
    </tr>
    <tr>
        <td>경북</td>
        <td>기타대출</td>
        <td>18616.4(70%)</td>
        <td>18912.8(70%)</td>
        <td>19667(69%)</td>
        <td>19850(70%)</td>
        <td>20232.7(69%)</td>
        <td>21706.6(69%)</td>
        <td>22060.9(69%)</td>
    </tr>
    <tr>
        <td>경북</td>
        <td>주택담보대출</td>
        <td>8162.1(30%)</td>
        <td>8277.4(30%)</td>
        <td>8665(31%)</td>
        <td>8680.7(30%)</td>
        <td>8907.1(31%)</td>
        <td>9684.4(31%)</td>
        <td>9862(31%)</td>
    </tr>
    <tr>
        <td>광주</td>
        <td>기타대출</td>
        <td>13900.7(61%)</td>
        <td>14209.9(61%)</td>
        <td>14645(61%)</td>
        <td>14720.2(61%)</td>
        <td>14956.4(61%)</td>
        <td>15490.5(61%)</td>
        <td>15709.6(61%)</td>
    </tr>
    <tr>
        <td>광주</td>
        <td>주택담보대출</td>
        <td>8731.7(39%)</td>
        <td>8964.2(39%)</td>
        <td>9461.4(39%)</td>
        <td>9458.6(39%)</td>
        <td>9596.4(39%)</td>
        <td>9931(39%)</td>
        <td>10062.2(39%)</td>
    </tr>
    <tr>
        <td>대구</td>
        <td>기타대출</td>
        <td>21882.3(61%)</td>
        <td>22117.7(61%)</td>
        <td>23195(60%)</td>
        <td>23366(60%)</td>
        <td>23966(60%)</td>
        <td>25462.8(60%)</td>
        <td>25769.5(60%)</td>
    </tr>
    <tr>
        <td>대구</td>
        <td>주택담보대출</td>
        <td>14043.5(39%)</td>
        <td>14229(39%)</td>
        <td>15154.5(40%)</td>
        <td>15260.8(40%)</td>
        <td>15684.4(40%)</td>
        <td>16821.8(40%)</td>
        <td>17043.7(40%)</td>
    </tr>
    <tr>
        <td>대전</td>
        <td>기타대출</td>
        <td>16360.6(61%)</td>
        <td>16722.5(61%)</td>
        <td>17225.4(61%)</td>
        <td>17347.9(61%)</td>
        <td>17502.9(61%)</td>
        <td>18559(60%)</td>
        <td>18833.4(61%)</td>
    </tr>
    <tr>
        <td>대전</td>
        <td>주택담보대출</td>
        <td>10394.2(39%)</td>
        <td>10733.2(39%)</td>
        <td>11169.9(39%)</td>
        <td>11210(39%)</td>
        <td>11349.4(39%)</td>
        <td>12126(40%)</td>
        <td>12285.7(39%)</td>
    </tr>
    <tr>
        <td>부산</td>
        <td>기타대출</td>
        <td>35163.1(59%)</td>
        <td>35570.6(59%)</td>
        <td>37191.2(59%)</td>
        <td>37286(59%)</td>
        <td>38040.2(59%)</td>
        <td>39979.4(58%)</td>
        <td>40463.3(58%)</td>
    </tr>
    <tr>
        <td>부산</td>
        <td>주택담보대출</td>
        <td>23936.8(41%)</td>
        <td>24328.3(41%)</td>
        <td>26128.9(41%)</td>
        <td>26137.2(41%)</td>
        <td>26795.2(41%)</td>
        <td>28422(42%)</td>
        <td>28794.1(42%)</td>
    </tr>
    <tr>
        <td>서울</td>
        <td>기타대출</td>
        <td>204102.8(61%)</td>
        <td>204275.7(61%)</td>
        <td>202557.1(61%)</td>
        <td>202887.5(61%)</td>
        <td>203344.9(61%)</td>
        <td>204918.8(62%)</td>
        <td>205644.3(62%)</td>
    </tr>
    <tr>
        <td>서울</td>
        <td>주택담보대출</td>
        <td>130105.9(39%)</td>
        <td>130452.6(39%)</td>
        <td>128097.2(39%)</td>
        <td>127706.5(39%)</td>
        <td>128227.4(39%)</td>
        <td>128066.1(38%)</td>
        <td>128418.4(38%)</td>
    </tr>
    <tr>
        <td>세종</td>
        <td>기타대출</td>
        <td>0(%)</td>
        <td>0(%)</td>
        <td>1655.1(65%)</td>
        <td>1766.3(64%)</td>
        <td>1814.5(64%)</td>
        <td>2583.7(62%)</td>
        <td>2653.4(62%)</td>
    </tr>
    <tr>
        <td>세종</td>
        <td>주택담보대출</td>
        <td>0(%)</td>
        <td>0(%)</td>
        <td>907.6(35%)</td>
        <td>1002.9(36%)</td>
        <td>1018.9(36%)</td>
        <td>1557.8(38%)</td>
        <td>1611.4(38%)</td>
    </tr>
    <tr>
        <td>울산</td>
        <td>기타대출</td>
        <td>11007.3(63%)</td>
        <td>11130(63%)</td>
        <td>12156.2(62%)</td>
        <td>12262.3(62%)</td>
        <td>12376.9(62%)</td>
        <td>13193.9(62%)</td>
        <td>13336.2(62%)</td>
    </tr>
    <tr>
        <td>울산</td>
        <td>주택담보대출</td>
        <td>6517(37%)</td>
        <td>6627.5(37%)</td>
        <td>7340.3(38%)</td>
        <td>7385.6(38%)</td>
        <td>7540.4(38%)</td>
        <td>8065.8(38%)</td>
        <td>8152.6(38%)</td>
    </tr>
    <tr>
        <td>인천</td>
        <td>기타대출</td>
        <td>40404.2(58%)</td>
        <td>40556.2(58%)</td>
        <td>40916.8(58%)</td>
        <td>40895.7(58%)</td>
        <td>41106(58%)</td>
        <td>40440.7(58%)</td>
        <td>40650.2(58%)</td>
    </tr>
    <tr>
        <td>인천</td>
        <td>주택담보대출</td>
        <td>29724.9(42%)</td>
        <td>29871.7(42%)</td>
        <td>30130.7(42%)</td>
        <td>30064.6(42%)</td>
        <td>30237.2(42%)</td>
        <td>29355.3(42%)</td>
        <td>29478.8(42%)</td>
    </tr>
    <tr>
        <td>전남</td>
        <td>기타대출</td>
        <td>12274.4(70%)</td>
        <td>12441.4(70%)</td>
        <td>12921.8(69%)</td>
        <td>13069.7(69%)</td>
        <td>13255.9(69%)</td>
        <td>13858.3(69%)</td>
        <td>14016.3(69%)</td>
    </tr>
    <tr>
        <td>전남</td>
        <td>주택담보대출</td>
        <td>5220.5(30%)</td>
        <td>5327.7(30%)</td>
        <td>5722.2(31%)</td>
        <td>5808.9(31%)</td>
        <td>5925.4(31%)</td>
        <td>6227.9(31%)</td>
        <td>6296.4(31%)</td>
    </tr>
    <tr>
        <td>전북</td>
        <td>기타대출</td>
        <td>14725.2(67%)</td>
        <td>14972.9(67%)</td>
        <td>15685.7(66%)</td>
        <td>15802.6(67%)</td>
        <td>16153.1(66%)</td>
        <td>17096.3(66%)</td>
        <td>17285.8(66%)</td>
    </tr>
    <tr>
        <td>전북</td>
        <td>주택담보대출</td>
        <td>7274.9(33%)</td>
        <td>7447.5(33%)</td>
        <td>7931.6(34%)</td>
        <td>7959.4(33%)</td>
        <td>8190.8(34%)</td>
        <td>8665.3(34%)</td>
        <td>8775.9(34%)</td>
    </tr>
    <tr>
        <td>제주</td>
        <td>기타대출</td>
        <td>4500(76%)</td>
        <td>4611(76%)</td>
        <td>4615.7(74%)</td>
        <td>4667.8(73%)</td>
        <td>4818.7(73%)</td>
        <td>5142.7(72%)</td>
        <td>5209.2(72%)</td>
    </tr>
    <tr>
        <td>제주</td>
        <td>주택담보대출</td>
        <td>1420.8(24%)</td>
        <td>1477.8(24%)</td>
        <td>1659.9(26%)</td>
        <td>1683.9(27%)</td>
        <td>1767.3(27%)</td>
        <td>1993.8(28%)</td>
        <td>2021.1(28%)</td>
    </tr>
    <tr>
        <td>충남</td>
        <td>기타대출</td>
        <td>21994.5(70%)</td>
        <td>22197.1(70%)</td>
        <td>21429.6(69%)</td>
        <td>21488.4(69%)</td>
        <td>21813.7(69%)</td>
        <td>22402.9(69%)</td>
        <td>22602.3(69%)</td>
    </tr>
    <tr>
        <td>충남</td>
        <td>주택담보대출</td>
        <td>9601.3(30%)</td>
        <td>9710.2(30%)</td>
        <td>9422.7(31%)</td>
        <td>9444.8(31%)</td>
        <td>9618.1(31%)</td>
        <td>9934.2(31%)</td>
        <td>10067.4(31%)</td>
    </tr>
    <tr>
        <td>충북</td>
        <td>기타대출</td>
        <td>11642.5(67%)</td>
        <td>11862(67%)</td>
        <td>12598.9(68%)</td>
        <td>12726.4(68%)</td>
        <td>13089.1(67%)</td>
        <td>13691.1(67%)</td>
        <td>13830.4(67%)</td>
    </tr>
    <tr>
        <td>충북</td>
        <td>주택담보대출</td>
        <td>5649.6(33%)</td>
        <td>5784.1(33%)</td>
        <td>6055(32%)</td>
        <td>6123.9(32%)</td>
        <td>6393.1(33%)</td>
        <td>6635.4(33%)</td>
        <td>6698.4(33%)</td>
    </tr>
</table>


