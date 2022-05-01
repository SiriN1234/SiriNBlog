---
title: "4장. SQL 함수 살펴 보기"
author: "Hanjeongin"
date: 2022-04-27 21:00:00
tags: SQL
---

출처 : 오라클 SQL과 PL/SQL을 다루는 기술


```python
%load_ext sql
```


```python
%sql oracle://ora_user:sirin@127.0.0.1:1521/myoracle
```

# 숫자 함수

## ABS(n)

- 매개변수로 숫자를 받아 그 절대값을 반환하는 함수


```sql
%%sql

SELECT
    ABS(10)
    , ABS(10.123)
    , ABS(-10)
    , ABS(-10.123)
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>ABS(10)</th>
        <th>ABS(10.123)</th>
        <th>ABS(-10)</th>
        <th>ABS(-10.123)</th>
    </tr>
    <tr>
        <td>10</td>
        <td>10.123</td>
        <td>10</td>
        <td>10.123</td>
    </tr>
</table>



## CEIL(n) & FLOOR(n)

- CEIL은 소숫점을 올림
- FLOOR는 소숫점을 버림


```sql
%%sql

SELECT
    CEIL(10.123)
    , CEIL(-10.123)
    , FLOOR(10.123)
    , FLOOR(-10.123)
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>CEIL(10.123)</th>
        <th>CEIL(-10.123)</th>
        <th>FLOOR(10.123)</th>
        <th>FLOOR(-10.123)</th>
    </tr>
    <tr>
        <td>11</td>
        <td>-10</td>
        <td>10</td>
        <td>-11</td>
    </tr>
</table>



## ROUND(n, i) & TRUNC(n1, n2)

- ROUND는 매개변수 n을 소수점 기준 (i+1)번 째에서 반올림한 결과를 반환한다
    + i는 생략 가능하고 디폴트 값은 0이다
- TRUNC는 반올림을 하지 않고 n1을 소수점 기준 n2자리에서 무조건 잘라낸 결과를 반환한다
    + n2는 생략 가능하며 디폴트 값은 0이다


```sql
%%sql

SELECT
    ROUND(10.154, 1)
    , ROUND(10.151, 2)
    , ROUND(-10.154, 1)
    , ROUND(-10.151, 2)
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>ROUND(10.154,1)</th>
        <th>ROUND(10.151,2)</th>
        <th>ROUND(-10.154,1)</th>
        <th>ROUND(-10.151,2)</th>
    </tr>
    <tr>
        <td>10.2</td>
        <td>10.15</td>
        <td>-10.2</td>
        <td>-10.15</td>
    </tr>
</table>




```sql
%%sql

SELECT
    ROUND(0, 3)
    , ROUND(115.155, -1) -- 1의 자리에서 반올림
    , ROUND(115.155, -2) -- 10의 자리에서 반올림
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>ROUND(0,3)</th>
        <th>ROUND(115.155,-1)--1의자리에서반올림</th>
        <th>ROUND(115.155,-2)--10의자리에서반올림</th>
    </tr>
    <tr>
        <td>0</td>
        <td>120</td>
        <td>100</td>
    </tr>
</table>




```sql
%%sql

SELECT
    TRUNC(115.155)
    , TRUNC(115.155, 1)
    , TRUNC(115.155, 2)
    , TRUNC(115.155, -2)
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>TRUNC(115.155)</th>
        <th>TRUNC(115.155,1)</th>
        <th>TRUNC(115.155,2)</th>
        <th>TRUNC(115.155,-2)</th>
    </tr>
    <tr>
        <td>115</td>
        <td>115.1</td>
        <td>115.15</td>
        <td>100</td>
    </tr>
</table>



## POWER(n2, n1)와 SQRT(n)

- POWER 함수는 n2를 n1 제곱한 결과를 반환한다
    + n1은 정수와 실수 모두 올 수 있다
    + n2가 음수일 때 n1은 정수만 올 수 있다
- SQRT 함수는 n의 제곱근을 반환한다


```sql
%%sql

