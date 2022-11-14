# Programiz

閱讀型的程式學習：[https://app.programiz.pro/course/learn-python-basics/variables?page=1](https://app.programiz.pro/course/learn-python-basics/variables?page=1)

優點：給你思考的空間大

費用低：一個月 382 台幣



while loop 和 if 的差別，while 會重複執行，會一直執行到 boolean expression 變成 `False`

下面是講 Tuple 的 slicing，要記住後面的 index 不會包含在裡面，所以像第三行的 -1，numbers\[-1] 不會算進去，會算到 numbers\[-2]

```python
numbers = (10, 20, 30, 40, 50, 60)
 
# items from index 0 to 3
print(numbers[0: 4])    # (10, 20, 30, 40)
 
# items from index 3 to 4
print(numbers[3: 5])   # (40, 50)
 
# items from index 3 to second last
# remember, last index is exclusive
print(numbers[3: -1])   # (40, 50)
```

List 和 Tuple 可以放重複的值。



Tuple 不能改 item 的值，這點跟 List 不同，但可以整個重新 assign，像下面這個會跑出 TypeError

```python
languages = ('Python', 'JavaScript', 'C++')
 
# trying to change the second item to 'Java'
languages[1] = 'Java'
```

但可以 assign new tuple 到變數內。

```python
languages = ('Python', 'JavaScript', 'C++')
 
# assigning new tuple to languages
languages = ('Python', 'JavaScript')
 
print(languages)   # ('Python', 'JavaScript')

```

所以也不能刪除單一的 item，只能刪除整個 tuple。



dictionary 裡面是 key 和 value 的關係，dictionary 內不能有重複的 key，像下面的 dict5變數。

dictionary 一定要是 unique 和 immutable object。dictionary 可以用字串、數字和 Tuple 當作 key，但不能用 list 當作 key，像下面的變數 dict4，但 list 可以作為 value，像下面的 dict3 變數。



```python
# empty dictionary
dict1 = {}
 
# dictionary containing one item
dict2 = {1: 'one'}
 
# dictionary containing three items
dict3 = {1: 10, 'greet': ['Hey', 'Hello'], 'one': 1}
 
# invalid dictionary
# list cannot use used as keys
dict4 = {[1, 2]: 'Hey'}
 
# invalid dictionary
# duplicate key
dict5 = {1: 'One', 1: 'Two'} 
```

Dictionary 可以新增、更改和刪除裡面的項目。



Set 不能包含重複的值。&#x20;



Set 刪除項目時是用 discard method

```python
animals = {'tiger', 'cat', 'dog'}
 
# Remove the 'cat' item
animals.discard('cat')
 
print(animals)   # {'dog','tiger'}
```

Set 沒有順序性，所以像下面聯集的案例，印出的結果不管是 {3,4,1,2}、{1,2,3,4}、{2,1,3,4} 都是對的。

```
odd_numbers = {1, 3}
even_numbers = {2, 4}

# union of sets
numbers = odd_numbers | even_numbers

print(numbers)

```



List, tuple, dictionary, set 這些 compound data 是可以用下面這些 functions 轉換。

* `list()` - converts to list
* `tuple()` - converts to tuple
* `dict()` - converts to dictionary
* `set()` - converts to set

Set 不能包含重複的值，如果轉換的時候 list 和 tuple 有重複的值變成一個，像下面這樣：

```python
# convert list to set
result = set([1, 2, 3])
print(result) # {1, 2, 3}
 
#convert string to set
result = set('abca')
print(result) # {'a', 'b', 'c'}
 
# convert tuple to set
result = set((1, 2, 3, 2, 3))
print(result) # {1, 2, 3}
 
# convert dictionary to set
result = set({2: 4, 10: 20}) # (2, 10)
print(result)
```

藉著這樣的特性，我們可以把 list 裡重複的值透過二次轉換移除：

```python
numbers = [1, 1, 2, 3, 4, 1, 1]
 
numbers = list(set(numbers))
print(numbers)   # [1, 2, 3, 4]

```

如果要找出 list 內交集的項目，也可以把 list 轉成 set，再用聯集的方式，如下案例：

```python
frontend_developers = ['Rob', 'Jane', 'Mary', 'Anne']
backend_developers = ['Jane', 'Jack', 'Lily']

# converting lists to sets and performing set intersection
both_developers = set(frontend_developers) & set(backend_developers)

# converting to list and printing it
print(list(both_developers))
```

![](<../../.gitbook/assets/image (1) (1) (1).png>)



Module

Module 是一個檔案，裡面包含很多可以用在 app 內的 function，只要 import 進來就可以使用。

你可能聽過 Python 可以用在  data analytics、web development 和 automation。為了解決這些面向的問題，我們可以不需要從頭開始寫，例如：

* data analytics, 有像 NumPy, pandas 的 modules
* web development, 有像  Django, Flask 的 modules
* csv files, 有像 csv, pandas 的 modules



程式上的寫法如下：

![](<../../.gitbook/assets/image (1) (1) (2).png>)

要想知道更多 math module 裡的 function，可以搜尋 Python math module。如果只要 import 一些 function，我們可以用  `from...import` ，像下面只有 import sqrt 和 floor 兩個 function。



```python
from math import sqrt, floor
 
number = 25
 
# compute square root
result = sqrt(number)
 
print(result)   # 5.0
```



## Nested Loops

loop 可以放進另一個 loop 裡，稱為 nested loop。裡面的 inner loop 會基於外層的 loop 每個 iteration 執行，以下面的案例，inner loop 內的每個項目會跑一次。

**外圈第一輪**

* **attribute** is `'Electric'`, **car** is `'Tesla'`
* **attribute** is `'Electric'`, **car** is `'Porsche'`
* **attribute** is `'Electric'`, **car** is `'Mercedes'`

**外圈第二輪**

* **attribute** is `'Fast'`, **car** is `'Tesla'`
* **attribute** is `'Fast'`, **car** is `'Porsche'`
* **attribute** is `'Fast'`, **car** is `'Mercedes'`

```python
attributes = ['Electric', 'Fast']
cars = ['Tesla', 'Porsche', 'Mercedes']

for attribute in attributes:
    for car in cars:
        print(attribute, car)
#Output        
#Electric Tesla
#Electric Porsche
#Electric Mercedes
#Fast Tesla
#Fast Porsche
#Fast Mercedes
```



## Assignment Operators

當我們 `=`  等於符號的時候，指的是把 value assign 到 variable 內。例如：

```python
pi = 3.1415
```

實際上還有很多 assignment operator

| Operator | Example   | Equivalent to |
| -------- | --------- | ------------- |
| `+=`     | x += 5    | x = x + 5     |
| `-=`     | x -= 5    | x = x - 5     |
| `*=`     | x \*= 5   | x = x \* 5    |
| `/=`     | x /= 5    | x = x / 5     |
| `%=`     | x %= 5    | x = x % 5     |
| `//=`    | x //= 5   | x = x // 5    |
| `**=`    | x \*\*= 5 | x = x \*\* 5  |

`/=` 跟  `//=`  跟 `%=`最容易搞混，第一個是整除，第二個是商數(quotient)，第三個是餘數。

```python
x = 15
x /= 4
print(y)

y = 15
y //= 4  
print(x)

z = 12
z %= 4
print(z)

# Output x : 3.75
# Output y : 3
# Output z : 0
```



None

* The `None` keyword is used to indicate null i.e. no value.
* None 是用來表示 null，沒有值

```python
def greet():
    print('Hey')
 
result = greet()
print(result)

#Output
#Hey
#None
```

這裡的 `greet()` function 沒有 return 任何值，意思是他回傳 `None`





Truthy and Falsy

如果值是 `True` 我們就稱作 **truthy** values，同樣的，如果值是`False` ，我們稱作 **falsy** values。

在 Python 裡，下面這些值被視為 **falsy。**

* `None`
* `False`
* **0**, **0.0**
* 空的 strings, lists, dictionaries&#x20;

下面的案例為何我們得到的結果是 20 而不是 30，就跟上面的概念有關。

```python
n1 = 20
n2 = 10
n3 = 30
 
if n1 > n2 and n3:
    print(n1)
elif n2 > n1 and n3:
    print(n2)
else:
    print(n3)

#Output
#20
```

因為, `n1 > n2` 被視為 `True`, 加上 `n3` 也被視為 `True` 因此 30，印出 20。應該改寫成

```python
if n1 > n2 and n1 > n3:
    print(n1)
```



pass Statement

假設我們有 loop 或 function 現在還不能 implemented 要之後才能，我們無法用 empty body 或 comment 因為會有 error 產生。

為此，我們可以用 `pass` statement 讓程式空著。

Here's how we can resolve the issue in the above program.

```python
n = 10

if n > 10:
    pass
 
print('Hello')

#Output
#Hello
```

`replace()` method

* 包含兩個 argument，以下面的案例為例，可以看 replace method 怎麼把 Python 的 yt 換成用戶輸入的字。
  * **oldValue** - a substring we want to replace
  * **newValue** - a substring that replaces the oldValue

```python
# Replace ___ with your code

language = 'Python'

# take string as input for ch variable
ch = input()

# use the replace() method to replace 'yt'
new_string = language.replace('yt', ch)

print(new_string)
```



合併 Dictionary

* 用 update() function
* [參考資料](https://favtutor.com/blogs/merge-dictionaries-python)

```python
# Replace ___ with your code

# create two dictionaries
A = {12: 'Kathmandu', 11: 'London', 3: 'Sydney'}
B = {10: 'New York', 2: 'Delhi'}

# merge the two dictionaries using update()
A.update(B)

# print the merged dictionary
print(A)
```

## Create a Dictionary <a href="#challenge-2746" id="challenge-2746"></a>



Write a program that uses a loop to take 3 key-value inputs from the user and create a dictionary using these keys and values.

* Create an empty dictionary named `my_dict`.
* Use `for` loop to iterate from **1** to **3**, including **3**.
* Inside the loop, take inputs for `key` and `value` and store them in `my_dict`.
* Print the updated `my_dict`.

```python
# Replace ___ with your code

# create an empty dictionary named my_dict
my_dict = {}

# use for loop to iterate from 1 to 3, including 3
for i in range(1,4):
    # inside the loop, take input for key and value and store them in my_dict
    key = input('Enter the key')
    value = input('Enter the value')
    my_dict[key] = value    

# print my_dict
print(my_dict)
```



