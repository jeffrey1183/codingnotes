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



## 跑 Server

要確定專案有沒有建立成功，可以透過下面的指令

```
$ python manage.py runserver
```



## 建立 App

在建立 App 前先了解 Projects 和 App 在 Django 裡有什麼不同。專案比 Apps 在上一層階級，一個 Project 可以有多個 App，一個 App 可以加進多個 Project 內。下面是新增一個 App 的指令，這個 App 叫做 polls

```
$ python manage.py startapp polls
```

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

接著跑 sqs  their SQL

```
$ python manage.py sqlmigrate polls 0001
```
