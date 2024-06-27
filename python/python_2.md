## 제어문

### 조건문
#### if문
1. 참/거짓을 판별 할 수 있는 조건식과 함께 사용
    ```
     if <조건식>:
        참인 경우
    else:
        거짓인 경우
    ```
#### elif문
1. if문과 동일 -> 조건이 여러개 일 때

    ```
    if <조건식>:
        if 조건식이 참인 경우
    elif <조건식>:
        elif 조건식이 참인 경우
    elif <조건식>:
        elif 조건식이 참인 경우
     ...
    else:
        모든 조건에 부합하지 않는 경우
    ```
2. 위부터 순서대로 검증

#### 조건표현식
1. if문 한 줄로 표현
    ```
    참인 경우 if <조건식> else 거짓인 경우
    ```

### 반복문
#### while문
1. 조건식이 참인 경우 코드를 반복해서 실행
2. 종료시키는 조건식이 주어져야 함 (무한루프에 빠지지않게 주의) 
    ```
    while <조건식>:
    ```

#### for문 
1. 정해진 범위 내의 반복
2. 시퀀스형 데이터 사용(directory도 가능)
    ```
    for variable(item) in sequence(시퀀스자료구조):
        code
    ```
    - variable - 시퀀스 자료구조 안의 데이터를 변수에 저장

#### Dictionary 반복문
1. 시퀀스 타입이 아니나 반복문 실행
2. for key in dict
3. for key in dict.keys():  딕셔너리의 key값 
4. for value in dict.values(): 딕셔너리의 value값
5. for key, value in dict.items() : 딕셔너리의 key, value값

#### break
1. 반복문을 종료시키는 키워드
2. if문으로 종료시키는 조건 부여

#### continue
1. continue 이후의 코드를 실행하지 않고 다음 반복을 진행 
2. 조건에 맞으면 넘어감 (skip)

#### else
1. else문은 끝까지 반복된 후 실행
2. break를 만나지 않았을 떄 실행

#### pass
1. 임시로 코드를 실행할 떄 사용(코드가 없을 떄 발생하는 에러 방지)

#### match
1. 조건에 부합하는 걸 바로 찾아냄

    ```
    match value:
        case 조건:
            code
        case 조건:
            code
        case _:
            code
    ```


