---
title: "怎麼估值？Aswath Damodaran 亞斯華斯‧達摩德仁的Valuation in Four Lessons 學習筆記"
date: "2021-02-25"
categories: 
  - "估值"
  - "投資學習"
tags: 
  - 投資學習
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

傳統意義上的教育是去大學學堂讀書，但是現在只要可以上網，就可以學習。網上有滿山滿海的資源，不用花錢，就可以找自己喜歡的老師感興趣的內容好好學起來，這真是天底下最棒的投資。

下面就是我看youtube視頻[Valuation in Four Lessons](https://www.youtube.com/watch?v=Z5chrxMuBoo&t=2667s) 後的學習筆記。

# Aswath Damodaran是誰？

Aswath Damodaran達摩德仁是NYU紐約大學商學院的教授，早在1986年就開始教授估值課程，也寫了很多估值方面的書，是估值方面的權威和大師。他現在主要在教MBA學生Corporate Finance（企業金融學） 和Valuation（估值）課程。

和其他的老師不同的是：他的課很有趣也很新。一開始在NYU他被要求教學生Security Analysis,也就是巴菲特的老師在1950年教的。Aswath沒有選擇比較無聊有歷史的證券分析課程，而是自創估值課程。在80年代，市場上沒有什麼相關材料可以用來教學，所以Aswath向市場學習，一路估值一路總結，一邊教一邊分析市場上的公司一邊學估值。所以30多年研究教學下來，Aswath奠定了估值教父的地位。

還有一點不一樣的是他不僅僅教MBA的學生，他有自己的[youtube頻](https://www.youtube.com/c/AswathDamodaranonValuation/featured)道和[自己的網站](http://people.stern.nyu.edu/adamodar/New_Home_Page/home.htm)，沒有任何廣告，他歡迎任何想學的人和他學習，提供所有的支援材料（ppt，表格，背景材料，閱讀材料，甚至習題和習題答案）。其實他的課程如果放到什麼Coursera上面，隨便都可以每人收成千上萬美金。他的網站和YouTube頻道，只要開放廣告，每月可以收成千上萬美金甚至更多。但是對於這樣業餘自學的學生，他分文不取，提供最多的資料和最大的便利。

從這一點上來看，他真的是我遇到過的最不愛錢的老師。

## **估值很簡單，基本上每個人都可以做到。但是相關從業人員故意選擇把它包裝的複雜化，因為他們要以此為生。**

 

演講只有40多分鐘，沒有辦法包含所以細節，所以Aswath主要從以下四個我們常常誤解的地方做闡述：

# 1.估值不是會計。Don't mistake accounting with finance. Valuation is not accounting.

絕大部分的人聽到估值，想到的是財務和財務報表，以為研究財務報表就可以做好估值。

Accountting，會計做的事是記錄歷史，而估值需要預測未來。

比如分析會計資產負債表的話：

1）Fixed Asset，固定資產是按成本價記錄，並且每年折舊，所以會有價值和價格不符的時候。

2）Fiancial Investment(Investment in securities and assets in other firms),金融資產根據不同的主觀動機記錄。有些是根據市場價格，有些是根據成本價，根據管理層的需要選擇什麼樣的記錄方法。

3）Intangible Assets,無形資產諸如品牌、專利、研發之類的並未列入，相反的沒什麼用的Goodwill（商譽）反而佔了無形資產的大部分。而商譽只是Plug variable，也就是為了收購時資產負債表的平衡創造出來的一個變量。商譽只有在收購時才會產生，比如收購價10億，但是公司市值5億，剩下5億的差額就需要當成商譽記錄以確保資產負債表平衡。如果一個好公司從來沒有收購過，那麼Goodwill為零。

4）Equity（股東權益），一般會計會教：資產-負債=股東權益。但是Aswath認為股東權益是反映原始的資本投資和企業從誕生以後累積的[留存收益](https://zh.wikipedia.org/wiki/%E7%95%99%E5%AD%98%E6%94%B6%E7%9B%8A)。股東權益反映的是過去的成績，如果一個公司剛剛新創，當然不能期待有多大的股東權益。

解決上面提到的問題，Aswath用Finance balance sheet 取代 Accounting balance sheet。

Finance balance sheet：

**Assets資產:**

**Assets in place：**公司已經做的投資，用內含價值根據現金流和風險計算，而非成本價格計算。

**Growth Assets：**是預期公司未來做的帶來高增長的投資。

商學院教的大部分的指標都是適用於成熟的公司，比如PE,ROC等等。如果用這些指標來評判成長型的公司，會覺得無從下手,因為很多的成長型公司都沒有盈利，PE無從計算。

所以對於成長型公司，大部分的研究時間要放在未來的增長預期上。因為會計只是記錄已有的交易，所以從一般的會計報表裡無法找到關於未來的Growth assets部分。

**Liabilities負債：**

**Debt：**負債，借的錢。

**Equity：**權益，自有資本。

# 2\. Don't mistake modeling for valuation.估值不僅僅是用excel做出的花俏報表。

我們常常會以為一堆報表做出來的好看電子表格的Excel專家是估值大師。其實不是。

在估值前，我們要思考的是是什麼主導了公司的價值增長。主要要尋找下面四個問題的答案：

1. What are the cash flows from existing assets?現有資產帶來的現金流量如何？

2. What is value-added by growth assets?未來增長資產創造的價值如何？

增長可以是正面的，中性的或者負面的，因為增長也需要成本，只有增長的價值大於成本才有價值。增長創造的正向價值才是重要的觀察點。

3. How risky are the cash flows from existing assets and growth assets?兩種資產的現金流風險多高？

4. When will the firm become a mature firm and what are the potential roadblocks?公司何時成為成熟公司？有哪些潛在障礙？

所以尋找這些問題的答案可不僅僅是報表製作的多漂亮而已。

有了上面四個問題的答案，就可以用Discount Cash Flow（現金流折現法）來估值。

為了更好的估值，Aswath提出了一個概念：

## Are you numbers people? Are you story people?你是數字派的？還是故事派的？

 

估值有兩派。

- 數字派。數字派只講數字，一切數字說話。

- 故事派。故事派只講故事，不看數字，只談夢想，比如VC（風險投資）產業。

問題在於，故事派的人認為數字派的都是宅男Geek，而數字派的人認為故事派的人都是瘋狂夢想家。

Aswath認為要做好估值，兩派要互相取長補短，也就是數字派的人需要有想象力，故事派的人需要足夠多的數據來檢視自己是不是太空想。

## 對於每個想學好估值的人來說，最關鍵的是：如果你是數字派的，逼自己多些想象力，如果你是故事派的，逼自己多找些數據來支撐自己的故事，想想自己哪方面比較欠缺，就在哪方面多努力。

 

# 3.價格和價值不一樣，Are you pricing or valuing？

比如一套房子售價99.5萬美金。價格是怎麼來的，是房屋經紀人比較周圍的房子價格然後根據這套的情況加加減減得出的。那banker對於公司的價格怎麼得出的，和房屋經紀人一樣，找出類似的公司，然後再稍作調整，得出價格。

所以是價格，而非估值。影響價格和估值的因素完全不同。

影響價格的因素有：Demand and Supply需求和供給，Mood and Momentum情緒和動能。

影響估值的因素有：Cash flow， growth and risk。

所以二者不同。

在比較了市場上所有的社交媒體價格後，Aswath發現市場給這些媒體公司的定價有個相似的規律，那就是用戶，一個用戶100美金。所以，市場上的大部分的價格都不是估值。

# 4.投資其實更靠運氣。Don't mistake luck for skill.

Investing is a game where luck is dominant paradigm.投資是由運氣起決定性作用的一種遊戲。

我們常常認為投資獲得成功是靠專業才能和努力，其實不是，其實是靠運氣。

當我們在投資上賺錢了後，我們會以為我們很有才能。比如有很多共同基金經理人覺得自己很厲害，其實所有共同基金的收益平均下來比標準普爾指數低一個百分點。

# 5.學估值就是要不斷的去估公司。The final lesson。。。I firmly believe you learn valuation by doing.

Aswath舉了 The Wizard of OZ這部電影當例子。Dorothy被龍捲風卷到一個奇怪的地方OZ，她問OZ的居民她要怎麼回家，大家告訴她：去找Wizard of OZ.他是OZ王國裡最偉大的魔法師。

Dorothy在路上認識了稻草人、錫人和獅子。稻草人覺得自己很笨想問wizard of OZ要一顆頭腦，錫人沒有愛心所以想要一顆心，獅子很膽小所以想要勇氣。他們四個人一路披荊斬棘來到Wizard of OZ的城堡才發現原來偉大的魔法師只不過是個躲在佈簾後面操作投影機的小老頭，沒有任何魔法。

雖然沒有魔法師，但他們卻發現正是通過這個旅程，因為有了這段歷練，他們全部的人最後都得到了自己想要的。

所以，就像想要做好飯就需要做飯一樣，想要做好估值就要不斷的去估。

 

最後Aswath提到，如果你竭盡所能做好了估值，價格回歸到價值還需要一段時間，而且不是100%的，這個時候就需要信念。

 

上面只是我的學習心得，歡迎有興趣的人一定要看Aswath的[Valuation in Four Lessons](https://www.youtube.com/watch?v=Z5chrxMuBoo&t=2667s) 的演講。除此之外，Aswath的頻道和網站還有更多的大師智慧等著感興趣的人去學習。

鏈接如下：

[youtube頻](https://www.youtube.com/c/AswathDamodaranonValuation/featured)道和[Aswath的網站](http://people.stern.nyu.edu/adamodar/New_Home_Page/home.htm)
