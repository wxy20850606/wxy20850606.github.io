---
title: "CS61A 學習筆記和心得3-通過學習用Python寫個Scheme Interpreter來學習編程語言"
date: "2023-01-05"
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

注：本文引用代碼來自CS61A(Structure and Interpretation of Computer Programs) Scheme作業，請小心劇透。個人學習筆記，水平有限，請小心參考。

心得：

本來我想學Scheme版本的CS61A，弄了很久環境都沒有設置成功，然後轉學Python版本的CS61A。

沒想到課程第三章是先用一個python寫的解釋器學Scheme，不用下載任何東西。然後作業是自己一步一步寫一個Scheme語言的解釋器，每完成一步還可以自己運行看看。完成作業後，我們自然了解到這個解釋器的程式也就這樣而已嘛，不難懂啊，沒什麼神秘的。

通過這個作業，除了讓我們了解**編程語言本質上就是一個程式**，還知道了這個程式大概是怎麼寫出來的，加深理解。

呼應Nand2tetris課程裡的說法，想要了解計算機，就自己造一台。

想要了解編程語言，就自己寫個編譯器或解釋器，創造一個語言。

另一方面，這個解釋器的程式幾乎涵蓋了課程所學的所有的主要內容，比如：Environment Frame/函數式編程/遞歸/Tree/OOP/Linked List/Iterator/空間複雜度等，還有新的尾遞歸。把這所有的概念融合到一個程式裡面，最終形成一個通用的可以運行各種程式的計算機。另外，整個scheme 語言就是一個大的data，學習寫解釋器的過程，就是學習處理data的過程。

這個作業和lab11我特別做了兩遍，因為第一遍的時候心思全部放在過關上了。做第二遍的時候更仔細讀了一下所有的代碼（約2000行）並特別複習了下相關的課程。

這麽精心編排的課程和作業，當然要好好學習，不然真對不起老師付出的心血。

# **Read-Eval-Print-Loop(REPL)**

一切從prompt（提示符）說起，我們在prompt裡輸入文字然後回車的話，就進入了一個**Read-Eval-Print-Loop**。

![](images/Screen-Shot-2023-01-04-at-11.35.38-AM-1024x151.png)

## 1.**Read-Eval-Print-Loop**

- **Read**\-讀取用戶輸入字符

- **Eval** 程式再後台運算執行

- **Print** 如果有運算結果的話print出來給用戶

REPL有如下三種情形：

- load整個程序然後執行**Read-Eval-Print-Loop**，有結果的話print結果，比如先load像tests.scm這樣的整個程序再REPL再print結果

- 用**Read-Eval-Print-Loop**執行用戶輸入的字符，使用while循環，執行完畢如果有結果的話print

- 如果有錯報錯，用try/except實現,print 報錯訊息給用戶

作業中的read\_eval\_print\_loop主要部分如下，使用大量的函數式編程技巧，用函數呼叫另一個函數。

```
def read_eval_print_loop(next_line, env, interactive=False,              quiet=False,startup=False, load_files=(), report_errors=False):

if startup:
    for filename in load_files:
        scheme_load(filename, True, env)
 
while True:
        try:
            src = next_line()
            while src.more_on_line():
                expression = scheme_read(src)
                result = scheme_eval(expression, env)
                if not quiet and result is not None:
                    print(repl_str(result))
        except (SchemeError, SyntaxError, ValueError, RuntimeError) as err
```

## **2.Read**

