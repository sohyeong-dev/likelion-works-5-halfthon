# likelion-works-5-halfthon

1. 바탕화면에 my_env 라는 가상환경을 만듭니다.

```
cd Desktop/
python -m venv my_env
cd my_env/
source Scripts/activate
```

2. 새롭게 만든 my_env에 Django 개발 환경을 구축합니다.

- Django 설치
```
python -m pip install Django
```
- PIP 업데이트
```
python -m pip install pip --upgrade
```

3. 새로운 Django Proejct를 만들어주세요.

```
django-admin startproject sample_project
cd sample_project
code .
```

4. 편리하게 Django 개발을 할 수 있게끔 VS Code 에 Python 세팅을 해주세요.

Extensions > Python  
Python version  
Formatter  
Linter  

5. 세팅이 완료되었으면, resume, posts App 을 만들어주세요.

New Terminal
```
python manage.py startapp posts
python manage.py startapp resume
```

`sample_project/settings.py`
```python
INSTALLED_APPS = [
  'posts',
  'resume',
]
```

6. resume는 여러분의 메인 랜딩 페이지로 활용할 App 입니다. http://localhost:8000/ 으로 접속했을 때, 아래와 같은 페이지를 볼 수 있게끔 개발해주세요. 구체적인 내용과 이미지는 바꿔주시면 됩니다.

- resume App과 관련된 URL Pattern 은 별도의 urls.py로 분리해주세요.

`resume/urls.py`
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index),
]
```

`sample_project/urls.py`
```python
from django.urls import path, include

urlpatterns = [
  path('', include('resume.urls')),
]
```

`resume/views.py`
```python
def index(request):
  return render(request, 'resume/index.html')
```

- Django Template 을 활용해주세요.

`resume/templates/resume/index.html`
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>sohyeong's Homepage</title>
</head>
<body>
  
</body>
</html>
```

```
python manage.py runserver
```

- Portfolios 에 posts 앱과 연동할 링크를 만들어주세요.

```html
<a href="/posts/">#Link</a>
```

- Static 을 활용해 CSS 파일과 이미지를 추가해주세요.

`resume/templates/resume/index.html`
```html
{% load static %}

<head>
  <link rel="stylesheet" href="{% static 'resume/style.css' %}">
</head>

<h1 class="title">sohyeong</h1>
<img src="{% static 'resume/images/image_name.jpg' %}" alt="">
```

`resume/static/resume/style.css`
```css
.title {
  color: brown;
  font-size: 48px;
}
```

7. posts 앱은 우리가 지금까지 배웠던 heestagram 프로젝트와 동일합니다. http://localhost:8000/posts/ 으로 접속했을 때, 아래와 같은 페이지를 볼 수 있게끔 개발해주세요. 구체적인 내용은 달라도 좋습니다.

- posts App과 관련된 URL Pattern 은 별도의 urls.py로 분리해주세요.

`posts/urls.py`
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index),
]
```

`sample_project/urls.py`
```python
path('posts/', include('posts.urls')),
```

`posts/views.py`
```python
def index(request):
  context = {}
  return render(request, 'posts/index.html', context)
```

- Django Template 을 활용해주세요.

`posts/templates/posts/index.html`
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Posts</title>
</head>
<body>
  <h1>My Posts</h1>
</body>
</html>
```

- context 에 게시물 데이터를 구성하여 Template 에 전달해주세요.

`posts/views.py`
```python
context = {
  'posts': [
    { 'author': 'sohyeong', 'body': 'sample body1' },
    { 'author': 'sohyeong', 'body': 'sample body2' },
    { 'author': 'sohyeong', 'body': 'sample body3' },
    { 'author': 'sohyeong', 'body': 'sample body4' },
  ]
}
```

- context 를 통해 전달된 게시물이 있는 경우 for Loop 을 활용해서 HTML 코드를 재활용하는 형식으로 개발해주세요.

`posts/templates/posts/index.html`
```html
    {% if posts %}
        <ul>
            {% for post in posts %}
                <li>Author: {{ post.author }} | Body: {{ post.body }}</li>
            {% endfor %}
        </ul>
    {% else %}
        <p>No Posts.</p>
    {% endif %}
```
