
---
title: "用Hugo將Wordpress部落格搬家到Github Pages"
date: 2024-02-17T14:36:03+08:00
# bookComments: false
# bookSearchExclude: false
---

# 決定從Wordpress搬家的原因

最近在寫關於Java學習文章的時候使用了markdown的插件，但是不好用，於是想不如折騰下自己的網站好了。學寫程式也有一陣子了，邊折騰邊學習也不錯。

用Wordpress部落格有幾年了，比較不喜歡：
1. 常常會擔心租的域名或託管服務有沒有到期，有心裡負擔
2. 太多我用不到的功能，不符合極簡主義
3. 我不喜歡我自己之前的域名

所以決定從Wordpress搬家，使用靜態網頁，託管在Github Page上。

在搬家的過程中，順便紀錄一下過程。

# 搬家所有步驟
搬家總共有如下幾個步驟：
- 從Wordpress 導出舊文章
- 創建username.github.io網站並部署與Github Pages上
- 選擇靜態網頁生成器
- 建立新的靜態網頁
- 用Hugo設置Github Pages
- 導入舊文章

## 1. 從wordpress導出

### 1. 導出xml

Wordpress的Tools選單裡有Export，點Export，選擇All content,然後就可以下載成xml文件。

### 2. xml to Markdown

靜態網頁要發文，要把文章以Markdown語法寫成「原始碼」，之後讓靜態網頁生成器「編譯」成一頁頁的HTML頁面。
所以要把舊博客里的文章變成**.md**結尾的文章

#### 下載 wordpress-export-to-markdown 軟體包
    mkdir oldpost  
    cd oldpost
    git clone https://github.com/lonekorean/wordpress-export-to-markdown
    cd wordpress-export-to-markdown

### 3. 轉檔

將之前下載的xml文件放入wordpress-export-to-markdown文件夾中

運行：
      `` npm install && node index.js `` 

按提示選擇yes或no,就可以成功轉檔。

### 4. 手動整理Markdown數據

初步整理Markdown文檔包括兩部分：
- 把vs code提示紅字的部分，按照Markdown的語法規則改掉
- 把內部連接刪除，這部分有待查證，反正我是改掉了
此步驟我花了大概75分鐘調整約200篇文章

## 2. 創建username.github.io網站

這一步比較簡單，按照官網提供的步驟一步一步設置即可。
https://pages.github.com

最後，會顯示網站已完成。

## 3.選擇靜態網站生成器

靜態網站生成器負責把Markdown文件轉為html頁面，有很多生成器可以選擇。
我有試過Vuepress/Vitepress/Docusaurus等，除此之外還有Jekyll/Gatsby，最後選擇了Hugo，因為使用其他的有遇到很多bug。
當然Hugo有很多優點，比如最大的優點就是速度快。


## 4. 生成Hugo網站

按照[這篇文章](https://ivonblog.com/posts/build-a-website-with-hugo/)的步驟設置Hugo網站，直到出現
http://localhost:1313。
{{ $image := .Resources.Get "hugofinish.png" }}
可以先不要安裝自己喜歡的theme，免得把問題複雜化。

增加文章測試是否成功。

## 5. 用Hugo設置Github Pages

### 1. 設置baseURL
在hugo.toml文件中更改baseURL
     `` baseURL = 'https://wxy20850606.github.io/' `` 
    
### 2. 將Hugo blog push到github
     `` hugo server -D `` 
     `` git add . && git commit -m 'new hugo blog' -a && git push `` 

### 3. 設置Github Pages，啟用Hugo blog
主要就是按照[指示](https://gohugo.io/hosting-and-deployment/hosting-on-github/)，新建文件``.github/workflows/hugo.yaml``。

## 6.導入舊文章

