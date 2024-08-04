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

4. 모델 등록
- admin에 모델 등록 (관리자페이지 모델 확인)   

admin id,password 설정
```bash 
python manage.py createsuperuser
```
모델 등록(pjt-admin.py)   
```python
admin.site.register(ModelName)
```
---

## CRUD 기능 구현

```python 
url 경로
urlpatterns = [
    path('admin/', admin.site.urls),
    # Read all
    path('', views.index),
    # Read (1)
    path('posts/<int:id>/', views.detail),
    
    # Create
    path('posts/new/', views.new),
    path('posts/create/', views.create),

    # Delete
    path('posts/<int:id>/delete/', views.delete),

    # Update
    path('posts/<int:id>/edit/', views.edit),
    path('posts/<int:id>/update/', views.update),
]
```

1. Read 기능 구현
- All (게시물 전체 보여주기) index page

```python
views.py
def index(request):
    posts = Post.objects.all() # Post클래스의 모든 객체 가져오는 함수

    context = {
        'posts': posts
    }

    return render(request, 'index.html', context)
``` 
```html
index.html
<body>
    <!-- for문 사용 {%%}, {{원하는 값 출력}} -->
    <h1>Index</h1>
    {% for post in posts %}
        <p>{{post.title}}</p> 
        <p>{{post.content}}</p>
    {% endfor %}
    <hr>
    <a href="/posts/{{post.id}}/">detail</a>
    <!-- 디테일페이지 링크 href=경로 -->
</body>
</html>
```

- 1 (게시물 1개 보여주기 - 상세페이지) detail page   
해당하는 id값 페이지에 보여주기 

```python
views.py
def detail(request, id):
    post = Post.objects.get(id=id) # get(컬럼명=value)

    context = {
        'post': post
    }

    return render(request, 'detail.html', context)
```
```html
detail.html
<body>
    <h1>Detail</h1>
    <h2>{{post.title}}</h2>
    <p>{{post.content}}</p>
</body>
</html>
```
2. Create 기능 구현
- new (게시물 작성 할 빈 페이지 보여주기)
```python
def new(request):
    return render(request, 'new.html')
```
```html
new.html
form tag로 작성할 수 있는 페이지 만들기
<body>
    <form action="/posts/create/">
        <label for="title">Title: </label>
        <input type="text", id="title", name="title">

        <label for="content">Content: </label>
        <input type="text", id="content", name="content">

        <input type="submit">
    </form>
</body>
</html>
```
- create(작성하고 저장하기)
```python
def create(request):
    title = request.GET.get('title')
    content = request.GET.get('content')

    post = Post()
    post.title = title
    post.content = content
    post.save()

    # 만들어놓은 기능 재사용 redirect 
    return redirect(f'/posts/{post.id}/')
```
`redirect`   
redirect는 특정한 URL로 요청을 보낸다. 예를 들어, `redirect'(f'/posts/{post.id}/')`로 redirect하면, 위의 URL로 이동하고, 이 URL에 연결된  함수가 다시 실행된다.

3. Delete
```python
def delete(request, id):
    post = Post.objects.get(id=id)
    post.delete()

    return redirect('/') # -> 가장 최상단페이지로 redirect
```

4. Update
- edit(수정할 페이지 보여주기 )
```python
def edit(request, id):
    post = Post.objects.get(id=id)

    context =  {
        'post': post
    }

    return render(request, 'edit.html', context)
```
```html
edit.html
<body>
    <form action="/posts/{{post.id}}/update/">
        <label for="title">Title: </label>
        <input type="text", id="title" value="{{post.title}}", name="title">

        <label for="content">Content: </label>
        <input type="text", id="content", value="{{post.content}}", name="content">

        <input type="submit">
    </form>
</body>
</html>
```

- update(수정한 페이지 저장/적용하기)
```python
def update(request, id):
    # 기존정보
    post = Post.objects.get(id=id)
    # 새로운 정보
    title = request.GET.get('title')
    content = request.GET.get('content')

    # 기존 정보를 새로운 정보로 덮어씌우기
    post.title = title
    post.content = content
    post.save()

    return redirect(f'/posts/{post.id}/')
```
 상세페이지로 redirect

> crud 기능!! 처음 따라할 땐 이해도 50%, 혼자 천천히 재구성할 땐 70%, TIL을 쓰면서 90%정도로 점점 올릴 수 있게 되었다. 웹 개발 무서운 녀석.. 😔😔😔