第一步驟是檢查用戶的輸入（String），如果沒有錯誤(比如像3.4.5這樣的數字就應該要報錯）

- _Lexical analysis_詞法分析：把每個字符變成一個token，比如(+ 1 2)會變成\['(', '`+`', '`1`', '`2`', ')'\]這5個token組成的list。

- _Syntactic analysis_ 句法分析：把token變成程式語言可以識別的Linked List，因為Scheme語言都是由()組成的list。（這裡是Pair類）

![](images/Screen-Shot-2023-01-03-at-9.58.04-PM-1024x287.png)

  
  
這個過程叫做Parsing，Lab11的作業就是寫個簡單Parsing程式，可以通過作業了解下實現方法。理解Lab11後，比較好理解Scheme解釋器版本的parser。我覺得有趣的設計是使用iterator將token一個一個的推送處理，然後用scheme\_read(）來處理第一個字符，用read\_tail(）來處理剩下的字符，然後彼此呼叫直到結束。  

```
def scheme_read(src):
    """Read the next expression from SRC, a Buffer of tokens.

    >>> scheme_read(Buffer(tokenize_lines(['nil'])))
    nil
    >>> scheme_read(Buffer(tokenize_lines(['1'])))
    1
    >>> scheme_read(Buffer(tokenize_lines(['true'])))
    True
    >>> scheme_read(Buffer(tokenize_lines(['(+ 1 2)'])))
    Pair('+', Pair(1, Pair(2, nil)))
    """
    if src.current() is None:
        raise EOFError
    val = src.pop_first()  # Get and remove the first token
    if val == 'nil':
        # BEGIN PROBLEM 2
        "*** YOUR CODE HERE ***"
        return nil
        # END PROBLEM 2
    elif val == '(':
        # BEGIN PROBLEM 2
        "*** YOUR CODE HERE ***"
        return read_tail(src)
        # END PROBLEM 2
    elif val == "'":
        # BEGIN PROBLEM 3
        "*** YOUR CODE HERE ***"
        return Pair('quote', Pair(scheme_read(src), nil))
        # END PROBLEM 3
    elif val not in DELIMITERS:
        return val
    else:
        raise SyntaxError('unexpected token: {0}'.format(val))


def read_tail(src):
    """Return the remainder of a list in SRC, starting before an element or ).

    >>> read_tail(Buffer(tokenize_lines([')'])))
    nil
    >>> read_tail(Buffer(tokenize_lines(['2 3)'])))
    Pair(2, Pair(3, nil))
    """
    try:
        if src.current() is None:
            raise SyntaxError('unexpected end of file')
        elif src.current() == ')':
            # BEGIN PROBLEM 2
            "*** YOUR CODE HERE ***"
            src.pop_first()
            return nil
            # END PROBLEM 2
        else:
            # BEGIN PROBLEM 2
            "*** YOUR CODE HERE ***"
            return Pair(scheme_read(src),read_tail(src))
            # END PROBLEM 2
    except EOFError:
        raise SyntaxError('unexpected end of file')
```

最後形成如下基於Pair類的Linked List來方便後續的evaluation。

```
read> (+ 1 (* 2 3) (-4 (+ 1 2)))
str :(+ 1 (* 2 3) (-4 (+ 1 2)))
repr: Pair('+', Pair(1, Pair(Pair('*', Pair(2, Pair(3, nil))), Pair(Pair(-4, Pair(Pair('+', Pair(1, Pair(2, nil))), nil)), nil))))
```

Lab11的Interpreter是只有簡單計算功能的解釋器，如果想要實現可以運行一切程式的計算機，必須先了解Environment Frame的概念。

## **3.Environment Frame**

因為複雜的軟體設計到很多綁定，比如各種變量和函數名都需要綁定。frame就是變量和函數運行的環境，這個環境裡包含各種綁定，也會存儲有無parent frame。取值的規則是先從當前的frame尋找；然後有parent的話，再從parent frame尋找；找parent frame的parent frame，只要有parent可以一路找上去；所有的parent都找完了還沒找到就返回錯誤。

```
class Frame:
    """An environment frame binds Scheme symbols to Scheme values."""

    def __init__(self, parent):
        """An empty frame with parent frame PARENT (which may be None)."""
        self.bindings = {}
        self.parent = parent
```

當一個function運行的時候，需要索取當下的變量值，所以就會開啟一個child frame來取值。

這個frame提供function和各種local variable的取值，運行的時候使用stack存儲區。

如果有三個function A, B和C，此時會開啟三個frame，A是global frame會call B， B 會call C，這個時候stack的最底層存儲的是function A和裡面的local variable綁定的值，然後往上存儲的function B和裡面的local variable，再往上是function C和裡面的local variable。當function C運行的時候，function A 和B是處於暫停狀態。當function C return後，返回值到function B，function C用完馬上自行釋放所佔用stack存儲區。然後換function B運行，return後自動釋放所佔用的stack，換function A運行，最後function A 運行完，返回結果，此程式佔用stack全部釋放。

stack是由電腦事先預留的空間，通常比heap小很多。當recursion不斷的循環，或者運行多重nested function，就需要開啟一堆pending 的frame，就有可能會產生stack overflow的問題。

CS61A課程開始的時候花了一些時間理解environment frame，看了不下100個類似下面這種運行步驟超過50的執行圖。

![](images/Screen-Shot-2023-01-04-at-7.03.05-AM-1024x628.png)

還手畫了很多圖，現在的作業是實作environment frame，增進理解。

最後看到實作方法不過就是用Frame class，把binding放在dictionary裡面。再多設置parent的屬性和define函數（用來綁定）、lookup（用來搜尋）和make\_child\_frame。

```
class Frame:
    """An environment frame binds Scheme symbols to Scheme values."""

    def __init__(self, parent):
        """An empty frame with parent frame PARENT (which may be None)."""
        self.bindings = {}
        self.parent = parent

    def __repr__(self):


    def define(self, symbol, value):


    def lookup(self, symbol):

    def make_child_frame(self, formals, vals):
```

上一篇筆記（[CS61A 學習筆記和心得3-通過學習用Python寫個Scheme Interpreter來學習編程語言 part1](https://fulltimemammy.com/scheme-interpreter-use-python/)）是提到了**Read-Eval-Print-Loop**，這篇筆記記錄其中的關鍵部分Eval。

## **4.EVAL**

EVAL是核心運算邏輯，要實現Scheme語言的所有功能。因為Scheme採用函數式編程，所有下面這些功能應該也是所有函數式編程語言基本的功能。

- 賦值（定義變量和函數）

- built-in procedure（+/-/abs/append/list/equal等）

- user-defined procedure(lambda/dynamically scoped procedure等)

- special forms(邏輯運算，包含and/or/if/let/condition)

- 優化性能部分(尾遞歸)

- 支持用戶自由擴展(marco)

### 流程：

![](images/Screen-Shot-2023-01-06-at-7.33.56-PM-1024x551.png)

實現EVAL主要通過下面3個程式。

```
def scheme_eval(expr, env, _=None)

def scheme_apply(procedure, args, env):

def eval_all(expressions, env,tail=False):
```

1.scheme\_eval 主要功能是識別，識別後呼叫scheme\_apply分別處理對應的procedure

```
def scheme_eval(expr, env, _=None)
```

2.scheme\_apply 應用參數並運算，當

```
def scheme_apply(procedure, args, env):
```

這一步主要是將參數傳入，把scheme\_eval 識別的procedure在適合的frame下面運算。

包含三個參數：

- procedure （通過第一步eval得出，比如是builtin procedure還是lambda procedure）

- args （需要傳入的參數）

- env （執行運算時正確的frame）

使用@trace 可以獲得運算步驟。

舉例：簡單的（+ 1 3）運算步驟：先scheme\_eval識別每個部分，再scheme\_apply調用BuiltinProcedure的運算方式。

```
scm> (+ 1 3)
scheme_eval(Pair('+', Pair(1, Pair(3, nil))), <Global Frame>):
    scheme_eval('+', <Global Frame>):
    scheme_eval('+', <Global Frame>) -> #[+]
    scheme_eval(1, <Global Frame>):
    scheme_eval(1, <Global Frame>) -> 1
    scheme_eval(3, <Global Frame>):
    scheme_eval(3, <Global Frame>) -> 3
    scheme_apply(<scheme_classes.BuiltinProcedure object at 0x10339d750>, Pair(1, Pair(3, nil)), <Global Frame>):
    scheme_apply(<scheme_classes.BuiltinProcedure object at 0x10339d750>, Pair(1, Pair(3, nil)), <Global Frame>) -> 4
scheme_eval(Pair('+', Pair(1, Pair(3, nil))), <Global Frame>) -> 4
4
```

3.eval\_all

舉例：對於lambda函數，如何計算？

```
scm> (define double (lambda (x) (* 2 x)))
scm> double 3
```

先將名字和表達式綁定。

```
scm> (define double (lambda (x) (* 2 x)))
scheme_eval(Pair('define', Pair('double', Pair(Pair('lambda', Pair(Pair('x', nil), Pair(Pair('*', Pair(2, Pair('x', nil))), nil))), nil))), <Global Frame>):
    scheme_eval(Pair('lambda', Pair(Pair('x', nil), Pair(Pair('*', Pair(2, Pair('x', nil))), nil))), <Global Frame>):
    scheme_eval(Pair('lambda', Pair(Pair('x', nil), Pair(Pair('*', Pair(2, Pair('x', nil))), nil))), <Global Frame>) -> (lambda (x) (* 2 x))
scheme_eval(Pair('define', Pair('double', Pair(Pair('lambda', Pair(Pair('x', nil), Pair(Pair('*', Pair(2, Pair('x', nil))), nil))), nil))), <Global Frame>) -> double
double
```

通過scheme\_eval 和scheme\_apply循環呼叫計算數值。

```
scm> double 3
scheme_eval('double', <Global Frame>):
scheme_eval('double', <Global Frame>) -> (lambda (x) (* 2 x))
(lambda (x) (* 2 x))
scheme_eval(3, <Global Frame>):
scheme_eval(3, <Global Frame>) -> 3
3
scm> (double 3)
scheme_eval(Pair('double', Pair(3, nil)), <Global Frame>):
    scheme_eval('double', <Global Frame>):
    scheme_eval('double', <Global Frame>) -> (lambda (x) (* 2 x))
    scheme_eval(3, <Global Frame>):
    scheme_eval(3, <Global Frame>) -> 3
    scheme_apply(LambdaProcedure(Pair('x', nil), Pair(Pair('*', Pair(2, Pair('x', nil))), nil), <Global Frame>), Pair(3, nil), <Global Frame>):
        eval_all(Pair(Pair('*', Pair(2, Pair('x', nil))), nil), <{x: 3} -> <Global Frame>>):
            scheme_eval(Pair('*', Pair(2, Pair('x', nil))), <{x: 3} -> <Global Frame>>, True):
                scheme_eval('*', <{x: 3} -> <Global Frame>>):
                scheme_eval('*', <{x: 3} -> <Global Frame>>) -> #[*]
                scheme_eval(2, <{x: 3} -> <Global Frame>>):
                scheme_eval(2, <{x: 3} -> <Global Frame>>) -> 2
                scheme_eval('x', <{x: 3} -> <Global Frame>>):
                scheme_eval('x', <{x: 3} -> <Global Frame>>) -> 3
                scheme_apply(<scheme_classes.BuiltinProcedure object at 0x10339d690>, Pair(2, Pair(3, nil)), <{x: 3} -> <Global Frame>>):
                scheme_apply(<scheme_classes.BuiltinProcedure object at 0x10339d690>, Pair(2, Pair(3, nil)), <{x: 3} -> <Global Frame>>) -> 6
            scheme_eval(Pair('*', Pair(2, Pair('x', nil))), <{x: 3} -> <Global Frame>>, True) -> 6
        eval_all(Pair(Pair('*', Pair(2, Pair('x', nil))), nil), <{x: 3} -> <Global Frame>>) -> 6
    scheme_apply(LambdaProcedure(Pair('x', nil), Pair(Pair('*', Pair(2, Pair('x', nil))), nil), <Global Frame>), Pair(3, nil), <Global Frame>) -> 6
scheme_eval(Pair('double', Pair(3, nil)), <Global Frame>) -> 6
6
```

## 5.Print

如果有運算結果的話，print運算結果的repl\_str。

```
result = scheme_eval(expression, env)
print(repl_str(result))
```

比如下面的例子：

```
scm> (+ 1 3)
4
```
