---
title: "Java基本數據結構——Array/List/Set/Queue/Map"
date: 2024-03-13T10:59:11+08:00
lastmod: 2024-03-13T10:59:11+08:00
author: ["Ariel"]
keywords: 
- Java 數據結構
- Java collection
categories: # 没有分类界面可以不填写
- 
tags: # 标签
- 數據結構
- Java
- CS自學
description: ""
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


Data Structure 是計算機存儲和組織數據的機制。wiki里有註明常見的數據結構如下：
![](/images/datastructure1.png)

數據結構也是一種data，先來看看Java里的data type。

# Java data type

在Java裡，data type分為兩種：primitive types和reference types。

## primitive types

primitive types 儲存的是一個實體的值。包括byte, short, int, long, float, double, boolean, char共8種。

> int x;    ——>給x分配一個32byte的地址，存儲在stack
>
> double y; ——>給y分配一個64byte的地址，存儲在stack
>
> x = -123; ——>在地址上直接存儲-123的二進制數值，存儲在stack
> 
> y = 5.23; ——>在地址上直接存儲5.23的二進制數值，存儲在stack

這些基礎類型不是object，所以Java不像Python，萬物都是object。

為了操作方便，Java編譯器會自動裝箱（antoboxing）和拆箱（unboxing），即把primitive types按需要自動轉成object，或者相反。比如將int轉為Integer。

每個primitive type都有自己對應的wrapper class如下。

![raper](/images/wraperclass.png)

## reference types

reference types 也就是object，裡面儲存的是一個參考位址（pointer）而非實體的值。

Data Structure是表示數據類型的object。這個數據類型既可以存儲數據，也包含很多提取或者更改數據的方法。目的是方便用戶使用，不用自己寫。

每次使用new創建實例的時候，返回的是一個pointer。這個pointer就像是一個房子一樣，裡面存儲了很多object的訊息，比如數據和方法。

{{% admonition type=info title="在Java裡：" %}}
- 所有的data structure都是reference type。
- 所有的reference type都是object。
{{% /admonition %}}

# Java基礎數據結構

數據結構中包含array/list/set/map這些最基礎且重要的內容。下面就來看下Java是如何處理這些基礎的數據結構的。

## Array

Array是一種非常基礎的數據結構，其優點是存取快速，缺點是不能改變length。
- string是由array構造的
- 可以用array構建list變成arraylist。
- queue/deque/stack本質上是list，也可以用arraylist構造。
- 可以用array當作hash裡面的bucket，構造hashmap。基於hashmap又可以構造hashset。

所以list/queue/set/map的某些實作裡面都有用到array，可見array有多基礎。

### Resizing array

Array的缺點是不能改變length，所以在基本的數據結構中，需要根據情況Resize array， 可以擴容，也可以縮容。
arraylist

[ArrayList 源码](https://github.com/openjdk/jdk/blob/master/src/java.base/share/classes/java/util/ArrayList.java)

[ArrayList 源码分析](https://javaguide.cn/java/collection/arraylist-source-code.html)

**為什麼ArrayList只擴容而沒有縮容呢？**

除了降低複雜性，也有可能現在的電腦的space都很多，沒必要為了節省一點space多運算增加時間消耗。

### 數組類Array
在Java里，array class存在於java.lang.reflect 包中，直接繼承自class java.lang.Object，並且裡面有final關鍵詞，不允許被繼承。

![arrayclass](/images/arrayclass.png)

上面的java.lang包無需手動import，直接默認import。在默認import的java.lang中，有一個非常重要的Object class，它是所有class的父類。

當developer用如下的不同方法創建一個array時，Java compiler直接生成一組特定的bytecode指令，這個指令讓JVM去按要求生成array。然後JVM在把這個array包裝成object，有點像primitive types相對應的Wrapper class一樣，所以Object class里所有的method(toString()，hashCode()，equals等)都可以被繼承。（參考自： https://blogs.oracle.com/javamagazine/post/java-array-objects）

```Java
    int[] myArray;
    myArray = new int[]{0, 1, 2, 3};
    int[] myArray = {0, 1, 2, 3};
    int myArray[] = {0, 1, 2, 3};
    int myArray[] = new int[4]; /*直接賦值：zero for the numeric types, null for objects*/
```
和C語言和Python不一樣，Java里的array直接賦值為0或者null。

### 靜態類Arrays
除了數組類Array，還有靜態類Arrays，存在於java.util 包中，提供很多操作array的靜態方法，比如Arrays.equals，Arrays.sort，Arrays.asList等等。（更多方法：[Guide to the java.util.Arrays Class](https://www.baeldung.com/java-util-arrays)）

![arraysclass](/images/arrays.png)

JDK把array放在java.lang包里，和primitive types一樣，是不是表示Java把array當作是一種像int一樣基礎的數據類型呢？

而把靜態Arrays放在Java.util包裡面，提供很多搜索排序等等的靜態方法，符合util包對基礎數據類型的處理特性。

安排在不同的包里，有不同的用處。





## Java Collections Framework

和array相比，集合框架中的list/map/set/queue等可以存儲不同類型和數量的對象、支持泛型、並且內建了很多算法可供選擇。

collection是什麼？

Collection在電腦科學中，是一種抽象的數據類型。和單個的int，float不同，collection是一個集合，裡面包含很多個同類型的數據。比如lists, sets, multisets, trees 和graphs。array不是collection，因為其長度固定。map也不是collection，因為map儲存的是key-value pair，不是單一的數據比如int。collection+map 應該就是container了。

可以想想為什麼Java 的Collection Framework沒有包含tree 和graph？

Framework又是什麼？

一般所說的framework，全稱應該是software framework,是處理某個應用開發上面的一整套的規範和解決方案。

### Java Collections Framework 是什麼？

wiki 里說雖然名字叫Framework，實際上只是個library，或者說是自帶的無需導入的library。是Java對collection這個抽象的集合數據類型的規範和具體實現（包括interface和implementation），不過此框架有把Map放進來。

其中Collection interface中主要包括三大類List、Set、Queue，只處理單一數據類型的集合。

Map interface主要有map，處理key-value pair的集合。 

List 和Set主要的不同是List有順序，可以重複，而Set裡面不管順序沒有重複。

下表可以看出Java Collections Framework主要的Interfaces和不同實現。

![](/images/2024/2024050801.png)

### List

存取有序，有索引，可以根據索引來進行取值，元素可以重覆。

理解了List，Queue就是先進先出的list，Deque就是double-ended queue，Stack就是後進先出的List （Java里建議用Deque來做）。甚至Tree退化到一條直線的狀態，就是Linked list。

List的實現如下：
主要可以通過Linked List 和 resizing array。

![](/images/2024/2024050802.png)

### Set

存取无序，元素不可以重覆。

[source code](https://github.com/openjdk/jdk/tree/master/src/java.base/share/classes/java/util)

[一文看透Java Collections Framework的设计思想](https://zhuanlan.zhihu.com/p/391247708)

{{< youtube id = "i-OuY5o_G8g" >}}
id="PLCfyKRKkYTrXDpuBzMpuyWvC8o9-v4tpV" autoplay="true"

https://www.youtube.com/watch?v=i-OuY5o_G8g&list=PL8FaHk7qbOD50LnOXTSpYgnVJQTIVFsmI


後記：
此文章沒寫完，因為自學了一陣子發現自己沒有興趣了。

