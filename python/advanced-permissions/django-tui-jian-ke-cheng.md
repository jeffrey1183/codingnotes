# Django 推薦課程

## [Django 入門與實踐](https://foofish.net/django-tutorial-00.html)

從虛擬環境搭建到系統設計的概念，不像官方文件那麼難消化，也與實際軟體開發流程符合，會有 PM 畫 wireframe 與工程師討論功能。另外這份教學的有提到光寫 demo 意義不大，要做[實際項目](https://foofish.net/django-tutorial-04.html)才有意義。



### 重點筆記

[第五章 模型設計](https://foofish.net/django-tutorial-05.html)

* `auto_add_new`[會在 model 物件第一次被創建時，將字段的值設成創建的時間，之後修改物件，字段的值不會再更新](https://agvszwk.github.io/2019/05/11/django%E7%9A%84model-auto-now-add%E5%92%8Cauto-now/)。設定 [DateTimeField](https://docs.djangoproject.com/en/4.1/ref/models/fields/#datetimefield) 的時候會用到。
* `ForeignKey` 是用在一對多的模型關係，其他關係像多對多，一對一請[參考文件](https://docs.djangoproject.com/en/4.1/ref/models/fields/#foreignkey)，參數 relate\_name 的細節可先看第五章的說明，他是一個 opotional 的項目，如果不設定會自動生成 class name set 的屬性，有需要了解其他參數再看[參考文件](https://docs.djangoproject.com/en/4.1/ref/models/fields/#django.db.models.ForeignKey.related\_name)
* 在定義 Django model 的欄位的時候，**blank 決定是否此欄位在表格中是否必填，** `False` 是指必填，不填會產生 error， `True` 是指可以允許留空。**null 決定在資料庫中此欄位能否設成** `NULL` 或 `NOT NULL` ，跟表格驗證無關。可參考 [stackoverflow 的討論](https://stackoverflow.com/questions/8159310/why-are-blank-and-null-distinct-options-for-a-django-model)。在課程中的 Post class 的 updated\_at argument，null=True 代表不一定會更新這個 post，在資料庫裡可以是 NULL。\




### 因版本更新，筆記內要更新的部分



[第三章 Hello World](https://foofish.net/django-tutorial-03.html)

在第三章，我們透過 urls.py 的檔案告訴 Django 什麼時候要用 home 這個 view。

```python
from django.contrib import admin
from django.urls import path

from boards import views

urlpatterns = [
    path('', views.home, name='home'),
    path('admin/', admin.site.urls),
]
```



[第五章 模型設計](https://foofish.net/django-tutorial-05.html)

從 Django 2.0 開始，ForeignKey 有二個 positional argument 要填，除了第一個 argument 要填 對應到的類別。第二個 argument 是 `on_delete` ，預設值是 CASCADE，on\_代表當對應的類別被刪除之後，這些對應到別人的資料要怎麼被處理，而 CASCADE 就是一倂刪除。其他一些值像 PROTECT、RESTRICT，可以在官方[文件](https://docs.djangoproject.com/en/4.1/ref/models/fields/#django.db.models.PROTECT)找到。

另外如果是在 Mac 用 Python 3.9 跑 migration，會遇到 `ModuleNotFoundError: No module named '_tkinter'，要透過安裝 tkinter` 解決，跑下面的指令：

```
brew install python-tk@3.9
```



[第八章 第一個測試用例](https://foofish.net/django-tutorial-08.html)

從 Django 2.0 版本移除了 [`django.core.urlresolvers` module](https://stackoverflow.com/questions/43139081/importerror-no-module-named-django-core-urlresolvers)，改成用：

```python
from django.urls import reverse
```

因此測試 url 的 status code 寫法改成：

```python
from django.test import TestCase
from django.urls import reverse

class HomeTests(TestCase):
    def test_home_view_status_code(self):
        url = reverse('home')
        response = self.client.get(url)
        self.assertEquals(response.status_code, 200)
```

assertEquals function 是用來確認兩個 variables 是不是一樣，目的是自動化測試。



[第九章 靜態文件設置](https://foofish.net/django-tutorial-09.html)

裡面使用的 Bootstrap 語法

* [間距的設定](https://getbootstrap.com/docs/4.0/utilities/spacing/)my-4 是 margin 的 top 和 bottom 設為 1.5 倍
* [麵包屑的語法](https://getbootstrap.com/docs/4.0/components/breadcrumb/)active 是目前所在的層級
* 表格的語法，裡面的 thead-inverse 在 [Bootstrap 4.0.0 之後就改名](https://github.com/twbs/bootstrap/releases/tag/v4.0.0-beta.2)成變成 thread-dark
* [small element](https://www.w3schools.com/bootstrap/bootstrap\_typography.asp) 是用在淺色字，[text-muted](https://getbootstrap.com/docs/4.0/utilities/colors/) 則是 Bootstrap 裡的淺色字 class 名稱
* [d-block](https://getbootstrap.com/docs/4.0/utilities/display/) 則是一種 display property
* [align-middle](https://getbootstrap.com/docs/4.0/utilities/vertical-align/) 則是垂直置中

還不知為何加載不了 Bootstrap 的本機檔案，先連 CDN

```html
<link crossorigin="anonymous" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" rel="stylesheet"></link>
```

[第十一章 URL 分發](https://foofish.net/django-tutorial-11.html)

高級 URLs 路由那一段，寫法可以參考[官方文件](https://docs.djangoproject.com/en/4.1/topics/http/urls/)

```python
from django.urls import path

from . import views

urlpatterns = [
    path('', views.home, name='home'),
    path('admin/', admin.site.urls),
    path('about/', views.about, name='about'),
    path('about/company', views.about_company, name='about_company'),
    path('<str: username>', views.user_profile, name='user_profile'),
]
```

在編寫測試 test.py，有運用到以下觀念

* reverse function 像是把 url 標記進程式裡，第一個 viewname argument 是放 url 的名稱或是呼叫 view object，更多細節可以參考 [reverse 文件](https://docs.djangoproject.com/en/4.1/ref/urlresolvers/) resolve 的概念可以參考[這一篇](https://www.cnblogs.com/JcHome/p/16097114.html)resolve 的第一個 [argument 是放路徑](https://docs.djangoproject.com/en/4.1/ref/urlresolvers/#django.urls.resolve)。
* `self.client` 是 [Django test client 不是真的瀏覽器](https://stackoverflow.com/questions/57425954/usage-of-self-client-get-vs-self-browser-get)，甚至不會發出真的 request
* [\*args 和 \*\*kwargs 的說明](https://skylinelimit.blogspot.com/2018/04/python-args-kwargs.html)

我們寫 **404 Page Not Found** 的頁面，根據[官方的教程](https://docs.djangoproject.com/en/4.1/intro/tutorial03/#raising-a-404-error)有一點調整，要 import Http404 並且 Http404 method 可以加上顯示在畫面上的字串：

```python
import imp
from django.shortcuts import render
from django.http import HttpResponse, Http404
from .models import Board

def home(request):
    boards = Board.objects.all()
    return render(request, 'home.html', {'boards': boards})

def board_topics(request, pk):
    try:
        board = Board.objects.get(pk=pk)
    except Board.DoesNotExist:
        raise Http404("Question does not exist")
    return render(request, 'topics.html', {'board': board})
```

另一個寫法是用 `get_object_or_404` method，argument 有 class，可以參考[官方文件](django-tui-jian-ke-cheng.md#django-ru-men-yu-shi-jian)

```python
import imp
from django.shortcuts import render, get_object_or_404
from django.http import HttpResponse, Http404
from .models import Board

def home(request):
    boards = Board.objects.all()
    return render(request, 'home.html', {'boards': boards})

def board_topics(request, pk):
    board = get_object_or_404(Board, pk=pk)
    return render(request, 'topics.html', {'board': board})
```

在為 `HomeTests` class 寫測試的練習中，我們有運用到 Django 裡 creating object 的概念，[create()](https://docs.djangoproject.com/en/4.1/ref/models/querysets/#django.db.models.query.QuerySet.create) method 比 save() method 方便，同時建立和儲存物件。

另外在 Django 測試的概念裡會用 Response object ，不是 Django views 裡的 HttpResponse object。Response object 的 attribute `client` 和 method `get()` ，可以參考[文件](https://docs.djangoproject.com/en/4.1/topics/testing/tools/#testing-responses)。

`test_home_view_contains_link_to_topics_page` 使用 [assertContains() method](https://docs.djangoproject.com/en/4.1/topics/testing/tools/#django.test.SimpleTestCase.assertContains) 驗證 response 主體的文本是否包含給定的文本。我們在測試中使用的文本是 a tag 的 href 部分，所以我們在測試 response 主體是否包含 href="/boards/1"，裡面使用到 [Python string format() method](https://www.w3schools.com/python/ref\_string\_format.asp)format() method 會格式化括號內的值，插入用 {} 定義的 placeholder 內，接著回傳格式化的字串。{} 內可以留空、命名或是放入數字，像課程裡的是用 {0} 作為 placeholder，可參考 [format 的文件](https://www.w3schools.com/python/ref\_string\_format.asp)了解細節。







