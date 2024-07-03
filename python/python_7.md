# 객체지향 프로그래밍(OOP)
- 클래스(class) : 같은 종류의 집단에 속하는 속성(attribute)과 행위(method)를 정의하는 것
- 인스턴스(instance) : 클래스를 실제로 메모리상에 할당한 것
- 속성(attribute) : 클래스/인스턴스가 가지고 있는 데이터/값
- 행위(method): 클래스/인스턴스가 가지고 있는 함수/기능

## Class
- 선언
```python
class ClassName():
    attribute1 = value1
    attribuet2 = value2
    ...

    def method_name(self):
        code
    def ...
```
- 인스턴스화(클래스 실행)
    - 독립적인 객체로 인스턴스화
```python
c = ClassName()
```
### self
- 새롭게 만들어진 인스턴스
- 인스턴스 안의 데이터를 활용하기 위해 메소드에 self(인스턴스)인자를 넣어줌

## 생성자, 소멸자
```python
class MyClass():
    
    def __init__(self):
        pass
    
    def __del__(self):
        pass
```
- 함수의 이름이 정해져 있음
- 클래스를 실행한다는 것은 클래스 안의 `__init__` 메소드 실행하는 것
- 소멸자는 클래스안에 존재하나 보이지 않음 인스턴스를 여러번 실행했을 때 인스턴스가 저장되는 메모리가 겹치지 않게 이전의 인스턴스 삭제

## 클래스 변수
 - 클래스 선언 블록 최상단에 위치

## 인스턴스 변수
 - 인스턴스 내부에서 생성한 변수 (`self.variable = ''`)
 ```python
class MyClass():
    class_variable = '클래스 변수'

    def __init__(self):
        self.instance_variable = '인스턴스 변수'

 ```

 ## 클래스 메소드, 인스턴스 메소드, 스택틱 메소드
 ```python
 class MyClass():
    def instance_method(self):
        code
    
    @classmethod
    def class_method(cls):
        code
    
    @staticmethod
    def static_method():
        code
```
- 클래스메소드 : 클래스에 있는 변수 사용
- 인스턴스메소드: 인스턴스가 가지고 있는 정보에 접근하려면 사용
- 클래스, 인스턴스에 담긴 데이터와 상관없이 사용

## 상속
- 객체의 관계를 상하 관계라 하면 상위 객체의 기능을 하위 객체에 따로 명시하지 않아도 상속받으면 사용가능
```python
class MyClass(상속받을 객체명):
    ...
```
- `__init__` 메소드를 상위 하위 객체에 둘 다 사용하면 가까운 객체의 메소드를 먼저 사용하기 때문에 상위 객체의 `__init__` 메소드를 사용하기 위해 super() 함수 사용
    - super()함수 사용시 `__init__`메소드로 받을 인자는 다 넣어 줘야함 

## 다중 상속
- 객체는 여러 객체를 상속받을 수 있음 
    - 최상위 -> 상위 -> 하위  하위 객체는 모든 기능을 상속받아 사용할 수 있음
    