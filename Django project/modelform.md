# ModelForm
model + form  장고에서 만들어주는 기능   
모델의 컬럼이 많을 때 form에서 input이 늘어남에 따라 유지보수가 어려워 장고에서 지원하는 기능 사용하기  모델에 어울리는 폼 생성

1. 생성

```python
from django import forms
from .models import Article

class ArticleForm(forms.ModelForm): 
    # 모델에 어울리는 html 폼을 생성(input만 생성)

    class Meta(): # Meta 클래스 안에 사용할 모델 설정 
        model = Article
        fields = '__all__'  
```

2. 사용
- 유효성검사   
사용자가 넣은 데이터가 형식에 맞는지 확인   
사용자가 제대로 입력한 데이터는 놔두고 잘못입력한 부분만 알려줄 수 있게 해주는게 고객 경험 관점에서 좋음   
if ~ else 문
```python
def create(request):
    # new : 빈종이를 보여주는 기능 
    # create : 사용자가 입력한 데이터를 저장
    # --------- 메소드 차이에 의해 보여지는 기능 
    # GET create/ 빈종이를 보여주는 기능
    # POST create/ 사용자가 입력한 데이터를 저장
    if request.method == 'POST':
        form = ArticleForm(request.POST)
        
        if form.is_valid(): # 들어온 데이터 유효한지 검증
            form.save()
            return redirect('articles:index')

        # else: 
            # 제대로 넣은 데이터는 남을 수 있게 새로운 form에 다시 request.POST 넣고 다시 보내주는 기능
            # form = ArticleForm(request.POST) 위에 선언된 form 사용

            # context = {
            #     'form': form
            # }
            # return render(request, 'create.html', context)

    else: 
        form = ArticleForm()

        # context = {
        #     'form': form
        # }
        # return render(request, 'create.html', context)
        # 같은 코드 밑에서 한번에 처리 
    context = {
        'form': form
    }
    return render(request, 'create.html', context)

```
-> 겹치는 코드는 삭제하고 한번에 처리 else: if문이 아니면 모두 같은 결과 리턴

# form.html 통합
create의 get요청과 update의 get요청이 보여주는 페이지가 같음   
-> 통합하여 같이 사용 form.html
```python

   
    else: 
        form = ArticleForm()
    context = {
        'form': form
    }
    # return render(request, 'create.html', context)
    return render(request, 'form.html', context)


def update(request, id):
    context = {
        'form': form
    }
    # return render(request, 'update.html', context)
    return render(request, 'form.html', context)
```

> 코드가 간결해지는 것 같은데 이해해야 되는건 왜 점점 더 많아지는가.. ㅎㅎ;;