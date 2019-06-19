# LinkedHashMap 学习 

----------
<!-- TOC -->
- [LinkedHashMap 学习](#linkedhashmap-学习)   
 - [1. 链表的建立](#1-链表的建立)      
 - [1.1 建立过程示意图](#11-建立过程示意图)  
 - [2. 链表的删除](#2-链表的删除)    
 - [3. 链表顺序的维护](#3-链表顺序的维护)   
 - [3. LinkedHashMap的应用](#3-linkedhashmap的应用)       
 - [3.1 缓存的应用](#31-缓存的应用)   
 - [参考](#参考)<!-- /TOC -->

![](https://s2.ax1x.com/2019/06/12/VR6jQs.png)

<br>

    public class LinkedHashMap<K,V>
    extends HashMap<K,V>
    implements Map<K,V>

>LinkedHashMap直接继承自HashMap,实际上他对于HashMap来说只是多了一条维护数据顺序的双向链表，因为HashMap没法按照数据的插入顺序打印数据。因此维护一条双向链表来记录顺序就实现了记录插入顺序的要求。

<br>

>正如图中展示的，一条绿色的链表正向记录顺序，一条红色的链表反向记录顺序。

<br>

>为了实现这个链表结构，他实现了如下的代码 

    static class Entry<K,V> extends HashMap.Node<K,V> {
        Entry<K,V> before, after;
        Entry(int hash, K key, V value, Node<K,V> next) {
            super(hash, key, value, next);
        }
    }

>实现了一个LinkedHashMap结构继承自HashMap，其中添加了befre,after结构记录他的前后节点

<br>

## 1. 链表的建立

<br>
>他的大部分代码基本都是使用了HashMap的逻辑，他主要是重写了newNode方法 


    Node<K,V> newNode(int hash, K key, V value, Node<K,V> e) {
    LinkedHashMap.Entry<K,V> p =
        new LinkedHashMap.Entry<K,V>(hash, key, value, e);
    // 将 Entry 接在双向链表的尾部
    linkNodeLast(p);
    return p;
	}

>在HashMap中添加新节点都要new 一个newNode的，重写这个方法之后，将p利用linkNodeLast链接起来  


    private void linkNodeLast(LinkedHashMap.Entry<K,V> p) {
    LinkedHashMap.Entry<K,V> last = tail;
    tail = p;
    // last 为 null，表明链表还未建立
    if (last == null)
        head = p;
    else {
        // 将新节点 p 接在链表尾部
        p.before = last;
        last.after = p;
    }
	}

>取到尾部，如果尾部是空说明新插入的是头。否则将其插入到尾部，就实现了链表的建立 


### 1.1 建立过程示意图


![](https://s2.ax1x.com/2019/06/12/VR4nvF.png)

>总结来说就是复写newNode方法，在put数据时，调用putVal方法然后再新创建node时，调用到重写的方法然后将新节点Link进数据中。

<br>

## 2. 链表的删除 

<br>
>删除的过程其实也很简单直接调用的仍然是HashMap中的删除方法，其实在之前的HashMap删除中有一个未实现的方法 

    afterNodeRemoval(node);    // 调用删除回调方法进行后续操作
>LinkedHashMap实现了该方法，他的作用就是删除链表中存储的目标节点 

    void afterNodeRemoval(Node<K,V> e) { // unlink
	    LinkedHashMap.Entry<K,V> p =
	        (LinkedHashMap.Entry<K,V>)e, b = p.before, a = p.after;
	    // 将 p 节点的前驱后后继引用置空
	    p.before = p.after = null;
	    // b 为 null，表明 p 是头节点
	    if (b == null)
	        head = a;
	    else
	        b.after = a;
	    // a 为 null，表明 p 是尾节点
	    if (a == null)
	        tail = b;
	    else
	        a.before = b;
	}

>所以链表的删除大概逻辑就是调用HashMap的方法删除然后再最后调用一个删除回调方法afterNodeRemoval然后进入链表中，将链表中的节点删除  



<br>

## 3. 链表顺序的维护  

<br>

> LinkedHashMap实际有两种维护顺序。一种是严格的按照插入顺序，另一种则是按照访问顺序，每访问一个数据之后，就将该数据放置到链表的末端 【调用get replace方法时，他是由accessOrder来控制的】 

    public V get(Object key) {
    Node<K,V> e;
	    if ((e = getNode(hash(key), key)) == null)
	        return null;
	    // 如果 accessOrder 为 true，则调用 afterNodeAccess 将被访问节点移动到链表最后
	    if (accessOrder)
	        afterNodeAccess(e);
	    return e.value;
	}

>实际上他就是调用了一个afterNodeAccess方法 这个方法与上边的remove方法很类似，就是移动链表节点 

<br>

## 3. LinkedHashMap的应用

<br>
>LinkedHashMap在缓存中是存在一定的应用的，比如存储的节点单位时间访问的次数很少，我们就可以直接将他删除  【  afterNodeInsertion(evict)存在于put方法中 】
    

    void afterNodeInsertion(boolean evict) { // possibly remove eldest
    LinkedHashMap.Entry<K,V> first;
	    // 根据条件判断是否移除最近最少被访问的节点
	    if (evict && (first = head) != null && removeEldestEntry(first)) {
	        K key = first.key;
	        removeNode(hash(key), key, null, false, true);
	    }
	}

	// 移除最近最少被访问条件之一，通过覆盖此方法可实现不同策略的缓存
	protected boolean removeEldestEntry(Map.Entry<K,V> eldest) {
    	return false;
	}
>实现这个方法，我们就可以用我们的逻辑，不满足该条件时候就删去链表头部节点。

### 3.1 缓存的应用  


    public class Hahs extends  LinkedHashMap{

    @Override
    protected boolean removeEldestEntry(Map.Entry eldest) {
        return size()>3;
    }

    public static void main(String[] args) {
        Hahs hs =new Hahs();
        hs.put(1,"李明");
        hs.put(2,"李明");
        hs.put(3,"李明");

        System.out.println(hs);
        hs.put(4,"李明");
        System.out.println(hs);
        hs.put(5,"李明");
        System.out.println(hs);
    }
	}


>输出结果：  
    {1=李明, 2=李明, 3=李明}
	{2=李明, 3=李明, 4=李明}
	{3=李明, 4=李明, 5=李明}	


<br>
<br><br>

##  参考 
[田小波博客](http://www.tianxiaobo.com/categories/foundation-of-java/collection/)

