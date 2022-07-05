# Lesson 2：Data Types And Operators

## 課程大綱

* Data Types: Integers, Floats, Booleans, Strings&#x20;
* Operators: Arithmetic, Assignment, Comparison, Logical&#x20;
* Built-In Functions, Type Conversion&#x20;
* Whitespace and Style Guidelines

## Operators: Arithmetic, Assignment, Comparison, Logical&#x20;



### Arithmetic Operators Arithmetic operators

* **`+` ** Addition
* `-`Subtraction
* `*`Multiplication
* `/` Division&#x20;
* `%` Mod (the remainder after dividing)&#x20;
* `**` Exponentiation (note that ^ does not do this operation, as you might have seen in other languages)&#x20;
  * 幾次方，比方說 print(2 \*\* 5)，就是 2 的 5 次方&#x20;
* `//` Divides and rounds down to the nearest integer&#x20;
  * 算出最接近的整數

Bitwise operators(各式各樣比較符號) are special operators in Python that you can learn more about here if you'd like.

Examples print(3 + 5) # 8 print(1 + 2 + 3 \* 3) # 12 print(3 \*\* 2) # 9 print(9 % 2) # 1

Python 的運算符號遵守數學規則，像 print( 1 + 2 \* 3)，會先做乘除後加減，要加法先就可以 print( (1 + 2) \* 3)。

print(3 \*\* 2) 是指 3 的二次方

print(9%2)\
% (modulo) 是算出餘數，所以這個會是 1

print(7 // 2) 7除以2是 3.5，向下取整數為 3 print(-7 // 2) -7 除以2是 -3.5，向下取整數為 -4

## ****

## **String Methods**&#x20;

學習重點

* Method 和 Function 的差異



除了上面提到 method 會吃(accept) argument，argumet 寫在引號裡面，沒寫是因為第一個 argument 是他們自己，有二個以上的 argument 就寫在引號內。

以下面的三行程式為例， count 和 find 兩個 method 都需要放 argument 進去，而 islower 不需要。不會有人記住所有 method，所以會使用文件很重要。

```python
>>> my_string.islower()
True
>>> my_string.count('a')
2
>>> my_string.find('a')
3
```



