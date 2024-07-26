# Django 
## 기본 설정

1. 프로젝트 생성
- 파일이 만들어진 상태
```bash
django-admin startproject {project_name} .
``` 
- 파일과 같이 생성
```bash
django-admin startproject {project_name}`
```
2. 서버 실행
```bash
python manage.py runserver
``` 

3. 앱 생성 및 등록

- 생성
```bash
django-admin startapp {app_name}
```
- 등록

settings.py -> INSTALLED_APPS 리스트 -> app 추가

### urls.py
- project_name/urls.py -> urlpatterns
#### path()
- path(요청받은 경로, 실행)
- path('ccc/<변수>/', views.ccc) -> <변수>: 어떤데이터가 들어와도 실행될 수 있도록 변수로 지정

```python
urlpatterns = [
    path('aaa/', views.aaa),
    path('bbb/', views.bbb),
    path('ccc/<변수>/', views.ccc),
    ...
]
```


### views.py
- app_name/views.py
```python
def aaa(request):
    return render(request, 'aaa.html')
```
```python
def bbb(request):
    code
    context = {
        'key': value
    }
    return render(request, 'bbb.html', context)
```
```python
def ccc(request, 변수):
    code
    
```
#### render()
- request: 요청을 response하기 위해 받는 필수 인자
- template_name : template full name, 필수 인자
- context : view에서 사용한 변수를 html로 넘기는 역할, 즉 template에서 쓰이는 변수명과 파이썬 객체를 연결. key:value 의 dict 형태로 담아야 한다
    - key: template에서 사용할 변수명
    - value: 파이썬 변수명
- content_type: 타입 정의, text/html 가 default다
- status : status code, 200이 default
- using : 템플릿 엔진 사용을 위한 이름 정의
