# Django

虛擬環境的設定和 Django 安裝

* 來源：[Django Girls](https://djangogirlstaipei.gitbooks.io/django-girls-taipei-tutorial/content/django/installation.html)
* 確認是否安裝 Python，在 Mac 是輸入 python3&#x20;

```
python3
```

* 建議先建立 Python 虛擬環境，先用 command line 移到要建立虛擬環境的位置

```
python3 -m venv django_venv
```

* 創建完成後，要 activate，啟動成功後會看到指令前面多了虛擬資料夾名稱(django\_venv)

```
source django_venv/bin/activate
```

* 安裝 Django

```
python -m pip install Django
```

安裝成功！Successfully installed Django-4.0.6 asgiref-3.5.2 sqlparse-0.4.2

## 確認安裝成功 <a href="#que-ren-an-zhuang-cheng-gong" id="que-ren-an-zhuang-cheng-gong"></a>

最後，讓我們最後來測試一下。請在虛擬環境下指令輸入 `python`，進入互動式命令列環境

```
python
```

輸入以下的指令取得 Django 版本資訊：

```
>>> import django
>>> django.VERSION
(4, 0, 6, 'final, 0')
```

如果看見類似上面的訊息，就代表安裝成功囉！就可以從[官方的 Tutorial 1 ](https://docs.djangoproject.com/en/4.0/intro/tutorial01/)開始走一遍。官方教學目前是給 Django 4.0 和 Python 3.8 以上版本。





* [建立 Django project 的指令](https://docs.djangoproject.com/en/4.0/intro/tutorial01/) [Creating Virtual Environments before Installing Django](https://docs.python.org/3/tutorial/venv.html)
* Change current path to where you want to create virtual environment and activate it.
* The path you run `pip3 install Django` will influence the command of `django-admin startproject PROJECT_NAME` to create a number of starter files for Django project. For example, I run `pip3 install Django` on the path of `/Users/jeffreywang/`, it influence my command become `/Users/jeffreywang/Library/Python/3.8/bin/django-admin startproject lecutre3` due to the path I install django-admin like below picture.

{% hint style="info" %}
**Good to know:** your product docs aren't just a reference of all your features! use them to encourage folks to perform certain actions and discover the value in your product.
{% endhint %}
