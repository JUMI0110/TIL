# HBASE
- HDFS 기반으로 구현한 컬럼 기반 분산 데이터베이스
- 대규모 데이터셋에서 실시간으로 읽고 쓰는 랜덤 엑세스가 필요할 때 사용할 수 있는 하둡 애플리케이션


NoSQL RDBMS


HBASE 실시간으로 데이터를 처리하는 곳에서 사용 
EX) 카카오톡 메시지 하나의 채팅방에 여러명의 메시지를 저장하는 공간
RDBMS
대용량의 데이터를 조회하는데 시간이 오래걸림
정렬이 안된 상태로 저장됨
HBASE 
순서대로 정렬을 미리 하고 정렬된 데이터를 조회
로우 키 : 직접적으로 지정
컬럼패밀리 : 
리전
그룹핑 리전 단위로 수평 분할 
row key별로 같은 위치에 있음


hbase:002:0> create 'students', 'info'
2024-08-29 10:56:24,337 INFO  [ReadOnlyZKClient-127.0.0.1:2181@0x131ba005] zookeeper.ZooKeeper (ZooKeeper.java:<init>(637)) - Initiating client connection, connectString=127.0.0.1:2181 sessionTimeout=90000 watcher=org.apache.hadoop.hbase.zookeeper.ReadOnlyZKClient$$Lambda$233/1237783515@32b823c9
2024-08-29 10:56:24,337 INFO  [ReadOnlyZKClient-127.0.0.1:2181@0x131ba005] zookeeper.ClientCnxnSocket (ClientCnxnSocket.java:initProperties(239)) - jute.maxbuffer value is 
1048575 Bytes
2024-08-29 10:56:24,337 INFO  [ReadOnlyZKClient-127.0.0.1:2181@0x131ba005] zookeeper.ClientCnxn (ClientCnxn.java:initRequestTimeout(1747)) - zookeeper.request.timeout value is 0. feature enabled=false
2024-08-29 10:56:24,339 INFO  [ReadOnlyZKClient-127.0.0.1:2181@0x131ba005-SendThread(127.0.0.1:2181)] zookeeper.ClientCnxn (ClientCnxn.java:logStartConnect(1177)) - Opening socket connection to server localhost/127.0.0.1:2181.
2024-08-29 10:56:24,339 INFO  [ReadOnlyZKClient-127.0.0.1:2181@0x131ba005-SendThread(127.0.0.1:2181)] zookeeper.ClientCnxn (ClientCnxn.java:logStartConnect(1179)) - SASL config status: Will not attempt to authenticate using SASL (unknown error)
2024-08-29 10:56:24,339 INFO  [ReadOnlyZKClient-127.0.0.1:2181@0x131ba005-SendThread(127.0.0.1:2181)] zookeeper.ClientCnxn (ClientCnxn.java:primeConnection(1013)) - Socket 
connection established, initiating session, client: /127.0.0.1:56670, server: localhost/127.0.0.1:2181
2024-08-29 10:56:24,347 INFO  [ReadOnlyZKClient-127.0.0.1:2181@0x131ba005-SendThread(127.0.0.1:2181)] zookeeper.ClientCnxn (ClientCnxn.java:onConnected(1453)) - Session establishment complete on server localhost/127.0.0.1:2181, session id = 0x1000a7f22180003, negotiated timeout = 40000
2024-08-29 10:56:26,824 INFO  [main] client.HBaseAdmin (HBaseAdmin.java:postOperationResult(3599)) - Operation: CREATE, Table Name: default:students, procId: 9 completed   
Created table students
Took 2.5092 seconds
=> Hbase::Table - students
hbase:003:0> list
TABLE
students
1 row(s)
Took 0.0224 seconds
=> ["students"]
hbase:004:0> 2024-08-29 10:57:24,481 INFO  [ReadOnlyZKClient-127.0.0.1:2181@0x131ba005] zookeeper.ZooKeeper (ZooKeeper.java:close(1232)) - Session: 0x1000a7f22180003 closed2024-08-29 10:57:24,482 INFO  [ReadOnlyZKClient-127.0.0.1:2181@0x131ba005-EventThread] zookeeper.ClientCnxn (ClientCnxn.java:run(569)) - EventThread shut down for session: 
0x1000a7f22180003

