# Intro

## Python 基礎知識

* [Python 入門](https://medium.com/ccclub/tagged/python-%E5%85%A5%E9%96%80) by [ccClub](https://www.ccclub.io/?fbclid=IwAR3oMQqJL159OCLdriqsNtSlw4EPB4RUtP5\_A44xR8osQAdd84HimfM2ysk)
* [Programiz](https://programiz.pro/)
* [Django girls](https://djangogirlstaipei.gitbooks.io/django-girls-taipei-tutorial/content/)
* [Digitalocean](https://www.digitalocean.com/community/tutorials)



## Python Windows 安裝

* [安裝步驟](https://docs.aws.amazon.com/zh\_tw/elasticbeanstalk/latest/dg/eb-cli3-install-windows.html)
* 安裝時要勾選 Add Python to PATH



### enumerate function <a href="#f2e0" id="f2e0"></a>

我們可以使用 Python 中的一個函數來同時取得一個元素在一個 list 中的 index 與他的值。使用的方法如下 ：

```python
for index, item in enumerate(list_name):
    print(index, item)
```

以 `dangerous` 為例：

```python
dangerous = ['Jam', 'In the Closet', 'Remember the Time',"Heal the World","Black or White","Who Is It","Give In to Me", "Dangerous"]
for index, song in enumerate(dangerous):
    print(index, song)
```

參考資料：[Hubspot 介紹 Enumerate function](https://blog.hubspot.com/website/python-enumerate)





#### Python math.pow() Method

計算一個數字的平方、三次方到 n 次方，可參考[文件](https://www.w3schools.com/python/ref\_math\_pow.asp)。





## Dry 原則

* [參考文章](https://shawnlin0201.github.io/Methodology/Methodology-001-DRY-principle/)



把 list 轉成字串

* 透過 join method

把字串轉成 list

* 透過 split method



參考資料：[ccClub](https://medium.com/ccclub/ccclub-python-for-beginners-tutorial-f1b4e7d2e5ac)





下面[兩層巢狀迴圈](https://medium.com/ccclub/ccclub-python-for-beginners-tutorial-4990a5757aa6)印出來會先空 4 格，再印出數字，每次 j 等於 9 的時候會換行。 \t 是空 4 格， end = '' 是不換行的意思，\n 則可以讓 print() 換行。

```python
for i in range(1, 10):
    for j in range(1, 10):
        if j == 9:
            print("\t", i*j) # j == 9時，換行
        else:
            print("\t", i*j, end = '') # j < 9時，不換行
```

