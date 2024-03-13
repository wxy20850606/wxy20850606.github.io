---
title: "Java 和 Python 的區別"
date: "2023-04-07"
categories: 
  - "電腦科學"
keywords: 
- 電腦語言
- python
- java
- 區別
tags: # 标签
- 語言
- Java
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

最近開始學Java，感覺和Python蠻不一樣，於是在網上找找資料，看看二者的不同之處。

先分享一篇好玩又很有啟發的文章：

[两年，我学会了所有的编程语言！](https://mp.weixin.qq.com/s?__biz=MzAxOTc0NzExNg==&mid=2665517522&idx=1&sn=758759e90d395311bfd33a68e2eeb116&chksm=80d66991b7a1e0874b452a9d3627fe995231eb5d80d88b5234b6b54e631b5c04827b16800e8d&scene=178&cur_album_id=1300063186770640897#rd)

下面進入正題。

## **Java是編譯式的語言， Python是直譯式的語言？**

想要清楚這個問題，要稍微了解下二者的程序是如何運行的。

### **Python是怎麼運行的？**

**[來源：is-python-a-compiled-language-or-an-interpreted-language](https://discuss.python.org/t/is-python-a-compiled-language-or-an-interpreted-language/6556)**

比如helloworld.py文件。如下是比較常見的運行方式：

- 使用interpreter（比如**CPython**），解釋器第一步將原代碼編譯（compile）為“byte code”。byte code和machine code區別在於前者是跨平台性的。

- 解釋器然後用自帶的虛擬機（virtual machine）來運行byte code。可以把虛擬機想像成虛擬的電腦。

- 解釋器運行編譯過的byte code（.pyc文件）。

- 和編譯語言一樣，在原代碼和機器代碼之間，有byte code這個中間的載體。

- 和編譯語言不一樣，byte code 不是直接在電腦上運行，而是需要解釋器來運行。

- 所以只要機器上裝有解釋器，就可以運行Python的文件。

- 因為Python是直譯式語言，所以可以先在終端運行解釋器，然後一句一句執行。

```
% python3
>>> print("hello world")
hello world
```

### **Java是怎麼運行的？**

Java運行步驟：

- 不同設備上提前安裝平台專屬的[虛擬機_Java Virtual Machine_（JVM](https://www.jyt0532.com/toc/jvm/)）

- 先用javac編譯器將java文件轉為.class的byte code文件

- javac命令執行後，在操作系統中啟動一個JVM進程，JVM將原文件通過類裝載器（Class Loader）加載到JVM的內存區域，生成bytecode

- JVM通過內建的Execution Engine來翻譯bytecode，Execution Engine裡有interpreter/JIT compiler將bytecode翻譯為機器語言

- JVM和操作系統相配合執行機器語言

Java程序運行核心是通過**虛擬機**。

#### **虛擬機是什麼？**

**虛擬機**組成圖：

![](images/Screenshot-2023-04-07-at-9.28.03-AM-1024x587.png)

source：[JVM Tutorial - Java Virtual Machine Architecture Explained for Beginners](https://www.freecodecamp.org/news/jvm-tutorial-java-virtual-machine-architecture-explained-for-beginners/)

可以把JVM想像成是一台模擬出來的電腦，此電腦具有完善的操作系統和處理器，也可以運行軟件。

> the _virtual machine_, providing an abstraction of the entire computer, including the operating system, the processor, and the programs. 
> 
> \-CSAPP

使用這樣的模擬電腦來運行class程序，無論是什麼操作系統，只要安裝了JVM，就能運行Java程序。正是由於JVM，Java這種偏編譯型的語言才可以實現一次開發，到處運行。

#### **Java為什麼採用虛擬機？**

主要是為了快速安全多線程的跨平台執行，下面這篇文章有介紹**虛擬機**的前世今生。

[每個程序員都該瞭解的JVM - JAVA虛擬機介紹](https://www.jyt0532.com/2020/02/14/jvm-introduction/)

**總結：**

**Java和Python都是同時採用編譯器與直譯器來運行的語言。**

不過Python是偏直譯式的語言，而Java是偏編譯式的語言。二者都可以 “compile once, run anywhere.” 

下面是本段筆記的參考文章🔗：

[JVM ( java virtual machine) architecture - tutorial](https://www.youtube.com/watch?v=ZBJ0u9MaKtM)

[JVM Tutorial - Java Virtual Machine Architecture Explained for Beginners](https://www.freecodecamp.org/news/jvm-tutorial-java-virtual-machine-architecture-explained-for-beginners/)

## **Java保留了primitive types，而Python裡全部都是對象（object）。**

Java是雙類型系統，除了reference type（object），Java還保留了8種primitive types，**boolean , byte , char , short , int , long , float , double**。

比如下面的int，變量i 的數值直接以二進制的方式儲存在JVM stack上面的某個地址（32bits）。

```
int i = 3;
```

而下面的Integer類裡的i則是一個object，每個object都分配有64個bits，不過這些bits存儲的不是object的內容，而是一個reference，這個reference指向存儲在JVM heap上面的object內容的地址。

```
Integer i = new Integer(3);
```

至於Java為什麼保留基本的數據類型，基本上就是為了提升運算速度和效能，可以參考如下Thinking in Java裡的解釋。

> One group of types, which you’ll use quite often in your programming, gets special treatment. You can think of these as “primitive” types. The reason for the special treatment is that to create an object with **new**—especially a small, simple variable—isn’t very efficient, because **new** places objects on the heap. For these types Java falls back on the approach taken by C and C++. That is, instead of creating the variable by using **new**, an “automatic” variable is created that is _not a reference_. The variable holds the value directly, and it’s placed on the stack, so it’s much more efficient.
> 
> \-Thinking In Java

也可以參考這篇文章：**[Java 为什么需要保留基本数据类型](https://blog.51cto.com/u_15127686/2832946)**

Python只有一種數據類型，那就是object。萬物皆object，Python裡原始的數據類型比如int/float都是用object構成的。

下圖為Python裡的int物件自帶的method。

![](images/Screen-Shot-2022-12-11-at-6.16.09-AM-1024x321.png)

## **Java是**static typing**和****dynamic typing****，Python是**dynamic typing**。**

先來學習下python 中沒有的static 關鍵字。

static，辭典意思為“[staying in one place without moving, or not changing for a long time](https://dictionary.cambridge.org/zhs/%E8%AF%8D%E5%85%B8/%E8%8B%B1%E8%AF%AD-%E6%B1%89%E8%AF%AD-%E7%B9%81%E4%BD%93/stay)”，翻譯為靜態，靜態的變量和方法不用依賴任何object可以直接訪問，是没有this的。比如Math.abs就可以不用object直接訪問。

有兩條原則：

一是不能從static 方法中呼叫non-static 方法。

```
With the this keyword in mind, you can more fully understand what it means to make a method static. It means that there is no this for that particular method. You cannot call non-static methods from inside static methods (although the reverse is possible), and you can call a static method for the class itself, without any object. In fact, that’s primarily what a static method is for. 

-Thinking in Java
```

二是在加載class時有static關鍵字的是最先加載的而且只加載一次。詳細可參考如下這篇文章：

[Java中的static關鍵字](https://www.cnblogs.com/dolphin0520/p/3799052.html)

那****static typing****和上面的static關鍵字好像沒有直接的聯繫。****static typing****和****dynamic typing****說的是[Type system](https://en.wikipedia.org/wiki/Type_system)。

電腦語言只有0和1，Type system的意義就在於給這個0和1賦予意義。比如安排有些0和1表示整數。

> _“The fundamental problem addressed by a type theory_ \[aka type system\] _is to ensure that programs have meaning._“
> 
> Mark Mannasse

### Java的Static Type vs. Dynamic Type

Java的“static type”也是“compile-time type”，指的是變量一旦設置type就一直保持不變，並且在complie的時候檢查type，如果有不一致的情況，會無法編譯也就不能運行。

比如下面的變量x一開始設置為int就不能在改變了。

```
public class Hello{
    public static void main(String[] args) {
        int x = 0;
        x = "hello";
        System.out.println(x);
    }
}
```

否則會報錯：

![](images/Screenshot-2023-03-23-at-10.48.13-AM-1024x595.png)

Java的變量還有一個type就是“dynamic type”即“run-time type”。

- 用new這個關鍵詞產生一個instance的時候產生

- type就是新產生的object（Equal to the type of the object being pointed at.）

下面這個例子中a的compile type是Object，run-time type是Corgi，但因為Object沒有bark這個方法，所以無法compile。

```
public class Dog {
    private String name;

    public Dog(String str) {
        name = str;
    }

    public void bark() {
        System.out.println("Whoof!");
    }
}

class Corgi extends Dog {
    public Corgi(String str) {
        super(str);
    }

    public void bark(){
        System.out.println("Bark!");
    }
    public static void main(String[] args) {
        Object a = new Corgi("Amy");
        a.bark();
    }
}
```

但是把Object改為Dog，此時compile type 是Dog，run-time type是Corgi。因為兩者都有bark這個方法，產生overide，此時根據runtime type選擇方法，此處選擇Corgi的方法。

```
    public static void main(String[] args) {
        Dog a = new Corgi("Amy");
        a.bark();
    }
```

Python不用compile只有採用****dynamic typing****。不需提前聲明變量類型，只在runtime的時候通過程式自帶的[Type inference](https://en.wikipedia.org/wiki/Type_inference)檢查類型，變量自然可以一直改變類型。

```
>>> number = 10
>>> numer = (number+20)/2
```

好處是簡化代碼，壞處是少了compile這個過程，導致有些錯誤沒法一開始就被檢查出來。比如上面的code第二行的number寫錯成numer了，但是由於是動態語言，不會檢查，所以還是可以運行，這就有可能埋入bug。

可參考CS61B課本[https://joshhug.gitbooks.io/hug61b/content/chap1/chap11.html](https://joshhug.gitbooks.io/hug61b/content/chap1/chap11.html)和[課程視頻](https://cs61b-2.gitbook.io/cs61b-textbook/11.-subtype-polymorphism-comparators-comparable/11.1-a-review-of-dynamic-method-selection)

## **Java對函數式編程支持有限，Python支持函數式編程。**

Python既支持面向對象的編程，也支持面向過程的函數式編程。

比如在Python中，函數也是object，所以可以當作參數傳遞。CS61A課程中前半部分也是在說Python如何支持函數式編程的[很多特性](https://fulltimemammy.com/higher-order-functions/)。

而Java中沒有函數這個type，想要實現函數式編程的一些思想比如HOF只能通過特別的interface。

下面這個例子（proj1c作業)可以看出想要使用某個函數（比如compare），必須把整個Comparator當參數傳入，然後再通過c.compare使用這個函數。比Python難用很多。詳細請參考課程：[Higher Order Functions in Java](https://www.youtube.com/watch?v=OcfTN1PZ7oA&list=PL8FaHk7qbOD62-LKdfz_V_uMg7f5GaSnC&index=10)

```
public T max(Comparator<T> c) {
        if (this.isEmpty()) {
            return null;
        }
        int maxIndex = 0;
        for (int i = 1; i < size(); i += 1) {
            if (c.compare(get(i),get(maxIndex)) > 0) {
                maxIndex = i;
            }
        }
        return get(maxIndex);
    }
```

Java對函數式編程只是間接的支持，支持度比較有限。

下面再來總結下Java和Python在OOP上的不同。

## **Java和Python在面向對象上的不同**

### **Java強迫使用Class，Python不強迫。**

Java裡所有的程式都必須存在於一個class裡面，程序入口main也必須存在於一個class裡面。OOP的核心object也是必須通過class來產生的，所以從這個意義上來說，Java強迫使用OOP。

```
public class HelloWorld {
    public static void main(String[] args){
        System.out.println("Hello World");
    }
}
```

而Python則沒有，因為有解釋器的關係，沒有main這個主程序入口，可以一行一行執行，不強迫OOP。

剛開始學Python的時候，甚至拿來當計算器用。

### **Java裡有封裝，Python沒有封裝。**

**Java**裡面的變量和方法有private/protected/public/default四種設定。尤其是通過使用private可以禁止外部訪問，隱藏細節，實現封裝。

在Java中，可以放心的使用helper function，把重複的code獨立成一個helper function，再設置成private，代碼變得簡潔邏輯也更清楚，一堆細節也會被封裝隱藏起來。

這個硬體上的只需要知道接口，不需要知道細節是一致的。

Python裡全部默認為public，沒有區分沒有封裝隱藏。哪怕是用雙下劃線\_\_來假裝隱藏，也還是可以通過\_從外部來獲取。

### **Java不支持多重繼承，Python支持多重繼承。**

Java的class只可以繼承一個super class（使用extends關鍵詞），而Python的class可以繼承很多個super class。

#### **Java為什麼不支持多重繼承呢？**

Java之父認為多重繼承帶來的好處大於其帶來的麻煩，所以在設計時就決定排除多重繼承。

> JAVA omits many rarely used, poorly understood, confusing features of C++ that in our experience bring more grief than benefit. This primarily consists of operator overloading (although it does have method overloading), multiple inheritance, and extensive automatic coercions.
> 
> _Java: an Overview_/James Gosling

但是Java的class允許實作多個interface，算是稍微彌補不能多重繼承的問題。

Java8開始允許interface可以有default method，有可能還是會產生多重繼承的問題。但是Java在編譯的時候會要求在class裡override有歧異的method，避免多重繼承的可能產生的衝突。

#### **Python支持多重繼承。**

比如下面這個菱形繼承，方法尋找的路徑是從下往上從左到右。

![](images/Screenshot-2023-03-24-at-2.08.02-PM.png)

source:[COMPOSING PROGRAMS](http://composingprograms.com/pages/25-object-oriented-programming.html) 

Python還有其他支持多重繼承的語言使用MRO algorithm解決菱形繼承的問題，下面這篇文章有解釋：

[Multiple Inheritance described by Python's primary author](http://python-history.blogspot.com/2010/06/method-resolution-order.html)

### **Java提供interface，多型直接明顯；Python中使用**[duck typing](https://docs.python.org/3/glossary.html#term-duck-typing)來多型**。**

Java中的interface是顯性的，比如下面這個Comparator的接口，裡面有compare這個方法。

```

public interface Comparator<T> {
    int compare(T o1, T o2);
}
```

class通過implements上面的interface可以實現多型。

比如這個subtype是比較int。

```
private class intComparator implements Comparator<Integer>{
        @Override
        public int compare(Integer o1, Integer o2) {
            return o1 - o2;
        }
    }
```

這個subtype是比較string。

```
    private class StringComparator implements Comparator<String>{
        @Override
        public int compare(String o1, String o2) {
            int minLength = Math.min(o1.length(),o2.length());
            for(int i = 0; i < minLength;i += 1){
                int i1 = o1.charAt(i) - o2.charAt(i);
                if(i1 != 0){
                    return -i1;
                }
            }
            return o2.length() - o1.length();
        }
    }
```

因為Java不允許**多重繼承，**但是可以允許有很多個接口，所以Java 引入Interface目的之一是來達到類似多重繼承。

Python允許多重繼承，所以不需要interface，另外也可以使用[duck typing](https://docs.python.org/3/glossary.html#term-duck-typing)技術來部分達到interface的目的。所以很多Python的書籍裡都沒有特別提到多型。

#### Python通過[duck typing](https://docs.python.org/3/glossary.html#term-duck-typing)實現多型

比如下面[wiki](https://en.wikipedia.org/wiki/Duck_typing)中提供的例子，不用interface先註明有哪些方法，直接呼叫，是鴨子的話就有swim和fly的方法，不是的話系統會報錯。

```
class Duck:
    def swim(self):
        print("Duck swimming")

    def fly(self):
        print("Duck flying")

class Whale:
    def swim(self):
        print("Whale swimming")

for animal in [Duck(), Whale()]:
    animal.swim()
    animal.fly()
```

## 總結：

之前在學習CS61A的時候，寫第二部分OOP的筆記的時候卡住了，現在知道原因了。因為Python幾乎沒有封裝，多型的使用也沒有Java這麽明顯，用Python解釋多型有些多餘。所以當時我才不知道怎麼寫筆記解釋OOP。現在稍微想通一點點了。

呼應開頭的漫畫，學語言的關注點如果放在基本概念的話，會有越來越深入的理解，也可以通過不同的語言不同的實作來慢慢加深對核心概念的理解。

其他：

這篇文章詳細的紀錄了很多細節上的不同：

[Python and Java - Comparisons and Contrasts](https://www.rose-hulman.edu/class/csse/csse220/201130/Resources/Python_vs_Java.html)
