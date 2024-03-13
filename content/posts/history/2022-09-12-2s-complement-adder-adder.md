---
title: "從0和1開始造一台電腦2-電腦如何運算/2補數/加法器/ALU"
date: "2022-09-12"
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

人類是高等動物，具有邏輯思考能力，所以我們使用文字和阿拉伯數字來傳遞信息。

但是有很多場合，當無法用文字和數字來處理信息時，也會用二進制來傳遞或處理信息。

## 二進制是最簡單的溝通語言

比如：

- 烽火

烽火有兩種狀態，點燃為1，表示有敵人來犯；沒點燃為0，表示沒有敵人。所以總共有2^1=2種。

- 盲文

盲文是用突起的點和不突的點來表示信號，但是只有2種遠不夠我們表達，所以使用6種，這樣總共有2^6=64種不同的方式，足夠表達26個字母和10個數字等等。

![](images/Screen-Shot-2022-09-08-at-7.16.52-AM-1-1024x716.png)

source:wiki

- 摩斯密碼

摩斯密碼使用點和長線這兩種基本元素來表示信息，比如SOS的表示法是三點三線三點，如果在夜晚使用手電筒來打SOS求救的話，就是亮三次短的S，再亮三次長的O，在亮三次短的（S）。

5位的摩斯密碼總共有2^1+2^2+2^3+2^4+2^5=2+4+8+16+32=62種不同的組合，表示字母和數字足夠了。

![](images/Screen-Shot-2022-09-11-at-10.21.19-AM-837x1024.png)

