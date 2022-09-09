---
description: 使用物件導向概念寫出圈圈叉叉遊戲
---

# 圈圈叉叉遊戲

## **圈圈叉叉玩法**

雖然大家應該都知道但還是說一下玩法。

* 遊戲會有一個 3 x 3 的九宮格，兩個人玩。
* 如果一個人選擇用 X，另一個人就是 O，兩個人輪流在空格劃圈圈叉叉。
* 先將符號連成一條線的就是贏家。
* 如果 9 格填完都沒有連成線就是平手。

<figure><img src="https://programiz-pro-spaces.sfo3.digitaloceanspaces.com/course-images/python-beyond-basics/python2-6.2.1.png" alt="Tic-tac-toe in Python"><figcaption></figcaption></figure>

## 專案設計

我們用物件導向來寫這個遊戲，遊戲中會包含這些 class：

* `Board` -處理圈圈叉叉的板子
* `Player` - 處理玩家下哪
* `Game` - 判斷輸贏或平手

### **圈圈叉叉的板子**

由於圈圈叉叉有 9 個格子，我們把每個格子都紀錄一個編號，像下面這張圖， **8** 指的是中間下面的那格。

<figure><img src="https://programiz-pro-spaces.sfo3.digitaloceanspaces.com/course-images/python-beyond-basics/python2-6.2.2.png" alt="Working of Tic-tac-toe in Python"><figcaption></figcaption></figure>

### **兩名玩家**

如同上面我們提到的，是遊戲是二個人玩，第一人用叉叉，第二個人用圈圈。

### **輸贏和平手的判斷**

我們會透過確認每一個位置的符號判斷勝負或平手，舉例來說：

* 如果位置 4, 5, 6 都是叉叉，第一個人就贏了
* 如果位置 3, 5, 7 都是圈圈，第二個人就贏了
* 如果位置 2, 5, 8 都是叉叉，第一個人就贏了
* 如果所有空格都填完，但都沒有連成線，就算平手

在寫程式前，有一些事情我們要了解，像一個物件是可以塞進另一個物件裡：

```python
class Vehicle:
    def __init__(self, wheels):
        self.wheels = wheels
        
        # creating an object of the Engine class
        self.engine = Engine(400)
    
    def run_method(self):
        print(f'vehicle wheels: {self.wheels}')
 
        # self.engine is an object of Engine
        # self.engine.power is an attribute of Engine
        print(f'vehicle power: {self.engine.power}')
 
class Engine:
    def __init__(self, power):
        self.power = power
        
v1 = Vehicle(4)
v1.run_method()

```

像上面的案例有`Vehicle` 和 `Engine`兩個 class，在 Vehicle class 的 init method，我們建立了一個 Engine 物件並且 assign 給 Vehicle 的 engine attribute。&#x20;

所以當上面的 `v1` object 建立的時候，會有`wheels` (值是 4)和 `engine` (和 `Engine`)一起建立。

現在我們要從 v1 物件 access engine 物件的 `power` attribute 要用`v1.engine.power`

### Step 1: 建立 Board Class

```python
class Board:
    def __init__(self):
        self.board = [' ', ' ', ' ',
                      ' ', ' ', ' ',
                      ' ', ' ', ' ']
    def print_borad(self):
         print('\n')
         print(' ' + self.board[0] + ' | ' + self.board[1] + ' | ' + self.board[2] )
         print('-----------')
         print(' ' + self.board[3] + ' | ' + self.board[4] + ' | ' + self.board[5] )
         print('-----------')
         print(' ' + self.board[6] + ' | ' + self.board[7] + ' | ' + self.board[8] )
    
board = Board()
borad.print_board()

#Output
#    |    |
#-----------
#    |    |
#-----------
#    |    |

    
```

目前每一格都是空格，接下來我們會用 **X** 或 **O** 替換玩家選擇的格子。

### Step 2: 建立玩家的 Class

接著我們定義 `Player` class，這個物件有兩個 attributes：

* `type` - 標記 player 是用 `'X'` 或 `'O'`
* `name` - 紀錄玩家的名字

我們設定第一個玩家都會用 `'X'` ，所以第二個玩家是用 `'O'` ，然後 `get_name()` method 是用來獲取玩家的名字並儲存。

```python
class Player:
    def __init__(self, type):
         self.type = type
         self.name = self.get_name()
    def get_name(self):
         if self.type = "X":
              name = input('Player selecting X, enter your name: ')
         else:
              name = input('Player selecting O, enter your name: ')
         return name


class Board:
    def __init__(self):
        self.board = [' ', ' ', ' ',
                      ' ', ' ', ' ',
                      ' ', ' ', ' ']
    def print_borad(self):
         print('\n')
         print(' ' + self.board[0] + ' | ' + self.board[1] + ' | ' + self.board[2] )
         print('-----------')
         print(' ' + self.board[3] + ' | ' + self.board[4] + ' | ' + self.board[5] )
         print('-----------')
         print(' ' + self.board[6] + ' | ' + self.board[7] + ' | ' + self.board[8] )

        
```



