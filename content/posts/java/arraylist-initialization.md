---
title: "Java Arraylist源碼中的三個構造器和三種對應的初始化方法"
date: 2024-05-10T14:06:42+08:00
lastmod: 2024-05-10T14:06:42+08:00
author: ["Ariel"]
keywords: 
- 
categories: # 没有分类界面可以不填写
- Java Arraylist
- 初始化方法
- 構造器
- Arraylist constuctor
- Arraylist initialization
tags: # 标签
- Java
- CS自學
description: ""
weight:
slug: "Java Arraylist源碼中的三個構造器和三種對應的初始化方法
"
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
ArrayList裡有三種構造函數，對應三種ArrayList的初始化方法。

## 第一種 容量為空初始化

源代碼：
```Java
/**
     * Constructs an empty list with an initial capacity of ten.
     */
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }

```

可使用如下方法初始化：
```Java
List<String> fruit = new ArrayList<>();
fruit.add("apple");
fruit.add("pear");
```

下面，來看一看代碼是如何一步一步執行的。

首先初始化arraylist：

```
List<String> fruit = new ArrayList<>();
```
呼叫ArrayList()構造函數執行：

```Java
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }
```

結果：this.elementData == {}

{{% admonition type=info title="note" %}}
**初始化後的ArrayList，是一個空的集合，容量為0。**
{{% /admonition %}}


當第一次add時，呼叫add(E e)，源碼：
```Java
    public boolean add(E e) {
        modCount++; /** modCount是為判斷是否有多線程操作某個集合，可忽略*/
        add(e, elementData, size);
        return true;
    }
```

其中的modCount在非線程安全的LinkedList、LinkedList和HashMap中都存在，為的判斷同一個JVM進程是否有多線程操作某個集合。

此處可忽略，執行add(e, elementData, size)，源碼：

```Java
    private void add(E e, Object[] elementData, int s) {
        if (s == elementData.length)
            elementData = grow();
        elementData[s] = e;
        size = s + 1;
    }
```
此時s==0，elementData==DEFAULTCAPACITY_EMPTY_ELEMENTDATA，length==0。

所以if條件為真，呼叫grow()

```Java
    private Object[] grow() {
        return grow(size + 1);
    }
```
呼叫grow(int minCapacity),執行grow(1)

```Java
    private Object[] grow(int minCapacity) {
        int oldCapacity = elementData.length;/** oldCapacity==0*/
        if (oldCapacity > 0 || elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            /** 條件不成立，跳過，進入else運算*/
            int newCapacity = ArraysSupport.newLength(oldCapacity,
                    minCapacity - oldCapacity, /* minimum growth */
                    oldCapacity >> 1           /* preferred growth */);
            return elementData = Arrays.copyOf(elementData, newCapacity);
        } else {
            return elementData = new Object[Math.max(DEFAULT_CAPACITY, minCapacity)];
            /** DEFAULT_CAPACITY==10， minCapacity==1，是上一步傳入的參數*/
        }
    }
```
條件不成立，跳過，進入else運算。

執行完畢後，elementData = new Object[10]。

即第一次add時，容量變更為10.

{{% admonition type=info title="note" %}}
**即第一次add後，容量變更為10。**
{{% /admonition %}}

接著執行add(E e, Object[] elementData, int s) 裡面接下來的代碼
```Java
elementData[s] = e;
size = s + 1;
```

這是第一種ArrayList的初始化方法後面執行的步驟。

## 第二種 自訂容量初始化

根據實際需要，指定容量初始化。

源碼：

```Java
    /**
     * Constructs an empty list with the specified initial capacity.
     *
     * @param  initialCapacity  the initial capacity of the list
     * @throws IllegalArgumentException if the specified initial capacity
     *         is negative
     */
    public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }
```

可使用如下方法初始化：
```Java
List<String> student = new ArrayList<>(32);
student.add("Bob");
student.add("Peter");
```

自訂容量初始化的好處是：
- 減少擴容的次數，提高效率。


## 第三種 根據現有集合初始化

根據現有的集合初始化。可以將其他集合轉為ArrayList，

源碼：

```Java
/**
     * Constructs a list containing the elements of the specified
     * collection, in the order they are returned by the collection's
     * iterator.
     *
     * @param c the collection whose elements are to be placed into this list
     * @throws NullPointerException if the specified collection is null
     */
    public ArrayList(Collection<? extends E> c) {
        Object[] a = c.toArray();
        if ((size = a.length) != 0) {
            if (c.getClass() == ArrayList.class) {
                elementData = a;
            } else {
                elementData = Arrays.copyOf(a, size, Object[].class);
            }
        } else {
            // replace with empty array.
            elementData = EMPTY_ELEMENTDATA;
        }
    }
```

比如可以將HashSet轉為ArrayList：

```Java
Set<String> students = new HashSet<>();
students.add("Bob");
students.add("Alice");

List<String> list = new ArrayList<>(students);
```
此時，list裡面包含兩個元素Bob和Alice。


如下方法通過Arrays.asList(array)方法構造ArrayList時，也是調用第三種構造函數。
```Java
String[] array = {"Bob", "Alice"};
ArrayList<String> students = new ArrayList<>(Arrays.asList(array));
```

參考：
[jdk source code](https://github.com/openjdk/jdk/blob/master/src/java.base/share/classes/java/util/ArrayList.java)