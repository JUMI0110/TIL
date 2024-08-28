
# HiveQL practice
## csv serde
- serde로 테이블 생성시 타입이 문자형으로 인식되는 행이 있음 
- views 이용해서 복제테이블 생성 후 CAST함수로 형변환
>sepratorChar: 칼럼간의 구분자     
quoteChar: 칼럼의 값을 지정한 문자로 묶어준다.   
escapeChar: 칼럼에 데이터를 입력할 때 파싱하지 않고 무시      
출처: https://118k.tistory.com/451 [개발자로 살아남기:티스토리]
```sql
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
WITH SERDEPROPERTIES (   
    "separatorChar" = ",",   
    "quoteChar"     = "'",   
    "escapeChar"    = "\\")  
```
## 테이블 생성
```sql
CREATE EXTERNAL TABLE books (
isbn STRING,
book_title STRING,
book_author STRING,
year_of_publication DATE,
publisher STRING,
image_url_s STRING,
image_url_m STRING,
image_url_l STRING
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
WITH SERDEPROPERTIES (
   "separatorChar" = ";",
   "quoteChar"     = "\""
)
STORED AS TEXTFILE
LOCATION '/user/ubuntu/input/book/books'
TBLPROPERTIES ("skip.header.line.count"="1");
```
```sql
CREATE EXTERNAL TABLE ratings (
User_ID INT,
ISBN STRING,
Book_Rating INT
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
WITH SERDEPROPERTIES (
   "separatorChar" = ";",
   "quoteChar"     = "\""
)
STORED AS TEXTFILE
LOCATION '/user/ubuntu/input/book/book_rating'
TBLPROPERTIES ("skip.header.line.count"="1");
```

```sql
CREATE EXTERNAL TABLE users (
User_ID INT,
Location STRING,
Age INT
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
WITH SERDEPROPERTIES (
   "separatorChar" = ";",
   "quoteChar"     = "\""
)
STORED AS TEXTFILE
LOCATION '/user/ubuntu/input/book/users'
TBLPROPERTIES ("skip.header.line.count"="1");
```

## 1. 데이터의 기초 정보 확인

### 1.1 중복 데이터 확인

- **Books 테이블에서 중복된 ISBN 확인**
```sql
SELECT isbn, COUNT(*) AS c1
FROM books
GROUP BY isbn
ORDER BY c1 DESC;
```
- **Ratings 테이블에서 중복된 사용자-책 평가 확인**
```sql
SELECT user_id, isbn, COUNT(book_rating) AS c2
FROM ratings
GROUP BY user_id, isbn
ORDER BY c2 DESC
LIMIT 10;

```

### 1.2 결측값 확인

- **Users 테이블에서 Age의 결측값 확인**
```sql
SELECT COUNT(Age)
FROM users
WHERE Age = 'Null'
GROUP BY Age;

```
- **Books 테이블에서 Year_Of_Publication의 결측값 확인**
```sql
SELECT COUNT(*)
FROM books
WHERE year_of_publication IS NULL;
```

## 2. 데이터의 기초 통계 확인

### 2.1 사용자 연령 통계 확인

- **사용자 연령의 기초 통계(최소, 최대, 평균)를 확인합니다.**
```sql
SELECT MIN(CAST(Age AS INT)), MAX(CAST(Age AS INT)), AVG(CAST(Age AS INT))
FROM users
WHERE CAST(Age AS STRING) != 'Null';


```

### 2.2 출판 연도 통계 확인
**책의 출판 연도에 대한 기초 통계(최소, 최대, 평균)를 확인합니다.**
```sql
SELECT MIN(CAST(year_of_publication AS INT)), MAX(CAST(year_of_publication AS INT)), AVG(CAST(year_of_publication AS INT))
FROM books
WHERE CAST(year_of_publication AS STRING) != 'Null';
```
### 2.3 평점의 분포 확인
```sql
SELECT CAST(book_rating AS INT), COUNT(*)
FROM ratings
GROUP BY CAST(book_rating AS INT);
```
## 3. 데이터의 주요 패턴 탐색
### 3.1 출판사별 책 수 및 평균 평점
**출판사별로 얼마나 많은 책이 있는지, 그리고 그 책들의 평균 평점이 어떤지 확인합니다.**
```SQL
SELECT b.publisher, COUNT(*) AS book_count, AVG(CAST(book_rating AS INT)) AS avg_rating
FROM books b
JOIN ratings r ON b.isbn = r.isbn
GROUP BY b.publisher
ORDER BY book_count DESC
LIMIT 10;
```
### 3.2 가장 많이 평가된 책과 그 평점

