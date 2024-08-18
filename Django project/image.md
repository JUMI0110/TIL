# Image 게시물
### Media
1. MEDIA_URL설정
- 인터넷을 통해 사진을 보기위해 접근할 수 있는 경로 설정   
```python
settings.py 
MEDIA_URL = '/media/'
```
2. MEDIA_ROOT 설정
- 업로드한 사진을 저장할 위치(실제 폴더 경로)
- ~/Desktop/DMF/insta/media
```python
settings.py
MEDIA_ROOT = BASE_DIR / 'media'
```
3. pillow 패키지 설치
- ImageField가 pillow패키지에 대해 의존성을 가짐 
```bash
pip install pillow
```
4. ImageField()
- FileField를 상속받는 서브 클래스로 FileField의 모든 속성 및 메서드 사용가능, 업로드 된 객체가 유효한 이미지인지 검사
- 모델링 단계 게시물에 이미지 넣을 시 models.ImageField(upload_to='image')로 생성

5. image resized
- 사진 크기 일괄처리(모델 수정 시 마이그레이션 다시 해줘야함)
```python
Post Model
    image = ResizedImageField(
        size=[500, 500],
        crop=['middle', 'center'],
        upload_to='image/%Y/%m'
    )
```

### Image CR(Create, Read)
1. Read
- urls.py 설정 django 정적파일 관리   
- 사용자가 업로드한 파일 제공하는 방법    
`static(경로요청, 사진의 실제위치)`

```python
from django.conf.urls.static import static
from django.conf import settings
urlpatterns = [
    path(),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

2. Create
-  encoding 타입(entype) 정보 바꿈 파일을 따로 FILES로 전송
```html
<form action="" method="POST" enctype="multipart/form-data"> 
        {% csrf_token %}
        {{form}}
        <input type="submit">
    </form>
```
> entype : 폼데이터 인코딩 방식 폼에서 데이터를 전송할 때, 데이터를 어떤 방식으로 인코딩할지 결정하는 것   
'application/x-2220form-unlencoded'  : 기본값    
'multipart/formdata' : 파일 업로드 포함된 폼에서 사용 데이터를 여러 부분으로 나누어 전송, 각 부분은 `boundary`로 구분

- views.py 경로 요청이 들어오면 실행 할 함수에 요청을 2가지 넣어 줘야 함 `POST,FILES`
```python
def create(request):
    if request.method == 'POST':
        form = PostForm(request.POST, request.FILES)
        if form.is_valid():
            form.save()
            return redirect('posts:index')

    else:
        form = PostForm()

    context = {
        'form': form,
    }

    return render(request, 'form.html', context)
```

> @require_POST   
`from django.views.decoraters.http import require_POST`   
GET요청인지 POST요청인지 확인    
GET요청이 들어왔을 때 어떤 문제인지 확인할 수 없기에 에러코드 보여주는 것이 좋음   
`http 405 Method Not Allowed`

