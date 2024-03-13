---
title: "CS61A 學習筆記和心得1：Python 函數式編程/遞歸"
date: "2022-10-25"
categories: 
  - "cs61a"
  - "電腦科學"
keywords: 
- 年終總結
tags: # 标签
- CS61A
- CS自學
weight:
slug: ""
draft: false # 是否为草稿
comments: true # 本页面是否显示评论
reward: true # 打赏
mermaid: true #是否开启mermaid
showToc: true # 显示目录
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
showbreadcrumbs: true #顶部显示路径
cover:
    image: "" #图片路径例如：posts/tech/123/123.png
    zoom: # 图片大小，例如填写 50% 表示原图像的一半大小
    caption: "" #图片底部描述
    alt: ""
    relative: false
---

CS61A（[Fall 2022](https://cs61a.org)）第一部分的課程在介紹函數式編程，對應課本第一章，主要包含：

- 高階函數Higher-Order Functions

- 遞歸（一直call自己循環的函數）

也簡單帶過如下內容

- Lambda( 匿名函數)

- Currying（只有一個argument的一串函數）

- Decorators（函數裝飾器，高階函數的替代）

提示：本文參考[課本](http://composingprograms.com)所寫的筆記，方便自己以後複習，舉例含有部分作業代碼，要上課的同學請不要閱讀。

## 筆記：

## 1.Environment Diagrams

第一章花了很多的時間學習Environment Diagrams。Environment Diagrams可以讓我們直觀的看到程式是如何被一步一步執行的。對於初學者，知道程式碼怎麼被執行能幫助我們寫出來自己需要的程式。

學習Environment Diagrams一靠畫圖，二靠把代碼敲進**[Python Tutor](https://pythontutor.com/visualize.html#mode=edit)**看，不是靠寫筆記。而且這篇文章總結了[Environment Diagrams](http://albertwu.org/cs61a/notes/environments)的很多細節，我就不贅述了。

下面是作業hog裡面我犯的一個錯誤，本來我helper的名字取的是original\_function，這樣就導致了重新綁定導致錯誤。

```
def make_averaged(original_function, total_samples=1000):    
    def helper(*args):
        i=0
        total=[]
        while i<total_samples:
            result=original_function(*args)
            total.append(result)
            i+=1
        return sum(total)/len(total)        
    return helper   
```

課程中我遇到的最難畫圖的HOF程式Church numerals：

```
def zero(f):
    return lambda x: x
def successor(n):
    return lambda f: lambda x: f(n(f)(x))
def one(f):
    return lambda x:f(x)
def two(f):
    return lambda x:f(f(x))
three = successor(two)
def church_to_int(n):
    return n(lambda x: x + 1)(0)
church_to_int(three)
```

## 2.HOF

高階函數Higher-Order Functions是更高一層的抽象，像硬體靠一層一層的抽象只管in和out這倆個管腳不管每個部分的實作細節一樣。像下面的Counter芯片，裡面用了5個芯片來完成，只把管腳（pin）接好就好。因為這種抽象，才可以從一個個基本的芯片門構建出龐大的計算機體系。

![](images/Screen-Shot-2022-10-25-at-5.54.53-AM-1024x682.png)

HOF就是實現軟體層面抽象的一種方法，把函數當作一般的數據類型，像整數一樣，可以作為函數的返回值和參數。

### 1\. Functions as Arguments

把函式當作參數傳入另一個函式：

```
def product(n, term):
    product, k = 1, 1
    while k <= n:
        product,k = product * term(k), k + 1
    return product

triple = lambda x: 3 * x

product(3, triple) 
```

### 2\. Return functions from other functions

在函式裡面回傳函式：

```
def outer(x):
    def inner(y):
        return x + y
    return inner

fn = outer(2)
fn(3)
```

### 3\.  Functions as General Methods

函式可以作為一種解決問題的通用方法。

這是第一章的難點，老師用了兩個例子來說明怎麼用無限接近的方法求的最大近似值。

第一個是求黃金比例（phi）的例子。

![](images/Screen-Shot-2022-10-25-at-9.12.54-AM.png)

[https://en.wikipedia.org/wiki/Golden\_ratio](https://en.wikipedia.org/wiki/Golden_ratio)

黃金比例，(a+b)/a=a/b。假設a=x,b=1，可得：

(x+1)/x=x 即x=1+1/x，就是下圖利用golden\_update(guess)這個函數來不斷的求近似值。

```
def improve(update, close, guess=1):
    while not close(guess):
        guess = update(guess)
    return guess

def golden_update(guess):
    return 1/guess + 1
	
def square_close_to_successor(guess):
    return approx_eq(guess * guess,guess + 1)
	
def approx_eq(x, y, tolerance=1e-3):
    return abs(x - y) < tolerance
	
phi = improve(golden_update,square_close_to_successor)
```

但是這裡update和 close函式只有一個參數，如果有兩個參數的話怎麼求最大近似值呢？

那就是利用高階函數的概念，把其中一個參數放在外層（比如a放在sqrt這個函數裡）。這樣裡面的函數sqrt\_update和sqrt\_close自動獲得參數a的數值。

```
def average(x, y):
    return (x + y)/2
	
def improve(update, close, guess=1):
    while not close(guess):
        guess = update(guess)
    return guess
	
def approx_eq(x, y, tolerance=1e-3):
    return abs(x - y) < tolerance

def sqrt(a):
    def sqrt_update(x):
        return average(x, a/x)
    def sqrt_close(x):
        return approx_eq(x * x, a)
    return improve(sqrt_update, sqrt_close)	

result = sqrt(256)
```

The sqrt\_update function carries with it some data: the value for a referenced in the environment in which it was defined. Because they "enclose" information in this way, locally defined functions are often called _closures_.

## 怎麼用高階函數寫程式

### 1.寫HOF的框架

比較常見的是在函數中回傳函式：

```
def outer(x):
    def inner(y):
        return x + y
    return inner
```

還可以傳遞數值進去：

```
def is_prime(n):
    def helper(i):
        if i>n**0.5:
            return True
        if n==1:
            return False
        if n%i==0:
            return False
        else:
            return helper(i+1)
    return helper(2)
        
is_prime(521)
```

HOF 傳遞一樣的argument還可以用\*args,也可以call：

```
def printed(f):
    def print_and_return(*args):
        result = f(*args)
        print('Result:', result)
        return result
    return print_and_return
printed_pow = printed(pow)
>>>printed_pow(2, 8)
Result: 256
256
>>> printed_abs = printed(abs)
>>> printed_abs(-10)
Result: 10
10
```

![](images/Screen-Shot-2022-10-20-at-11.50.44-AM-1024x616.png)

### 2.寫Recursive的框架

Recursive是一種自己call自己的HOF。

1.iteration轉為recursion

```
def fn(input):
    if <base case>:
        return ...
    else:
        return <combine input and fn(input_reduced)>
```

![](images/Screen-Shot-2022-10-09-at-6.24.37-AM-1024x428.png)

其中的關鍵在於利用return“部分結果和剩下問題”，達成計算效果。

```
if pos<10 and pos%10!=8:
    return 0
if pos%10==8:
    return 1+num_eights(pos//10)
else:
    return num_eights(pos//10)
```

![](images/Screen-Shot-2022-10-13-at-8.01.03-AM-1024x927.png)

如果沒有return，默認return：None

![](images/Screen-Shot-2022-10-11-at-12.14.31-PM-1024x461.png)

可以利用helper function 遞歸：

```
def is_prime(n):
    def helper(i):
        if i>n**0.5:
            return True
        if n==1:
            return False
        if n%i==0:
            return False
        else:
            return helper(i+1)
    return helper(2)
        
is_prime(521)
```

可以在helper中設計多個變量來控制結果，下面的這個作業利用變量step”負負得正“的方法來控制方向。

```
def num_eights(pos):
    if pos < 10 and pos % 10 != 8:
        return 0
    if pos % 10 == 8:
        return 1 + num_eights(pos//10)
    else:
        return num_eights(pos//10)   
        
def pingpong(n):
    def helper(i,step,j):
        elif i==n:
            return j
        elif  i%8==0 or num_eights(i)>0:
            return helper(i+1,-step,j-step)
        else:
            return helper(i+1,step,j+step)

    return helper(1,1,1)
pingpong(19)
```

Tree recursion：

```
def next_smaller_coin(coin):
    if coin == 25:
        return 10
    elif coin == 10:
        return 5
    elif coin == 5:
        return 1
def count_coins(change):
    def helper(largest,change):
        if change==0:
            return 1
        if change<0:
            return 0
        elif largest==1:
            return 1
        else:
            with_largest=helper(largest,change-largest)
            without_largest=helper(next_smaller_coin(largest),change)
            return with_largest+without_largest
    if change>25:
        return helper(25,change)
    elif change in range(10,25):
        return helper(10,change)
    elif change in range(5,10):
        return helper(5,change)
    elif change in range(1,5):
        return helper(1,change)
count_coins(20)
```

recursion裡包含while/for loop(例子摘自disc04)

```
def count_k(n, k):
    if n==0:
        return 1
    elif n==1:
        return 1
    elif n<0:
        return 0
    else:
        i=1
        total=0
        while i<=k:
            total=total+count_k(n-i,k)
            i+=1
        return total
    
count_k(3, 3) 
```

tree 和 recursion結合，下面這個是[disc05](https://cs61a.org/disc/sol-disc05/)的作業，比較特別的是黑體標註的地方，根據判斷返回結果。

```
def tree(label, branches=[]):
    """Construct a tree with the given label value and a list of branches."""
    return [label] + list(branches)
def label(tree):
    """Return the label value of a tree."""
    return tree[0]

def branches(tree):
    """Return the list of branches of the given tree."""
    return tree[1:]

def is_leaf(tree):
    """Returns True if the given tree's list of branches is empty, and False
    otherwise.
    """
    return not branches(tree)

def find_path(t, x):
    if label(t) == x:
        return [label(t)]
    for b in branches(t):
        path = find_path(b,x)
        if path:
            return [label(t)] + path
t = tree(3, [tree(5, [tree(1)]), tree(2)])
find_path(t, 10)
```

（筆記：在做CS61B的gitlet作業時，增加了一個recursion的例子）

```
    private static Map<String,Integer> getCommitDepthMap(Commit commit,Integer i){
        Map<String,Integer> map = new HashMap<>();
        if(!commit.havaParent1()){
            map.put(commit.getSHA1(),i);
            return map;
        }
        map.put(commit.getSHA1(),i);
        i = i + 1;
        for(Commit x:commit.getParent()){
            map.putAll(getCommitDepthMap(x,i));
        }
        return map;
    }
```

## 3.Lambda

匿名函數，不重要的函數，常常是只用一次，所以直接用一句話來代替，使得代碼簡潔。

可以當參數傳入：

```
def product(n, term):
    product, k = 1, 1
    while k <= n:
        product,k = product * term(k), k + 1
    return product
product(3, lambda x: 3 * x)
```

lambda的結果可以回傳函數：

```
def lambda_curry2(func):
    return lambda x: lambda y: func(x,y)

from operator import add, mul, mod
curried_add = lambda_curry2(add)
add_three = curried_add(3)
add_three(5)
```

## 4.Currying

Currying只是把有很多個參數的函數，變成只有一個參數的一串函數。

![](images/Screen-Shot-2022-10-06-at-11.24.40-AM.png)

```
from operator import add

curry2 = lambda f:lambda x:lambda y:f(x,y)
m=curry2(add)
m(2)(3)
```

![](images/Screen-Shot-2022-10-06-at-10.27.10-AM-1024x843.png)

from:[https://pythontutor.com](https://pythontutor.com)

參考資料：

官方筆記：[https://cs61a.org/assets/pdfs/61a-mt1-study-guide.pdf](https://cs61a.org/assets/pdfs/61a-mt1-study-guide.pdf)

## 心得：

1.理解code怎麼被執行才能知道怎麼寫（[Environment Diagrams](http://albertwu.org/cs61a/notes/environments.html)）

把所有理解有困難的code比如HOF敲進**[Python Tutor](https://pythontutor.com/visualize.html#mode=edit)**看看電腦怎麼一步一步執行code。

在寫作業的時候也可以用**[Python Tutor](https://pythontutor.com/visualize.html#mode=edit)**來觀察看看怎麼執行的，再進行修改。

2.當作業卡關的時候，先看教材（Class Material）和所有的輔助材料（每週的Resources），一開始寫HOF毫無頭緒，看了教材和輔助材料後就會寫了。

3.每個章節最後再做project，我第一章所有的作業和lab，disc都做完了，才做hog，發現沒有很難。做起來比CS50簡單多了。

4.我把作業放在github上，我的id：wxy20850606
