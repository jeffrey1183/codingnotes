# Django 推薦課程

## [Django 入門與實踐](https://foofish.net/django-tutorial-00.html)

從虛擬環境搭建到系統設計的概念，不像官方文件那麼難消化，也與實際軟體開發流程符合，會有 PM 畫 wireframe 與工程師討論功能。另外這份教學的有提到光寫 demo 意義不大，要做[實際項目](https://foofish.net/django-tutorial-04.html)才有意義。



### 重點筆記

[第五章 模型設計](https://foofish.net/django-tutorial-05.html)

* `auto_add_new`[會在 model 物件第一次被創建時，將字段的值設成創建的時間，之後修改物件，字段的值不會再更新](https://agvszwk.github.io/2019/05/11/django%E7%9A%84model-auto-now-add%E5%92%8Cauto-now/)。
* `ForeignKey` 是用在一對多的模型關係，其他關係像多對多，一對一請[參考文件](https://docs.djangoproject.com/en/4.1/ref/models/fields/#foreignkey)參數 relate\_name 的細節可[參考文件](https://docs.djangoproject.com/en/4.1/ref/models/fields/#django.db.models.ForeignKey.related\_name)。&#x20;



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