SELECT
    POWER(3, 2)
    , POWER(3, 3)
    , SQRT(9)
    , SQRT(8)
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>POWER(3,2)</th>
        <th>POWER(3,3)</th>
        <th>SQRT(9)</th>
        <th>SQRT(8)</th>
    </tr>
    <tr>
        <td>9</td>
        <td>27</td>
        <td>3</td>
        <td>2.82842712474619009760337744841939615714</td>
    </tr>
</table>



## MOD(n2, n1) & REMAINDER(n2, n1)

- MOD 함수는 n2를 n1으로 나눈 나머지 값을 반환한다
- REMAINDER 함수도 n2를 n1으로 나눈 나머지 값을 반환하는데 나머지를 구하는 내부적 연산 방법이 다르다
    + MOD -> n2 - n1 * FLOOR(n2/n1)
    + REMAINDER -> n2 - n1 * ROUND(n2/n1)


```sql
%%sql

SELECT
    MOD(19, 4)
    , REMAINDER(19, 4)
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>MOD(19,4)</th>
        <th>REMAINDER(19,4)</th>
    </tr>
    <tr>
        <td>3</td>
        <td>-1</td>
    </tr>
</table>



## EXP(n) & LN(n) & LOG(n2, n1)

- EXP는 지수함수로 e의 n제곱 값을 반환
- LN은 자연 로그 함수로 밑수가 e인 로그 함수
- LOG는 n2를 밑수로 하는 n1의 로그 값을 반환


```sql
%%sql

SELECT
    EXP(2)
    , LN(2.713)
    , LOG(10, 100)
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>EXP(2)</th>
        <th>LN(2.713)</th>
        <th>LOG(10,100)</th>
    </tr>
    <tr>
        <td>7.3890560989306502272304274605750078132</td>
        <td>0.9980550336767946922014710783755035594696</td>
        <td>2</td>
    </tr>
</table>



# 문자 함수

## INITCAP(char), LOWER(char), UPPER(char)

- INITCAP 함수는 매개변수로 들어오는 char의 첫 문자는 대문자로, 나머지는 소문자로 반환
    + 첫 문자를 인식하는 기준은 공백과 알파벳(숫자 포함)을 제외한 문자
- LOWER 함수는 매개변수로 들어오는 문자를 모두 소문자로 반환
- UPPER 함수는 매개변수로 들어오는 문자를 모두 대문자로 반환


```sql
%%sql

SELECT
    INITCAP('never say goodbye')
    , INITCAP('never6say*good가bye')
    , LOWER('NEVER SAY GOODBYE')
    , UPPER('never say goodbye')
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>INITCAP(&#x27;NEVERSAYGOODBYE&#x27;)</th>
        <th>INITCAP(&#x27;NEVER6SAY*GOOD가BYE&#x27;)</th>
        <th>LOWER(&#x27;NEVERSAYGOODBYE&#x27;)</th>
        <th>UPPER(&#x27;NEVERSAYGOODBYE&#x27;)</th>
    </tr>
    <tr>
        <td>Never Say Goodbye</td>
        <td>Never6say*Good가Bye</td>
        <td>never say goodbye</td>
        <td>NEVER SAY GOODBYE</td>
    </tr>
</table>



## CANCAT(char1, char2), SUBSTR(char, pos, len), SUBSTRB(char, pos, len)

- CONCAT 함수는 '||' 연산자처럼 매개변수로 들어오는 두 문자를 붙여 반환한다
- SUBSTR 함수는 char 문자열에서 pos번째 문자부터 길이가 len인만큼 잘라낸 결과를 반환한다
    + pos 값으로 0이 들어오면 디폴트 값인 1, 즉 첫 번째 문자를 가리킨다
    + pos 값으로 음수가 들어오면 char 문자열 맨 끝에서 시작한 상대적 위치를 의미한다
    + len 값은 생략 가능하고 생락되면 pos번째 문자부터 모든 문자를 반환한다
- SUBSTRB 함수는 문자 개수가 아닌 바이트 수만큼 잘라낸 결과를 반환한다


```sql
%%sql

