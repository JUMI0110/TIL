# CRUD (Create Read Update Delete)
## Model
- 모델(객체)을 저장하면 그 내용이 데이터베이스에 저장됨

1. 모델 정의
```python
class ModelName(models.Model):
    title = models.CharField(max_length)
    content = models.TextField()
```
- 속성 정의(데이터타입) 
    - CharField() : 글자 수가 제한된 제목같은 짧은 텍스트(max_length 같이 사용)
    - TextField() : 글자 수가 제한이 없는 긴 텍스트
    - SlugField() : 제목을 쓸 때 중요한 의미의 단어만 이용해 작성   
    ...

2. 번역본 생성
- 파이썬 코드를 SQL언어로 번역 makemigrations -> sql언어로 만들기 전단계

```bash
python manage.py makemigrations
```

3. DB에 반영
- SQL에 적용

```bash
python manage.py migrate
```
