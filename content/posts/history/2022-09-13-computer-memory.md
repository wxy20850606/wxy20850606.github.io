---
title: "從0和1開始造一台電腦3-電腦如何記憶"
date: "2022-09-13"
categories: 
  - "電腦科學"
keywords: 
- 如何製造一台電腦
tags: # 标签
- NandToTetris
- 電腦體系
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

註：本文為[From Nand to Tetris 課程](https://www.nand2tetris.org/)學習筆記，目的為總結個人所學。如有點閱，請謹慎參考。

前一篇文章（總結了電腦如何計算，但是電腦光是計算還不夠，還需要有時間的概念和記憶的能力。

如果我們要用loop計算1+2+3+4，我們就要先設定一個sum，讓sum=0，還要有用(i=0;i<4;i++)來loop，然後計算：

- i=0，sum+1，得到sum=1
- i=1，sum+2，得到sum=3
- i=2，sum+3，得到sum=6
- i=3，sum+4，得到sum=10

電腦需要記憶sum的數值，這要求電腦會記憶。

電腦需要分開四步驟來執行，這需要電腦要有時間的概念，知道自己執行了幾個步驟。

有了記憶和時間的概念，就可以充分發揮電腦計算的功能，因為同一個硬體我們就可以重複無限次使用。

## 時間

之前的基本邏輯門（AND/NAND/OR/XOR/MUX等等）都是獨立的輸入和輸出，有輸入，再輸出，就結束。假裝時間不存在。

不考慮時間的電路叫做Combinatorial Logic，輸入直接輸出。

out\[t\]= function(in\[t\])

考慮時間的電路叫做Sequential Logic，輸入後需要時間把結果傳到輸出，這樣這個時間單位的輸出是上個時間單位輸入導致的。

out\[t\]= function(in\[t-1\])

Nand2tetris課程覺得沒有必要告訴DFF的電路實現，直接提供DFF的芯片給學生使用。我還蠻好奇的，就從[CODE（The Hidden Language of Computer Hardware and Software)](https://bobcarp.files.wordpress.com/2014/07/code-charles-petzold.pdf)的第14章找了如下的內容來幫助學習。

### Oscillator

下面的電路，閉合開關後，電路連通，電磁鐵有電產生磁力，把上方的彈簧片拉下來，這個結果又讓電路斷電，電磁鐵失去磁性，彈簧片彈起，這又導致電路連通，產生電流。只要開關保持關閉狀態，彈簧片會一直這樣彈起來又掉下來然後又彈起來，一直循環下去。

只要開關閉合，電路就會一直輸出0和1.

![](images/Screen-Shot-2022-09-12-at-8.00.07-PM.png)

這個電路叫做振盪器（Oscillator），只要開關閉合，就會一直輸出0和1，而且每個週期(一個0和1）的時間是一樣的。

振盪器就可以作為CPU的時鐘，統一電腦的行動，像一個音樂會的指揮家一樣，協調電腦各個部分統一行動。

![](images/Screen-Shot-2022-09-13-at-8.14.26-AM.png)

如果每個週期（1個0和1）是0.05秒，每秒可以循環20次，頻率用Hz（赫茲）計算，這個頻率就是20Hz。這是幫助我們了解 CPU 速度的基本單位。

比如我的電腦的頻率是 1.7GHz，1個GHz表示1秒鐘內CPU可以進行10億次的週期（1個0和1），我的電腦1秒內可以進行17億次的0和1的循環。

## Flip-flop

用兩個NOR gate（或非門）連接出下面的電路，左邊或非門的輸出是右派或非門的輸入，右邊或非門的輸出是左邊或非門的輸入。

![](images/Screen-Shot-2022-09-12-at-8.00.34-PM-1024x404.png)

或非門的特點是兩個輸入都為0時，結果才為1。

當上面的開關關閉，左邊的或非門的輸入為0和1，輸出為0，右邊的或非門輸入為0和0，結果為1，此時電燈點亮。

![](images/Screen-Shot-2022-09-12-at-8.03.18-PM-1024x481.png)

這時，如果打開上面的開關，左邊或非門的輸入為1和0，輸出為0，右邊或非門的輸入為0和0，輸出為1。電燈保持點亮。

![](images/Screen-Shot-2022-09-12-at-8.04.37-PM-1024x474.png)

此時若關閉下面的開關，右邊的或非門的輸入為0和1，輸出為0，電燈熄滅。

![](images/Screen-Shot-2022-09-12-at-8.04.00-PM.png)

所以，同樣兩個開關打開，電路有2種穩定的狀態，電燈亮或者不亮。這個電路就叫做flip-flop，翻譯為正反器。

這樣電路就有了記憶和時間的功能：

- 當電燈亮時，表示上面的開關在上一個週期閉合
- 當電燈滅時，表示下面的開關在上一個週期閉合

這個叫做R-S正反器，可以記得上一個時間哪個輸入端信號為1.

![](images/Screen-Shot-2022-09-13-at-10.30.43-AM.png)

但是這樣只記得1還不夠，1位元的信息有0和1，我們需要電腦記得某個特定時間點上的信號是0還是1.

## D Flip-flop-記憶一個位元

把前面的Flip-flop前面加上兩個AND門，Data為數據輸入，Hold That Bit為控制按鈕輸入。

當Hold That Bit為0時，無論data為0還是1，因為AND門，不會影響結果。

![](images/Screen-Shot-2022-09-13-at-11.02.58-AM-1024x433.png)

當Hold That Bit為1時，AND起作用。

Data=1，Q=1:

![](images/Screen-Shot-2022-09-13-at-11.06.34-AM-1024x433.png)

再將Hold That Bit變為0，因為NOR門的關係，Q的輸出還是為1。

此時Data無論變成0還是1，因為AND門的關係，都不會影響電路的結果，電路記得輸出為1。

![](images/Screen-Shot-2022-09-13-at-11.09.53-AM-1024x417.png)

再次把Hold That Bit變為1，此時把Data設置為0，Q也變成0:

![](images/Screen-Shot-2022-09-13-at-11.38.44-AM-1024x456.png)

再把Hold That Bit變為0，此時Q=0，保持不變。

此時Data無論變成0還是1，因為AND門的關係，都不會影響電路的結果，電路記得輸出為0。

![](images/Screen-Shot-2022-09-13-at-11.44.07-AM-1024x430.png)

所以以上就完成了記得0和1一個位元的電路。

總結一下，可以把Hold That Bit變成Load輸入，把Data輸入變成in輸入，上面的電路可以抽象成：

![](images/Screen-Shot-2022-09-13-at-11.52.28-AM.png)

當load為1時，in的輸入才對電路有影響，考慮電路流通電子流動需要時間，輸出為上一個週期的輸入。

當load為0時，in的輸入對電路沒有影響，輸出保持為上一個週期的輸出。

## 暫存器和Memory

有了1位元的記憶，想要擴展成多位元非常容易。

如果想要16位元的暫存器(Register），把16個一位元的記憶體連在一起就好了。

![](images/Screen-Shot-2022-09-13-at-12.03.21-PM.png)

有了1個Register，把很多Register連在一起就可以組合成Memory。

至於很多的Register怎麼組合成Memory的，可以想像成下圖這種堆疊的方式。

![](images/Screen-Shot-2022-09-13-at-12.08.03-PM-1024x663.png)

Nand2tris作業是用DMux8Way和Mux8Way16這兩個邏輯門，從RAM8到RAM64到RAM512到RAM4K，最後在用DMux4Way和Mux4Way16構建出RAM16K的Memory。

## Program Counter

上面做出來的16位元的Register，再加入inc和reset這兩個控制管腳，用MUX數據選擇器芯片就可做出一個program counter。

![](images/Screen-Shot-2022-09-13-at-1.29.09-PM-1024x388.png)

Program Counter，翻譯為程序計數器，是儲存電腦指令的Register。

三個控制管腳作用：

- reset 重啟按鈕，比如電腦一啟動，就開始自動執行第一條程式代碼
- inc 自動增加按鈕，按順序從上到下一條一條執行程式代碼
- load 在需要跳轉時跳轉到某條程式代碼

reset和inc比較好理解，load按鈕涉及到跳轉（go to)。

下面是一個乘法程式的部分程式碼，這一行行的指令前面都有編號。

![](images/Screen-Shot-2022-09-13-at-1.21.43-PM.png)

當電腦重啟（reset）後，開始執行第一條指令，然後一條接著一條（increment）。因為乘法本質上是連續的加法，比如3\*4，可以分成3+3+3+3，變成加法運算。先設置sum=0，在執行四次加法運算，這就需要使用跳轉。這也是程式中的迴圈（loop）可以被執行的原因。

本文參考資料：

[CODE（The Hidden Language of Computer Hardware and Software)](https://bobcarp.files.wordpress.com/2014/07/code-charles-petzold.pdf)

[From Nand to Tetris 課程](https://www.nand2tetris.org/)

Crash Course Compter Science視頻：

https://www.youtube.com/watch?v=tpIctyqH29Q&list=PLH2l6uzC4UEW0s7-KewFLBC1D0l6XRfye