hbase:005:0> put 'students', '1', 'info:name', 'kim'
2024-08-29 10:59:04,349 INFO  [ReadOnlyZKClient-127.0.0.1:2181@0x131ba005] zookeeper.ZooKeeper (ZooKeeper.java:<init>(637)) - Initiating client connection, connectString=127.0.0.1:2181 sessionTimeout=90000 watcher=org.apache.hadoop.hbase.zookeeper.ReadOnlyZKClient$$Lambda$233/1237783515@32b823c9
2024-08-29 10:59:04,350 INFO  [ReadOnlyZKClient-127.0.0.1:2181@0x131ba005] zookeeper.ClientCnxnSocket (ClientCnxnSocket.java:initProperties(239)) - jute.maxbuffer value is 
1048575 Bytes
2024-08-29 10:59:04,350 INFO  [ReadOnlyZKClient-127.0.0.1:2181@0x131ba005] zookeeper.ClientCnxn (ClientCnxn.java:initRequestTimeout(1747)) - zookeeper.request.timeout value is 0. feature enabled=false
2024-08-29 10:59:04,352 INFO  [ReadOnlyZKClient-127.0.0.1:2181@0x131ba005-SendThread(127.0.0.1:2181)] zookeeper.ClientCnxn (ClientCnxn.java:logStartConnect(1177)) - Opening socket connection to server localhost/127.0.0.1:2181.
2024-08-29 10:59:04,352 INFO  [ReadOnlyZKClient-127.0.0.1:2181@0x131ba005-SendThread(127.0.0.1:2181)] zookeeper.ClientCnxn (ClientCnxn.java:logStartConnect(1179)) - SASL config status: Will not attempt to authenticate using SASL (unknown error)
2024-08-29 10:59:04,352 INFO  [ReadOnlyZKClient-127.0.0.1:2181@0x131ba005-SendThread(127.0.0.1:2181)] zookeeper.ClientCnxn (ClientCnxn.java:primeConnection(1013)) - Socket 
connection established, initiating session, client: /127.0.0.1:42130, server: localhost/127.0.0.1:2181
2024-08-29 10:59:04,355 INFO  [ReadOnlyZKClient-127.0.0.1:2181@0x131ba005-SendThread(127.0.0.1:2181)] zookeeper.ClientCnxn (ClientCnxn.java:onConnected(1453)) - Session establishment complete on server localhost/127.0.0.1:2181, session id = 0x1000a7f22180004, negotiated timeout = 40000
Took 0.1649 seconds
hbase:006:0> put 'students', '1', 'info:age', '10'
Took 0.0052 seconds
hbase:007:0> put 'students', '1', 'info:gender', 'M'
Took 0.0061 seconds

hbase:009:0> put 'students', '2', 'info:name', 'hong'
Took 0.0037 seconds
hbase:010:0> put 'students', '2', 'info:age', '20'
Took 0.0042 seconds
hbase:011:0> put 'students', '2', 'info:gendr', 'F'
Took 0.0042 seconds
hbase:012:0> scan 'students'
ROW                    COLUMN+CELL
 1                     column=info:age, timestamp=2024-08-29T10:59:40.210, value=10   
 1                     column=info:gender, timestamp=2024-08-29T11:00:00.211, value=M 
 1                     column=info:name, timestamp=2024-08-29T10:59:04.467, value=kim 
 2                     column=info:age, timestamp=2024-08-29T11:00:38.073, value=20   
 2                     column=info:gendr, timestamp=2024-08-29T11:00:57.521, value=F  
 2                     column=info:name, timestamp=2024-08-29T11:00:30.424, value=hong2 row(s)
Took 0.0375 seconds
hbase:013:0> get 'students', '1'
COLUMN                 CELL
 info:age              timestamp=2024-08-29T10:59:40.210, value=10
 info:gender           timestamp=2024-08-29T11:00:00.211, value=M
 info:name             timestamp=2024-08-29T10:59:04.467, value=kim
1 row(s)
Took 0.0188 seconds

## update
hbase:016:0> put 'students', '1', 'info:age', '100'
Took 0.0083 seconds
hbase:017:0> scan 'students'
ROW                    COLUMN+CELL
 1                     column=info:age, timestamp=2024-08-29T11:10:10.299, value=100 
 1                     column=info:gender, timestamp=2024-08-29T11:00:00.211, value=M 1                     column=info:location, timestamp=2024-08-29T11:06:47.567, value                       =seoul
 1                     column=info:name, timestamp=2024-08-29T10:59:04.467, value=kim 2                     column=info:age, timestamp=2024-08-29T11:00:38.073, value=20  
 2                     column=info:gendr, timestamp=2024-08-29T11:00:57.521, value=F 
 2                     column=info:name, timestamp=2024-08-29T11:00:30.424, value=hon                       g
2 row(s)
Took 0.0179 seconds

## delete
- delete : 컬럼까지 지정
hbase:018:0> delete 'students', '1', 'info:location'
Took 0.0130 seconds
- deleteall:  모든 정보 삭제
hbase:019:0> deleteall 'students', '2'
Took 0.0041 seconds
- 테이블 삭제 disable 테이블 비활성화 후 drop 삭제
hbase:020:0> disable 'students'
hbase:021:0> drop 'students'

