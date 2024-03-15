---
title: "用Java寫個簡單的Git：CS61B Gitlet項目筆記"
date: "2023-06-13"
categories: 
  - "電腦科學"
keywords: 
- Gitlet
tags: # 标签
- Git
- 工具
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

在寫[Git 原理學習筆記](https://fulltimemammy.com/git-internal/)的時候，就有考慮要不要跟著網上的教程：[Write yourself a Git!](https://wyag.thb.lt/#orga22d15c) 用Python寫個或者是照抄一個Git，但是當時不會I/O的處理，就先放棄了。沒想到CS61B的其中一個[Gitlet](https://sp21.datastructur.es/materials/proj/proj2/proj2)項目就是用Java寫一個簡單版本的Git（包含checkout，merge，reset等），就試試看吧。

其實自己OOP的理解和I/O的處理都還不是很會，Java也是剛學，CS61B才學了一半，要自己手寫個Git，頗有難度，非常考驗耐心。不過這個項目有種魔力，一旦開始就很難放棄，所以我才能中途沒有放棄。（之前學CS50做tidman，覺得太難，我獲得70分後就放棄了）

Gitlet項目之前，有專門一個介紹I/O處理和序列化的lab6.

## **序列化[Serialization](https://en.wikipedia.org/wiki/Serialization)**

序列化就是保存軟件運行時的一些數據到電腦到硬盤，實現永久保存。

運行程序時所有的數據都是保存在電腦內存中，一旦斷電或者關閉程序，內存會被清空。

拿玩遊戲來舉例，有些時候希望每次都從頭開始，比如很古老的俄羅斯方塊，此時關閉程式數據消失無所謂。

有的遊戲希望可以一直接著玩，比如可以保存裝備/生命值等，這是就需要把暫存的內存數據持久化，即通過文件系統的操作把數據以byte的格式寫入硬盤，這樣下次再玩時可以從硬盤上恢復數據找回上次的狀態。

## 為什麼要學習序列化？

每個Git command 對應的都是對.git裡面對應的object的處理。因為幾乎每個操作都改變了狀態。需要更新硬盤文件來記憶這些改變。

比如當我們用git add時，就是在硬盤寫入。

```
example % touch hello.txt
git add hello.txt
```

可以看到隱藏的文件夾.git下面新增了一個object文件：

![s](/images/2024031401.png)

這就是為什麼我們如果忘記了commit，哪怕一年以後再用git status還是可以看到這個沒有commit的文件，因為資料已經寫入了硬盤。

```
example % git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   hello.txt
```

### Java的序列化

可以使用`[java.io.Serializable](https://docs.oracle.com/en/java/javase/19/docs/api/java.base/java/io/Serializable.html)` 標記需要序列化的class，然後Java的虛擬機自動幫我們處理序列化。

```
public class Dog implements Serializable{
...}
```

利用I/O class中的java.io.ObjectOutputStream來執行序列化和用java.io.ObjectInputStream來反序列化。

這兩個比較複雜，簡化邏輯如下，將標記序列化的Dog class物件m寫入硬盤中的文件。

```
Dog m = new Dog(...);
File outFile = new File(saveFileName);

// Serializing the Dog object
writeObject(outFile, m);
```

需要的時候在將已經序列化的文件反序列化，將硬盤文件讀取成Dog物件。

```
Dog m；
File inFile = new File(saveFileName);

// Deserializing the Dog object
m = readObject(inFile, Dog.class);
```

## 重新理解**Git原理**

之前學習的時候有稍微思考了[Git 原理](https://fulltimemammy.com/git-internal/)。寫完後再梳理下git的原理。

Git的核心是commit，每個commit就是當下文件夾的一個快照。

比如在做一個項目，如果沒有Git的話，要如何快照呢？這裡假設每天保存一份版本。方法就是把今天的文件夾存檔名為，第二天在copy前一天的文件夾上面工作，第三天也在copy前一天的文件夾上工作。

比如在做一個項目，如果沒有Git的話，要如何管理文件呢？這裡假設每天保存一份版本。方法就是把今天的文件夾存檔為2023/2/18，第二天先把所有的文件夾複製到2023/02/19，修改後再保存，第三天也是如此類推。

Git的每個commit就相當於幫我們這樣手動的工作完全自動化了。

![s](/images/Screenshot-2023-02-18-at-5.49.39-AM.png)

至於如何存儲或復原任一個時間點的文件夾，這個靠的就是把內容（blob）和commit的物件用一個隱藏文件夾(.git) 存在硬盤上，利用序列化來處理。

Git很聰明的地方在於使用SHA1這個hash function產生的值作為文件名儲存一切信息。這樣一方面優化尋找速度，另一方面環環相套，保證內容不被竄改。

## 設計文件-Gitlet Design Document

Design Document 也可以叫pseudocode。

在學CS50的時候，老師就說編寫代碼之前要先寫pseudocode。

我是寫完整個作業才寫Design Document，再來一次的話，我會在作業開始之前寫。然後過程中邊寫code邊修改Design Document。

先寫的好處是可以提前規劃設計，逼自己設計好了再寫。比如我之前的merge功能的實現邏輯很差，很雜亂，沒有一步一步區分開來，導致自己都看不懂自己的code，是後面重新修改後才邏輯比較清楚。

如果一開始就仔細設計整個流程，把每個小步驟分開的話，應該會節省更多debug的時間。

Design Document：[Gitlet design document](https://fulltimemammy.com/gitlet-design-document/)

## Code Review

這次因為code超過1000行，所以有按照課程中Software Engineering部分的建議自己review code，也有和Github上面其他的答案比較，盡自己最大能力從如下方面改進：

- 修改函數和變量命名更narrative

- 修改大段冗長的程式碼--邏輯混亂的地方重新梳理

- 按照Gradescope的規定修改style

- 檢查注釋是否清楚

- 修改每行超過100字符的程式碼

- 獨立重複使用的function

- 修改多重迴圈

- 使用string builder代替“+”

- 刪除和簡化不必要的變量和方法

- 刪除不是final的static variable（老師在課程中強調了好多次）

## 總結：

對於初學者來說，這是一個很好的項目，幾乎從無到有，給人極大的成就感和信心。

![s](/images/Screenshot-2023-06-11-at-10.19.57-PM-1024x427.png)

項目需要的最多的是耐心，估計我前後花了40天，總共超過80個小時。不過這80個小時專注在這個任務上面，心無旁騖，也是很快樂的。

最後感謝下教授和整個教學團隊，這麽棒的課程竟然免費，唯一需要的是時間和耐心，感興趣的同學快點來學吧。

我的repo：[https://github.com/wxy20850606/cs61b2021-proj2-Gitlet](https://github.com/wxy20850606/cs61b2021-proj2-Gitlet)
