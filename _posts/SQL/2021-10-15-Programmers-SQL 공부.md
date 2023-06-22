---
defaults:
- scope:
  path: ""
  type: posts
  values:
  layout: single
  author_profile: true
  comments: true
  related: true

title: "[Programmers] SQL 공부"
excerpt: "[Programmers] SQL 공부"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - SQL 
tags:
  - [SQL, Programmers]
date: 2021-10-15
last_modified_at: 2021-10-15
---
Problem URL : [모든 레코드 조회하기](https://programmers.co.kr/learn/courses/30/lessons/59034)

```sql
SELECT * FROM ANIMAL_INS ORDER BY ANIMAL_ID
```

Problem URL : [최댓값 구하기](https://programmers.co.kr/learn/courses/30/lessons/59415)

```sql
SELECT MAX(DATETIME) AS '시간' 
FROM ANIMAL_INS
```

Problem URL : [고양이와 개는 몇 마리 있을까](https://programmers.co.kr/learn/courses/30/lessons/59040)

```sql
SELECT ANIMAL_TYPE, COUNT(*)
FROM ANIMAL_INS
GROUP BY ANIMAL_TYPE
ORDER BY ANIMAL_TYPE
```

Problem URL : [이름이 없는 동물의 아이디](https://programmers.co.kr/learn/courses/30/lessons/59039)

```sql
SELECT ANIMAL_ID
FROM ANIMAL_INS
WHERE NAME IS NULL
ORDER BY ANIMAL_ID
```

Problem URL : [없어진 기록 찾기](https://programmers.co.kr/learn/courses/30/lessons/59042)

```sql
SELECT OUTS.ANIMAL_ID, OUTS.NAME
FROM ANIMAL_OUTS OUTS
       LEFT OUTER JOIN ANIMAL_INS INS
                       ON OUTS.ANIMAL_ID = INS.ANIMAL_ID
WHERE INS.ANIMAL_ID is NULL
ORDER BY OUTS.ANIMAL_ID
```

Problem URL : [루시와 엘라 찾기](https://programmers.co.kr/learn/courses/30/lessons/59046)

```sql
SELECT ANIMAL_ID, NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
WHERE NAME IN ('Lucy', 'Ella', 'Pickle', 'Rogan', 'Sabrina', 'Mitty')
```

Problem URL : [우유와 요거트가 담긴 장바구니](https://programmers.co.kr/learn/courses/30/lessons/62284)

```sql
SELECT DISTINCT CART_ID
FROM CART_PRODUCTS
WHERE NAME = 'Milk' AND
    CART_ID IN (SELECT CART_ID
                FROM CART_PRODUCTS
                WHERE NAME = 'Yogurt')
```
서브 쿼리 결과에서 특정 물건을 여러개 가지고 있을 수 있기 때문에, 본 절의 SELECT에 DISTINCT를 사용해주자.

```sql
SELECT DISTINCT A.CART_ID
FROM (SELECT CART_ID FROM CART_PRODUCTS WHERE NAME = 'Yogurt') A
INNER JOIN (SELECT CART_ID FROM CART_PRODUCTS WHERE NAME = 'Milk') B
ON A.CART_ID = B.CART_ID
```
조인을 활용할 수도 있다.