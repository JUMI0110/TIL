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