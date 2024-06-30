# 함수 (function)
- 코드의 재사용을 위해서 함수 사용

## 함수의 선언
def(정의) 함수의 이름(기능할 수 있게 인수1, 인수2):\
    code1\
    code2\
    ...\
    return value-함수의 결과를 값에 반환

```python
def func_name(parameter1, parameter2, ...):
    code1
    code2
    ...
    return value
```

## 함수의 호출(실행)
함수의 이름(인수1, 인수2, ...)

```python
func_name(인수1, 인수2)
```
 \* 파이썬에 원래 있는 함수 - 내장함수

    `dir(__builtins__)`

## 함수의 return
- 함수가 return을 만나면 해당 값을 반환하고 함수를 종료
- 만약 return이 없는 경우 None을 자동으로 반환
- return은 오직 하나의 객체만 반환\
\* 파이썬 기능적으로 두 개의 데이터를 하나의 객체로 묶어서 결과 반환 
    ```python
    def my_def(x):
        return x, x * 2
    my_def(10)
    -> (10, 20)
    ```
    - return 결과물은 변수에 저장하여 사용할 수 있음 
    - return 결과는 데이터가 나오지만 print()하지 않으면 저장만 되어있음

## 함수의 인수
### 위치 인수
- 기본적으로 함수는 위치를 기준으로 인수를 판단

### 기본값
- '인수=값'으로 기본값을 넣어주면 인수를 넣지 않아도 기본값 출력
- 기본값을 앞에 쓰면 에러, 인수가 2개 이상일 때 기본값은 데이터가 선택적이기 때문에 뒤에 작성

```python
def func(p1, p2=v1)
    code
    ...
    return value
```
### 키워드 인수
- 함수를 호출(실행)할 때 내가 원하는 위치에 직접적으로 특정 인수를 전달 
```
func(p1='',p2='')
```

### 가변 인수
- 들어오는 데이터가 변할 수 있음
- *parms: 몇개의 데이터가 들어올지 모르나 Tuple형으로 묶어서 관리

```python
def func(*parms):
    code
```
### 정의되지 않은 키워드 인수 처리하기
- 'key=value' 형태로 값을 넣으먄 딕셔너리 형태로 변환

```python
def func(**kwrgs):
    code...

func(asdf='', qwer='')
->{'asdf'='', 'qwer'=''}
```

### dictionary를 인수로 넣기(unpacking)
- dictionary로 패키지화 시켜서 인수로 넣고 함수는 unpacking하여 기능

```python
def func(p1, p2, p3)
    code

func(p1, p2, p3)
↓
info = {
    p1 ='',
    p2 ='',
    p3 ='',
}
func(**info)
```

### lambda 표현식
- 함수를 축약해서 정리 -> 속도가 빠름
```python
lambda parameter : expression
```

### 타입힌트
- '인수: 타입' 인수에 넣어야 할 타입을 알려주는 힌트

```python
def func(a: int, b: int)-> int:
    code
```

### 이름공간
- python에서 사용되는 이름(변수, 함수의 이름)들은 이름공간에 저장
- 아래 순서대로 인지하여 기능

1. Local: 정의된 함수 내부
2. Enclosed: 상위함수
3. Global: 함수 밖
4. Built-in: python이 기본적으로 가지고 있는 함수 / 변수

### 재귀
- 재귀 함수는 함수 내부에서 자기 자신을 호출하는 함수
```python
def fact(n):
    if n == 1:
        return 1
    else:
        return fact(n-1) * n
```

