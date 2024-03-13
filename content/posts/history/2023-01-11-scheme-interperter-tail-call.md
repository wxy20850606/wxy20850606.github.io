---
title: "CS61A 學習筆記和心得4-Tail Call"
date: "2023-01-11"
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

注：本文部分引用代碼來自CS61A(Structure and Interpretation of Computer Programs) Scheme作業，請小心劇透。個人學習筆記，請小心參考。

上一篇筆記提到了**Read-Eval-Print-Loop**，這篇筆記記錄其中的附加題-tail call optimization。

## 尾調用（tail call）

> 在[计算机学](https://zh.wikipedia.org/wiki/%E8%A8%88%E7%AE%97%E6%A9%9F%E5%AD%B8)裡，**尾调用**（tail call）是指一个函数里的最后一个动作是返回一个[函数](https://zh.wikipedia.org/wiki/%E5%AD%90%E7%A8%8B%E5%BA%8F)的调用结果的情形，即最后一步新调用的返回值直接被当前函数的返回结果。
> 
> 維基百科

我的理解就是，和一般遞歸返回值有可能還需要一些運算不同，尾遞歸直接return原函數。

函數式編程靠的是遞歸，而不是循環。因為imutable（變量不可變）的特性，不會使用for/while loop來改變變量數值。但是使用遞歸會導致開啟無數個frame，使得空間複雜度是O(n)。

使用尾遞歸優化就是來解決因為函數式編程imutable的特性不能使用循環而只能使用遞歸導致的問題，把空間複雜度從O(n)降為O(1)。

一開始很難理解tail call，因為下面的符合tail call的factorial還是開了很多的frame。

![](images/Screen-Shot-2023-01-09-at-8.06.14-PM-1024x644.png)

最後用@trace讀了一下scheme怎麼一步一步運行factorial的，才最終明白。

```
@trace
def scheme_eval(expr, env, _=None):
```

原來tall call的優化是通過解釋器的層面來實現的。

具體是直接關閉上一個frame，使用上一個frame的參數重新開啟一個frame，一直循環下去直到函數有返回值。這樣可以保證從始至終都只有一個frame，實現空間複雜度為O(1)。

```

class Unevaluated:
    """An expression and an environment in which it is to be evaluated."""

    def __init__(self, expr, env):
        """Expression EXPR to be evaluated in Frame ENV."""
        self.expr = expr
        self.env = env

def optimize_tail_calls(unoptimized_scheme_eval):
    """Return a properly tail recursive version of an eval function."""
    def optimized_eval(expr, env, tail=False):
        """Evaluate Scheme expression EXPR in Frame ENV. If TAIL,
        return an Unevaluated containing an expression for further evaluation.
        """
        if tail and not scheme_symbolp(expr) and not self_evaluating(expr):
            return Unevaluated(expr, env)

        result = Unevaluated(expr, env)

        # BEGIN PROBLEM EC
        "*** YOUR CODE HERE ***"
        while (isinstance(result,Unevaluated)):
            result = unoptimized_scheme_eval(result.expr, result.env)
        return result
        # END PROBLEM EC
    return optimized_eval
```

上面的代碼比較抽象，可以用一個具體的例子幫忙理解：

同樣是tail call的如下程式：

```
def factorial(n, a):
    if n == 0:
        return a
    else:
        return factorial(n - 1, n * a)
factorial(3,1)
```

對於沒有支持尾遞歸優化的python來說，**factorial(3,1)**的運算步驟是：

![](images/Screen-Shot-2023-01-09-at-9.30.47-PM.png)

但是仔細看一下發現，其實從factorial(3,1)=factorial(2,3)=factorial(1,6)，也就是從factorial(3,1)到factorial(2,3)，參數已經傳入了，根本不需要保留原本factorial(3,1)的frame了。

所以對於同樣邏輯的Scheme程式：

```
(define (factorial n k)
  (if (= n 0) k
    (factorial (- n 1)
               (* k n))))
```

因為Scheme支持尾遞歸優化，運算方式是這樣的：

- factorial(3,1)不能得到直接的結果，但factorial(3,1)=factorial(2,3)，factorial(3,1)的frame已經無用，所以關閉frame，返回factorial(2,3)，即開啟一個factorial(2,3)的frame，開始循環（while loop）

- factorial(2,3)也不能得到直接結果，factorial(2,3)=factorial(1,6)，關閉factorial(2,3)的frame，返回factorial(1,6)，即開啟factorial(1,6)的frame

- factorial(1,6)也無法返回直接結果，factorial(1,6)=factorial(0,6)，所以關閉factorial(1,6)的frame返回factorial(0,6)，開啟factorial(0,6)的frame

- factorial(0,6)可以直接返回結果，循環結束

以上，通過關閉上一層的frame然後再開下一層的frame，從始至終都只有一個frame。才可以把原本的tail call的空間複雜度為O(n)變為O(1).

對於Scheme程式而言，如下尾位置（tail context）地方的call才有可能是tail call：

![](images/Screen-Shot-2023-01-03-at-4.12.56-PM-1024x356.png)

from CS61A

而普通的遞歸，因為返回的不僅僅是函數本身，還需要有進一步的相乘，所以無法使用Scheme解釋器自帶的優化功能。仔細讀了@trace返回的運算流程發現，還是有調用尾遞歸優化，但是因為需要返回乘積，所以無法完全關閉frame。

```
(define (factorial n)
  (if (= n 0) 1
    (* n (factorial (- n 1)))))
```

當我有點懂了尾遞歸優化後，又發現了下面這些不錯的資源：

[尾递归为啥能优化？](https://zhuanlan.zhihu.com/p/36587160)

[Tail Recursion (video)](https://www.coursera.org/lecture/programming-languages/tail-recursion-YZic1)

附註：

用有尾遞歸優化版本的解釋器來運行tail call 版本的(factorial 3 1)：

```
scm>(define (factorial n k)
  (if (= n 0) k
    (factorial (- n 1)
               (* k n))))

scm>(factorial 3 1)

scheme_eval(Pair('factorial', Pair(3, Pair(1, nil))), <Global Frame>):

scheme_eval(Pair('factorial', Pair(3, Pair(1, nil))), <Global Frame>) -> <scheme_eval_apply.Unevaluated object at 0x101547f40>
scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 1, n: 3} -> <Global Frame>>):

scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 1, n: 3} -> <Global Frame>>) -> <scheme_eval_apply.Unevaluated object at 0x101547010>
scheme_eval(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), <{k: 1, n: 3} -> <Global Frame>>):

scheme_eval(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), <{k: 1, n: 3} -> <Global Frame>>) -> <scheme_eval_apply.Unevaluated object at 0x101547df0>
scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 3, n: 2} -> <Global Frame>>):

scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 3, n: 2} -> <Global Frame>>) -> <scheme_eval_apply.Unevaluated object at 0x1015470d0>
scheme_eval(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), <{k: 3, n: 2} -> <Global Frame>>):

scheme_eval(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), <{k: 3, n: 2} -> <Global Frame>>) -> <scheme_eval_apply.Unevaluated object at 0x101547f10>
scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 6, n: 1} -> <Global Frame>>):

scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 6, n: 1} -> <Global Frame>>) -> <scheme_eval_apply.Unevaluated object at 0x101547f40>
scheme_eval(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), <{k: 6, n: 1} -> <Global Frame>>):

scheme_eval(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), <{k: 6, n: 1} -> <Global Frame>>) -> <scheme_eval_apply.Unevaluated object at 0x101547010>
scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 6, n: 0} -> <Global Frame>>):

scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 6, n: 0} -> <Global Frame>>) -> 6
6
```

用有尾遞歸優化版本的解釋器來運行一般遞歸**非tail call** 版本的(factorial 3 1)：

可以發現有些地方還是返回了<scheme\_eval\_apply.Unevaluated object at 0x100a79090>。這表示解釋器還是儘量在做優化，但是因為不是尾遞歸，可以優化的有限。

```
scm>(define (factorial n)
  (if (= n 0) 1
    (* n (factorial (- n 1)))))

scm> (factorial 3 1)
scheme_eval(Pair('factorial', Pair(3, Pair(1, nil))), <Global Frame>):
    scheme_eval('factorial', <Global Frame>):
    scheme_eval('factorial', <Global Frame>) -> (lambda (n k) (if (= n 0) k (factorial (- n 1) (* k n))))
    scheme_eval(3, <Global Frame>):
    scheme_eval(3, <Global Frame>) -> 3
    scheme_eval(1, <Global Frame>):
    scheme_eval(1, <Global Frame>) -> 1
scheme_eval(Pair('factorial', Pair(3, Pair(1, nil))), <Global Frame>) -> <scheme_eval_apply.Unevaluated object at 0x100a79090>
scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 1, n: 3} -> <Global Frame>>):
    scheme_eval(Pair('=', Pair('n', Pair(0, nil))), <{k: 1, n: 3} -> <Global Frame>>):
        scheme_eval('=', <{k: 1, n: 3} -> <Global Frame>>):
        scheme_eval('=', <{k: 1, n: 3} -> <Global Frame>>) -> #[=]
        scheme_eval('n', <{k: 1, n: 3} -> <Global Frame>>):
        scheme_eval('n', <{k: 1, n: 3} -> <Global Frame>>) -> 3
        scheme_eval(0, <{k: 1, n: 3} -> <Global Frame>>):
        scheme_eval(0, <{k: 1, n: 3} -> <Global Frame>>) -> 0
    scheme_eval(Pair('=', Pair('n', Pair(0, nil))), <{k: 1, n: 3} -> <Global Frame>>) -> False
scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 1, n: 3} -> <Global Frame>>) -> <scheme_eval_apply.Unevaluated object at 0x100a78a90>
scheme_eval(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), <{k: 1, n: 3} -> <Global Frame>>):
    scheme_eval('factorial', <{k: 1, n: 3} -> <Global Frame>>):
    scheme_eval('factorial', <{k: 1, n: 3} -> <Global Frame>>) -> (lambda (n k) (if (= n 0) k (factorial (- n 1) (* k n))))
    scheme_eval(Pair('-', Pair('n', Pair(1, nil))), <{k: 1, n: 3} -> <Global Frame>>):
        scheme_eval('-', <{k: 1, n: 3} -> <Global Frame>>):
        scheme_eval('-', <{k: 1, n: 3} -> <Global Frame>>) -> #[-]
        scheme_eval('n', <{k: 1, n: 3} -> <Global Frame>>):
        scheme_eval('n', <{k: 1, n: 3} -> <Global Frame>>) -> 3
        scheme_eval(1, <{k: 1, n: 3} -> <Global Frame>>):
        scheme_eval(1, <{k: 1, n: 3} -> <Global Frame>>) -> 1
    scheme_eval(Pair('-', Pair('n', Pair(1, nil))), <{k: 1, n: 3} -> <Global Frame>>) -> 2
    scheme_eval(Pair('*', Pair('k', Pair('n', nil))), <{k: 1, n: 3} -> <Global Frame>>):
        scheme_eval('*', <{k: 1, n: 3} -> <Global Frame>>):
        scheme_eval('*', <{k: 1, n: 3} -> <Global Frame>>) -> #[*]
        scheme_eval('k', <{k: 1, n: 3} -> <Global Frame>>):
        scheme_eval('k', <{k: 1, n: 3} -> <Global Frame>>) -> 1
        scheme_eval('n', <{k: 1, n: 3} -> <Global Frame>>):
        scheme_eval('n', <{k: 1, n: 3} -> <Global Frame>>) -> 3
    scheme_eval(Pair('*', Pair('k', Pair('n', nil))), <{k: 1, n: 3} -> <Global Frame>>) -> 3
scheme_eval(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), <{k: 1, n: 3} -> <Global Frame>>) -> <scheme_eval_apply.Unevaluated object at 0x100a78cd0>
scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 3, n: 2} -> <Global Frame>>):
    scheme_eval(Pair('=', Pair('n', Pair(0, nil))), <{k: 3, n: 2} -> <Global Frame>>):
        scheme_eval('=', <{k: 3, n: 2} -> <Global Frame>>):
        scheme_eval('=', <{k: 3, n: 2} -> <Global Frame>>) -> #[=]
        scheme_eval('n', <{k: 3, n: 2} -> <Global Frame>>):
        scheme_eval('n', <{k: 3, n: 2} -> <Global Frame>>) -> 2
        scheme_eval(0, <{k: 3, n: 2} -> <Global Frame>>):
        scheme_eval(0, <{k: 3, n: 2} -> <Global Frame>>) -> 0
    scheme_eval(Pair('=', Pair('n', Pair(0, nil))), <{k: 3, n: 2} -> <Global Frame>>) -> False
scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 3, n: 2} -> <Global Frame>>) -> <scheme_eval_apply.Unevaluated object at 0x100a78b20>
scheme_eval(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), <{k: 3, n: 2} -> <Global Frame>>):
    scheme_eval('factorial', <{k: 3, n: 2} -> <Global Frame>>):
    scheme_eval('factorial', <{k: 3, n: 2} -> <Global Frame>>) -> (lambda (n k) (if (= n 0) k (factorial (- n 1) (* k n))))
    scheme_eval(Pair('-', Pair('n', Pair(1, nil))), <{k: 3, n: 2} -> <Global Frame>>):
        scheme_eval('-', <{k: 3, n: 2} -> <Global Frame>>):
        scheme_eval('-', <{k: 3, n: 2} -> <Global Frame>>) -> #[-]
        scheme_eval('n', <{k: 3, n: 2} -> <Global Frame>>):
        scheme_eval('n', <{k: 3, n: 2} -> <Global Frame>>) -> 2
        scheme_eval(1, <{k: 3, n: 2} -> <Global Frame>>):
        scheme_eval(1, <{k: 3, n: 2} -> <Global Frame>>) -> 1
    scheme_eval(Pair('-', Pair('n', Pair(1, nil))), <{k: 3, n: 2} -> <Global Frame>>) -> 1
    scheme_eval(Pair('*', Pair('k', Pair('n', nil))), <{k: 3, n: 2} -> <Global Frame>>):
        scheme_eval('*', <{k: 3, n: 2} -> <Global Frame>>):
        scheme_eval('*', <{k: 3, n: 2} -> <Global Frame>>) -> #[*]
        scheme_eval('k', <{k: 3, n: 2} -> <Global Frame>>):
        scheme_eval('k', <{k: 3, n: 2} -> <Global Frame>>) -> 3
        scheme_eval('n', <{k: 3, n: 2} -> <Global Frame>>):
        scheme_eval('n', <{k: 3, n: 2} -> <Global Frame>>) -> 2
    scheme_eval(Pair('*', Pair('k', Pair('n', nil))), <{k: 3, n: 2} -> <Global Frame>>) -> 6
scheme_eval(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), <{k: 3, n: 2} -> <Global Frame>>) -> <scheme_eval_apply.Unevaluated object at 0x100a78fa0>
scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 6, n: 1} -> <Global Frame>>):
    scheme_eval(Pair('=', Pair('n', Pair(0, nil))), <{k: 6, n: 1} -> <Global Frame>>):
        scheme_eval('=', <{k: 6, n: 1} -> <Global Frame>>):
        scheme_eval('=', <{k: 6, n: 1} -> <Global Frame>>) -> #[=]
        scheme_eval('n', <{k: 6, n: 1} -> <Global Frame>>):
        scheme_eval('n', <{k: 6, n: 1} -> <Global Frame>>) -> 1
        scheme_eval(0, <{k: 6, n: 1} -> <Global Frame>>):
        scheme_eval(0, <{k: 6, n: 1} -> <Global Frame>>) -> 0
    scheme_eval(Pair('=', Pair('n', Pair(0, nil))), <{k: 6, n: 1} -> <Global Frame>>) -> False
scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 6, n: 1} -> <Global Frame>>) -> <scheme_eval_apply.Unevaluated object at 0x100a7b280>
scheme_eval(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), <{k: 6, n: 1} -> <Global Frame>>):
    scheme_eval('factorial', <{k: 6, n: 1} -> <Global Frame>>):
    scheme_eval('factorial', <{k: 6, n: 1} -> <Global Frame>>) -> (lambda (n k) (if (= n 0) k (factorial (- n 1) (* k n))))
    scheme_eval(Pair('-', Pair('n', Pair(1, nil))), <{k: 6, n: 1} -> <Global Frame>>):
        scheme_eval('-', <{k: 6, n: 1} -> <Global Frame>>):
        scheme_eval('-', <{k: 6, n: 1} -> <Global Frame>>) -> #[-]
        scheme_eval('n', <{k: 6, n: 1} -> <Global Frame>>):
        scheme_eval('n', <{k: 6, n: 1} -> <Global Frame>>) -> 1
        scheme_eval(1, <{k: 6, n: 1} -> <Global Frame>>):
        scheme_eval(1, <{k: 6, n: 1} -> <Global Frame>>) -> 1
    scheme_eval(Pair('-', Pair('n', Pair(1, nil))), <{k: 6, n: 1} -> <Global Frame>>) -> 0
    scheme_eval(Pair('*', Pair('k', Pair('n', nil))), <{k: 6, n: 1} -> <Global Frame>>):
        scheme_eval('*', <{k: 6, n: 1} -> <Global Frame>>):
        scheme_eval('*', <{k: 6, n: 1} -> <Global Frame>>) -> #[*]
        scheme_eval('k', <{k: 6, n: 1} -> <Global Frame>>):
        scheme_eval('k', <{k: 6, n: 1} -> <Global Frame>>) -> 6
        scheme_eval('n', <{k: 6, n: 1} -> <Global Frame>>):
        scheme_eval('n', <{k: 6, n: 1} -> <Global Frame>>) -> 1
    scheme_eval(Pair('*', Pair('k', Pair('n', nil))), <{k: 6, n: 1} -> <Global Frame>>) -> 6
scheme_eval(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), <{k: 6, n: 1} -> <Global Frame>>) -> <scheme_eval_apply.Unevaluated object at 0x100a78d90>
scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 6, n: 0} -> <Global Frame>>):
    scheme_eval(Pair('=', Pair('n', Pair(0, nil))), <{k: 6, n: 0} -> <Global Frame>>):
        scheme_eval('=', <{k: 6, n: 0} -> <Global Frame>>):
        scheme_eval('=', <{k: 6, n: 0} -> <Global Frame>>) -> #[=]
        scheme_eval('n', <{k: 6, n: 0} -> <Global Frame>>):
        scheme_eval('n', <{k: 6, n: 0} -> <Global Frame>>) -> 0
        scheme_eval(0, <{k: 6, n: 0} -> <Global Frame>>):
        scheme_eval(0, <{k: 6, n: 0} -> <Global Frame>>) -> 0
    scheme_eval(Pair('=', Pair('n', Pair(0, nil))), <{k: 6, n: 0} -> <Global Frame>>) -> True
    scheme_eval('k', <{k: 6, n: 0} -> <Global Frame>>):
    scheme_eval('k', <{k: 6, n: 0} -> <Global Frame>>) -> 6
scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 6, n: 0} -> <Global Frame>>) -> 6
6
```

如下為解釋器沒有提供尾遞歸優化版本：

如同Python一樣，即使程式本身是tail call，但是解釋器沒有支持，還是會開啟無數個frame，導致空間複雜度為O(n)。

```
scm>(define (factorial n k)
  (if (= n 0) k
    (factorial (- n 1)
               (* k n))))
scm> (factorial 3 1)

scheme_eval(Pair('factorial', Pair(3, Pair(1, nil))), <Global Frame>):
    scheme_eval('factorial', <Global Frame>):
    scheme_eval('factorial', <Global Frame>) -> (lambda (n k) (if (= n 0) k (factorial (- n 1) (* k n))))
    scheme_eval(3, <Global Frame>):
    scheme_eval(3, <Global Frame>) -> 3
    scheme_eval(1, <Global Frame>):
    scheme_eval(1, <Global Frame>) -> 1
    scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 1, n: 3} -> <Global Frame>>, True):
        scheme_eval(Pair('=', Pair('n', Pair(0, nil))), <{k: 1, n: 3} -> <Global Frame>>):
            scheme_eval('=', <{k: 1, n: 3} -> <Global Frame>>):
            scheme_eval('=', <{k: 1, n: 3} -> <Global Frame>>) -> #[=]
            scheme_eval('n', <{k: 1, n: 3} -> <Global Frame>>):
            scheme_eval('n', <{k: 1, n: 3} -> <Global Frame>>) -> 3
            scheme_eval(0, <{k: 1, n: 3} -> <Global Frame>>):
            scheme_eval(0, <{k: 1, n: 3} -> <Global Frame>>) -> 0
        scheme_eval(Pair('=', Pair('n', Pair(0, nil))), <{k: 1, n: 3} -> <Global Frame>>) -> False
        scheme_eval(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), <{k: 1, n: 3} -> <Global Frame>>, True):
            scheme_eval('factorial', <{k: 1, n: 3} -> <Global Frame>>):
            scheme_eval('factorial', <{k: 1, n: 3} -> <Global Frame>>) -> (lambda (n k) (if (= n 0) k (factorial (- n 1) (* k n))))
            scheme_eval(Pair('-', Pair('n', Pair(1, nil))), <{k: 1, n: 3} -> <Global Frame>>):
                scheme_eval('-', <{k: 1, n: 3} -> <Global Frame>>):
                scheme_eval('-', <{k: 1, n: 3} -> <Global Frame>>) -> #[-]
                scheme_eval('n', <{k: 1, n: 3} -> <Global Frame>>):
                scheme_eval('n', <{k: 1, n: 3} -> <Global Frame>>) -> 3
                scheme_eval(1, <{k: 1, n: 3} -> <Global Frame>>):
                scheme_eval(1, <{k: 1, n: 3} -> <Global Frame>>) -> 1
            scheme_eval(Pair('-', Pair('n', Pair(1, nil))), <{k: 1, n: 3} -> <Global Frame>>) -> 2
            scheme_eval(Pair('*', Pair('k', Pair('n', nil))), <{k: 1, n: 3} -> <Global Frame>>):
                scheme_eval('*', <{k: 1, n: 3} -> <Global Frame>>):
                scheme_eval('*', <{k: 1, n: 3} -> <Global Frame>>) -> #[*]
                scheme_eval('k', <{k: 1, n: 3} -> <Global Frame>>):
                scheme_eval('k', <{k: 1, n: 3} -> <Global Frame>>) -> 1
                scheme_eval('n', <{k: 1, n: 3} -> <Global Frame>>):
                scheme_eval('n', <{k: 1, n: 3} -> <Global Frame>>) -> 3
            scheme_eval(Pair('*', Pair('k', Pair('n', nil))), <{k: 1, n: 3} -> <Global Frame>>) -> 3
            scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 3, n: 2} -> <Global Frame>>, True):
                scheme_eval(Pair('=', Pair('n', Pair(0, nil))), <{k: 3, n: 2} -> <Global Frame>>):
                    scheme_eval('=', <{k: 3, n: 2} -> <Global Frame>>):
                    scheme_eval('=', <{k: 3, n: 2} -> <Global Frame>>) -> #[=]
                    scheme_eval('n', <{k: 3, n: 2} -> <Global Frame>>):
                    scheme_eval('n', <{k: 3, n: 2} -> <Global Frame>>) -> 2
                    scheme_eval(0, <{k: 3, n: 2} -> <Global Frame>>):
                    scheme_eval(0, <{k: 3, n: 2} -> <Global Frame>>) -> 0
                scheme_eval(Pair('=', Pair('n', Pair(0, nil))), <{k: 3, n: 2} -> <Global Frame>>) -> False
                scheme_eval(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), <{k: 3, n: 2} -> <Global Frame>>, True):
                    scheme_eval('factorial', <{k: 3, n: 2} -> <Global Frame>>):
                    scheme_eval('factorial', <{k: 3, n: 2} -> <Global Frame>>) -> (lambda (n k) (if (= n 0) k (factorial (- n 1) (* k n))))
                    scheme_eval(Pair('-', Pair('n', Pair(1, nil))), <{k: 3, n: 2} -> <Global Frame>>):
                        scheme_eval('-', <{k: 3, n: 2} -> <Global Frame>>):
                        scheme_eval('-', <{k: 3, n: 2} -> <Global Frame>>) -> #[-]
                        scheme_eval('n', <{k: 3, n: 2} -> <Global Frame>>):
                        scheme_eval('n', <{k: 3, n: 2} -> <Global Frame>>) -> 2
                        scheme_eval(1, <{k: 3, n: 2} -> <Global Frame>>):
                        scheme_eval(1, <{k: 3, n: 2} -> <Global Frame>>) -> 1
                    scheme_eval(Pair('-', Pair('n', Pair(1, nil))), <{k: 3, n: 2} -> <Global Frame>>) -> 1
                    scheme_eval(Pair('*', Pair('k', Pair('n', nil))), <{k: 3, n: 2} -> <Global Frame>>):
                        scheme_eval('*', <{k: 3, n: 2} -> <Global Frame>>):
                        scheme_eval('*', <{k: 3, n: 2} -> <Global Frame>>) -> #[*]
                        scheme_eval('k', <{k: 3, n: 2} -> <Global Frame>>):
                        scheme_eval('k', <{k: 3, n: 2} -> <Global Frame>>) -> 3
                        scheme_eval('n', <{k: 3, n: 2} -> <Global Frame>>):
                        scheme_eval('n', <{k: 3, n: 2} -> <Global Frame>>) -> 2
                    scheme_eval(Pair('*', Pair('k', Pair('n', nil))), <{k: 3, n: 2} -> <Global Frame>>) -> 6
                    scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 6, n: 1} -> <Global Frame>>, True):
                        scheme_eval(Pair('=', Pair('n', Pair(0, nil))), <{k: 6, n: 1} -> <Global Frame>>):
                            scheme_eval('=', <{k: 6, n: 1} -> <Global Frame>>):
                            scheme_eval('=', <{k: 6, n: 1} -> <Global Frame>>) -> #[=]
                            scheme_eval('n', <{k: 6, n: 1} -> <Global Frame>>):
                            scheme_eval('n', <{k: 6, n: 1} -> <Global Frame>>) -> 1
                            scheme_eval(0, <{k: 6, n: 1} -> <Global Frame>>):
                            scheme_eval(0, <{k: 6, n: 1} -> <Global Frame>>) -> 0
                        scheme_eval(Pair('=', Pair('n', Pair(0, nil))), <{k: 6, n: 1} -> <Global Frame>>) -> False
                        scheme_eval(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), <{k: 6, n: 1} -> <Global Frame>>, True):
                            scheme_eval('factorial', <{k: 6, n: 1} -> <Global Frame>>):
                            scheme_eval('factorial', <{k: 6, n: 1} -> <Global Frame>>) -> (lambda (n k) (if (= n 0) k (factorial (- n 1) (* k n))))
                            scheme_eval(Pair('-', Pair('n', Pair(1, nil))), <{k: 6, n: 1} -> <Global Frame>>):
                                scheme_eval('-', <{k: 6, n: 1} -> <Global Frame>>):
                                scheme_eval('-', <{k: 6, n: 1} -> <Global Frame>>) -> #[-]
                                scheme_eval('n', <{k: 6, n: 1} -> <Global Frame>>):
                                scheme_eval('n', <{k: 6, n: 1} -> <Global Frame>>) -> 1
                                scheme_eval(1, <{k: 6, n: 1} -> <Global Frame>>):
                                scheme_eval(1, <{k: 6, n: 1} -> <Global Frame>>) -> 1
                            scheme_eval(Pair('-', Pair('n', Pair(1, nil))), <{k: 6, n: 1} -> <Global Frame>>) -> 0
                            scheme_eval(Pair('*', Pair('k', Pair('n', nil))), <{k: 6, n: 1} -> <Global Frame>>):
                                scheme_eval('*', <{k: 6, n: 1} -> <Global Frame>>):
                                scheme_eval('*', <{k: 6, n: 1} -> <Global Frame>>) -> #[*]
                                scheme_eval('k', <{k: 6, n: 1} -> <Global Frame>>):
                                scheme_eval('k', <{k: 6, n: 1} -> <Global Frame>>) -> 6
                                scheme_eval('n', <{k: 6, n: 1} -> <Global Frame>>):
                                scheme_eval('n', <{k: 6, n: 1} -> <Global Frame>>) -> 1
                            scheme_eval(Pair('*', Pair('k', Pair('n', nil))), <{k: 6, n: 1} -> <Global Frame>>) -> 6
                            scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 6, n: 0} -> <Global Frame>>, True):
                                scheme_eval(Pair('=', Pair('n', Pair(0, nil))), <{k: 6, n: 0} -> <Global Frame>>):
                                    scheme_eval('=', <{k: 6, n: 0} -> <Global Frame>>):
                                    scheme_eval('=', <{k: 6, n: 0} -> <Global Frame>>) -> #[=]
                                    scheme_eval('n', <{k: 6, n: 0} -> <Global Frame>>):
                                    scheme_eval('n', <{k: 6, n: 0} -> <Global Frame>>) -> 0
                                    scheme_eval(0, <{k: 6, n: 0} -> <Global Frame>>):
                                    scheme_eval(0, <{k: 6, n: 0} -> <Global Frame>>) -> 0
                                scheme_eval(Pair('=', Pair('n', Pair(0, nil))), <{k: 6, n: 0} -> <Global Frame>>) -> True
                                scheme_eval('k', <{k: 6, n: 0} -> <Global Frame>>, True):
                                scheme_eval('k', <{k: 6, n: 0} -> <Global Frame>>, True) -> 6
                            scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 6, n: 0} -> <Global Frame>>, True) -> 6
                        scheme_eval(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), <{k: 6, n: 1} -> <Global Frame>>, True) -> 6
                    scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 6, n: 1} -> <Global Frame>>, True) -> 6
                scheme_eval(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), <{k: 3, n: 2} -> <Global Frame>>, True) -> 6
            scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 3, n: 2} -> <Global Frame>>, True) -> 6
        scheme_eval(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), <{k: 1, n: 3} -> <Global Frame>>, True) -> 6
    scheme_eval(Pair('if', Pair(Pair('=', Pair('n', Pair(0, nil))), Pair('k', Pair(Pair('factorial', Pair(Pair('-', Pair('n', Pair(1, nil))), Pair(Pair('*', Pair('k', Pair('n', nil))), nil))), nil)))), <{k: 1, n: 3} -> <Global Frame>>, True) -> 6
scheme_eval(Pair('factorial', Pair(3, Pair(1, nil))), <Global Frame>) -> 6
6
```
