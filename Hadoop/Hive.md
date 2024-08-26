# Hive
- HDFS에 저장된 데이터를 SQL로 추출할 수 있는 서비스
- 스키마 작성하여 스키마에 맞게 데이터를 불러옴
    - 메타스토어 - 스키마와 같은 메타데이터 저장하는 데이터베이스
- HDFS -> 메타스토어 -> SQL쿼리 -> MapReduce job 변환 -> 하둡 클러스터 구동
- 하이브서버 접근할 때 쓰는 프로그램 (Beeline 사용)

## Hive vs RDBMS
- RDBMS 데이터 로드(읽고 저장)하는 시점 - 쓰기 스키마
    - 쓰기 스키마 :
데이터 로드하는 시간 오래걸리나 쿼리 빠르게 수행
- Hive 쿼리 실행 하는 시점 - 읽기 스키마
    - 읽기스키마 : 
직렬화 할 필요 없어서 빠른 속도로 데이터 로드


### HiveQL 
- SQL과 유의한 하이브 질의 언어
- 명령어는 반드시 ';'으로 끝나야 실행
- 대소문자 구분 X, tab키로 예약어와 함수 자동완성 가능


### hive 설치 후 경로 설정 저장
- ubuntu/.bashrc 환경변수 등록
```bash
ubuntu@2-12:~$ source .bashrc -> bashrc 새로고침
ubuntu@2-12:~$ echo $HIVE_HOME -> 경로 출력
/home/ubuntu/hive-3.1.3
```

### metadata_store 위한 DB생성 
hive_home에서 (설치되는 곳에 db폴더 생성됨)
```bash
#schematool -dbType <database-type> -upgradeSchema   
schematool -dbType derby -initSchema
```

### hive 설치확인 (터미널 환경조성)
terminal hive
```bash
hive> show tables;
OK
Time taken: 0.437 seconds
```
```bash
# hive.metastore.warehouse.dir의 기본 설정 위치. 데이터 웨어하우스를 저장하는 기본 디렉토리
$ hadoop fs -mkdir -p /user/hive/warehouse
$ hadoop fs -chmod g+w /user/hive/warehouse # chmod : changemode g+w : group + write
```


## hive server
- $HIVE_HOME/conf/hive-site.xml 생성
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <property>
        <name>hive.server2.enable.doAs</name>
        <value>false</value>
    </property>
</configuration>
```

### 실행 
```bash
# 10000포트로 열고 , log 출력하는 옵션
hiveserver2 --hiveconf hive.server2.thrift.port=10000 --hiveconf hive.root.logger=DEBUG,consol

# 실행이 잘 되는지 확인
Hive Session ID = 4cf46837-09c8-462f-a740-542415622a21
Hive Session ID = e93023f6-2a24-4711-a2ff-0742d7e8f017

```
​




