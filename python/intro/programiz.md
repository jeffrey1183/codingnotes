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

![](../../.gitbook/assets/image.png)