source：[https://en.wikipedia.org/wiki/Morse\_code](https://en.wikipedia.org/wiki/Morse_code)

- Punched card（打孔卡）,是利用機器在一張紙卡上用打洞和不打洞來表示信息。基本的構成也是二進制。

![](images/Screen-Shot-2022-09-11-at-10.16.59-AM-1024x844.png)

IBM 96-column punched card

## 電腦為什麼使用二進制

電腦遠遠沒有人類聰明，給電腦使用簡單易懂的二進制是最適合的選擇。

### 1.二進制是傳遞信息最小的進制

比二進制還小的進制就只有一進制了，但是一進制始終只有一種狀態，無法溝通。

比如點頭和搖頭是二進制，就算不會講話，只靠點頭和搖頭就可以基本溝通。但是如果只有一種點頭，顯然無法溝通。

### 2.不容易出錯

二進制只有兩種狀態，非此即彼，不容易出錯。

比如烽火，只有點或不點，沒有模糊不清，可以保證信息有效傳遞。設想電腦每秒運行兆次，要保證中間沒有任何差錯。

### 3.二進制和電流電路相聯繫

電腦是電器，靠的是電流的流動。沒有電流的流動，再精密的電腦也成一堆廢鐵。

電流只有兩種狀態，就是通電和斷電。通電表示1，斷電表示0。開表示0，關表示1。

比如上篇文章提到的NAND 邏輯閘。兩個輸入都是斷電（0）時，結果是通電（1）。

![](images/Screen-Shot-2022-09-10-at-8.07.17-AM.png)

NAND gate示意圖，source：CODE

真值表：

<table><tbody><tr><td>NAND</td><td>0</td><td>1</td></tr><tr><td>0</td><td>1</td><td>1</td></tr><tr><td>1</td><td>1</td><td>0</td></tr></tbody></table>

### 4.二進制計算簡單

乘法實際上是加法，除法實際上是減法，減法實際上是加個負數，所以只要有加法計算和負數表示兩點，就可以實現所有的基礎運算。

而我們設定電腦中負數使用2的補碼+1的方式來表達。所以負數的表示也變成了加法。

所以，只要有加法就有了一切的基本運算。

而二進制的加法也最簡單，只有0+0，0+1，1+1三種。

所以，我們利用電腦強大的運算能力，用二進制，從最簡單的0和1（開和關），最後形成聯繫數十億人的有史以來最大的交流網絡。就像道德經裡說的：大道至簡，衍化至繁。

## 用二進制表示數字

像在摩斯密碼中用3個點表示S一樣，我們可以給電腦規定如何表示數字。

如果只有3個位元，總共有8種組合，可以用規定如下表達方式：000（0），001（1），010（2），011（3），100（4），101（5），110（6），111（7）。

如果有8個位元，總共可以表示2^8=256個組合。

我們可以用這256個組合來表示全部的正整數（0到255），這樣就是unsigned。

也可以用這256個組合來表示正數和負數，這樣就可以表示（-128到127），這樣叫做signed，意思是第一位可以視為符號位。

![](images/Screen-Shot-2022-09-11-at-3.46.36-PM.png)

## 負數表示法-2's complement

我們想怎麼表示負數都可以，但是怎麼設計負數的表示法來讓運算更加方便呢？

電腦系統採用”1補數+1“即2補數來表示負數。在2進制中，1的補數是1，1的補數是0。轉化成10進制，就是對9取補數，5的補數是4，3的補數是6等等。

使用此方法主要是為了讓減法更方便運算，避免借位問題。另外2進制中取反即是相反數，用一個反向器（NOT gate）即可實現。

比如1+（-1）=0，1的8位元二進制是表示是0000 0001，先計算1的補數即每一位取反：1111 1110。

0000 0001+1111 1110 = 1111 1111 答案並不是0。

想要讓答案是0，必須再加1，也就是變成1 0000 0000，最高位是overflow，超出8位元之外，自動捨棄。先取1的補數再加1，這樣就是2補數。這樣減法操作起來更簡單，減一個正數就變成加上負數。

[CODE（The Hidden Language of Computer Hardware and Software)](https://bobcarp.files.wordpress.com/2014/07/code-charles-petzold.pdf)這本書的第13章有更詳細說明為什麼用2補數來表示負數。

下面用10進制來幫助理解，比如要計算81-37，個位數1減9不夠減，涉及到借位，這增加了複雜度。為了避免借位，那就用：

81-37+99+1-100

可以變成81+**( (99-37）+ 1** ) - 100

99-37=62，即對3和7取9的補數，3的補數是6，7的補數是2。所以十進制中-37的表示法就是9的補數再加1在減100。

81+（62+1）-100 =44

轉換成二進制的話，81的2進制是101 0001，37的2進制是010 0101。

81+**( (99-37）+ 1** ）-100 變成（十進制最大數字9，二進制最大數字1）

101 0001 - 010 0101 +（111 1111 +1 ）-1000 0000

101 0001+（111 1111-010 0101+1）- 1000 0000

\=101 0001 +（101 1010 +1） -1000 0000

\=101 0001 +101 1011 - 1000 0000

\=1010 1100 -1000 0000

\=010 1100 即10進制的44。

所以所謂的2補數表示負數的方法，後面還有一個捨棄最高位overflow的問題。

```
81-37=
81+（-37）=
81+（-37）+99 +1 -100=
81+（99-37）+1-100=
81+（（（99-37）+1）-100）
```

**即（-37）**\=**（（（99-37）+1）-100）**

其中**（99-37）+1）**即是9的補數加1，**\-100**即是捨棄最高位overflow。

所以，因為電腦會自動捨棄overflow的位元，所以我們才可以理所當然的使用1補數加1來表示負數。

小結：

負數等2補數很難理解，就是因為沒注意到電腦的特性自動會最高位overflow。

**正確的表達應該是負數等於”1的補數+1-最高位overflow“，因為最高位自動會overflow，所以負數的簡化表達即是**”1的補數+1****，**簡稱”2補數“。**

## 加法如何計算-半加器/全加器/16位加法器

電腦也叫計算機，其最大的能力就是計算。蘋果最新發布的A16 CPU，每秒實現近17 兆次運算。

負數是”1的補數+1“，所以減法可以變成加法；乘法實際上是連續的加法；除法實際上是連續的減法。只要有加法計算就可以實現所有的基礎運算。

### 半加器

想要做加法，先從一位的加法開始。

一位的加法有四種可能：

![](images/Screen-Shot-2022-09-12-at-8.27.03-AM.png)

sum表示加法位，carry表示進位。

仔細觀察可以發現sum的輸入輸出和XOR一樣，carry的輸入輸出和AND一樣。所以用XOR和AND就可以做出半加器。

（提醒：下圖含有課程作業代碼，請仔細考慮是否閱讀）

下面是用Nand2tetris 課程提供的HDL語言寫出的半加器，只是把信號輸入兩個邏輯門，然後每個邏輯門各輸出一個信號。可以把a/b看成兩個輸入管腳，把sum/carry看成兩個輸出管腳。所以雖然沒有自己動手連結管線（工程太大），用硬體模擬的形式幫助理解背後的原理。

```
CHIP HalfAdder {
    IN a, b;    // 1-bit inputs
    OUT sum,    // Right bit of a + b 
        carry;  // Left bit of a + b
PARTS:
    Xor(a=a,b=b,out=sum);
    And(a=a,b=b,out=carry);
}
```

### 全加器

半加器只是加了兩個數字，但是遇到有進位的時候，還需要加上第三個進位的數字。全加器就是考慮了前兩個數字的進位，把進位當作第三個輸入管腳，把三個數字相加。

![](images/Screen-Shot-2022-09-12-at-9.04.28-AM.png)

來源：[From Nand to Tetris](https://www.nand2tetris.org/)課程

實作起來也很簡單，就是把兩個半加器相連，先加兩個數字，用得出的結果再和進位相加。

```
CHIP FullAdder {
    IN a, b, c;  // 1-bit inputs
 c=carry bit
    OUT sum,     // Right bit of a + b + c
        carry;   // Left bit of a + b + c

    PARTS:
    // Put you code here:
    HalfAdder(a=a,b=b,sum=tmp1,carry=tmp2);
    HalfAdder(a=tmp1,b=c,sum=sum,carry=tmp3);
    Or(a=tmp2,b=tmp3,out=carry);
}
```

### 16位加法器

上述的全加器是1個bit的加法器，如果有很多的bit，可以把很多的全加器連在一起，形成多位元的加法器。

此加法器不考慮位元溢出（overflow），即最後一位進位的問題。

```
CHIP Add16 {
    IN a[16], b[16];
    OUT out[16];

    PARTS:
   // Put you code here:
   HalfAdder(a=a[0],b=b[0],sum=out[0],carry=temcarry0);
   FullAdder(a=a[1],b=b[1],c=temcarry0,sum=out[1],carry=temcarry1);
   FullAdder(a=a[2],b=b[2],c=temcarry1,sum=out[2],carry=temcarry2);
   FullAdder(a=a[3],b=b[3],c=temcarry2,sum=out[3],carry=temcarry3);
   FullAdder(a=a[4],b=b[4],c=temcarry3,sum=out[4],carry=temcarry4);
   FullAdder(a=a[5],b=b[5],c=temcarry4,sum=out[5],carry=temcarry5);
   FullAdder(a=a[6],b=b[6],c=temcarry5,sum=out[6],carry=temcarry6);
   FullAdder(a=a[7],b=b[7],c=temcarry6,sum=out[7],carry=temcarry7);
   FullAdder(a=a[8],b=b[8],c=temcarry7,sum=out[8],carry=temcarry8);
   FullAdder(a=a[9],b=b[9],c=temcarry8,sum=out[9],carry=temcarry9);
   FullAdder(a=a[10],b=b[10],c=temcarry9,sum=out[10],carry=temcarry10);
   FullAdder(a=a[11],b=b[11],c=temcarry10,sum=out[11],carry=temcarry11);
   FullAdder(a=a[12],b=b[12],c=temcarry11,sum=out[12],carry=temcarry12);
   FullAdder(a=a[13],b=b[13],c=temcarry12,sum=out[13],carry=temcarry13);
   FullAdder(a=a[14],b=b[14],c=temcarry13,sum=out[14],carry=temcarry14);
   FullAdder(a=a[15],b=b[15],c=temcarry14,sum=out[15],carry=temcarry15);
}
```

## 電腦如何計算-ALU

[Arithmetic logic unit](https://en.wikipedia.org/wiki/Arithmetic_logic_unit),簡稱ALU，**算術邏輯單元**是一種可對二進位整數執行算術運算或邏輯運算的芯片，是CPU中的計算核心。

### ALU的示意圖：

![](images/Screen-Shot-2022-09-12-at-9.35.28-AM-1024x793.png)

來源：[From Nand to Tetris](https://www.nand2tetris.org/)課程

輸入：

- 數據輸入x和y，為兩個16位元的二進制數字
- 執行操作輸入（op code），為6個獨立的管腳（zx/nx/zy/ny/f/no)。

輸出:

- 數據輸出out，輸出執行操作後的數據結果
- 邏輯輸出zr/ng，對數據結果做邏輯判斷，如果數據輸出=0，zr為1，如果數據輸出為負值，ng為1。這兩個輸出為以後CPU執行程式語言的loop時使用JUMP。

### Op code，操作輸入管腳

ALU的操作輸入管腳包含算數運算和邏輯運算。

算數運算：

- add 比如f

邏輯運算：

- AND 比如f,zx,zy
- NOT 比如nx,ny,no

更多的op code請參考維基百科[Arithmetic logic unit](https://en.wikipedia.org/wiki/Arithmetic_logic_unit)。

神奇的是，這些op code組合在一起，就可以產生很多不同的結果。

![](images/Screen-Shot-2022-09-12-at-9.25.42-AM-1024x878.png)

來源：[From Nand to Tetris](https://www.nand2tetris.org/)課程

比如想要獲得x+y的結果，就要篩選op code: zx=0,nx=0,zy=0,ny=0,f=1,no=0.

也就是說輸入x和y,op code設置000010，得出的結果是x+y。

### 數據選擇器（MUX)

對x和y做了這麼多操作，怎麼讓電路給出正確的結果呢？

答案是數據選擇器（MUX)，單位元MUX的介面如下：

```
* Multiplexor:
 * out = a if sel == 0
 *       b if sel == 1
 */

CHIP Mux {
    IN a, b, sel;
    OUT out;

    PARTS:
    // Put your code here:
}
```

當控制管腳為0時，選擇數據a，當控制管腳為1時選擇數據b。

### ALU實作

可以看到下圖的ALU的作業代碼，使用了很多的16位元的MUX邏輯芯片來篩選數據。

```
CHIP ALU {
    IN  
        x[16], y[16],  // 16-bit inputs        
        zx, // zero the x input?
        nx, // negate the x input?
        zy, // zero the y input?
        ny, // negate the y input?
        f,  // compute out = x + y (if 1) or x & y (if 0)
        no; // negate the out output?

    OUT 
        out[16], // 16-bit output
        zr, // 1 if (out == 0), 0 otherwise
        ng; // 1 if (out < 0),  0 otherwise

    PARTS:
   // Put you code here:
        Mux16(a=x,b=false,sel=zx,out=x1);

        Not16(in=x1,out=notx1);
        Mux16(a=x1,b=notx1,sel=nx,out=x2);

        Mux16(a=y,b=false,sel=zy,out=y1);

        Not16(in=y1,out=noty1);
        Mux16(a=y1,b=noty1,sel=ny,out=y2);

        Add16(a=x2,b=y2,out=xAddy);
        And16(a=x2,b=y2,out=xAndy);
        Mux16(a=xAndy,b=xAddy,sel=f,out=result1);

        Not16(in=result1,out=result2);
        Mux16(a=result1,b=result2,sel=no,out[15]=tmp,out[0..7]=first8,out[8..15]=second8,out=out);

        Or8Way(in=first8,out=zr1);
        Or8Way(in=second8,out=zr2);
        Or(a=zr1,b=zr2,out=zr3);
        Not(in=zr3,out=zr);

        And(a=tmp,b=true,out=ng);
}
```

本文參考資料：

[CODE（The Hidden Language of Computer Hardware and Software)](https://bobcarp.files.wordpress.com/2014/07/code-charles-petzold.pdf)

[From Nand to Tetris 課程](https://www.nand2tetris.org/)

Crash Course Compter Science視頻：

https://www.youtube.com/watch?v=tpIctyqH29Q&list=PLH2l6uzC4UEW0s7-KewFLBC1D0l6XRfye