SELECT
    CONCAT('I Have', 'A Dream')
    , 'I Have' || 'A Dream'
    , SUBSTR('ABCDEFG', 1, 4)
    , SUBSTR('ABCDEFG', -6, 4)
    , SUBSTRB('ABCDEFG', 1, 4)
    , SUBSTRB('가나다라마바사', 1, 4)
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>CONCAT(&#x27;IHAVE&#x27;,&#x27;ADREAM&#x27;)</th>
        <th>&#x27;IHAVE&#x27;||&#x27;ADREAM&#x27;</th>
        <th>SUBSTR(&#x27;ABCDEFG&#x27;,1,4)</th>
        <th>SUBSTR(&#x27;ABCDEFG&#x27;,-6,4)</th>
        <th>SUBSTRB(&#x27;ABCDEFG&#x27;,1,4)</th>
        <th>SUBSTRB(&#x27;가나다라마바사&#x27;,1,4)</th>
    </tr>
    <tr>
        <td>I HaveA Dream</td>
        <td>I HaveA Dream</td>
        <td>ABCD</td>
        <td>BCDE</td>
        <td>ABCD</td>
        <td>가 </td>
    </tr>
</table>



## LTRIM(char, set), RTRIM(char, set)

- LTRIM 함수는 char 문자열에서 set으로 지정된 문자열을 왼쪽 끝에서 제거한 후 나머지 문자열을 반환한다
    + set은 생략 가능하고, 디폴트는 공백 문자 한 글자이다
- RTRIM 함수는 오른쪽 끝에서 제거한 뒤 나머지 문자열을 반환한다


```sql
%%sql

SELECT
    LTRIM('ABCDEFGABC', 'ABC')
    , LTRIM('가나다라', '가')
    , RTRIM('ABCDEFGABC', 'ABC')
    , RTRIM('가나다라', '다라')
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>LTRIM(&#x27;ABCDEFGABC&#x27;,&#x27;ABC&#x27;)</th>
        <th>LTRIM(&#x27;가나다라&#x27;,&#x27;가&#x27;)</th>
        <th>RTRIM(&#x27;ABCDEFGABC&#x27;,&#x27;ABC&#x27;)</th>
        <th>RTRIM(&#x27;가나다라&#x27;,&#x27;다라&#x27;)</th>
    </tr>
    <tr>
        <td>DEFGABC</td>
        <td>나다라</td>
        <td>ABCDEFG</td>
        <td>가나</td>
    </tr>
</table>



## LPAD(expr1, n, expr2), RPAD(expr1, n, expr2)

- LPAD는 expr2 문자열을 n자리만큼 왼쪽부터 채워 expr1을 반환하는 함수이다
    + expr은 생략 가능하고, 디폴트는 공백 한 문자이다
- RPAD는 오른쪽부터 채운다

```
SELECT
    LPAD(phone_num, 12, '(02)')
    , RPAD(phone_num, 12, '(02)')
FROM ex4_1;
```

![image.png](data:image/png;base64,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)

## REPLACE(char, search_str, replace_str), TRANSLATE(expr, from_str, to_str)

- REPLACE는 char 문자열에서 search_str 문자열을 찾아 이를 replace_str 문자열로 대체한다
- TRANSLATE 함수는 expr 문자열에서 from_str에 해당하는 문자를 찾아 to_str로 바꾼 결과를 반환하는데, 문자열 자체가 아닌 문자 한 글자씩 매핑해서 바꾼 결과를 반환한다



```sql
%%sql

SELECT
    REPLACE('나는 너를 모르는데 너는 나를 알겠는가?', '나', '너')
    , LTRIM(' ABC DEF ')
    , RTRIM(' ABC DEF')
    , REPLACE(' ABC DEF ', ' ', '')
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>REPLACE(&#x27;나는너를모르는데너는나를알겠는가?&#x27;,&#x27;나&#x27;,&#x27;너&#x27;)</th>
        <th>LTRIM(&#x27;ABCDEF&#x27;)</th>
        <th>RTRIM(&#x27;ABCDEF&#x27;)</th>
        <th>REPLACE(&#x27;ABCDEF&#x27;,&#x27;&#x27;,&#x27;&#x27;)</th>
    </tr>
    <tr>
        <td>너는 너를 모르는데 너는 너를 알겠는가?</td>
        <td>ABC DEF </td>
        <td> ABC DEF</td>
        <td>ABCDEF</td>
    </tr>
</table>



## INSTR(str, substr, pos, occur), LENGTH(chr), LENGTHB(chr)

- INSTR 함수는 str 문자열에서 substr과 일치하는 위치를 반환
    + pos는 시작 위치로 디폴트 값은 1
    + occur은 몇 번째 일치하는지를 명시하며 디폴트 값은 1
- LENGTH 함수는 매개변수로 들어온 문자열의 개수를 반환
- LENGTHB 함수는 문자열의 바이트 수를 반환


```sql
%%sql

SELECT
    INSTR('내가 만약 외로울 때면.내가 만약 괴로울 때면.내가 만약 즐거울 때면', '만약') AS INSTR1
    , INSTR('내가 만약 외로울 때면, 내가 만약 괴로울 때면, 내가 만약 즐거울 때면', '만약', 5) AS INSTR2
    , INSTR('내가 만약 외로울 때면, 내가 만약 괴로울 때면, 내가 만약 즐거울 때면', '만약', 5, 2) AS INSTR3
    , LENGTH('대한민국')
    , LENGTHB('대한민국')
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>instr1</th>
        <th>instr2</th>
        <th>instr3</th>
        <th>LENGTH(&#x27;대한민국&#x27;)</th>
        <th>LENGTHB(&#x27;대한민국&#x27;)</th>
    </tr>
    <tr>
        <td>4</td>
        <td>18</td>
        <td>32</td>
        <td>4</td>
        <td>12</td>
    </tr>
</table>



# 날짜 함수

- DATE 함수나 TIMESTAMP 함수와 같은 날짜형을 대상으로 연산을 수행해 결과를 반환하는 함수

## SYSDATE, SYSTIMESTAMP

- 현재일자와 시간을 각각 DATE, TIMESTAMP 형으로 반환한다


```sql
%%sql

SELECT SYSDATE, SYSTIMESTAMP
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>sysdate</th>
        <th>systimestamp</th>
    </tr>
    <tr>
        <td>2022-04-27 14:35:56</td>
        <td>2022-04-27 14:35:56.835000</td>
    </tr>
</table>



## ADD_MONTH(date, integer)

- 매개변수로 들어온 날짜에 integer 만큼의 월을 더한 날짜를 반환한다


```sql
%%sql

SELECT
    ADD_MONTHS(SYSDATE, 1)
    , ADD_MONTHS(SYSDATE, -1)
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>ADD_MONTHS(SYSDATE,1)</th>
        <th>ADD_MONTHS(SYSDATE,-1)</th>
    </tr>
    <tr>
        <td>2022-05-27 14:40:39</td>
        <td>2022-03-27 14:40:39</td>
    </tr>
</table>



## MONTHS_BETWEEN(date1, date2)

- 두 날짜 사이의 개월 수를 반환
- date2가 date1보다 빠른 날짜가 온다


```sql
%%sql

SELECT
    MONTHS_BETWEEN(SYSDATE, ADD_MONTHS(SYSDATE, 1)) mon1
    , MONTHS_BETWEEN(ADD_MONTHS(SYSDATE, 1), SYSDATE) mon2
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>mon1</th>
        <th>mon2</th>
    </tr>
    <tr>
        <td>-1</td>
        <td>1</td>
    </tr>
</table>



## LAST_DAY(date)

- 해당 월의 마지막 일자를 반환한다


```sql
%%sql

SELECT
    LAST_DAY(SYSDATE)
    , LAST_DAY(ADD_MONTHS(SYSDATE, 1))
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>LAST_DAY(SYSDATE)</th>
        <th>LAST_DAY(ADD_MONTHS(SYSDATE,1))</th>
    </tr>
    <tr>
        <td>2022-04-30 14:50:04</td>
        <td>2022-05-31 14:50:04</td>
    </tr>
</table>



## ROUND(date, format), TRUNC(date, format)

- ROUND는 format에 따라 반올림한 날짜를 반환한다
- TRUNC는 잘라낸 날짜를 반환한다


```sql
%%sql

SELECT
    SYSDATE
    , ROUND(SYSDATE, 'month')
    , TRUNC(SYSDATE, 'month')
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>sysdate</th>
        <th>ROUND(SYSDATE,&#x27;MONTH&#x27;)</th>
        <th>TRUNC(SYSDATE,&#x27;MONTH&#x27;)</th>
    </tr>
    <tr>
        <td>2022-04-27 14:51:45</td>
        <td>2022-05-01 00:00:00</td>
        <td>2022-04-01 00:00:00</td>
    </tr>
</table>



# 변환 함수

- 다른 유형의 데이터 타입으로 변환해 결과를 반환하는 함수
- 변환 함수를 통해 형변환을 직접 처리하는 것을 명시적 형변환이라고 한다

## TO_CHAR (숫자 혹은 날짜, format)

- 숫자나 날짜를 문자로 변환해주는 함수
- 매개변수로 오는 숫자나 날짜에 따라 자주 사용하는 포맷을 정리하면 다음과 같다

![image.png](data:image/png;base64,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)

![image.png](data:image/png;base64,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)


```sql
%%sql

SELECT
    TO_CHAR(123456789, '999,999,999')
    , TO_CHAR(SYSDATE, 'YYYY-MM-DD')
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>TO_CHAR(123456789,&#x27;999,999,999&#x27;)</th>
        <th>TO_CHAR(SYSDATE,&#x27;YYYY-MM-DD&#x27;)</th>
    </tr>
    <tr>
        <td> 123,456,789</td>
        <td>2022-04-27</td>
    </tr>
</table>



## TO_NUMBER(expr, format)

- 문자나 다른 유형의 숫자를 NUMBER 형으로 변환하는 함수


```sql
%%sql

SELECT TO_NUMBER('123456')
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>TO_NUMBER(&#x27;123456&#x27;)</th>
    </tr>
    <tr>
        <td>123456</td>
    </tr>
</table>



## TO_DATE(char, format), TO_TIMESTAMP(char, format)

- 문자를 날짜형으로 변환하는 함수
- 형식 매개변수로는 [표 4-1]에 있는 항목이 올 수 있다
- TO_DATE는 DATE 형으로 TO_TIMESTAMP는 TIMESTAMP 형으로 반환해 값을 반환한다


```python
%sql ALTER SESSION SET NLS_DATE_FORMAT = 'YYYY-MM-DD HH24:MI:SS'
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




    []




```sql
%%sql

SELECT
    TO_DATE('20140101', 'YYYY-MM-DD')
    , TO_DATE('20220427 14:06:10', 'YYYY-MM-DD HH24:MI:SS')
FROM DUAL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>TO_DATE(&#x27;20140101&#x27;,&#x27;YYYY-MM-DD&#x27;)</th>
        <th>TO_DATE(&#x27;2022042714:06:10&#x27;,&#x27;YYYY-MM-DDHH24:MI:SS&#x27;)</th>
    </tr>
    <tr>
        <td>2014-01-01 00:00:00</td>
        <td>2022-04-27 14:06:10</td>
    </tr>
</table>



# NULL 관련 함수

## NVL(expr1, expr2), NVL2((expr1, expr2, expr3))

- NVL 함수는 expr1이 NULL일 때 expr2를 반환한다
- NVL2 함수는 expr1이 NULL이 아니면 expr2를, NULL이면 expr3를 반환한다


```sql
%%sql

SELECT
    NVL(manager_id, employee_id)
FROM employees
WHERE manager_id IS NULL
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>NVL(MANAGER_ID,EMPLOYEE_ID)</th>
    </tr>
    <tr>
        <td>100</td>
    </tr>
</table>




```sql
%%sql

SELECT
    employee_id
    , NVL2(commission_pct
           , salary + (salary + commission_pct)
           , salary) as final_salary
FROM employees
WHERE rownum <= 5
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>employee_id</th>
        <th>final_salary</th>
    </tr>
    <tr>
        <td>198</td>
        <td>2600</td>
    </tr>
    <tr>
        <td>199</td>
        <td>2600</td>
    </tr>
    <tr>
        <td>200</td>
        <td>4400</td>
    </tr>
    <tr>
        <td>201</td>
        <td>13000</td>
    </tr>
    <tr>
        <td>202</td>
        <td>6000</td>
    </tr>
</table>



## COALESCE(expr1, expr2, ...)

- 매개변수로 들어오는 표현식에서 NULL이 아닌 첫 번째 표현식을 반환한다


```sql
%%sql

SELECT
    employee_id
    , salary
    , commission_pct
    , COALESCE(salary * commission_pct, salary) AS salary
FROM employees
WHERE rownum <= 5
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>employee_id</th>
        <th>salary</th>
        <th>commission_pct</th>
        <th>salary_1</th>
    </tr>
    <tr>
        <td>198</td>
        <td>2600</td>
        <td>None</td>
        <td>2600</td>
    </tr>
    <tr>
        <td>199</td>
        <td>2600</td>
        <td>None</td>
        <td>2600</td>
    </tr>
    <tr>
        <td>200</td>
        <td>4400</td>
        <td>None</td>
        <td>4400</td>
    </tr>
    <tr>
        <td>201</td>
        <td>13000</td>
        <td>None</td>
        <td>13000</td>
    </tr>
    <tr>
        <td>202</td>
        <td>6000</td>
        <td>None</td>
        <td>6000</td>
    </tr>
</table>



## LNNVL(조건식)

- 매개변수로 들어오는 조건식의 결과가 FALSE나 UNKNOWN이면 TRUE를, TRUE이면 FALSE를 반환한다


```sql
%%sql

SELECT
    employee_id
    , commission_pct
FROM employees
WHERE commission_pct < 0.2

-- NULL값은 나오지 않음
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>employee_id</th>
        <th>commission_pct</th>
    </tr>
    <tr>
        <td>155</td>
        <td>0.15</td>
    </tr>
    <tr>
        <td>163</td>
        <td>0.15</td>
    </tr>
    <tr>
        <td>164</td>
        <td>0.1</td>
    </tr>
    <tr>
        <td>165</td>
        <td>0.1</td>
    </tr>
    <tr>
        <td>166</td>
        <td>0.1</td>
    </tr>
    <tr>
        <td>167</td>
        <td>0.1</td>
    </tr>
    <tr>
        <td>171</td>
        <td>0.15</td>
    </tr>
    <tr>
        <td>172</td>
        <td>0.15</td>
    </tr>
    <tr>
        <td>173</td>
        <td>0.1</td>
    </tr>
    <tr>
        <td>178</td>
        <td>0.15</td>
    </tr>
    <tr>
        <td>179</td>
        <td>0.1</td>
    </tr>
</table>




```sql
%%sql

SELECT
    COUNT(*) -- 행 갯수 반환
FROM employees
WHERE NVL(commission_pct, 0) < 0.2
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>COUNT(*)--행갯수반환</th>
    </tr>
    <tr>
        <td>83</td>
    </tr>
</table>




```sql
%%sql

-- 위의 코드와 동일한 값 출력

SELECT
    COUNT(*) -- 행 갯수 반환
FROM employees
WHERE LNNVL(commission_pct >= 0.2)
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>COUNT(*)--행갯수반환</th>
    </tr>
    <tr>
        <td>83</td>
    </tr>
</table>



## NULLIF(expr1, expr2)

- expr1과 expr2를 비교해 같으면 NULL을, 같지 않으면 expr1을 반환한다


```sql
%%sql

SELECT
    employee_id
    , TO_CHAR(start_date, 'YYYY') start_year
    , TO_CHAR(end_date, 'YYYY') end_year
    , NULLIF(TO_CHAR(end_date, 'YYYY'), TO_CHAR(start_date, 'YYYY')) nullif_year
FROM job_history
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>employee_id</th>
        <th>start_year</th>
        <th>end_year</th>
        <th>nullif_year</th>
    </tr>
    <tr>
        <td>102</td>
        <td>2001</td>
        <td>2006</td>
        <td>2006</td>
    </tr>
    <tr>
        <td>101</td>
        <td>1997</td>
        <td>2001</td>
        <td>2001</td>
    </tr>
    <tr>
        <td>101</td>
        <td>2001</td>
        <td>2005</td>
        <td>2005</td>
    </tr>
    <tr>
        <td>201</td>
        <td>2004</td>
        <td>2007</td>
        <td>2007</td>
    </tr>
    <tr>
        <td>114</td>
        <td>2006</td>
        <td>2007</td>
        <td>2007</td>
    </tr>
    <tr>
        <td>122</td>
        <td>2007</td>
        <td>2007</td>
        <td>None</td>
    </tr>
    <tr>
        <td>200</td>
        <td>1995</td>
        <td>2001</td>
        <td>2001</td>
    </tr>
    <tr>
        <td>176</td>
        <td>2006</td>
        <td>2006</td>
        <td>None</td>
    </tr>
    <tr>
        <td>176</td>
        <td>2007</td>
        <td>2007</td>
        <td>None</td>
    </tr>
    <tr>
        <td>200</td>
        <td>2002</td>
        <td>2006</td>
        <td>2006</td>
    </tr>
</table>



# 기타 함수

## DECODE(expr, search1, result1, search2, result2, ..., default)

- expr과 search1을 비교해 두 값이 같으면 result1을, 같지 않으면 다시 search2와 비교해 값이 같으면 result2를 반환... 이런 식으로 계속 비교한 뒤 최종적으로 같은 값이 없으면 default 값을 반환한다


```sql
%%sql

SELECT
    prod_id
    , DECODE(channel_id, 3, 'Direct'
                       , 9, 'Direct'
                       , 5, 'Indirect'
                       , 4, 'Indirect'
                          , 'Others') decodes
FROM sales
WHERE rownum < 10
```

     * oracle://ora_user:***@127.0.0.1:1521/myoracle
    0 rows affected.
    




<table>
    <tr>
        <th>prod_id</th>
        <th>decodes</th>
    </tr>
    <tr>
        <td>128</td>
        <td>Direct</td>
    </tr>
    <tr>
        <td>128</td>
        <td>Direct</td>
    </tr>
    <tr>
        <td>120</td>
        <td>Direct</td>
    </tr>
    <tr>
        <td>120</td>
        <td>Direct</td>
    </tr>
    <tr>
        <td>120</td>
        <td>Direct</td>
    </tr>
    <tr>
        <td>120</td>
        <td>Direct</td>
    </tr>
    <tr>
        <td>120</td>
        <td>Direct</td>
    </tr>
    <tr>
        <td>120</td>
        <td>Direct</td>
    </tr>
    <tr>
        <td>120</td>
        <td>Direct</td>
    </tr>
</table>


