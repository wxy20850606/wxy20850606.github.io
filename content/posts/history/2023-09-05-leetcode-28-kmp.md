---
title: "字符串匹配，Leetcode 28，KMP算法"
date: "2023-09-05"
categories: 
  - "電腦科學"
keywords: 
- 算法
- 字符串匹配
- KMP
tags: # 标签
- 算法
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

最近在做Leetcode，遇到有關字符串匹配的如下題目：

[28. Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)

要求從haystack 字符串裡找是否包含needle字符串，並找出首次出現位置。比如sadbutsad裡是否包含sad字符串。

發現除了暴力循環解法外，還有神奇的KMG算法可以把複雜度從O(m\*n)簡化為O(m+n)。雖說大家都說用處不大，但是還是覺得很有意思，於是花了點時間學習，順便寫這篇文章記錄一下。

## KMP算法本質：

有些詞包含部分重複，比如ABCDABD，其中的部分字段ABCDAB，其首尾都是AB，在搜索遇到不匹配時，若不匹配的前幾個字母和後幾個字母一樣，可以跳過一樣的部分繼續匹配。

比如下面例子，發現第一個不匹配的字幕C。

![](images/Screenshot-2023-09-04-at-8.56.19-AM.png)

按照暴力解法，需要從haystack的第二個字母開始從頭搜索，直到再次找到第一個字母A。

![](images/Screenshot-2023-09-04-at-9.00.16-AM.png)

KMP算法利用了ABCDAB首尾相同的特點，即首尾都是AB，在搜索時跳過了相同的部分，直接從不相同的部分開始匹配，減少了很多重複的工作。

![](images/Screenshot-2023-09-04-at-9.07.32-AM.png)

那怎麼知道首尾有幾個字母相同呢，這就要靠特別計算表格。

比如ABCDA，最前面字母和最後面字母都是A，所以相同字符數為1。

![](images/Screenshot-2023-09-04-at-9.15.46-AM.png)

比如AAAAA，最前面有4個A，最後面有4個A，所以相同字符數為4。注意數相同字符數時不包含首尾字母本身。

![](images/Screenshot-2023-09-04-at-9.20.37-AM.png)

## 計算needle表格的最大首尾相同字母數：

最大首尾相同字母數的英文叫法是LPS。

Prefix，前綴，比如BABA前綴為：B,BA,BAB。

Suffix，後綴，比如BABA前綴為：ABA,BA,A。

![](images/Screenshot-2023-09-04-at-10.29.03-PM.png)

pseudocode code如下：

- 將needle string變為char array

- 初始化數組lps，java自動賦值0

- 設置prev指針，初始化為首字母，方便後來數首尾幾個字母相同

- 設置遍歷指針i，初始化為第二個字母，因為第一個字母已被設置為0

- 如果和首字母相同，lps\[i\]為lps\[prev\] + 1，兩指針同時後移接著比較

- 如果和prev指針不同，此時如果prev指針為0，直接設置lps\[i\]為0，因為前面已經沒有字母可以比較了。此時如果prev指針大於0，表示前面還有字母，此時將prev指針往前移動，看看前面是否有相同的字母，直到prev指針指向首字母為止。

舉例：

![](images/Screenshot-2023-09-04-at-9.20.37-AM.png)

遍歷到B，發現不同，此時prev指向第5個字母。

![](images/Screenshot-2023-09-04-at-9.54.38-AM.png)

一直讓prev往前移動，即往前找有沒有相同的首尾字母，直到prev為0。所以每次有不同的字母出現時，總會重新調整prev指針的位置。

![](images/Screenshot-2023-09-04-at-9.57.18-AM.png)

本來prev指針調整是用prev = prev -1，後來發現邏輯有誤。

如下面的例子，如果prev = prev - 1，此時prev指向B，和i指向的B相同，此時空白處的最長首字母更新為2，顯然事實不符。

![](images/Screenshot-2023-09-04-at-9.54.13-PM.png)

為什麼會出現這個錯誤呢？因為忘記了lps數組求的是最大首尾相同字母數，需要從首字母開始比較。

所以正確的作法是將prev=0，即發現不同直接退回第一個搜索，然後首尾比較，如果相同的話，需要將第二個和倒數第二個進行比較。這樣才能算出來首尾有多少個相同。但是這個方法同時移動prev和i指針，容易使代碼更複雜。

![](images/Screenshot-2023-09-05-at-8.15.41-AM.png)

最後看了如下視頻，才真的了解為什麼遇到不同時prev退回到lps\[prev-1\] 。

https://www.youtube.com/watch?v=4jY57Ehc14Y

原來邏輯和主程序一樣，也就是和上面舉的ABCDAB一樣，跳過重複的AB部分，直接從不同的C來比較一樣。此處lps\[prev-1\] 為3，表示首三位和尾三位相同，所以直接跳過前三個一樣的字母，從第四個開始比較。

![](images/Screenshot-2023-09-05-at-8.21.03-AM.png)

將prev跳過重複的部分，直接指向第一個不同的位置。

![](images/Screenshot-2023-09-05-at-8.29.33-AM.png)

比較後發現還是不同，繼續按同樣邏輯跳過首尾相同的部分。直到prev跳到首字母位置。

![](images/Screenshot-2023-09-05-at-8.32.15-AM.png)

Java代碼：

```
public static int[] countLps(String s){
        char[] str = s.toCharArray();
        int[] las = new int[s.length()];
        int prev = 0;
        int i = 1;
        while(i < s.length()){
            if(str[i] == str[prev]){
                lps[i] = lps[prev] + 1;
                prev++;
                i++;
            }else if(str[i] != str[prev]){
                if(prev == 0){
                    lps[i] = 0;
                    i++;
                }else{
                    prev=lps[prev-1];
                }
            }
        }
        return ops;
    }
```

## 遍歷haystack表格，跳過首尾相同的字母部分：

代碼如下，邏輯開頭已經解釋過了，就不贅述了。

```
    public int strStr(String haystack, String needle) {
        int[] lps = new int[needle.length()];
        lps = countLps(needle);
        int hayIndex = 0;
        int neeIndex = 0;
        while(hayIndex < haystack.length() && neeIndex < needle.length()){
            if(haystack.charAt(hayIndex) == needle.charAt(neeIndex)){
                hayIndex++;
                neeIndex++; 
            }else{
                if(neeIndex != 0){
                    neeIndex = lps[neeIndex - 1];   
                }
                else{
                    hayIndex++;
                }
            }  
        }
        if(neeIndex == needle.length()){
                return hayIndex - needle.length();
            }
        return -1;
    }
```

## 總結：

如果needle字符串首尾沒有任何相同的部分，比如ABCD，其效能和一般的暴力解法一樣。我查了下英文字典，發現符合條件的英文字母很少。如果有時間的話，可以下載所有的英文單詞表，寫個程式計算符合條件的字母占多少比例，我猜比例應該低於10%。

這個算法在字符串匹配上的實際用處沒有想像的大，比如Java的String.indexof()方法，也並沒有使用KMP算法。

[https://stackoverflow.com/questions/19543547/why-does-string-indexof-not-use-kmp](https://stackoverflow.com/questions/19543547/why-does-string-indexof-not-use-kmp)

好像在DNA搜索上面用的倒是比較多。

不過KMP背後的思路還是很巧妙的，尤其是使用雙指針移動的部分。
