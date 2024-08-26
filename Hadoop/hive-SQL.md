#  Managed Table
- 테이블의 위치, 데이터의 위치 하나로 묶어서 관리
- 테이블 지우면 데이터 또한 삭제 
## 테이블 생성
- employees 파일 다운 받고 그 파일 규격 정해서 테이블 생성
```SQL
0: jdbc:hive2://localhost:10000> CREATE TABLE employees (
. . . . . . . . . . . . . . . .> emp_id INT,
. . . . . . . . . . . . . . . .> birth_dat DATE,
. . . . . . . . . . . . . . . .> first_name STRING,
. . . . . . . . . . . . . . . .> last_name STRING,
. . . . . . . . . . . . . . . .> gender STRING,
. . . . . . . . . . . . . . . .> hire_date DATE,
. . . . . . . . . . . . . . . .> dept_id STRING
. . . . . . . . . . . . . . . .> )
. . . . . . . . . . . . . . . .> ROW FORMAT DELIMITED  -- 규격
. . . . . . . . . . . . . . . .> FIELDS TERMINATED BY ',' -- 구분
. . . . . . . . . . . . . . . .> LINES TERMINATED BY '\n'; -- row 새로운 라인
```

## 스키마 연결
- Data Load : 데이터 파일 경로
- 하이브 로컬파일 로드   
경로 hive>warehouse>employees (로컬파일) 복제하여 HDFS 저장
```sql
LOAD DATA LOCAL INPATH '/home/ubuntu/dmf/dataset/employees'
OVERWRITE INTO TABLE employees;
```
- overwrite : 테이블과 데이터 연결


#### 데이터 확인
```sql
SELECT * FROM employees LIMIT 10;
```
- 생일이 같은 사람 수 카운트
```SQL
SELECT birth_dat, count(birth_dat)
FROM employees
GROUP BY birth_dat
ORDER BY count(birth_dat) DESC
LIMIT 10;
```


# External Table
- 직접관리 
- HDFS 파일 업로드 (파일 저장 할 폴더생성)
- 경로 폴더까지만 작성 (hive의 규칙)

## HDFS 파일 저장
```bash
hdfs dfs -mkdir input/employees
hdfs dfs -mkdir input/departments

hdfs dfs -put ~/dmf/dataset/employees input/employees
hdfs dfs -put ~/dmf/dataset/departments input/departments
```

## 테이블 생성
- CREATE TABLE 사이에 EXTERNAL 넣어서 생성
- 저장 형식과 불러올 파일 위치 넣어서 생성
```sql
employees
CREATE EXTERNAL TABLE employees (
	emp_id INT,
	birth_date DATE,
	first_name STRING,
	last_name STRING,
	gender STRING,
	hire_date DATE,
	dept_id STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
STORED AS TEXTFILE
LOCATION 'user/ubuntu/input/employees';
```
```sql
departments
CREATE EXTERNAL TABLE departments (
    dept_id STRING,
    dept_name STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
STORED AS TEXTFILE
LOCATION '/user/ubuntu/input/departments';
```
#### 데이터 확인
- JOIN test
```SQL
SELECT e.first_name, d.dept_name
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
LIMIT 10;
```

- 성별이 남성인 직원 호출
```SQL
SELECT *
FROM employees
WHERE gender='M'
LIMIT 10;
```

- Development 부서에 속한 사람들의 정보 출력
```SQL
SELECT *
FROM employees e
JOIN departments d ON e.dept_id=d.dept_id
WHERE d.dept_name = 'Development'
LIMIT 10;
```

- 부서별 직원 수 
```SQL
SELECT d.dept_name, COUNT(e.emp_id) 
FROM employees e
JOIN departments d ON e.dept_id=d.dept_id
GROUP BY d.dept_name;
```


#### movie lens query
- 데이터저장, 데이터생성 -> External Table   
[수업Notion](https://echo-edu.notion.site/hive-1dc85a2757304b0cb8c541e1752c534c)

1. **사용자별 평균 평점 내림차순 계산하기:**
```sql
SELECT user_id, AVG(rating) AS avg_rating 
FROM u_data 
GROUP BY user_id
ORDER BY avg_rating
LIMIT 10;
```
2. **평점 분포 계산하기:**
```sql
SELECT rating, COUNT(*)
FROM u_data
GROUP BY rating;
```
3. **가장 많이 평가한 사용자 찾기:**
```sql
SELECT user_id, COUNT(*) AS rating_cnt
FROM u_data
GROUP BY user_id
ORDER BY rating_cnt DESC
LIMIT 10;
```
4. **영화별 평균 평점 계산하기:**
```sql
SELECT i.title, AVG(d.rating) AS avg_rating, COUNT(d.rating)
FROM u_data d
JOIN u_item i ON d.movie_id=i.movie_id
GROUP BY i.title
HAVING COUNT(d.rating) >= 10
ORDER BY avg_rating DESC
LIMIT 10;
```
5. **특정 연령대(예: 18-24세)의 영화 평점 분포:**
```sql
SELECT i.title, AVG(d.rating) AS avg_rating
FROM u_data d
JOIN u_user u ON d.user_id = u.user_id
JOIN u_item i ON d.movie_id=i.movie_id
WHERE u.age BETWEEN 18 AND 24
GROUP BY i.title
ORDER BY avg_rating
LIMIT 10;

```
6. **가장 많이 평가된 영화 찾기:**
```sql
SELECT i.title, COUNT(d.rating) AS rating_cnt 
FROM u_data d
JOIN u_item i ON d.movie_id = i.movie_id
GROUP BY i.title
ORDER BY rating_cnt DESC
LIMIT 10;

```
7. **성별에 따른 영화 평점 차이 분석:**
```sql
SELECT i.title, u.gender, AVG(d.rating) AS avg_rating
FROM u_data d
JOIN u_item i ON d.movie_id = i.movie_id
JOIN u_user u ON d.user_id = u.user_id
GROUP BY i.title, u.gender
LIMIT 10;
```
8. **특정 직업군(예: 학생)의 영화 선호도 분석:**
```sql
SELECT i.title, AVG(d.rating) AS avg_rating
FROM u_data d
JOIN u_item i ON d.movie_id = i.movie_id
JOIN u_user u ON d.user_id = u.user_id
WHERE u.occupation = 'student'
GROUP BY i.title
ORDER BY avg_rating DESC
LIMIT 10;
```

>  흠... SQL 문법 공부 다시 해야겠음... 기본만 기억이 남 😢
요즘 너무 머리에 집어넣는게 많다는 핑계 아닌 핑계 열심히 해보자! 