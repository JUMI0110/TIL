# Django 
## 기본 설정
1. 프로젝트 생성- 파일이 만들어진 상태
```bash
django-admin startproject {project_name} .
``` 
- 파일과 같이 생성
```bash
django-admin startproject {project_name}
```
2. 서버 실행
```bash
python manage.py runserver
``` 
3. 앱 생성 및 등록- 생성
```bash
django-admin startapp {app_name}
```
- 등록settings.py -> INSTALLED_APPS 리스트 -> app 추가
---
### django MTV(Model Template View) 이해
![MTV]https://github.com/DMF-DA1/first_pjt/raw/master/assets/MTV.png
- django는 MVC(Model View Controller) 기반의 프레임워크 이나 같은 개념을 MTV라고 부른다
- 데이터저장 형태를 어떻게할지 설정 → Model
- 유저에게 보여지는 화면 수정 → Template
- 데이터를 처리해서 가공 → View
\* MVC패턴은 데이터(model), 사용자 인터페이스(view), 데이터 처리 로직(controller)을 구분해 한 요소가 다른 요소들에게 영향을 주지 않도록 설계하는 방식
---
### urls.py
project_name/urls.py -> urlpatterns
#### path()
- path(요청받은 경로, 실행)
- path('ccc//', views.ccc) -> : 어떤데이터가 들어와도 실행될 수 있도록 변수로 지정
```python
urlpatterns = [    path('aaa/', views.aaa),    path('bbb/', views.bbb),    path('ccc//', views.ccc),    ...]
```
---
### views.py
app_name/views.py
```python
1. 기본 viewsdef aaa(request):    
    return render(request, 'aaa.html')
```
```python
2. context 사용 
def bbb(request):    
    code    
    context = {        
        'key': value    
    }    
    return render(request, 'bbb.html', context)
```
```python
3. url 경로 변수 사용
def ccc(request, 변수):  # 변수: 웹 서비스에서 url 경로에 들어갈 데이터가 들어갈 이름    
    code    
    context = {
        'key': value    
    }    
    return render(request, 'ccc.html', context)  -> ccc.html에서 변수 사용
```
#### render()
- request: 요청을 response하기 위해 받는 필수 인자
- template_name : template full name, 필수 인자
- context : view에서 사용한 변수를 html로 넘기는 역할, 즉 template에서 쓰이는 변수명과 파이썬 객체를 연결. 
key:value 의 dict 형태로 담아야 한다    
    - key: template에서 사용할 변수명    
    - value: 파이썬 변수명
- content_type: 타입 정의, text/html 가 default다
- status : status code, 200이 default- using : 템플릿 엔진 사용을 위한 이름 정의
---
### templates 
app_name 폴더 안 `templates` 폴더 생성
- django 사용 시 무조건 `templates` 이름으로 사용
- html 파일 생성하여 유저에게 보여지는 웹페이지 수정
- views.py 에서 사용한 변수 사용가능
```html
1. {{변수 사용}}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>웹페이지 상단</h1>
    <h2>{{views.py의 context 'key'}}</h2>
</body>
</html>
```
```html
2. ccc.html 변수 사용
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>{{views.py의 def ccc(request, 변수) 변수 }}</h1>
</body>
</html>
```

```html
3. {% python의 함수 구문 사용 %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>posts</h1>

    {% python의 함수 구문 사용 %}
        <div class="alert alert-light">
            {{views.py의 context 'key'}}
        </div>
    {% 구문 끝 %}
</body>
</html>
```

            
> 기본적인 문법만 배웠는데 머리가 빙글빙글 도는 느낌이다.... 다음주부터는 templates에서 html문서를 많이 만들지 않고 사용한는 방법을 배울거라는데 벌써 걱정... 😢

#내맘대로TIL챌린지 #동아일보 #미디어 프론티어 #글로벌소프트웨어캠퍼스 #GSC신촌

글로벌소프트웨어캠퍼스와 동아일보가 함께 진행하는 챌린지입니다.