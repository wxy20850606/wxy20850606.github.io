---
title: "從0和1開始造一台電腦1-邏輯門"
date: "2022-09-10"
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

在學習CS50wk0的時候有學到電腦最底層都是一堆的0和1，當時就產生一堆的好奇心。感嘆每天自己使用的電子產品竟然都是從0和1這麼簡單的開和關一步步構造出來的。

所以在看到Nand2tetris part1課程號稱從NAND邏輯閘開始造一台可以跑簡單程式的電腦時，沒多想就開始上課了。

上完了前五章，我從一開始連Nor這種邏輯門都不能理解的人，看書然後跟著課程，慢慢從簡單的邏輯門開始，用模擬器構造出RAM/register/ALU，最後做出CPU。這個過程使用教授專門為了課程設計的硬體模擬語言HDL，在老師的指導下通過5週的作業做出了簡單的HACK電腦。不是真的連一堆電線，而是用模擬的方式學習背後的邏輯。

我的感受就是：哇！太神奇了。造就了現在聯繫數十億人的有史以來的最大的人類網路，只是電流加上最簡單的開和關（0和1）。也感嘆電腦科學的抽象化通過將各個部分模塊化然後組合在一起，最後可以產生如此複雜的系統。

雖然寫完作業做出電腦，但是心裡還是沒底，所以準備寫些文章總結下課程wk1-wk5所學，以提高學習留存率。

以下是正文：

電腦科學廣泛使用[抽象化](https://zh.m.wikipedia.org/zh/抽象化_(計算機科學))Abstraction的概念，把這門學科分為很多[抽象層](https://zh.m.wikipedia.org/wiki/%E6%8A%BD%E8%B1%A1%E5%B1%A4)（Abstraction layer）。每一層關注自己的實作就可以了，把所有的底層知識當作理所當然。

下圖是Nand2tetris 課程實作項目，可以看出分了很多層，每一個底層完成就封裝起來直接使用。像是很多補習班會開設JAVA班或者C++班，直接教授高階語言，學員學習3個月就可就業，不用深入學習此層以下其他的知識，就得益於電腦科學的抽象化，也可以說是分層化或模塊化。

![](images/Screen-Shot-2022-09-10-at-7.55.51-AM-1024x588.png)

來源：Nand2tetris課程

## 繼電器與邏輯門

上述層級最底層是由晶體管構建的邏輯門，而晶體管是建立在固態物理和量子理論基礎上的，理論深奧難理解。

晶體管本質上是一種開關，它有兩個前輩：繼電器和真空管。1944 第一台電腦Harvard Mark1使用繼電器， 1946 第一台可編程電腦ENIAC使用真空管，1947貝爾實驗室發明晶體管後，科技進步然後形成現在一個指甲大小到芯片上有數十億個晶體管。

我一開始學Nand2tetris課程看不懂邏輯門，然後看到CODE這本書，才發現用繼電器就可以幫助理解邏輯門後面的邏輯。

繼電器是利用電磁鐵產生的磁力吸引開關的金屬條，來達到控制開關的目的。有電流通過電磁鐵，電磁鐵產生磁力，會吸下上方的開關金屬片，從而閉合開關。

![](images/Screen-Shot-2022-09-10-at-7.23.06-AM-300x203.png)

繼電器示意圖，source：CODE

通過不同的方式連結繼電器就會製造出如下基本的的邏輯門。

### 1）AND gate

與門，兩個開關都閉合，讓電磁鐵產生磁力吸引兩個開關的彈簧片，從而閉合電路，點亮燈泡。

![](images/Screen-Shot-2022-09-10-at-7.20.10-AM-300x280.png)

與門示意圖，source：CODE

其中，V表示電源輸入，燈泡亮表示輸出為1，燈泡不亮表示輸出為0，下同。

真值表：

<table class="has-fixed-layout"><tbody><tr><td>And</td><td>0</td><td>1</td></tr><tr><td>0</td><td>0</td><td>0</td></tr><tr><td>1</td><td>0</td><td>1</td></tr></tbody></table>

### 2）NOT gate(inverter）

反向器，開關閉合會拉下上方的金屬彈簧片，導致電流斷開，燈泡熄滅。

![](images/Screen-Shot-2022-09-10-at-7.32.26-AM.png)

NOT gate示意圖，source：CODE

真值表：

<table class="has-fixed-layout"><tbody><tr><td>NOT</td><td></td></tr><tr><td>0</td><td>1</td></tr><tr><td>1</td><td>0</td></tr></tbody></table>

### 3）OR gate

或門，有任意一個開關或者兩個都閉合，都可以接通電路。

![](images/Screen-Shot-2022-09-10-at-7.36.59-AM.png)

OR gate示意圖，source：CODE

真值表：

<table class="has-fixed-layout"><tbody><tr><td>OR</td><td>0</td><td>1</td></tr><tr><td>0</td><td>0</td><td>1</td></tr><tr><td>1</td><td>1</td><td>1</td></tr></tbody></table>

### 4)NOR gate

或非門，兩個開關都打開，電路才能通，如果有任一開關閉合，電磁鐵產生電流，會吸引上方的彈簧片，導致電路斷開。

![](images/Screen-Shot-2022-09-10-at-8.02.58-AM.png)

NOR gate示意圖，source：CODE

真值表：

<table><tbody><tr><td>NOR</td><td>0</td><td>1</td></tr><tr><td>0</td><td>1</td><td>0</td></tr><tr><td>1</td><td>0</td><td>0</td></tr></tbody></table>

### 5）NAND gate

因為燈泡有兩個電源輸入，任一個電路連通都會點亮燈泡。

此電路只有兩個輸入開關都閉合的情況下，才會導致燈泡熄滅。

![](images/Screen-Shot-2022-09-10-at-8.07.17-AM.png)

NAND gate示意圖，source：CODE

真值表：

<table><tbody><tr><td>NAND</td><td>0</td><td>1</td></tr><tr><td>0</td><td>1</td><td>1</td></tr><tr><td>1</td><td>1</td><td>0</td></tr></tbody></table>

## 總結

所有的門都可以通過AND/OR/NOT來構造，也可以通過NAND來構造。比如:

- NOT(x)=(x)NAND(x)，NAND的兩個信號一樣，兩個開關為0，輸出為1，兩個開關為1輸出為0。
- (x)AND(y)=NOT(x NAND y)，把NAND門反過來，就獲得AND門。
- (x)OR(y)=(NOT x) NAND (NOT y)，把信號取反，然後在連NAND門，獲得OR門。

這也是Nand2tetris這門課名字的來由，老師提供NAND，學生用NAND門構造出NOT/AND/OR後，在運用這些基礎邏輯門構造出如下的邏輯門，然後在這些邏輯門的基礎上構建整個電腦體系。

- Xor
- Mux
- DMux
- Not16
- And16
- Or16
- Mux16
- Or8Way
- Mux4Way16
- Mux8Way16
- DMux4Way
- DMux8Way

本文參考資料：

[CODE（The Hidden Language of Computer Hardware and Software)](https://bobcarp.files.wordpress.com/2014/07/code-charles-petzold.pdf)

[From Nand to Tetris](https://www.nand2tetris.org/)課程

Crash Course Compter Science視頻：

https://www.youtube.com/playlist?list=PL8dPuuaLjXtNlUrzyH5r6jN9ulIgZBpdo