**가장 많이 평가된 책이 무엇인지 확인합니다.**
```SQL
SELECT b.book_title, COUNT(r.book_rating) AS rating_count, AVG(CAST(book_rating AS INT)) AS avg_rating
FROM books b
JOIN ratings r ON b.isbn = r.isbn
GROUP BY b.book_title
ORDER BY rating_count DESC
LIMIT 10;
```
## 4. 데이터 관계 분석

### 4.1 책 평점과 출판 연도 간의 관계

**책의 출판 연도와 평점 간의 관계를 확인합니다.**
```sql
SELECT year_of_publication, AVG(book_rating) AS avg_rating
FROM books b
JOIN ratings r ON b.isbn = r.isbn
GROUP BY year_of_publication
LIMIT 10;
```
### 4.2 사용자 위치별 평점 차이
**위치에 따라 평균 평점을 출력합니다. 적어도 10개 이상의 평가를 한 경우만 출력합니다.**
```sql
SELECT u.location, AVG(book_rating) AS avg_rating, COUNT(r.book_rating) AS rating_cnt
FROM users u
JOIN ratings r ON u.user_id = r.user_id
GROUP BY u.location 
HAVING rating_cnt >= 10
ORDER BY avg_rating DESC
LIMIT 10;
```
### 4.3 책 저자별 평균 평점

**각 저자별로 평균 평점이 어떻게 다른지 확인합니다. 적어도 10개 이상의 평가를 한 경우만 출력합니다.**
```sql
SELECT b.book_author, AVG(r.book_rating) AS avg_rating, COUNT(r.book_rating) AS rating_cnt
FROM books b
JOIN ratings r ON b.isbn = r.isbn
GROUP BY b.book_author
HAVING rating_cnt > 10
ORDER BY avg_rating DESC
LIMIT 10;
```

### 4.4 평점 상위 10개의 책이 출판된 연도 분포

**평점 상위 10개의 책이 어느 연도에 많이 출판되었는지 확인합니다.**
```sql
WITH avg AS(
    SELECT b.book_title, b.year_of_publication, AVG(r.book_rating)
    FROM books b
    JOIN ratings r ON b.isbn = r.isbn
    GROUP BY r.isbn, b.year_of_publication, b.book_title
    HAVING COUNT(r.book_rating) >= 10
    ORDER BY AVG(r.book_rating) DESC
    LIMIT 10
)
SELECT year_of_publication, COUNT(book_title) AS number_of_books
FROM avg
GROUP BY year_of_publication
ORDER BY number_of_books DESC;

```
```bash
숫자가 약간 다르게 나옴.. ㅎ
+----------------------+------------------+
| year_of_publication  | number_of_books  |
+----------------------+------------------+
| 1999                 | 2                |
| 2000                 | 1                |
| 1998                 | 1                |
| 1993                 | 1                |
| 1986                 | 1                |
| 1979                 | 1                |
| 1971                 | 1                |
| 1961                 | 1                |
| 0                    | 1                |
+----------------------+------------------+
```
**맞는 코드**
```sql
SELECT b.year_of_publication, COUNT(b.book_title) AS cnt_book_title
FROM books b
JOIN 
    (SELECT a.isbn, 
            SUM(c.book_rating) / COUNT(c.book_rating) AS avg_rating
     FROM ratings c
     JOIN books a
     ON a.isbn = c.isbn
     GROUP BY a.isbn
     ORDER BY avg_rating DESC
     LIMIT 10) r
ON b.isbn = r.isbn
GROUP BY b.year_of_publication
ORDER BY cnt_book_title DESC;
```