### Step 3: 建立 Game Class

這邊我們要運用到 class 之間的關係，建立 game class

* `board` - 儲存由 `Board` class 建立的物件
* `player1` - 儲存第一個玩家
* `player2` - 儲存第二個玩家
* `current_player` - 辨識現在的玩家，一開始先從 `player1` 開始

當 Game object 建立好，上面這些變數就會自動建立，play method 下面來講

```python
class Board:
    def __init__(self):
        self.board = [' ', ' ', ' ', 
                      ' ', ' ', ' ', 
                      ' ', ' ', ' ']

    def print_board(self):
        print('\n')
        print(' ' + self.board[0] + ' | ' + self.board[1] + ' | ' + self.board[2])
        print('-----------')
        print(' ' + self.board[3] + ' | ' + self.board[4] + ' | ' + self.board[5])
        print('-----------')
        print(' ' + self.board[6] + ' | ' + self.board[7] + ' | ' + self.board[8])

class Player:
    def __init__(self, type):
        self.type = type
        self.name = self.get_name()
    
    def get_name(self):
        if self.type == 'X':
            name = input('Player selecting X, enter your name: ')
        else:
            name = input('Player selecting O, enter your name: ')
        return name

class Game:
    def __init__(self):
        self.board = Board()

        self.player1 = Player('X')
        self.player2 = Player('O')

        self.current_player = self.player1

    # this method will be later used to play the game
    def play(self):
         pass

game = Game()
game.play()
```

### Step 4: 定義 play 和 update\_board method

* 更新 game class 裡的 `play()` method

**The update\_board() method**

新增 `update_board()` method 到 `Board` class，有 3 個 arguments：

* `self` - 物件本身
* `position` - 用戶輸入的數字，表示位置
* `type` - 如果現在是玩家 1，會是`'X'` 不然就是 `'O'`.

這個 method 確認 `position` 是否已經被填，我們會更新這個 board 並回傳  `True` ，如果已經被填會回傳 `False`

**The play() method**

&#x20;method 內我們用 `while` loop 如果是 True 就請玩家輸入 position 介於 1 到 9 之間。

由於我們是在 game class 裡定義這個 method，裡面的 self 是表示 game class，但我們要用 board class 的 update\_board() method，因此這邊要寫成 self.board 才能去呼叫 Board class 的 method。

&#x20;`update_board()` method 裡面有 `position` 和 `type` attributes，如果 `update_board()`  method 裡用戶選的位置是空的會 return`True` ，我們就可以把 board 印出來，接著把玩家換成另一個人。

<pre class="language-python"><code class="lang-python"><strong>class Board:
</strong>    def __init__(self):
        self.board = [' ', ' ', ' ', 
                      ' ', ' ', ' ', 
                      ' ', ' ', ' ']

    def print_board(self):
        print('\n')
        print(' ' + self.board[0] + ' | ' + self.board[1] + ' | ' + self.board[2])
        print('-----------')
        print(' ' + self.board[3] + ' | ' + self.board[4] + ' | ' + self.board[5])
        print('-----------')
        print(' ' + self.board[6] + ' | ' + self.board[7] + ' | ' + self.board[8])
    
    def update_board(self, position, type):
        if self.board[position - 1] == ' ':
            self.board[position - 1] == type
            return True
        else:
            print('Position already selected. Select another position.')
            return False
        

class Player:
    def __init__(self, type):
        self.type = type
        self.name = self.get_name()
    
    def get_name(self):
        if self.type == 'X':
            name = input('Player selecting X, enter your name: ')
        else:
            name = input('Player selecting O, enter your name: ')
        return name

class Game:
    def __init__(self):
        self.board = Board()

        self.player1 = Player('X')
        self.player2 = Player('O')

        self.current_player = self.player1

    # this method will be later used to play the game
    def play(self):
         while True:
             message = f'{self.current_player.name}, enter the position 1-9: '
             position = int(input(message))
             
             if self.board.update_board(position, self.current_player.type):
                 self.board.print_board()
                 
                 if self.current_player == self.player1:
                     self.current_player = self.player2
                 else:
                     self.current_player = self.player1

game = Game()
game.play()</code></pre>

### Step 5: 增加判斷輸贏或平局的邏輯

**check\_winner() method**

