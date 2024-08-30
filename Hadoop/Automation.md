위키백과 cron

cron
잡 스케줄러

쉘 스크립트
echo 'hello' >> ./test.log
왼쪽의 결과물 오른쪽에 append
echo 'hello' > ./test.log
왼쪽의 결과물 오른쪽에 덮어씌우기


권한 
rwx rwx rwx
420 420 420




```sql
CREATE EXTERNAL TABLE IF NOT EXISTS logs (
    ip_address STRING,               -- IP 주소
    log_timestamp STRING,            -- 로그 타임스탬프 (날짜 및 시간)
    http_method STRING,              -- HTTP 메소드 (GET, POST, PUT, DELETE)
    request_path STRING,             -- 요청 경로 (URI)
    protocol STRING,                 -- 프로토콜 (HTTP 버전)
    status_code INT,                 -- HTTP 상태 코드
    response_size INT                -- 응답 크기 (바이트)
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.RegexSerDe'
WITH SERDEPROPERTIES (
    "input.regex" = "^(\\S+) \\[(\\S+ \\S+)\\] \\\"(\\S+) (\\S+) (\\S+)\\\" (\\d+) (\\d+)$"
)
STORED AS TEXTFILE
LOCATION 'input/logs';
```
정규표현식   
"^(\\S+) \\[(\\S+ \\S+)\\] \\\"(\\S+) (\\S+) (\\S+)\\\" (\\d+) (\\d+)$"   
"문자열 \t 대괄호 문자열 \t 문자열 대괄호 \t "문자열" \t   

웹크롤링 통해서 자동화 