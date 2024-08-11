# Login 기능 구현 
1. accounts 모델링
- 모델을 정의하지 않고 마이그레이션 시 django가 가진 파일로 표 생성 
- 현재 테이블 초기화해야 할 땐 db.sqlite3 파일 삭제(모델링 파일 삭제)
##### 모델링
- User 모델 생성 시 AbstractUser 상속   
models.Model -> AbstractBaseUser(pw) -> AbstractUser(id,name) -> User    
기능을 포함하고 있는 AbstractUser 상속받아서 새로운 myUser 생성   
```python
from django.db import models
from django.contrib.auth.models import AbstractUser

# Create your models here.
class User(AbstractUser):
    pass
```
- setting.py 사용할 모델 정의
```python
AUTH_USER_MODEL = "myapp.MyUser"(accounts.User)
```
2. signup 기능 구현

- form 생성   
django가 가지고 있는 form 상속
```python
from django.contrib.auth.form import UserCreationForm
class CustormUserCreationForm(UserCreationForm):
    class Meta:
        model = User
        fields = ('username', )
```
- signup 페이지 및 경로 설정   
signup 페이지 - username, pw, pw확인하는 폼 출력
```html
<form action="" method="POST">
    {% csrf_token %}
    {{form}}
    <input type="submit">
</form>
```
경로 설정   
```python
path('signup/', views.signup, name='signup')
```
- signup 함수 구현

```python
def signup(request):
    if request.method == 'POST':
        form = CustomUserCreationForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('accounts:login')
    else:
        form = CustomUserCreationForm()
    
    context = {
        'form': form,
    }
    return render(request, 'signup.html', context)
```

3. login 기능 구현
- login form    
`class AuthenticationForm(forms.Form):` login form은 ModelForm이 아닌 Form 상속 -> 주는 정보와 생성하는 정보가 다름    
AuthenticationForm을 상속받아 기능을 사용하기에 다른 기능 지정할 필요 없음   
```python
class CustomAuthenticationForm(AuthenticationForm):
    pass
```
- login 페이지 및 경로 설정   
```html
{% extends 'base.html' %}
{% load bootstrap5 %}

{% block body %}
<form action="" method="POST">
    {% csrf_token %}
    {% bootstrap_form form %}
    <input type="submit" class="btn btn-primary">
</form>
{% endblock %}
```
```python
path('login/', views.login, name='login'),
```
- login 함수 구현


```python
from .forms import CustomUserCreationForm,CustomAuthenticationForm
from django.contrib.auth import login as auth_login
def login(request):
    if request.method == 'POST':
        form = CustomAuthenticationForm(request, request.POST)
        # request session값을 쿠키에 넣어줘야하기 때문에 넣는 인자
        if form.is_valid():
            # login 함수 불러와서 사용 login(request, 유저정보)
            auth_login(request, form.get_user())
            next_url = request.GET.get('next')
            # next 인자에 url이 있을 때 => 'articles/1/' or 'articles:index'
            # next 인자에 url이 없을 때 => None or 'articles:index' 
            return redirect(next_url or 'articles:index')
    
    else:
        form = CustomAuthenticationForm()

    context = {
        'form': form
    }
    return render(request, 'login.html', context)
```

4. logout 기능 구현 
- logout 경로, 함수 구현
```python
path('logout/', views.logout, name='logout'),
```
```python
from django.contrib.auth import logout as auth_logout
# logout 함수 불러와서 사용 logout(request)
# 쿠키에서 sessionid 삭제
def logout(request):
    auth_logout(request)
    return redirect('accounts:login')
```

## 로그인 유무에 따른 게시물 출력
- 로그인 유무에 따라 웹페이지를 어떻게 출력할 것인가    
decorators -> login_required   
로그인을 했는지 확인과 동시에 next인자 넣어줌 (?next=/url)   
articles app의 views.py 로그인이 필요한 웹페이지 경로로 들어가는 함수 위에 `@login_required` 
```python
from django.contrib.auth.decorators import login_required
@login_required (로그인 하지 않으면 함수 동작 X)
def detail():
```

- 로그인 유무에 따른 게시물(댓글) 삭제   
로그인 한 사람이 게시물(댓글) 작성자이면 삭제    
`request.user == comment.user`
```python
def delete(request, id):
    article = Article.objects.get(id=id)
    if request.user == article.user:
        article.delete()
    return redirect('articles:index')

@login_required
def comment_delete(request, article_id, id):
    comment = Comment.objects.get(id=id)
    if request.user == comment.user:
        comment.delete()
    
    return redirect('articles:detail', id=article_id)

```

> 코드가 점점 복잡해지지만 기능을 어떻게 구현해야하는지만 알면 잘 할 수 있을 것같다..... 생각보다 잘 맞을지도...?
