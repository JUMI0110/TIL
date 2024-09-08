# Spark 
- SQL, 스트리밍, 머신러닝 및 그래프 처리를 위한 기본제공 모듈이 있는 대규모 데이터처리용 통합 분석 엔진
- 메모리 내에서 계산하기 때문에 디스크에서 처리하는 프레임워크보다 빠르게 데이터 처리 가능
> Hadoop과 Spark   
    - 하둡은 데이터를 일괄적으로 처리, 스파크 일괄처리 및 실시간 스트리밍   
    - 스파크 인메모리 데이터를 사용하기 때문에 하둡보다 빠름   
    - 하둡은 2단계 실행 프로세스, 스파크는 프로세스를 동시에 효율적으로 처리 


## 장점 
1. 속도 - 데이터가 분산 클러스터 노드 전체에 걸쳐 인메모리 처리를 확장하도록 구성
2. 다양한 언어 지원 - Scala로 작성되어 Java, Python, R 등의 언어사용 가능
3. 편리한 사용 - 병렬 데이터 처리와 데이터 추상화 수행
4. 보편성 - SQL, DataFrame, 머신러닝용 MLlib, GraphX, Spark Streaming을 비롯한 다양한 라이브러리 지원

## Spark 시작하기
- install => 경로설정 => pyspark   
> ** pyspark **
python 환경에서 spark 사용할 수 있는 인터페이스

- 실행
```python
rdd = sc.parallelize([1, 2, 3, 4, 5])

print(rdd.collect())

# ===

mapped_rdd = rdd.map(lambda x: x * 2)
print(mapped_rdd.collect())

# ===

filtered_rdd = rdd.filter(lambda x: x % 2 == 0)
print(filtered_rdd.collect())

# ===

sum_value = rdd.reduce(lambda x, y: x + y)
print(sum_value)

# ===

count = rdd.count()
average_value = sum_value / count
print(average_value)
```
### RDD
- Resilient Distributed Dataset 스파크의 기본 데이터 구조로 분산 변경 불가능한 객체 모음
- 스파크의 모든 작업은 새로운 RDD를 만들거나 존재하는 RDD를 변형하거나 결과 계산을 위해 RDD에서 연산하는 것을 표현 
- read-only 메모리 내용을 갱신하지 않음
- 스파크는 액션을 할 때 까지 변환을 실행하지 않음 Action을 해야 변환하여 실행!!!

#### function
- rdd.map(function) : Map 작업   
- rdd.reduce(function) : Reduce
- rdd.filter(function) : 조건 조회
- rdd.collect() : 확인

