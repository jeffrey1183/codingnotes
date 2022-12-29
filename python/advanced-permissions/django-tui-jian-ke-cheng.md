# Django 學習筆記

## [Django 入門與實踐](https://foofish.net/django-tutorial-00.html) <a href="#https-foofish.net-django-tutorial-00.html" id="https-foofish.net-django-tutorial-00.html"></a>

這份 Django 教學從虛擬環境搭建、系統設計的概念到實作的講解，我覺得不像官方文件那麼難消化，也與實際軟體開發流程符合，會有 PM 畫 wireframe 與工程師討論功能。下面的筆記是我自己閱讀這份教學時的學習紀錄，包含一些 method 和 css 的細節和 Django 更新之後的調整。



### [第三章 Hello World](https://foofish.net/django-tutorial-03.html)

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

### [第五章 模型設計](https://foofish.net/django-tutorial-05.html)

* 每個 Django model 都帶有一個特殊的 property，我們稱之為模型管理器(Model Manager)可以通過[屬性 **objects**](https://docs.djangoproject.com/en/4.1/ref/models/class/#objects) 來訪問這個管理器，主要用於數據庫操作。透過每個 model 的模型管理器(Model Manager) 我們可以得到一組 QuerySet，QuerySet 是物件的集合，源自我們的資料庫。因此我們會在 views.py 看到這樣的寫法：

```python
def home(request):
    boards = Board.objects.all()
```

官方文件請參考[這一段](https://docs.djangoproject.com/en/4.1/topics/db/queries/#retrieving-objects)，Queryset 的 method 相當多，請參考[此文件](https://docs.djangoproject.com/en/4.1/ref/models/querysets/#django.db.models.query.QuerySet)。

* `auto_add_new`[會在 model 物件第一次被創建時，將字段的值設成創建的時間，之後修改物件，字段的值不會再更新](https://agvszwk.github.io/2019/05/11/django%E7%9A%84model-auto-now-add%E5%92%8Cauto-now/)。設定 [DateTimeField](https://docs.djangoproject.com/en/4.1/ref/models/fields/#datetimefield) 的時候會用到。
* `ForeignKey` 是用在一對多的模型關係，其他關係像多對多，一對一請[參考文件](https://docs.djangoproject.com/en/4.1/ref/models/fields/#foreignkey)，參數 relate\_name 的細節可先看第五章的說明，他是一個 opotional 的項目，如果不設定會自動生成 class name set 的屬性，有需要了解其他參數再看[參考文件](https://docs.djangoproject.com/en/4.1/ref/models/fields/#django.db.models.ForeignKey.related\_name)
* 在定義 Django model 的欄位的時候，**blank 決定是否此欄位在表格中是否必填，** `False` 是指必填，不填會產生 error， `True` 是指可以允許留空。**null 決定在資料庫中此欄位能否設成** `NULL` 或 `NOT NULL` ，跟表格驗證無關。可參考 [stackoverflow 的討論](https://stackoverflow.com/questions/8159310/why-are-blank-and-null-distinct-options-for-a-django-model)。在課程中的 Post class 的 updated\_at argument，null=True 代表不一定會更新這個 post，在資料庫裡可以是 NULL。



從 Django 2.0 開始，ForeignKey 有二個 positional argument 要填，除了第一個 argument 要填 對應到的類別。第二個 argument 是 `on_delete` ，預設值是 CASCADE，on\_代表當對應的類別被刪除之後，這些對應到別人的資料要怎麼被處理，而 CASCADE 就是一倂刪除。其他一些值像 PROTECT、RESTRICT，可以在官方[文件](https://docs.djangoproject.com/en/4.1/ref/models/fields/#django.db.models.PROTECT)找到。

另外如果是在 Mac 用 Python 3.9 跑 migration，會遇到 `ModuleNotFoundError: No module named '_tkinter'，要透過安裝 tkinter` 解決，跑下面的指令：

```
brew install python-tk@3.9
```



### [第八章 第一個測試用例](https://foofish.net/django-tutorial-08.html)

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



### [第九章 靜態文件設置](https://foofish.net/django-tutorial-09.html)

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

### [第十一章 URL 分發](https://foofish.net/django-tutorial-11.html)

Django 2.0 後已經將 url 函數換成 path 函數，Django 怎麼處理 request 找到對應的 URL 並啟動 view 請[閱讀官方文件](https://docs.djangoproject.com/en/4.1/topics/http/urls/#how-django-processes-a-request)，寫的非常好。在 Path Converter 那一段有介紹 slug 這種 url 轉換的方式，[stackoverflow 有詳細說明](https://stackoverflow.com/questions/427102/what-is-a-slug-in-django)。



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

* reverse function 像是把 url 標記進程式裡，第一個 viewname argument 是放 url 的名稱或是呼叫 view object，更多細節可以參考 [reverse 文件](https://docs.djangoproject.com/en/4.1/ref/urlresolvers/) ，resolve 可以找出路徑對應的 view function，第一個 argument 是放路徑，可以參考 [resolve 的官方說明](https://docs.djangoproject.com/en/4.1/ref/urlresolvers/#resolve)。
* `self.client` 是 [Django test client 不是真的瀏覽器](https://stackoverflow.com/questions/57425954/usage-of-self-client-get-vs-self-browser-get)，甚至不會發出真的 request。
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

另一個寫法是用 `get_object_or_404` method，argument 有 class，可以參考[官方文件](https://docs.djangoproject.com/en/4.1/topics/http/shortcuts/#get-object-or-404)

不過為什麼我們可以直接使用 pk？我們的 model 裡沒有這個值吧？其實 pk 是 Django model 的特殊性質，永遠指向該 model 的 primary key（當然）。前面提過，Django model 一定要有 primary key，而且如果你沒有特別設定，預設會在每一個 model 加上一個 id 欄位來當作 primary key。所以在這個例子中使用 pk 就等同於 id，但是用 pk 在大多時候比較有彈性，因為事實上 primary key 可以隨意設定，不見得要是 id。

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

在設定 board 的連結，運用到 [url template tag](https://docs.djangoproject.com/en/4.1/ref/templates/builtins/#url) 的寫法，第一個 argument 是寫 URL pattern name，使用在 url.py 定義的名稱，其他 argument 按照順序用空格分開。



return 的語法

* 上面用到 [Python 的 return 語法](https://www.digitalocean.com/community/tutorials/python-return-statement)return 語法只用在 function 內，不能在 function 之外的地方。每個 function 都會 return something，如果一個 function 沒有 return 就是 return `None。`
* 我們也可以在 function 內計算，像 x+y 然後 return 結果。回傳值可以是 boolean, string, tuple, list 或 dictionary object。
* return 的值不只可以是 string, tuple, dictionary，也可以是 function，這邊用 render function。

reder function

* render function 的 arguments 請參考[官方文件](https://docs.djangoproject.com/en/4.1/topics/http/shortcuts/#render)。



### [第十二章 複用模板](https://foofish.net/django-tutorial-12.html)

#### 讀取靜態檔案

在這一章談到讀取靜態檔案(load static file)，即使照教學的步驟也一直讀取失敗，主要原因是 STATICFILES\_DIRS 設定錯誤，這個值預設是 [empty list](https://docs.djangoproject.com/en/4.1/ref/settings/#staticfiles-dirs)。在官方的文件裡的[參考寫法](https://docs.djangoproject.com/en/4.1/howto/static-files/)是：

```
STATICFILES_DIRS = [
    BASE_DIR / "static",
]
```

但實務上要看你的相對路徑調整，如果你是每個 app 都有獨自的 staic file 資料夾，寫法要調整成：

```
STATICFILES_DIRS = [
BASE_DIR / "app1/static",
BASE_DIR / "app2/static",
```

#### 套用 Google 字型

先到 [Google 字體的網站](https://fonts.google.com/)找到你喜歡的字體，從右上角召喚出程式碼，將 link 貼到 HTML 檔案和 CSS 檔案，下面有參考的程式碼，[官方文件](https://developers.google.com/fonts/docs/css2)也有說明。



<figure><img src="https://cdn-images-1.medium.com/max/800/1*I9cM2kEY2Khm8glnNQly5w.png" alt=""><figcaption></figcaption></figure>

HTML 檔案

```html
    <head>
        <meta charset="utf-8">
        <title>{% raw %}
{% block title %} Django Boards {% endblock %}</title>
        <link href="https://fonts.googleapis.com/css2?family=Arsenal:ital,wght@1,700&family=Henny+Penny&display=swap" rel="stylesheet">
        <link rel="stylesheet" href="{% static 'css/app.css' %}">
        <link rel="stylesheet" href="{% static 'css/bootstrap.min.css' %}
{% endraw %}">
    </head>
```

CSS 檔案

```css
.navbar-brand{ font-family: 'Henny Penny', cursive; }

```

#### 其他學習

* &#x20;\{{ block.super \}} 可以撈模板裡的預設值來用，這個案例裡，我們改變了 \{%  block title %\} 的預設值，可以參考[官方文件](https://docs.djangoproject.com/en/4.1/ref/templates/language/)的說明。原本網頁模板 base.html 定義預設 title tag 是 "Django Boards"，對於 Python 的 board 頁面，title tag 會是 "Python - Django Boards"。
* Bootstrap 的 navbar 寫法，可以參考[官方文件](https://getbootstrap.com/docs/4.0/components/navbar/)，`navbar 和 navbar-expand-lg 是為了 RWD，顏色的寫法請參考`[`這一段`](https://getbootstrap.com/docs/4.0/components/navbar/#color-schemes)`。`
* Navbar 裡面包了一個 container，這寫法是因為 Navbar 和 content 預設是 fluid，這樣可以避免 padding 重複出現，可參考[官方文件](https://getbootstrap.com/docs/4.0/components/navbar/#containers)。
* Navbar brand 是專門給公司、產品或專案名稱的 class，可參考[官方文件](https://getbootstrap.com/docs/4.0/components/navbar/#supported-content)。



一些第九章出現過的語法：

* [間距的設定](https://getbootstrap.com/docs/4.0/utilities/spacing/) my-4 是 margin 的 top 和 bottom 設為 1.5 倍
* [麵包屑的語法](https://getbootstrap.com/docs/4.0/components/breadcrumb/) active 是目前所在的層級
* 表格的語法，裡面的 thead-inverse 在 [Bootstrap 4.0.0 之後就改名](https://github.com/twbs/bootstrap/releases/tag/v4.0.0-beta.2)成變成 thread-dark
* [small element](https://www.w3schools.com/bootstrap/bootstrap\_typography.asp) 是用在淺色字，[text-muted](https://getbootstrap.com/docs/4.0/utilities/colors/) 則是 Bootstrap 裡的淺色字 class 名稱
* [align-middle](https://getbootstrap.com/docs/4.0/utilities/vertical-align/) 則是垂直置中

### [第十三章 表單處理](https://foofish.net/django-tutorial-13.html)

* Django 透過 CSRF Token 保護所有 POST 請求，每一次 POST 都會先檢查 CSRF Token，沒有 token 或是 token 無效就會拋棄提交的數據。
* Bootstrap 裡的 [form group](https://getbootstrap.com/docs/4.0/components/forms/#form-groups) class 是最簡單建立表格的寫法，搭配一行 label，一行 input 的寫法。而這些 label 和 input 是 form control，[form control](https://getbootstrap.com/docs/4.0/components/forms/#form-controls) 是指 \<form> 表單內的那些使用者介面元素，像是文字輸入欄位，密碼輸入欄位，日期輸入欄位，下拉選單，複選框，提交按鈕等。這個範例是用 textarea 可以設定要有幾列。
* [Button](https://getbootstrap.com/docs/4.0/components/buttons/) 的寫法可以參考 Bootstrap 官方文件。
* fieldset 的基本寫法和屬性，請參考[此文件](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/fieldset)，legend 是 fieldset 裡的標題。[在 Boostrap 裡](https://getbootstrap.com/docs/4.0/components/forms/#horizontal-form)，也是包在 form tag 裡，class 用 form-group。
* \<th> tag 是 [table head](https://www.computerhope.com/jargon/h/html-th-tag.htm) 的意思。
* 在 Bootstrap 裡有不同的 [table class](https://getbootstrap.com/docs/4.0/content/tables/) 可以設定樣式。
* 在寫 topics.html 的時候，用戶輸入的標題、用戶名稱和更新時間會顯示在 topics.html 上，我們運用在 views.py 建立的 topic 物件和 models.py 的 Topic 物件欄位，我們可以撈出資料庫裡的數據。比較特別的是 topic.starter 會 access user model，我們用 [user model](https://docs.djangoproject.com/en/4.1/ref/contrib/auth/#user-model) 的 username 欄位撈出資料。
* [url template tag](https://docs.djangoproject.com/en/4.1/ref/templates/builtins/#url) 可以回傳特定路徑，路徑裡的 argument 要記得設定，這一章節就有用到 boark.pk。[pk 是 model 會自動產生的欄位](https://docs.djangoproject.com/en/4.1/topics/db/models/#automatic-primary-key-fields)。
* 在 Python 代碼中，我們必須使用括號 `board.topics.all()` ，因為 all() 是一個 method。在使用 Django 模板語言寫程式的時候，在 HTML 模板文件裡我們不使用括號，就只是 `board.topics.all`。
* `mb-4` 是 Bootstrap 裡設定間距的寫法，m 是 margin，b 是 bottom，可[參考官方文件](https://getbootstrap.com/docs/4.0/utilities/spacing/)。
* 透過 User model 的 [create\_user method](https://docs.djangoproject.com/en/4.1/ref/contrib/auth/#django.contrib.auth.models.UserManager.create\_user) 創造測試用的 user instance。
* 在寫測試的時候，我們用到 csrf middleware token，講到 [middleware](https://zh.wikipedia.org/zh-tw/%E4%B8%AD%E9%97%B4%E4%BB%B6) 和 [csrf](https://www.squarefree.com/securitytips/web-developers.html#CSRF) ，[csrfmiddlewaretoken](https://docs.djangoproject.com/en/4.1/ref/csrf/#how-it-works) 是 post form 的隱藏欄位。
* `self.client` 是 [Django test client 不是真的瀏覽器](https://stackoverflow.com/questions/57425954/usage-of-self-client-get-vs-self-browser-get)，之前用到 [get method](https://docs.djangoproject.com/en/4.1/topics/testing/tools/#django.test.Client.get)，這邊是用 [post method](https://docs.djangoproject.com/en/4.1/topics/testing/tools/#django.test.Client.post)，除了路徑也把資料寫進 argument。



QuerySet Objects

* QuerySet 有一個 [exists method](https://docs.djangoproject.com/en/4.1/ref/models/querysets/#exists) 用來判斷 QuerySet 是否存在，會回傳 True 或 False。
* Python’s 的 [`unittest.TestCase`](https://docs.python.org/3/library/unittest.html#unittest.TestCase) class 有 [`assertTrue`](https://docs.python.org/3/library/unittest.html#unittest.TestCase.assertTrue)`,` [`assertFalse`](https://docs.python.org/3/library/unittest.html#unittest.TestCase.assertFalse) `和` [`assertEqual`](https://docs.python.org/3/library/unittest.html#unittest.TestCase.assertEqual)`幫助作判斷。`



QueryDict Objects

* 對於 `HttpRequest` object 來說，他的 `GET` and `POST` attributes 都是 `django.http.QueryDict 的 instance，是一個`dictionary-like 的 class，常運用在 HTML 表格，對於一個 key 傳遞多個 value，例如一個留言的功能，用戶可以上傳不同內容。
* 在這一章裡我們將新增的標題和內文 assign 到 subject 和 message 的變數內，[request.POST 從官方文件的說明得知是一個 dictionary](https://docs.djangoproject.com/en/4.1/ref/request-response/#django.http.HttpRequest.POST)，所以我們下面用 [assign dictionary value 寫法](https://www.tutorialspoint.com/How-do-I-assign-a-dictionary-value-to-a-variable-in-Python)。文件也提到 POST 的表格可能是空的，因此要用 if request.method == “POST” 去做檢查，不能直接 request.POST 資料。

```
subject = request.POST['subject']
message = request.POST['message']
```

* QuerySet 的 [first method](https://docs.djangoproject.com/en/4.1/ref/models/querysets/#first) 可以回傳 set 裡的第一個物件，如果沒有要求順序，會根據 primary key 的順序。
* POST 的時候，會建立 topic 和 post 兩個 object，透過 QuerySet 的 [create method](https://docs.djangoproject.com/en/4.1/ref/models/querysets/#create)。



Form API

*   在 HTML 裡， form element 包了一些 elements 像 label 和 input element，而 input element 處理 2 件事：

    * where: 用戶輸入的資料，要傳到哪個 URL&#x20;
    * how: 要用哪種 HTTP method&#x20;

    例如 Django 的後台有很多 input element，type = “text” 是設定 username，type=”password” 是設定 password，`type="submit"` 是設定登入按鈕。資料要傳到哪裡會用`<form> 的 action` attribute - `/admin/，用 method` attribute 指定 HTTP method 是 POST 還是 GET。
* 以 Django 官方文件為例，使用 Form class 定義表單欄位的時候，欄位裡的 [label argument](https://docs.djangoproject.com/en/4.1/ref/forms/fields/#label) 會預設先拿命名的 field name 作為預設值，field name 中的底線轉成空白，把第一個字母變大寫，大多會手動設定 label 要顯示什麼。
* Field class 預設每一個欄位都是[必填(required)](https://docs.djangoproject.com/en/4.1/ref/forms/fields/#django.forms.Field.required)，如果是空字串或是 None 都會觸發 ValidationError，如果此欄位非必填請在 class 的 argument 中設定 **required=False。**
* Widget 負責處理 Django 與 HTML input element 的轉換，[每種欄位都有預設要轉成哪種 HTML element](https://docs.djangoproject.com/en/4.1/ref/forms/fields/#charfield)，例如 CharField 預設是 TextInput，對應 HTML 的 **\<input type=”text” …>。**如果你不喜歡可以直接指定你要哪個 widget，像[官方的案例](https://docs.djangoproject.com/en/4.1/ref/forms/widgets/#specifying-widgets)中，就把 CharField 預設的 TextInput widget 改成 Textarea。至於有哪些 widget 可以使用，可以參考[官方文件](https://docs.djangoproject.com/en/4.1/ref/forms/widgets/#widgets-handling-input-of-text)。
* Meta class 是用來設定 metadata，metadata 不是欄位也非必填，可以從這份 [Model Meta options文件](https://docs.djangoproject.com/en/4.1/ref/models/options/#model-meta-options)去找，比較常用像 ordering。
* 如果你的 app 上的欄位跟資料庫欄位有相關，就不用在 form 裡面再定義一次欄位，可以用 ModelForm。在 models.py 裡定義的 model field 會轉成 form field，像 `Charfield` 一樣轉成 `Charfield`，而`ManyToManyField` 會轉成 `MultipleChoiceField`**`。`**`轉換表可以參考`[**`此文件`**](https://docs.djangoproject.com/en/4.1/topics/forms/modelforms/#field-types)**`。`**
* ModelForm 和 Form class 的差異是什麼？可以看[官方的 ModelForm 這篇](https://docs.djangoproject.com/en/4.1/topics/forms/modelforms/#a-full-example)，有把寫法列出來，看起來差在 save method 的使用，範例裡使用的 [string method](https://www.quora.com/What-does-def-str\_\_-self-method-does-in-Django) 是用來回傳欄位的名稱。案例中可以看到 DateField 怎麼用，[null 和 blank 兩個 argument 預設都是 False](https://www.geeksforgeeks.org/datefield-django-models/)，表示此欄位必填，如果是非必填欄位要改成 True。
* DateField 和 DateTimeField 的差異是，DateField 只有日期，DateTimeField 還加上時間。
* ManyToManyField 的欄位要怎麼應用，請參考此[官方文件](https://docs.djangoproject.com/en/4.1/topics/db/examples/many\_to\_many/)。

## Pluralsight 線上課程

* [付費課程](https://app.pluralsight.com/search/?q=django\&type=conference%2Cvideo-course%2Cdemo%2Cguide%2Cinteractive-course%2Clab%2Cpath%2Cproject%2Cwebinar\&m\_sort=relevance\&query\_id=a7965885-77a1-4d16-a22f-b9e6f70736b1\&source=user\_typed)(還沒研究)

