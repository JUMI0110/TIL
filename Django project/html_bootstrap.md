# Bootstrap
-Bootstrap은 웹 개발에서 널리 사용되는 오픈 소스 프레임워크로, HTML, CSS, JavaScript를 사용하여 반응형(responsive) 웹사이트와 웹 애플리케이션을 쉽게 구축할 수 있도록 돕는 도구 

## Bootstrap의 주요 특징
1. 반응형 디자인   
Bootstrap은 반응형 그리드 시스템을 제공하여, 다양한 화면 크기와 장치(데스크톱, 태블릿, 스마트폰 등)에 맞게 웹사이트가 자동으로 조정   
col, row, container 등의 클래스를 사용하여 레이아웃을 쉽게 구성

2. 사전 제작된 컴포넌트   
버튼, 네비게이션 바, 카드, 모달, 폼 등 다양한 UI 컴포넌트가 사전 제작되어 있어, 이를 활용해 빠르게 디자인   
각 컴포넌트는 간단한 클래스만 추가하여 사용 가능

3. CSS 및 JavaScript 확장   
기본적인 스타일링을 제공하는 CSS와 다양한 상호작용을 가능하게 하는 JavaScript 플러그인이 함께 포함
도구 팁, 드롭다운, 슬라이드, 탭과 같은 기능을 쉽게 추가 가능

4. 커스터마이징 가능   
개발자는 Bootstrap의 기본 스타일을 그대로 사용할 수도 있지만, 필요에 따라 색상, 폰트, 레이아웃 등을 커스터마이징할 수 있음

## Bootstrap5
- templates에서(.html) form bootstrap적용(Form에서 길게 쓰지않고 바로 적용)
- bootstrap5 install -> app등록
- `{% load bootstrap5 %}` 로 불러와서 사용
```html
form.html
{% extends 'base.html' %}
{% load bootstrap5 %}

{% block body %}
    <form action="" method="POST" enctype="multipart/form-data">
        {% csrf_token %}
        {{form}}
        {% bootstrap_form form %}
        <input type="submit">
    </form>
{% endblock %}
```


# html 기능 단위로 저장
- 웹페이지를 보여줄 때 base.html과 달리 페이지를 구성하는 기능별로 .html 분리
- `_기능.html` 로 저장
- ex) _card.html, _nav.html
```html
_nav.html
<nav class="navbar navbar-expand-lg bg-body-tertiary">
    <div class="container-fluid">
      <a class="navbar-brand" href="{% url 'posts:index' %}">Insta</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavAltMarkup" aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
        <div class="navbar-nav">
          {% if user.is_authenticated %}
            <a class="nav-link active" href="{% url 'posts:create' %}">Create</a>
            <a class="nav-link" href="{% url 'accounts:logout' %}">Logout</a>
            <a class="nav-link disabled" >{{user}}</a>
          {% else %}
          <a class="nav-link" href="{% url 'accounts:signup' %}">Signup</a>
          <a class="nav-link" href="{% url 'accounts:login' %}">Login</a>
          {% endif %}
        </div>
      </div>
    </div>
  </nav>
```
