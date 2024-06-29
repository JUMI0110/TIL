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