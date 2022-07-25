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



\
