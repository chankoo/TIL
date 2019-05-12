#### 19.05.12

## Django form

[HTML form](https://github.com/chankoo/TIL/blob/master/general/HTML_form.md)을 다루는 일은 상당히 복잡한 과정이다. 다양한 유형의 데이터 항목을 폼으로 표시해야하고, 편집, 유효성 검사, 서버로 넘겨주는 일련의 작업이 필요하다

Django의 폼은 웹의 폼을 처리하는 작업을 추상화 한다. 즉, 이 작업의 많은 부분을 단순화, 자동화하여 손쉬운 처리를 가능하게 한다. 이는 대부분의 프로그래머가 직접 수행한 작업보다 더 안전하기까지 할 것이다 

## Django form 처리

Django form은 세가지 크게 작업을 처리한다

1. 렌더링을 위해 데이터 준비 및 재구성(입력폼의 값 검증)
2. 데이터에 대한 HTML 양식 작성(as_table(), as_p(), as_ul() 기본 제공)
3. 클라이언트로부터 제출 된 양식 및 데이터 수신 및 처리(Redirect or Error)

![스크린샷, 2019-05-12 17-13-58](https://user-images.githubusercontent.com/38183218/57579581-5daf3700-74d9-11e9-8835-70136af0ccec.png)

## Form vs Model Form

- Form(일반 폼) : 직접 필드를 정의해야하고 위젯 설정이 필요하다

- Model Form(모델 폼) : 모델과 필드를 지정하면 모델폼이 자동으로 폼 필드를 생성한다

```python
from django import forms
from .models import Post

# Form (일반 폼)
class PostForm(forms.Form):
	title = forms.CharField()
	content = forms.CharField(widget=forms.Textarea)

# Model Form (모델 폼)
class PostForm(forms.ModelForm):
	class Meta:
		model = Post
		fields = ['title', 'content']
```

## 예제 코드 작성

1. model 구현

```python
## AuthServer/auth_/models.py

from django.db import models

class User(models.Model):
    created_at = models.DateTimeField()
    account_expired = models.BooleanField()
    authority = models.CharField(max_length=255)
    sns_at = models.BooleanField()
    enable = models.BooleanField()
```

2. ModelForm 구현

```python
## AuthServer/auth_/forms.py

from django import forms
from .models import NonSNS

class NonSNSForm(forms.ModelForm):
    class Meta:
        model = NonSNS
        fields = {
            '_id',
            'password',
            'email'
        }
```

3. 템플릿 통해 HTML 폼 생성

```html
<!-- AuthServer/templates/create_nSNS.html -->

<form action='.' method='POST'>
    {% csrf_token %}

    {{ form.as_table }}

    <input type='submit' value='Save' />
</form>

```

4. view 구현

```python
## AuthServer/auth_/views.py

from django.shortcuts import render
from .forms import NonSNSForm

def nSNS_create_view(request):
    form = NonSNSForm()
    context = {
        'form': form
    }
    return render(request, 'create_nSNS.html', context)

```

5. `AuthServer/AuthServer/urls.py` 에서 url과 view(nSNS_create_view) 매핑

### Reference

- [docs.djangoproject](https://docs.djangoproject.com/ko/2.2/topics/forms/)

- [Sean Kang 님의 Django form 폼나게 쓰기](https://www.slideshare.net/SeanKang19/django-form-78716437)

- [초보몽키의 개발공부로그](https://wayhome25.github.io/django/2017/05/06/django-form/)

- [Python Django Web Framework - Full Course for Beginners](https://www.youtube.com/watch?v=F5mRW0jo-U4)