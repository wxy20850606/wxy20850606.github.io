---
title: "9種方法寫Fibonacci（Python）"
date: "2023-01-18"
categories: 
  - "cs61a"
  - "電腦科學"
keywords: 
- Fibonacci
tags: # 标签
- CS61A
- Python
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

受[A Python Guide to the Fibonacci Sequence](https://realpython.com/fibonacci-sequence-python/#optimizing-the-recursive-algorithm-for-the-fibonacci-sequence)啟發，我也來試試看用CS61A課程所學寫Fibonacci，感覺又好玩，又可以順便複習下課程。是的，好的課程一定要花些時間好好複習。



## Fibonacci是什麼？

Fibonacci，翻譯為**斐波那契数**。

請參考如下維基百科的定義。

![](images/Screen-Shot-2023-01-16-at-9.15.35-AM-1024x455.png)

source：[Fibonacci number](https://en.wikipedia.org/wiki/Fibonacci_number)

維基百科中文版也有不同語言版本的程式參考：

[https://zh.wikipedia.org/wiki/斐波那契数#程式參考](https://zh.wikipedia.org/wiki/斐波那契数#程式參考)

## 1.Iteration

迴圈是在CS50學到的。只有這一個是上這門課之前自己會的。

```
def fib_iter(n):
    previous, current = 0, 1
    k = 1
    while n > k:
        previous,current = current,previous+current
        k += 1
    if n == 0:
        return previous
    return current
```

```
>>> fib_iter(10)
55
>>> [fib_iter(i) for i in range(10)]
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

計算fib(300000)，共花費1秒多。

```
from datetime import datetime
start_time = datetime.now()
fib_iter(300000)
end_time = datetime.now()
print('Duration: {}'.format(end_time - start_time))

Duration: 0:00:01.171638
```

我也真夠閒，還特別丟到LeetCode看看。我的LeetCode第一題。

發現速度不錯，但是Memory成績不理想。

![](images/Screen-Shot-2023-01-18-at-10.21.43-AM-1024x264.png)

## 2.Recursion

遞歸的代碼簡單明瞭，非常好懂。

```
def fib_recur(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fib_recur(n-1) + fib_recur(n-2)
```

但是速度真慢，光是計算到fib\_recur(40)就花了快22秒。

```
>>> fib_recur(40)
end_time = datetime.now()
print('Duration: {}'.format(end_time - start_time))
102334155
>>> end_time = datetime.now()
>>> print('Duration: {}'.format(end_time - start_time))
Duration: 0:00:21.863339
```

因為普通的遞歸會產生無數的frame和大量的重複計算，所以使用尾遞歸。

丟到LeetCode發現速度果然夠慢，但是memory成績這麽好有點失真，我猜有可能是測試的數據太小導致的。

![](images/Screen-Shot-2023-01-18-at-11.04.31-AM-1024x211.png)

## 3.Tail recursion

尾調用（tail call）是返回單純原函數的recursion，和上面的遞歸返回兩個函數相加不同，尾調用只返回原函數，沒有任何多餘的其他運算。

對於有支持尾遞歸的解釋器的語言（比如Scheme）來說，在解釋器的層面會進行優化，使得空間複雜度變為O(1)。（詳情參考[CS61A 學習筆記和心得4-Tail Call](https://fulltimemammy.com/scheme-interperter-tail-call/)）

```
def fib_tail(n,a=0,b=1):
    if n == 0:
        return a
    elif n == 1:
        return b
    else:
        return fib_tail(n-1,b,a+b)
```

```
>>> fib_tail(990)
353410009178752575339944833520459068284945046358154977604109175253890696634271360121583566110064725510836075851584985143412396868586425109102723291106570618750075392710633321729992106743321640281356794177320
>>> end_time = datetime.now()
>>> print('Duration: {}'.format(end_time - start_time))
Duration: 0:00:00.001396
```

可以看到即使Python不支持尾遞歸優化，但是尾遞歸還是傳入了部分結果，比傳統的遞歸更有效率。

![](images/Screen-Shot-2023-01-18-at-11.00.59-AM-1024x228.png)

![](images/Screen-Shot-2023-01-16-at-3.45.50-PM-1024x593.png)

source：pythontutor

不過最後還是overflow了。

```
>>> fib_tail(1000)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 7, in fib_tail
  File "<stdin>", line 7, in fib_tail
  File "<stdin>", line 7, in fib_tail
  [Previous line repeated 995 more times]
  File "<stdin>", line 2, in fib_tail
RecursionError: maximum recursion depth exceeded in comparison
```

因為Python不支持尾遞歸，所以改用cache來手動存取數字避免重複計算。

## 4.Memoization/Decorator

如下程式參考自CS61A[教學視頻](https://www.youtube.com/watch?v=IB8VSP9EZQs&list=PL6BsET-8jgYWM8A8vfI5mdIocAvgxVmJt&index=3)。

這裡先設立一個memo的函數來存取cache，再用此裝飾器來修飾原來的recursion版本的fib函數。

```
def memo(f):
    cache = {}
    def memoized(n):
        if n not in cache:
            cache[n] = f(n)
        return cache[n]
    return memoized

@memo
def fib_recur(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fib_recur(n-1)+fib_recur(n-2)
```

雖然有記憶的功能，但是還是需要從大數一直計算到小數，開啟很多的frame，導致空間複雜度增加。

![](images/Screen-Shot-2023-01-16-at-7.26.30-PM-1024x744.png)

點擊[pythontutor](https://pythontutor.com/visualize.html#code=def%20memo%28f%29%3A%0A%20%20%20%20cache%20%3D%20%7B%7D%0A%20%20%20%20def%20memoized%28n%29%3A%0A%20%20%20%20%20%20%20%20if%20n%20not%20in%20cache%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20cache%5Bn%5D%20%3D%20f%28n%29%0A%20%20%20%20%20%20%20%20return%20cache%5Bn%5D%0A%20%20%20%20return%20memoized%0A%20%20%20%20%0A%40memo%0Adef%20fib_recur%28n%29%3A%0A%20%20%20%20if%20n%20%3D%3D%200%3A%0A%20%20%20%20%20%20%20%20return%200%0A%20%20%20%20elif%20n%20%3D%3D%201%3A%0A%20%20%20%20%20%20%20%20return%201%0A%20%20%20%20else%3A%0A%20%20%20%20%20%20%20%20return%20fib_recur%28n-1%29%2Bfib_recur%28n-2%29%0Afib_recur%2810%29&cumulative=false&curInstr=22&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=3&rawInputLstJSON=%5B%5D&textReferences=false)觀看

計算到fib(500)時overflow。

```
fib_recur(500)
RecursionError: maximum recursion depth exceeded
```

fib\_recur(400)所費時間：

```
fib_recur(400)
176023680645013966468226945392411250770384383304492191886725992896575345044216019675
>>> end_time = datetime.now()
>>> print('Duration: {}'.format(end_time - start_time))
Duration: 0:00:00.000881
```

不過很有意思的是，可以不斷累加call下去。好像每下一次都能夠自動獲得上一次存取的數值一樣。

```
fib_recur(300)
fib_recur(600)
fib_recur(900)
fib_recur(1200)
...
fib_recur(10000)
...
```

## 5.High Order Function

高階函數一來可以把函數當參數，二來可以函數回傳函數。

下面就用到了函數回傳函數。

利用高階函數(下面的helper)從小到大將所有比n小的fibonacci放入cache{}，避免重複計算。

```
def fib_memo(n):
    def helper(n,k,cache):
        if n < k:
            return cache[n]
        while n > k:
            k += 1
            cache[k] = cache[k-1] + cache[k-2]
        return cache[k]
    return helper(n,1,cache={0:0,1:1})
```

```
>>> fib_memo(10)
55
>>> [fib_memo(i) for i in range(10)]
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

才花了0.3秒輕鬆計算第300000個fibnocci。

```
fib_memo(300000) 
end_time = datetime.now()
print('Duration: {}'.format(end_time - start_time))
Duration: 0:00:03.162044
```

好奇查了下到底有多大，結果發現這個數字有62696個位數。

```
>>>x=fib_memo(300000)
>>>import math
>>>def countDigit(n):
...     return math.floor(math.log10(n)+1)
... 
>>> countDigit(x)
62696
```

LeetCode的分數尚可。

![](images/Screen-Shot-2023-01-18-at-8.09.27-AM-1-1024x219.png)

## 6.Generator

### 1）使用Generator獲得list

和list comprehension相比，generator不會先產生全部的數值，而是惰性求值，被要求多少產生多少，節省內存消耗。

```
def fib_gener(n):
    a,b = 0,1
    for _ in range(n):
        yield a
        a,b = b,a+b
```

也可以將generator產生的數值變成list：

```
>>> print list(fib_gener(10))
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

可以處理非常龐大的fibonacci list。

```
x=list(fib_gener(10000))
end_time = datetime.now()
print('Duration: {}'.format(end_time - start_time))
Duration: 0:00:00.482573
```

list最後一個數字有2090個位數。

```
x=list(fib_gener(10000))
>>> countDigit(x[-1])
2090
```

上面的方法只能獲得一個list，如果想要獲得第n位數的fibonacci，需要多一個步驟。

### 2）使用Generator獲得fibonacci number：

```
def fib_gener(n):
    a,b = 0,1
    for _ in range(n+1):
        yield a
        a,b = b,a+b

def fib_gener_number(n):
    return list(fib_gener(n))[-1]
```

只用了4秒鐘，就計算出了第30萬個fibnacci。

```
>>> x=fib_gener_number(300000)
end_time = datetime.now()
print('Duration: {}'.format(end_time - start_time))

>>> end_time = datetime.now()
>>> print('Duration: {}'.format(end_time - start_time))
Duration: 0:00:04.249106
```

Leetcode分數和迴圈差不多。

![](images/Screen-Shot-2023-01-18-at-10.53.37-AM-1024x223.png)

## 7.List Comprehension

List Comprehension 模板：

```
[<expression> for <element> in <sequence> if <conditional>] 
>>> [i**2 for i in [1, 2, 3, 4] if i % 2 == 0] 
[4, 16]
```

代碼如下：

```
s = [0, 1]
s += [(s := [s[1], s[0] + s[1]]) and s[1] for k in range(10)]
>>> s
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
```

參考自文章：[How can I create the fibonacci series using a list comprehension?](https://stackoverflow.com/questions/42370456/how-can-i-create-the-fibonacci-series-using-a-list-comprehension)

其中這個 **:=** 是Python3.8後才有的，叫做Assignment Expressions。

用了18秒計算出來第30萬個fibonacci.

```
start_time = datetime.now()
s = [0, 1]
s += [(s := [s[1], s[0] + s[1]]) and s[1] for k in range(300000)]
fib_lis = s[-3]
print(fib_lis)
end_time = datetime.now()
print('Duration: {}'.format(end_time - start_time))
Duration: 0:00:17.897750
```

花了點時間弄懂，解釋如下：

```
#賦值
(s := [s[1], s[0] + s[1]]) 

#s := [s[1], s[0] + s[1]]的目的在於從初始動[0,1]滾動變為[1,1]，然後[2, 3]等等。
>>> s=[0,1]
>>> s += [(s := [s[1], s[0] + s[1]]) for i in range(3)]
>>> s
[0, 1, [1, 1], [1, 2], [2, 3]]

#and的意思是取s的第二個數值
>>>s=[1,2]
>>> s and s[1]
2
```

其他參考文章：[How To Use Assignment Expressions in Python](https://www.digitalocean.com/community/tutorials/how-to-use-assignment-expressions-in-python)

## 8.Reduce and Lambda

Python中的reduce是來自於funtools模塊，借鑑自函數式編程。

[https://docs.python.org/3/library/functools.html](https://docs.python.org/3/library/functools.html)

```
functools.reduce(function, iterable[, initializer])
```

代碼受如下這篇文章啟發：

[Python | Find fibonacci series upto n using lambda](https://www.digitalocean.com/community/tutorials/how-to-use-assignment-expressions-in-python)

```
from functools import reduce

def fib_reduce(n):
    if n < 2:
        return n
    lst = reduce(lambda x,y:x+[x[-1]+x[-2]], range(n - 1), [0,1])
    return lst[-1]
```

解釋：

一般的reduce運作方式如下，如下的range(5)是迭代器（iterator），此處的迭代器也參與運算。

```
>>> reduce(lambda x,y:x+y,range(5),100)
55
執行的順序如下：((((100+1)+2)+3)+4)
>>> reduce(lambda x,y:x+[y],range(5),[100])
[100, 0, 1, 2, 3, 4]
```

下面的y對應從迭代器range(10,20)裡一直迭代的值在lambda裡沒有起到計算的作用，但是起到了類似迴圈的作用。

```
>>> reduce(lambda x,y:x+[x[-1]+x[-2]],range(10,20),[1,2,3])
[1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377]
```

下面的y即range(10,20)就參與運算了。

```
>>> reduce(lambda x,y:x+[y],range(10,20),[1,2,3])
[1, 2, 3, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19]
```

為什麼可以產生類似迴圈的特性呢？

是因為上面兩個的起始值都是list，list是一個pointer，不是object本身，只是一個地址。所以reduce函數起到了不斷往這個地址裡放東西的作用，就累積成有點迴圈的感覺。

解釋完畢，最後看下效能。

因為是先生成list的關係，所以處理到第50000個fibonacci就花了4.8秒。

```
fib_reduce(50000)
Duration: 0:00:04.815144
```

LeetCode成績也普通。

![](images/Screen-Shot-2023-01-18-at-10.47.27-AM-1024x174.png)

## 9.Map and Lambda

代碼參考自：[Python | Find fibonacci series upto n using lambda](https://www.geeksforgeeks.org/python-find-fibonacci-series-upto-n-using-lambda/)

```
def fib_map(n):
    lst=[0,1]
    any(map(lambda _:lst.append(sum(lst[-2:])),range(2,n)))
    return lst[-1]
```

這麽高深的寫法我自己肯定想破頭也想不出。

解釋：

- map因為是iterator，所以只會運行一次，用any讓程式可以一直運行直到超出range。

- \_表示不用給這個lambda名字，也可以給任意名字比如lambda x:lst.append(sum(lst\[-2:\]))。

```
>>>lst=[1,2]
>>>x=map(lambda x:lst.append(3),range(2))
>>> x
<map object at 0x100f731f0>
>>> next(x)
>>> lst
[1, 2, 3]
>>> next(x)
>>> lst
[1, 2, 3, 3]
>>> next(x)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

重新翻看了下[課程視頻](https://www.youtube.com/watch?v=bYtkZGU4h_E&list=PL6BsET-8jgYWtQwrnaluW2W6NdCuuHEW2&index=5)，發現可以用list來迫使所有的iterator運行完畢。

所以修改以上程式如下：

```
def fib_map(n):
    lst=[0,1]
    if n < 2:
        return n
    list(map(lambda x:lst.append(sum(lst[-2:])),range(1,n)))
    return lst[-1]
```

使用list版本的fib\_map運行時間為4秒。

```
>>>fib_map(300000)
0:00:04.120279
```

![](images/Screen-Shot-2023-01-18-at-10.41.38-AM-1024x231.png)

## 總結：

CS61A之前我連fibonacci是什麼都不知道，上完課，現在我可以看得懂這麼多種思路寫的fibonacci。非常有成就感。

感謝老師最好的方式就是自己好好學習，再推薦更多的人來學習。

希望有更多的人來學習這麽課程。最後再次強烈推薦CS61A。
