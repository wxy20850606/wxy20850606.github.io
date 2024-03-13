---
title: "Git 原理學習筆記"
date: "2023-02-18"
categories: 
  - "電腦科學"
keywords: 
- Git
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

之前學CS50W的時候學過一些Git指令覺得不好理解，這次開始CS61B課程之前又再次需要用Git。既然無可避免，那就花些時間試著了解看看git到底是什麼。

注：本文為初學者學習筆記，請謹慎參考。

名詞解釋：

- hash function，給予內容（value），通過hash function產生某種key，形成key和value的對應關係

- SHA1，一種Git使用的加密hash程式，給予內容產生由40個16進制組成的key

- Merkle tree，hash tree的一種，是一種數據結構

- [Directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph)，有向無環圖，不斷前進、不會後退、有時分化、有時聚合的圖

- blob/tree/commit 三種Git主要的物件類型

- working/staging/committing三種工作階段

## Git是什麼？

Git is a content-addressable filesystem. Great. What does that mean? It means that at the core of Git is a simple key-value data store. What this means is that you can insert any kind of content into a Git repository, for which Git will hand you back a unique key you can use later to retrieve that content.
--[https://git-scm.com/book/en/v2/Git-Internals-Git-Objects](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects)

Git是基於內容（而不是文件名稱）的文件管理系統。

content-addressable指的是根據內容，與之對應的是檔案名稱。

比如文件夾裡最重要的每個文件在Git裡面對應的是blob物件，hash的時候根據這個物件的內容來產生SHA-1。相同內容但是不同文件名的文件的SHA1值一樣。這個就是content-addressable。

可以把Git想像成是標明地址的內容商店（content address store）。可以將任意內容交給這個商店，商店的工作是把內容處理下，產生一個40位16進制的地址key，這樣每個內容（value）就有了與之匹配的地址（key），形成key和value配對。如果之後想知道之前交給商店的內容是什麼，只要告訴商店地址，商店就會返回給你內容。如果提交相同的內容給商店，商店不會再存儲一次，而是使用原本的唯一的key。

## Git的核心-snapshots of commit

Git thinks about its data more like a **stream of snapshots**.
-[https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F)

比如在做一個項目，如果沒有Git的話，要如何管理文件呢？這裡假設每天保存一份版本。方法就是把今天的文件夾存檔，第二天在copy前一天的文件夾上面工作，第三天也在copy前一天的文件夾上工作。

下面這些文件夾相當於Git的每個commit。通過每個commit可以獲得一個文件夾裡面包含當時所有的文件。

![](images/Screenshot-2023-02-18-at-5.49.39-AM.png)

Git的每個commit就是每個時點上此文件夾下所有的文件組成的文件夾。

Git通過序列化將文件內容/commit內容存在名為40位字符（即SHA-1）的文件中（前2個字符當作文件夾名稱，後面的38個當作文件名稱，方便查找）。

當後續使用checkout需要讀取內容時，再使用反序列化從對應的文件(文件名為SHA-11)讀取內容。

通過commit的hash值方便我們立即獲得每個version的文件夾，時間複雜度為O(1)。

![](images/Screenshot-2023-04-14-at-4.13.59-PM.png)

## Git 的三種主要的object-blob/tree/commit

1.blob為文件的內容，不包含文件名稱。

```
blob = array(byte)
```

2.tree，一個tree裡可以包含blob也可以包含tree。

```
tree = map(string, tree/blob)
```

tree主要就是文件名稱和其hash值的對應關係。

```
100644 blob 05a682bd4e7c7117c5856be7142fea67465415e3	hello.txt
```

如果兩個文件hello.txt和helloworld.txt裡面的內容都是Hello World! 的話，其blob的hash值是一樣的，但是tree的hash值不一樣，因為tree裡面包含了文件名。那怕是一個字節的內容改變，通過SHA1這個hash程式得到的hash值都不一樣。

3.commit

可以把commit看成一個struct，裡面包含如下內容，主要是紀錄對應關係：

```
commit = struct
        {tree:array
         parents:array
         author:string
         message:string
         ...
        }
```

用git show查看任一commit如下：

```
% git show --pretty=raw 0deda1
commit 0deda15f3ab2c19306dd4fa07bf1454a3c9f7472
tree 0601df2007844393efb5adc315209398d21bf21b
parent dcf59207ff975ead86621fe56cc621a1b8aff4fb
author wangxiaoyan <wxy20850606@gmail.com> 1676687546 +0800
committer wangxiaoyan <wxy20850606@gmail.com> 1676687546 +0800

    add hello.py
```

還有第四種叫做tag，很多Git internal裡都會先跳過，這裡也先跳過。

至於怎麼回到任一版本的commit？可以通過git checkout，下面Git branch小節有說明。

## Git的數據結構-Merkle Tree

Git的數據結構是[Merkle Tree](https://en.wikipedia.org/wiki/Merkle_tree)，是一種hash tree。

hash是一種function，可以把一長串的內容加工成一個值，順便運行某種checksum（查核）。

```
了解hash function可參考：
Hash Tables - CS50 Shorts
```

一個好的hash function必須：

- 只用需要hash的內容

- 用全部的需要hash的內容

- 根據非常相同的內容（比如之差一個bit）可以產生差異巨大的hash值。

hash tree是什麼？簡單來說就是由hash組成的tree。最底層的是內容，由內容產生hash值，在由這個hash值和其他的內容產生新的hash值，再由這個新的hash值和其他的內容產生新的hash值，以此類推，直到最上層。

![](images/Screenshot-2023-02-18-at-8.57.09-AM-1024x650.png)

source：[https://zh.wikipedia.org/zh-tw/哈希树](https://zh.wikipedia.org/zh-tw/哈希树)

Git是由hash tree構成的commit形成的[Directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph)(DAG)。

directed 表示有方向，acyclic表示無循環，所以DAG中文可以叫做有向無循環圖。

![](images/Screenshot-2023-02-17-at-9.49.51-PM-1024x553.png)

具體來說，Git 在拍照片的同時用了一種單向 Hash 雜湊加密演算法，即_SHA-1_。（可參考：[https://en.wikipedia.org/wiki/SHA-1](https://en.wikipedia.org/wiki/SHA-1)）

這個SHA-1就是某種hash function，這個function有兩個功能：

1.通過hash function生成key-value pair，之後通過key可以輕鬆獲得內容。

給定任意內容（value），將內容轉為一串40個的校驗碼（key）。這樣就有了key-value pair。想要內容，通過key可以輕鬆獲取，使得時間複雜度為O(1)。

2.計算hash值的同時也運行某種checksum（驗證機制）。

checksum是一種通過內容本身的核實機制，比如信用卡的checksum（[Luhn algorithm](https://zh.wikipedia.org/zh-hans/卢恩算法)）可以通過卡號本身驗證信用卡是否有效。

Git對object計算hash值時哪怕只有一個bit內容的改變也會產生不同的hash值，這就同時運行了核實機制，保證歷史的commit不被竄改。這是Git設計最精妙的地方。

## 文件操作三階段working/staging/committing

可以想像成在編輯文件到挑選準備存儲的文件到保存文件的三個階段。本質上都是在進行文件的操作。

- working是打開各種文件，進行各種工作。

- staging是挑選出想要保存的文件，通過git add進入預備存儲區。

- committing就是把預備存儲的文件通過git commit進行存儲。最終保存在文件夾中。

Git diff可以比較各階段和commit之間的變化。詳細可查詢[git diff](https://git-scm.com/docs/git-diff)。

舉例用git diff --staged 查看staging階段有無變化：

```
% git diff
% code example.txt
% git diff
% git add example.txt
% git diff
% git diff --staged
diff --git a/example.txt b/example.txt
new file mode 100644
index 0000000..226d8eb
--- /dev/null
+++ b/example.txt
@@ -0,0 +1 @@
+git diff
```

## Git存在本地客戶端

可以安裝下面兩個指令：brew install tree可以看到文件夾的樹狀結構，brew install watch可以看到操作過程中文件夾到實時變化。

brew install tree

brew install watch

首先，mkdir example生成一個文件夾。

```
mkdir example
```

用git init生成一個Git 文件夾

```
% git init
Initialized empty Git repository in /Users/xiaoyanwang/Desktop/example/.git/
```

使用tree 指令可以看到此.git文件夾的結構：

![](images/Screenshot-2023-02-18-at-6.39.16-AM.png)

加入文件hello.txt，內容為Hello!

```
example % touch hello.txt
example % ls
hello.txt
```

此時處在working階段，可以查看到.git文件夾無任何變化。

使用git add 把working的內容加入到staging 階段。

```
git add hello.txt
```

可以觀察到.git文件夾發生了如下變化：

![](images/Screenshot-2023-02-18-at-6.47.25-AM.png)

使用git show，發現多出來的hash值對應的內容是文檔的內容。

```
% git show 05a682
Hello!
```

commit後發現.git文件夾發生兩個變化：objects裡的id從一個變為3個，heads裡面多了一個main。這個main即是main branch的意思。

```
xiaoyanwang@Xiaoyans-MacBook-Air example % git commit -m 'first commit'
[main (root-commit) deb9253] first commit
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt
```

![](images/Screenshot-2023-02-18-at-6.57.38-AM.png)

檢查這兩個object的hash值，發現tree裡面存儲了文件名稱和其對應的hash值。這也從一方面反映了只要文件內容不變，文件名稱改變不會影響blob的hash值，所以Git 是content-addressable。

```
% git show --pretty=raw e1f29ae
tree e1f29ae

hello.txt

% git ls-tree e1f29ae
100644 blob 05a682bd4e7c7117c5856be7142fea67465415e3	hello.txt
```

檢查commit的hash值發現commit包含的內容如下：

```
 % git show --pretty=raw deb925
commit deb92531534a8401aac0fa57b34faa99ef7226ab
tree e1f29ae07480fdb3d0337926ddbcebf9977266cf
author wangxiaoyan <wxy20850606@gmail.com> 1676674557 +0800
committer wangxiaoyan <wxy20850606@gmail.com> 1676674557 +0800

    first commit

diff --git a/hello.txt b/hello.txt
new file mode 100644
index 0000000..05a682b
--- /dev/null
+++ b/hello.txt
@@ -0,0 +1 @@
+Hello!
```

由上面三個objects可以發現：

- 先產生Blob的hash值，就是將內容生成SHA1

- Tree 記錄該目錄下有哪些檔案(檔名、內容的SHA1)和 別的Trees（如果有的話），通過這所有的內容產生新的SHA1

- Commit 記錄 commit 訊息、Root tree 和 Parent commits 的 SHA1等，通過這所有的內容產生新的SHA1

這就是Git的資料結構Merkle tree。也就是由hash值一層層往上產生新的hash值。

## Git遠程託管在Github

可以把Github看做是雲端託管文件夾的平台，在Github上生成一個repository，使用git clone 把文件夾複製到本地客戶端，然後修改完再使用git push把修改的內容推送至Github。

```
git clone [remote-repo-URL]
git remote add [remote-repo-name] [remote-repo-URL]
git remote -v
git pull [remote-repo-name] master
git push [remote-repo-name] master
```

## Git branch

Git 的branch就是一個pointer。

main是默認的branch，可以用git branch 產生新的branch：

```
% git branch cat
% git branch
  cat
* main
```

發現refs下面多了一個cat：

![](images/Screenshot-2023-02-18-at-10.56.24-AM.png)

檢查發現這兩個branch其實都是SHA1，可以想像成pointer。

```
% cat .git/refs/heads/main  
58d258ddfb2bf799c604fe756a224db9ac8f75fd
% cat .git/refs/heads/cat 
0deda15f3ab2c19306dd4fa07bf1454a3c9f7472
```

這個pointer指向某個commit：

```
% git show --pretty=raw 58d258
commit 58d258ddfb2bf799c604fe756a224db9ac8f75fd
tree 9fc3dc87ef995e394a6120d4ff1c56ef9e9a98ac
parent 0deda15f3ab2c19306dd4fa07bf1454a3c9f7472
author wangxiaoyan <wxy20850606@gmail.com> 1676687733 +0800
committer wangxiaoyan <wxy20850606@gmail.com> 1676687733 +0800

    add test.py
```

可以使用git checkout在不同branch轉換，這會影響下一個commit裡面的parent是指向誰。

```
%git checkout main
Switched to branch 'main'
```

查詢在哪一個branch可以用git status：

```
% git status
On branch main
```

不同branch的作用如下：

假設現在在cat branch，新增加一個文件test2.txt，git add 然後git commit。

```
commit e04cc362df3d92db26d1a4dc40851792f507a434 (HEAD -> cat)
Author: wangxiaoyan <wxy20850606@gmail.com>
Date:   Sat Feb 18 11:02:15 2023 +0800

    test2.txt
```

用git show查看，可以看到裡面的parent的hash值和上面通過cat .git/refs/heads/cat 查看的值一樣。

```
git show --pretty=raw e04cc3
commit e04cc362df3d92db26d1a4dc40851792f507a434
tree dd98fab7f54945f94975bb2cb564cafb94a0ea8b
parent 0deda15f3ab2c19306dd4fa07bf1454a3c9f7472
author wangxiaoyan <wxy20850606@gmail.com> 1676689335 +0800
committer wangxiaoyan <wxy20850606@gmail.com> 1676689335 +0800

    test2.txt
```

這個pointer是什麼呢？查看後發現這個pointer是上次的commit。也證明了commit後parent的SHA1是此branch上的上一次的commit。

```
git show --pretty=raw 0deda15
commit 0deda15f3ab2c19306dd4fa07bf1454a3c9f7472
tree 0601df2007844393efb5adc315209398d21bf21b
parent dcf59207ff975ead86621fe56cc621a1b8aff4fb
author wangxiaoyan <wxy20850606@gmail.com> 1676687546 +0800
committer wangxiaoyan <wxy20850606@gmail.com> 1676687546 +0800

    add hello.py
```

如何回到過去的commit？

比如當前我的git log如下：

```
 % git log     
commit e04cc362df3d92db26d1a4dc40851792f507a434 (HEAD -> cat)
Author: wangxiaoyan <wxy20850606@gmail.com>
Date:   Sat Feb 18 11:02:15 2023 +0800

    test2.txt

commit 0deda15f3ab2c19306dd4fa07bf1454a3c9f7472
Author: wangxiaoyan <wxy20850606@gmail.com>
Date:   Sat Feb 18 10:32:26 2023 +0800

    add hello.py

commit dcf59207ff975ead86621fe56cc621a1b8aff4fb
Author: wangxiaoyan <wxy20850606@gmail.com>
Date:   Sat Feb 18 06:06:33 2023 +0800

    first commit
```

對應的工作區文件夾如下：

![](images/Screenshot-2023-02-18-at-2.04.35-PM-1024x270.png)

使用git checkout可以恢復成當時第一個commit時的工作區文件夾情況（working tree）。

```
git checkout dcf5920
```

![](images/Screenshot-2023-02-18-at-2.04.01-PM-1024x294.png)

不過不用擔心，文件並沒有被刪除，使用git checkout main即可恢復。

```
% git checkout main
```

![](images/Screenshot-2023-02-18-at-2.04.35-PM-1024x270.png)

也就是說使用git checkout指令可以把工作區恢復到任何一個commit的時點。

## 用python寫一個Git

[Write yourself a Git!](https://wyag.thb.lt)

我只是看了一遍，還沒有自己跟著寫，覺得裡面有很多operating system的內容，避免坑挖的太多，先暫停。

總結：

Git的各種操作就是可以看成是對文件夾的操作。稍微理解了Git的原理後發現Git設計的很簡單，但是功能很強大，也感覺Git的指令親近了很多。

接下來會開始CS61B的學習，希望學完一點演算法後可以對Git有更深入的了解。

下面是我覺得不錯的Git學習資料，也是寫本文的參考資料：

CS61B課程 [Live lecture 12](https://youtu.be/fvhqn5PeU_Q)，教你怎麼自己做個git。

[https://csdiy.wiki/必学工具/Git/](https://csdiy.wiki/必学工具/Git/)

[Git internal](https://www.youtube.com/watch?v=lG90LZotrpo)

[Introduction to Git with Scott Chacon of GitHub](https://www.youtube.com/watch?v=ZDR433b0HJY)

[Understanding Git — Data Model](https://konrad126.medium.com/https-medium-com-zspajich-understanding-git-data-model-95eb16cc99f5)

[https://missing.csail.mit.edu/2020/version-control/](https://missing.csail.mit.edu/2020/version-control/)

[https://git-scm.com/book/en/v2/Git-Internals-Git-Objects](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects)

Git 內部原理：[https://ihower.tw/git/internal.html](https://ihower.tw/git/internal.html)

[Git 天天用 但是 Git 原理你了解吗？](https://blog.csdn.net/ljk126wy/article/details/101064186)

[Complete list of all commands](https://git-scm.com/docs)
