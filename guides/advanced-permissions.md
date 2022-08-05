# Django

## 虛擬環境的設定和 Django 安裝

* 先確認是否已安裝 Python，在 Mac 是輸入 python3&#x20;

```
$ python3
```

* 建議先建立 Python 虛擬環境，先用 command line 移到要建立虛擬環境的位置

```
$ python3 -m venv django_venv
```

* 創建完成後，要 activate，啟動成功後會看到指令前面多了虛擬資料夾名稱(django\_venv)

```
$ source django_venv/bin/activate
```

* 安裝 Django

```
$ python -m pip install Django
```

安裝成功！Successfully installed Django-4.0.6 asgiref-3.5.2 sqlparse-0.4.2



### 確認安裝成功 <a href="#que-ren-an-zhuang-cheng-gong" id="que-ren-an-zhuang-cheng-gong"></a>

最後，讓我們最後來測試一下。請在虛擬環境下指令輸入 `python`，進入互動式命令列環境

```
$ python
```

輸入以下的指令取得 Django 版本資訊：

```
>>> import django
>>> django.VERSION
(4, 0, 6, 'final, 0')
```

如果看見類似上面的訊息，就代表安裝成功囉！就可以從[官方的 Tutorial 1 ](https://docs.djangoproject.com/en/4.0/intro/tutorial01/)開始走一遍。官方教學目前是給 Django 4.0 和 Python 3.8 以上版本。



參考資料：

* [Django Girls](https://djangogirlstaipei.gitbooks.io/django-girls-taipei-tutorial/content/django/installation.html)
* [Python 官方文件](https://docs.python.org/3/tutorial/venv.html)



## 建立 Django 專案

到你的虛擬環境資料夾建立專案，mysite 是專案的名稱，專案名稱不能跟 Python 內建專案或 Django 元件有衝突，像 test 或 Django 不能取這種名稱。

```
$ django-admin startproject mysite
```

建立之後會有一個 mysite 資料夾，資料夾的內的檔案和結構可以參考[官方介紹](https://docs.djangoproject.com/en/4.0/intro/tutorial01/)。





## 建立 App

在建立 App 前先了解 Projects 和 App 在 Django 裡有什麼不同。專案比 Apps 在上一層階級，一個 Project 可以有多個 App，一個 App 可以加進多個 Project 內。下面是新增一個 App 的指令，這個 App 叫做 polls

```
$ python manage.py startapp polls
```



## 跑 Server

要確定專案有沒有建立成功，可以透過下面的指令

```
$ python manage.py runserver
```

接著連到 app 的網址：[http://127.0.0.1:8000/polls/](http://127.0.0.1:8000/polls/)

## 寫第一個 View

打開 polls/views.py 檔案

```python
from django.http import HttpResponse
def index(request):
    return HttpResponse("Hello, world. You're at the polls index")
```

在 polls 資料夾新增`urls.py`

```python
from django.urls import path 

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

接著要在專案的 urls.py 新增剛剛設定好的路徑，用 include

```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]h
```

path() 函式有 4 個 argument，route 和 view 是必填，name 和 kwargs 是選填。

* route 是 url pattern 內的字串，當用戶輸入網址 url 的 request 送出，就會在 urlpatterns 的 list 內尋找符合的字串。

參考資料：[官方 Tutorial](https://docs.djangoproject.com/en/4.0/intro/tutorial01/)



## Database 設定

打開 mysite/settings.py 檔案，Django 預設使用 SQLite，如果你只是想了解 Django，SQLite 對新手是個好選擇，而且 SQLite 含在 Python 內，你不用另外安裝。但如果你要運用在公司的產品，考慮到 DB 的擴充性，你可能會想用 PostgreSQL 去避免轉換 DB 這種麻煩事，在 setting.py 裡找到 DATABASES 這一項，裡面的 ENGINE 可以調整成你要的語言。NAME 則是你 DB 的名稱。

```
django.db.backends.postgresql
```

如果你用 SQLite，還有像 USER、PASSWORD 和 HOST 的參數可以設定，詳細內容請參考[官方文件](https://docs.djangoproject.com/en/4.0/ref/settings/#std-setting-DATABASES) DB 創造 Table

```
$ python manage.py migrate
```



INSTALLED\_APPS 裡的 App 可以用在不同專案，預設有 admin 網站、authentication system、session framework 和 messaging framework 等。



## 建立 Models&#x20;

定義 model，事實上就是 database layout 和 metadata。

一個 model 是獨立的資料，包含你想儲存的的欄位和執行的行為。Django 跟隨 [DRY Principle](https://docs.djangoproject.com/en/4.0/misc/design-philosophies/#dry)，只要在一個地方定義 data model，會自動撈取資料，包含 migration。 class 都繼承 `django.db.models.Model`，所以類別名稱後方的() 填寫 models.Model



```python
from django.db import models

class Question(models.Model):
    question_text = models.Charfield(max_length=200)
    pub_date = models.DateTimefield('date published')

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)


```

定義完成後要啟動 models，要在 settings.py 的 [installed app 加入  polls.apps.PollsConfig](https://docs.djangoproject.com/en/4.0/intro/tutorial02/#activating-models) 再去下 migration 的指令，models 有更動就要做 migration。



```
$ python manage.py makemigrations polls
```

接著跑下面 sqlmigrate 的指令，讓 migration 的名稱 return 到 SQL

```
$ python manage.py sqlmigrate polls 0001
```

然後再下面 migrate 的指令把改動連結到 DB

```
$ python manage.py migrate
```



## Django Admin 後台

新增用戶指令，設定新用戶的帳號、密碼和 email

```
 $python manage.py createsuperuser
```

啟動 server

```
$ python manage.py runserver
```

在 polls/admin.py 檔案新增 Question 的程式後，在啟動伺服器的狀態下登入[後台](http://127.0.0.1:8000/admin/)

```python
from django.contrib import admin

from .models import Question

admin.site.register(Question)
```

## View

view 是一種 web page 在 Django內，application 有特定的 function 和 template，例如 blog application 可能有以下頁面：

* Blog homepage – 顯示最新幾篇文件
* Entry “detail” page – 文章內頁
* Year-based archive page – 過去一年的文章
* Month-based archive page – 過去一個月的文章
* Day-based archive page – 過去一天的文章
* Comment action – 一篇文章下的評論



在 poll application，我們有下面四種 views:

* Question “index” page – 顯示最新幾篇問題
* Question “detail” page – 顯示問題的內容，有表格可以投票
* Question “results” page – 顯示投票結果
* Vote action – 處理對一個問題投票的動作

接著可以先寫一些 views 顯示文字，跟 URL 做 mapping，可以參考 [Tutorial 03](https://docs.djangoproject.com/en/4.0/intro/tutorial03/#writing-more-views) 在 polls/views.py 和 polls/urls.py 新增完程式後可以在以下網址看到相應的文案。

* [http://127.0.0.1:8000/polls/34/](http://127.0.0.1:8000/polls/34/)
* [http://127.0.0.1:8000/polls/34/results](http://127.0.0.1:8000/polls/34/results)
* [http://127.0.0.1:8000/polls/34/vote/](http://127.0.0.1:8000/polls/34/vote/)

當使用者輸入第一個 /polls/34/ 的網址，Django 會先去 mysite/urls.py 找 urlpatterns 裡 path 函式內有 polls 字串的那行，那行引導到 polls 應用內的 urls 檔案。剩下的 34/ 找到對應的 \<int:question\_id>/ 然後 跑下面的 detail 函式。

```python
detail(request=<HttpRequest object>, question_id=34)
```

`question_id=34` 的部分是來自 `<int:question_id>，用` angle brackets 去捕捉 URL 的部分資料當作 keyword argument 傳到 view 函數。`<int:question_id> 裡的 question_id` 是字串， `int 是` converter 決定什麼模式要符合 URL path，. 用 colon (`:`) 將 converter 和 pattern name 分開。



## 更複雜、實用的 View&#x20;

每一個 view 負責兩件事的其中一件事：

1. 回傳 [`HttpResponse`](https://docs.djangoproject.com/en/4.0/ref/request-response/#django.http.HttpResponse) 物件包含頁面的內容
2. 處理像 [`Http404`](https://docs.djangoproject.com/en/4.0/topics/http/views/#django.http.Http404)

view 可以從 database 讀取資料，也可以用 Django template system 或第三方 Python template system，產生 PDF file, 輸出XML, 即時新增 ZIP 檔案，使用任何 Python libraries。

Django 需要處理的就是 [`HttpResponse`](https://docs.djangoproject.com/en/4.0/ref/request-response/#django.http.HttpResponse)

在第二章 [Tutorial 2](https://docs.djangoproject.com/en/4.0/intro/tutorial02/) 裡，我們知道 Django 自己的 database API 很方便。我們現在在首頁插入一塊 view，會顯示至少系統裡 5 個問題，用逗號隔開，順序會依照問題的發布日期(publication date)。\


```python
from django.http import HttpResponse
from .models import Quesiton

def index(HttpRequest):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    output = ','.join([q.question_text for q in lastest_question_list])
    return HttpResponse(output)
```

這邊先寫一個 list 是撈出最近的 5 個問題且依照 pub\_date，再寫一個 output 變數用 for loop 把剛剛 list 內的 question text 都抓出來用逗號連結。



## Templates

如果你要改變頁面的樣式，可以使用 Django’s template system 將設計分開，創造一個 template。

先在 polls 資料夾下新增 templates 這個資料夾，Django 會從這裡找設計模板。



專案中 [`TEMPLATES`](https://docs.djangoproject.com/en/4.0/ref/settings/#std-setting-TEMPLATES) 的設定跟 Django 會怎麼讀取和 render 模板可以參考文件。

在剛剛 `templates` 資料夾下新增 `polls 資料夾和 index.html。換句話說，你的 template 會在polls/templates/polls/index.html。然後把下面的程式寫進 index.html。`

``

```html
<html>
    <head>
    </head>
    <body>
        {% raw %}
{% if latest_question_list %}
        <ul>
            {% for question in latest_question_list %}
                <li>
                    <a href="/polls/{{ question.id }}/">{{question.question_text}}</a>
                </li>
            {% endfor %}
         </ul>
         {% else %}
            <p>No polls are available.</p>
        {% endif %}
{% endraw %}
    </body>
</html>
```

並要更新 index view，我們新增了 template 的變數，讀取 polls/index.html，新增一個 context 的 dictionary

```python
from django.http import HttpResponse
from django.template import loader

from .models import Question


def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    template = loader.get_template('polls/index.html')
    context = {
        'latest_question_list': latest_question_list,
    }
    return HttpResponse(template.render(context, request))
```



## render()

讀取 template 的時候會更常用 render()，上面的程式改寫後可以省掉儲存 template 變數。



```python
from django.shortcuts import render

from .models import Question


def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    context = {'latest_question_list': latest_question_list}
    return render(request, 'polls/index.html', context)
```

上面的改寫，其實我們就不需要讀取 loader 和 HttpResponse，render() 會把 request object 的第一個 argument，然後第三個 optional argument 是放 dictionary 的資料類型，這邊是放 context，可放可不放。根據 Template render 出 context 回傳[`HttpResponse`](https://docs.djangoproject.com/en/4.0/ref/request-response/#django.http.HttpResponse) object。



## 處理 404 error

如果 question 不存在，就會有 404 的情況要處理，Django 提供 get object or 404 function 處理 404 的情況，function 的第一個 argument 是你之前寫的 model，同時也傳到 get() function，如果物件不存在 Http404 就會 raise。

```python
from django.shortcuts import get_object_or_404, render
from .models import Question

def detail(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, 'polls/detail.html', {'question': question})
```

## 使用模板

\
回到 `detail()` view ，我們用 `question 變數改寫 polls/detail.html` 模板檔案：

```
<h1>{{ question.question_text }}</h1>
<ul>
{% raw %}
{% for choice in question.choice_set.all %}
    <li>{{ choice.choice_text }}</li>
{% endfor %}
{% endraw %}
</ul>
```

這些模板我們是用 dot-lookup 語法去找 variable attributes。以 question.question\_text 為例，Django 先做了 dictionary lookup 找尋 question 這個 object，失敗後 Django 試 attribute lookup 才找到。



### 移除模板裡寫死的 URL

之前在 polls/index.html 的模板中，我們把問題條列出來，程式有些部分是寫死的，像這樣：

```python
<li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
```

使用 \{% url %\} template tag，如果改成下面這樣，其實就是運用了 polls.urls.py 內定義好的 name argument，像 detail 是其中一條命名的 url，如果今天有很多 template 都吃這個 url，今天 url 就算變動，我只要改 polls.urls.py 裡面的程式就好，不用每一個 template 都調整。

```python
<li><a href="{% raw %}
{% url 'detail' question.id %}
{% endraw %}"></li>
```

假設你今天要把 url 從 polls/12/ 變成 polls/specifics/12，你就只要在 polls.url 檔案中把

```python
path('<int:question_id>/', views.detail, name='detail'),
```

改成

```python
path('specifics/<int:question_id>', views.detail, name='detail')
```



## Namespacing URL names&#x20;

如果你的 Django project 內有好幾個 App，那 Django 要怎麼辨識每個 app 對應哪個 url 呢？解法是在 polls/urls.py 增加 app\_name。



```python
from django.urls import path

from . import views

app_name = 'polls'
urlpatterns = [
    path('', views.index, name='index'),
    path('<int:question_id>/', views.detail, name='detail'),
    path('<int:question_id>/results/', views.results, name='results'),
    path('<int:question_id>/vote/', views.vote, name='vote'),
]
```



## 設計表格

接著在 polls/detail.html 檔案內加入表格，表格的寫法用到 [fieldset tag](https://www.w3schools.com/tags/tag\_fieldset.asp) 的概念，fieldset tag 會把裡面的 label 都包起來，變成一個大表格。在 HTML 5.2 之後，[ legend tag 裡面可以塞 ](https://stackoverflow.com/questions/8005887/is-header-in-legend-valid-legendh1caption-h1-legend)h1, h2...h6 這些標題(heading)，會讓每個 label 都變成標題。

form tag 裡面有 label tag 和 input tag，他們有相關性，一般會用 [label 的 for attribute 去綁 input 的 id](https://www.w3school.com.cn/tags/att\_label\_for.asp)。input tag 有 type, name, id, value 幾個 attribute，input 代表輸入框，[value 代表輸入框裡的值](https://matthung0807.blogspot.com/2019/08/html-input-value.html) id 和 name 常常會搞混， id 是唯一的，而 name 是可以重複的。



[forloop.counter](https://docs.djangoproject.com/en/4.0/ref/templates/builtins/#std-templatetag-for) 是從 index 1 開始跑 loop

form tag 的 action attribute 是指提交表單後要向何處發送。

最下面有一個 [submit 的按鈕](https://developer.mozilla.org/zh-TW/docs/Web/HTML/Element/input/submit) input tag，type 等於 submit 。



Since we’re creating a POST form (which can have the effect of modifying data), we need to worry about Cross Site Request Forgeries. Thankfully, you don’t have to worry too hard, because Django comes with a helpful system for protecting against it. In short, all POST forms that are targeted at internal URLs should use the [`{% csrf_token %}`](https://docs.djangoproject.com/en/4.0/ref/templates/builtins/#std-templatetag-csrf\_token) template tag.



