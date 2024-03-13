---
title: "CS50 wk8 TCP,IP,HTTP,HTML,CSS,JavaScript課程筆記"
date: "2022-05-18"
categories: 
  - "cs50"
  - "電腦科學"
keywords: 
- CS50
tags: # 标签
- CS50
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

## **本週學習：**

- TCP(Transmission Control Protocol)
- IP(Internet Protocol)
- HTTP(HyperText Transfer Protocol)
- DNS（Domain NameS）
- HTML
- CSS
- JavaScript
- DOM

本週課程前面40分鐘講解了Computer Networking相關知識，教授用傳輸信件這個方式來形象的解釋了互聯網是如何傳輸信息的。

在2016年的wk6課程中，教授是用google網上的信息這個例子來說明信息是如何傳遞的。其中還有一個動畫短片Warrior of the Net形象的說明信息是如何傳遞的。

https://www.youtube.com/watch?v=6iXhAZKOVGE&list=PLhQjrBD2T382VRUw5ZpSxQSFrxMOdFObl&index=8

## **課程筆記：**

### **1.什麼是internet**

**從組成部分的角度：**

- 各種聯網的設備，比如手機、電腦、服務器等
- 信息交換器，比如routers和switches
- 信息傳輸設備，比如光纖、衛星等
- 各種小網絡，比如學校和企業的小網絡

![](images/Screen-Shot-2022-04-28-at-2.40.43-PM-1024x582.png)

source and copyright:[https://gaia.cs.umass.edu/kurose\_ross/index.php](https://gaia.cs.umass.edu/kurose_ross/index.php)

上面一系列基礎設施的存在，是為了提供使用者各種服務。

**從提供服務的角度：**

- 網頁瀏覽
- 文件傳輸
- 郵件傳輸
- 直播
- 遊戲
- 網上購物
- 社交媒體
- 等等

Internet是有史以來人類最大的、可以連結最多設備和最多人的工程，互相交換傳輸信息。如何讓這個龐大的工程有效運作，如何按需求傳輸信息，那就是靠各種**Protocol**網絡協議。

### **2.什麼是Protocol**

各種協議定義了網絡個體間**傳送和接收信息**的**格式和順序**，和收到信息後需要**執行的任務**。

所有的電腦、手機和智能設備本身只是一堆電子元器件構成的工具，他們的世界只有0和1。因為這些工具有龐大的運算能力，所以我們需要他們為我們工作。我們必須提供一系列的規定，比如第一個協議是[ASCII](https://en.wikipedia.org/wiki/ASCII)，讓電腦知道怎麼用0和1表示人類世界的文字和數字。那怎麼讓這麼多電腦和智能設備知道怎麼互相交流互相傳遞信息呢？那就是各種協議和規定。具體可以從下到上或者從上到下來說明。

### **3.互聯網協議分層**

- 應用層，HTTP等
- 傳輸層,TCP/UDP
- 網路層,IP/Routing protocols
- 數據鏈路層,WIFI/PPP等
- 實體層

互聯網有大大小小非常多的協議：[List of RFCs](https://en.wikipedia.org/wiki/List_of_RFCs)。其中最重要是傳輸層的TCP和網路層的IP。

網路層的IP只負責傳送，不保證送達，所以需要加上傳輸層的TCP，保證送達。所以TCP/IP是連在一起用的。

TCP的傳輸服務有提供很多的端口（[TCP/UDP端口列表](https://zh.wikipedia.org/wiki/TCP/UDP端口列表)），分別對應的是各種不同的服務，每個具體的服務又有本服務的協議來規範每個服務行為。比如郵件服務端口25，協議是FMTP；文件傳輸服務端口20，協議是FTP；網頁服務端口80，協議是HTTP等等。這個服務是可以無限擴展的，比如最近幾年出現的線上會議和影音串流服務等等。每出現一種服務，IETF這個組織就會出一個對應的協議。

由此可以看出信息傳輸協議（TCP）就是這個網的核心協議之一。協議的撰寫人之一Vinton Cerf和另一位撰寫者因此獲得圖靈獎，並稱為互聯網之父。

![](images/Screen-Shot-2022-04-28-at-9.06.25-PM-1024x573.png)

source and copyright:[https://gaia.cs.umass.edu/kurose\_ross/index.php](https://gaia.cs.umass.edu/kurose_ross/index.php)

### **4.應用層需求的服務是如何執行的**

以我們google貓的照片為例：

第一步：發送

我們google關鍵詞時相當於給google發送了一封信，信封上有：

- To：google ip地址
- From：自己的ip地址
- Subject：服務類型，比如是要求email還是要視訊還是要瀏覽網頁等，每個服務類型需要通過不同的TCP端口。

信件裡

- 需求信息：搜尋cats的圖片

第二步：傳遞

第一步的所有信息被封裝在一封信裡，這封信就是一個package。然後這封信經過家裡的wlan，在經過中華電信提供的光纖傳輸，信息會通過很多router最後傳遞到google的ip地址，就很像送快遞一樣。這個包裹通過router間傳遞是用某種algorithm，就像我們開車時用google map，每次路徑根據實時路況調整，不一定是一樣的。教授在2016年的課程中還展示了routing通過的站點和所需時間，非常形象。

![](images/Screen-Shot-2022-04-30-at-10.15.56-AM-1024x479.png)

source：cs50 2016 wk6

第三步：回應

當google收到我們的包裹後，根據協議，需要做出相應回應。所以google會把所有的有貓咪的照片的結果傳送回來，避免單個包裹太大賽車，一般會限制每個包裹的大小。比如把包裹分成10份，在信封上標明1/10，2/10等等，這樣就可以避免遺漏。

## **學習HTML/CSS/JavaScript**

CS50課程從開始到現在已經通過學習C語言學習了絕大部分的編程基礎知識（除了union和function pointer等進階知識），之後花一週課程用和C語言對比的方式學Python，然後再學SQL的基礎，現在又一下子教三種前端語言。因為克服了最開始的C，之後所有的和C語言比都是小菜一碟，無非是多花點時間自學而已。

HTML是構建網頁的框架。CSS是裝修，讓網頁變美觀，還可以直接使用bootstrap這個現成的工具。那**JavaScript**是讓網頁變的可以互動，比如使用按鈕；或者讓網頁變得即時，比如通過使用API即時更新網頁信息。使用**JavaScript**主要要學習的是BOM，即把整個的html網頁看成一個巨大的疊套的object。

除了CS50課程外，另外學習了如下兩節課：

- CS50 Web課程 HTML/CSS：[https://cs50.harvard.edu/web/2020/weeks/0/](https://cs50.harvard.edu/web/2020/weeks/0/)
- CS50 Web課程 JavaScript [https://cs50.harvard.edu/web/2020/weeks/5/](https://cs50.harvard.edu/web/2020/weeks/5/)

## **其他：**

部分課程筆記和圖片來自於如下Computer Networking課程：

[Computer Networking: A Top-Down Approach, 7th Edition](https://www.ucg.ac.me/skladiste/blog_44233/objava_64433/fajlovi/Computer%20Networking%20_%20A%20Top%20Down%20Approach,%207th,%20converted.pdf)

[https://gaia.cs.umass.edu/kurose\_ross/index.php](https://gaia.cs.umass.edu/kurose_ross/index.php)

Computer Networking的課程還蠻有趣的，希望未來可以單獨學習。

CS50 wk8和wk9 是Web相關的，但是只是學個入門基礎，CS50團隊提供的更全面的CS50Web課程供以後學習。
