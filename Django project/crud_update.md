# CRUD 기능 Update

### html 통합구성 
- 모든 html페이지의 기본이 되는 페이지 구성 -> 모든페이지에서 base.html 상속   

1. base.html 생성
- 프로젝트 최상단 폴더에 templates폴더 생성
- base.html 생성
- 새로 만든 `templates`폴더 django가 실행 할 수 있도록 settings.py에서 DIR 수정   
```python
DIR = [BASE_DIR / 'templates']
# BASE_DIR = Path(__file__).resolve().parent.parent
            #경로(settings.py위치기준).부모.부모(상위폴더)
```

2. base.html 구성
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
   
</head>
<body>
    <h1>여기는 base입니다.</h1>
    <nav class="nav">
        <a class="nav-link" href="/articles/">Home</a>
    </nav>
        {% block body %}
        {% endblock %}
</body>
</html>
```
> base.html
{% block body %}   
다른 html에서 보여주는 공간      
{% endblock%}   

> 상속받는 html
{% extends 'base.html' %}
{% block body %}
보여줄 코드
{% endblock %}


### app.urls 생성
최상단 urls에서 app_name/url이 포함되면 하위 app의 urls로 경로 보냄
유지보수편하기 위해 분리 작업

- pjt.urls (include()사용)   
'app_name'으로 들어오는 모든 요청 app.urls로 보내기
```python
from django.urls import path, include


urlpatterns = [
    path('admin/', admin.site.urls),
    path('articles/', include('articles.urls'))
]
```
- app.urls (app폴더에 urls.py 파일 생성)
```python 
from django.urls import path
from . import views
# '.' 현재 위치(폴더)

app_name = 'app_name'# 'articles'

urlpatterns = [
    path('', views.index, name='index'),
]
```

- html에서 사용할 때 
url -> 'app_name(여러 app_name이 있을 때 html이 찾기 쉬움):name' 매칭되는 경로를 찾아서 문자열(경로의 이름) 생성하여 적용
{% url 'articles:index' %}

### GET, POST method
1. GET -> R    
주로 데이터를 요청하는 용도로 사용되며, 요청 URL에 데이터가 노출되므로 보안에 취약할 수 있으나 빈번하게 요청되는 데이터에 적합


2. POST -> CUD  
 데이터의 양이 많거나 민감한 정보를 전송할 때 사용 URL에 데이터가 노출되지 않으므로, 보안성이 높음

```html
{% block body %}
<!-- method="POST" 데이터를 담아서 보내는 메소드 -->
<form action="{% url 'articles:create' %}" method="POST">
    <!-- method를 POST로 바꿔서 csrf_token 넣어줘야함 없으면 에러 -->
    {% csrf_token %}
    <div class="mb-3">
        <label for="title" class="form-label">Title</label>
        <input type="text" id="title" class="form-control" name="title">
    </div>
    <div class="mb-3">
        <label for="content" class="form-label">Content</label>
        <textarea id="content" class="form-control" rows="10" name="content"></textarea>
        <!-- textarea 여러줄의 데이터 rows=세로줄 -->
    </div>

    <button type="submit" class="btn btn-primary">submit</button>
</form>
{% endblock %}
```
```python
def create(request):
    title = request.POST.get('title')
    content = request.POST.get('content')

    article = Article()
    article.title = title
    article.content = content
    article.save()

    return redirect('articles:detail', id=article.id)
```
- POST 방식으로 요청이 왔을 때, request.method 속성이 'POST'로 설정