看有沒有一樣的符號連成一線，如果有就回傳 `True` 不然回傳 `False`，另外如果 `if` condition 非常多可以用 `\` 換行。

**check\_draw() method**

如果格子都填滿了，但沒有贏家就是平手，所以就確認有沒有空格，回傳 True 或 False。

最後把這兩個 method 加進 play() method 內。



```python
class Board:
    def __init__(self):
        self.board = [' ', ' ', ' ', 
                      ' ', ' ', ' ', 
                      ' ', ' ', ' ']

    def print_board(self):
        print('\n')
        print(' ' + self.board[0] + ' | ' + self.board[1] + ' | ' + self.board[2])
        print('-----------')
        print(' ' + self.board[3] + ' | ' + self.board[4] + ' | ' + self.board[5])
        print('-----------')
        print(' ' + self.board[6] + ' | ' + self.board[7] + ' | ' + self.board[8])
    
    def update_board(self, position, type):
        if self.board[position - 1] == ' ':
            self.board[position - 1] == type
            return True
        else:
            print('Position already selected. Select another position.')
            return False
    
    def check_winner(self, type):
        if(self.borad[0] == type and self.board[1] == type and self.board[2] == type) or \
          (self.board[3] == type and self.board[4] == type and self.board[5] == type) or \
          (self.board[6] == type and self.board[7] == type and self.board[8] == type) or \
          (self.board[0] == type and self.board[3] == type and self.board[6] == type) or \
          (self.board[1] == type and self.board[4] == type and self.board[7] == type) or \
           (self.board[2] == type and self.board[5] == type and self.board[8] == type) or \
           (self.board[0] == type and self.board[4] == type and self.board[8] == type) or \
           (self.board[2] == type and self.board[4] == type and self.board[6] == type):
           return True
        else:
            return False
    
    def check_draw(self):
        if ' ' not in self.board:
            return True
        else:
            return False
            
class Player:
    def __init__(self, type):
        self.type = type
        self.name = self.get_name()
    
    def get_name(self):
        if self.type == 'X':
            name = input('Player selecting X, enter your name: ')
        else:
            name = input('Player selecting O, enter your name: ')
        return name

class Game:
    def __init__(self):
        self.board = Board()

        self.player1 = Player('X')
        self.player2 = Player('O')

        self.current_player = self.player1

    # this method will be later used to play the game
    def play(self):
         while True:
             message = f'{self.current_player.name}, enter the position 1-9: '
             position = int(input(message))
             
             if self.board.update_board(position, self.current_player.type):
                 self.board.print_board()
                 
                 if self.current_player == self.player1:
                     self.current_player = self.player2
                 else:
                     self.current_player = self.player1
  

game = Game()
game.play()p
```

### Step 6: 判斷輸贏或平局

將我們上面寫的 `check_winner()` 和 `check_draw()` method 要加進`play()` method 裡去判斷每一次下面後的輸贏，如果有輸贏就印出結果並 break 掉整個 while loop。&#x20;

&#x20;同樣的我們每次也會呼叫 `check_draw()` 看是不是格子都填滿不分輸贏。

```python
class Board:
    def __init__(self):
        self.board = [' ', ' ', ' ', 
                      ' ', ' ', ' ', 
                      ' ', ' ', ' ']

    def print_board(self):
        print('\n')
        print(' ' + self.board[0] + ' | ' + self.board[1] + ' | ' + self.board[2])
        print('-----------')
        print(' ' + self.board[3] + ' | ' + self.board[4] + ' | ' + self.board[5])
        print('-----------')
        print(' ' + self.board[6] + ' | ' + self.board[7] + ' | ' + self.board[8])

    def update_board(self, position, type):

         # if a player selects position n, n-1 index should be updated
         # if the position is not already filled, update the board 
         if self.board[position - 1]  == ' ':      
             self.board[position - 1] = type
             return True 

         # if the position is already filled, ask user to select another position
         else: 
              print('Position already selected. Select another position.')
              return False

    # If three symbols appears in a row, returning True
    def check_winner(self, type):
        if (self.board[0] == type and self.board[1] == type and self.board[2] == type) or \
           (self.board[3] == type and self.board[4] == type and self.board[5] == type) or \
           (self.board[6] == type and self.board[7] == type and self.board[8] == type) or \
           (self.board[0] == type and self.board[3] == type and self.board[6] == type) or \
           (self.board[1] == type and self.board[4] == type and self.board[7] == type) or \
           (self.board[2] == type and self.board[5] == type and self.board[8] == type) or \
           (self.board[0] == type and self.board[4] == type and self.board[8] == type) or \
           (self.board[2] == type and self.board[4] == type and self.board[6] == type):
            return True
        else:
            return False

    # If all fields are selected and there is no winner, it's a draw
    # Returning True if it's draw
    def check_draw(self):
        if ' ' not in self.board:
            return True
        else:
            return False

