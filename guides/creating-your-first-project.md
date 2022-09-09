# Intro

Python 基礎

* [ccClub](https://medium.com/ccclub/tagged/python-%E5%85%A5%E9%96%80)
* Programiz
* [Django girls](https://djangogirlstaipei.gitbooks.io/django-girls-taipei-tutorial/content/)



下面[兩層巢狀迴圈](https://medium.com/ccclub/ccclub-python-for-beginners-tutorial-4990a5757aa6)印出來會先空 4 格，再印出數字，每次 j 等於 9 的時候會換行。 \t 是空 4 格， end = '' 是不換行的意思，\n 則可以讓 print() 換行。

```python
for i in range(1, 10):
    for j in range(1, 10):
        if j == 9:
            print("\t", i*j) # j == 9時，換行
        else:
            print("\t", i*j, end = '') # j < 9時，不換行
```

