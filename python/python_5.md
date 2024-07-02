# 모듈
- import 모듈: 모듈을 불러와서 실행
- 재사용할 함수를 파일 단위로 관리 (수정이 용이하다)

```python
import 파이썬파일명
```
# Package 패키지
- 패키지 안에 `__init__ `파일이 존재해야만 패키지로 인식

```
myPackage/
    __init__.py
    math/
        __init__.py
```

# 파이썬 내장 패키지
## math
```python
import math
math.함수()
```

## random
```python
import random
random.함수()
```
- random함수
    - random.random(): 0과 1 사이의 랜덤한 소수
    - random.randint(x, y): x부터 y까지 랜덤한 숫자 1개(복원)
    - random.seed(x): 임의의 숫자를 넣어, 결과값 고정을 필요로한 함수와 함께 사용 
    - random.shuffle(범위): 배열을 무작위로 섞어줌
    - random.choice(범위): 배열 안의 랜덤한 숫자 1개 
    - random.sample(범위, x): 배열 안의 x개 숫자 (비복원)

## datetime
- 날짜와 시간
```python
from datetime import datetime
datetime.함수()
```

# 라이브러리(외장패키지)
```python
import requests
payload = 'url?' ? 뒤에 사이트를 여는 조건 (ex query = '야구')
r = requests.get('url', payload)
```
-> 실행 했을 때 Response [200] 나오면 실행 완료