class Player:
    def __init__(self, type):
        self.type = type
        self.name = self.get_name()
    
    def get_name(self):
        if self.type == 'X':
            name = input('Player selecting X, enter your name: ')
        else:
            name = input('Player selecting O, enter your name: ')
        return name

class Game:
    def __init__(self):
        self.board = Board()

        self.player1 = Player('X')
        self.player2 = Player('O')

        self.current_player = self.player1

    # updating the play method
    def play(self):

        # using an infinite loop to run the game
        # we will later add conditions to terminate the loop
         while True:
              message = f'{self.current_player.name}, enter the position (1 - 9): '
              position = int(input(message))

              # the update_board() method return True if
              # the user selected position is not already filled
              if self.board.update_board(position, self.current_player.type):
                  self.board.print_board()
                    
                  # checking winner each time after updating the board 
                  if self.board.check_winner(self.current_player.type):
                      print(self.current_player.name, 'wins!')
                      break

                  # checking draw each time after updating the board
                  elif self.board.check_draw():
                      print('Game is a draw!')
                      break               

                  # changing current player when board is updated 
                  else:
                      if self.current_player == self.player1:
                          self.current_player = self.player2
                      else:
                          self.current_player = self.player1 

game = Game()
game.play()
```

### Step 7: 例外情況的處理

最後做些小修補在 play 和 update\_board method 裡，考慮到玩家可能在輸入位置的時候輸入了 1\~9 以外的數字，會需要處理這種例外情況，印出訊息提醒玩家。

```python
class Board:
    def __init__(self):
        self.board = [' ', ' ', ' ', 
                      ' ', ' ', ' ', 
                      ' ', ' ', ' ']

    def print_board(self):
        print('\n')
        print(' ' + self.board[0] + ' | ' + self.board[1] + ' | ' + self.board[2])
        print('-----------')
        print(' ' + self.board[3] + ' | ' + self.board[4] + ' | ' + self.board[5])
        print('-----------')
        print(' ' + self.board[6] + ' | ' + self.board[7] + ' | ' + self.board[8])

    def update_board(self, position, type):

        try :
            # if a player selects position n, n-1 index should be updated
            # if the position is not already filled, update the board 
            if self.board[position - 1]  == ' ':      
                self.board[position - 1] = type
                return True 

            # if the position is already filled, ask user to select another position
            else: 
                 print('Position already selected. Select another position.')
                 return False
        except:
            print('Invalid position! Select another position.')

    # If three symbols appears in a row, returning True
    def check_winner(self, type):
        if (self.board[0] == type and self.board[1] == type and self.board[2] == type) or \
           (self.board[3] == type and self.board[4] == type and self.board[5] == type) or \
           (self.board[6] == type and self.board[7] == type and self.board[8] == type) or \
           (self.board[0] == type and self.board[3] == type and self.board[6] == type) or \
           (self.board[1] == type and self.board[4] == type and self.board[7] == type) or \
           (self.board[2] == type and self.board[5] == type and self.board[8] == type) or \
           (self.board[0] == type and self.board[4] == type and self.board[8] == type) or \
           (self.board[2] == type and self.board[4] == type and self.board[6] == type):
            return True
        else:
            return False

    # If all fields are selected and there is no winner, it's a draw
    # Returning True if it's draw
    def check_draw(self):
        if ' ' not in self.board:
            return True
        else:
            return False

class Player:
    def __init__(self, type):
        self.type = type
        self.name = self.get_name()
    
    def get_name(self):
        if self.type == 'X':
            name = input('Player selecting X, enter your name: ')
        else:
            name = input('Player selecting O, enter your name: ')
        return name

class Game:
    def __init__(self):
        self.board = Board()

        self.player1 = Player('X')
        self.player2 = Player('O')

        self.current_player = self.player1

    # updating the play method
    def play(self):

        try:
            # using an infinite loop to run the game
            # we will later add conditions to terminate the loop
             while True:
                  message = f'{self.current_player.name}, enter the position (1 - 9): '
                  position = int(input(message))

                  # the update_board() method return True if
                  # the user selected position is not already filled
                  if self.board.update_board(position, self.current_player.type):
                      self.board.print_board()
                    
                      # checking winner each time after updating the board 
                      if self.board.check_winner(self.current_player.type):
                          print(self.current_player.name, 'wins!')
                          break

                      # checking draw each time after updating the board
                      elif self.board.check_draw():
                          print('Game is a draw!')
                          break               

                      # changing current player when board is updated 
                      else:
                          if self.current_player == self.player1:
                              self.current_player = self.player2
                          else:
                              self.current_player = self.player1 
        except:
                print('Invalid input! Enter a number between 1 and 9.') 

game = Game()
game.play()
```
