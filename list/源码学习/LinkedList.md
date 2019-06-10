# LinkedList

----------

<!-- MarkdownTOC -->
<!-- TOC -->
- [LinkedList](#linkedlist)   
	-  [1. 常量](#1-常量)  
	- [2. 构造函数](#2-构造函数)  
	- [3. 常见方法](#3-常见方法)   
	- [4. 应用](#4-应用)       
		- [4.1 应用为队列](#41-应用为队列)        
		- [4.2 应用为栈](#42-应用为栈)<!-- /TOC -->
<!--/ MarkdownTOC -->


<br>
>LinkedList较ArrayList与Vector还是有很大区别的，前两者都是基于数组实现的，后者是基于链表实现的  


    public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
> 首先从继承关系看来，他并没有实现快速随机访问，但是他实现了双端列表  

<br>

## 1. 常量 

<br>

     transient int size = 0;

    
    transient Node<E> first;

    
    transient Node<E> last;

>他设置了Node类型的头尾常量与长度 以下是Node节点类型结构 

    private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }

<br>

## 2. 构造函数 

<br>


    public LinkedList() {
    }

    public LinkedList(Collection<? extends E> c) {
        this();
        addAll(c);
    }

>有两种构造函数，第一种是默认的，第二种是将整个容器添加到链表中 （不常用）


<br>

## 3. 常见方法  

<br>


>在头部添加节点 


      private void linkFirst(E e) {
        final Node<E> f = first;
        final Node<E> newNode = new Node<>(null, e, f);
        first = newNode;
        if (f == null)
            last = newNode;
        else
            f.prev = newNode;
        size++;
        modCount++;
    }

>复制首节点，然后另新创节点的尾部指向复制后的节点，头部设为空，另first等于他，然后将复制后的节点的头部指向他 完成复制 



<br>

>在尾部添加节点 
>
    void linkLast(E e) {
        final Node<E> l = last;
        final Node<E> newNode = new Node<>(l, e, null);
        last = newNode;
        if (l == null)
            first = newNode;
        else
            l.next = newNode;
        size++;
        modCount++;
    }

<br>

>在一个节点前插入节点

    void linkBefore(E e, Node<E> succ) {
        // assert succ != null;
        final Node<E> pred = succ.prev;
        final Node<E> newNode = new Node<>(pred, e, succ);
        succ.prev = newNode;
        if (pred == null)
            first = newNode;
        else
            pred.next = newNode;
        size++;
        modCount++;
    }

>所有的方法都类似于一个链子，一条一条的将数据增加到链子上,接下来是删除  

<br>

    E unlink(Node<E> x) {
        // assert x != null;
        final E element = x.item;
        final Node<E> next = x.next;
        final Node<E> prev = x.prev;

        if (prev == null) {
            first = next;
        } else {
            prev.next = next;
            x.prev = null;
        }

        if (next == null) {
            last = prev;
        } else {
            next.prev = prev;
            x.next = null;
        }

        x.item = null;
        size--;
        modCount++;
        return element;
    }

>之后 LinkedList提供了addFirst ,addLast,removeFirst,removeLast，add,remove等方法，实际上他们就是直接调用了上边介绍的link unlink方法

     public E getLast() {
        final Node<E> l = last;
        if (l == null)
            throw new NoSuchElementException();
        return l.item;
    }

    public E removeFirst() {
        final Node<E> f = first;
        if (f == null)
            throw new NoSuchElementException();
        return unlinkFirst(f);
    }
 
    public boolean add(E e) {
        linkLast(e);
        return true;
    }

 
    public boolean remove(Object o) {
        if (o == null) {
            for (Node<E> x = first; x != null; x = x.next) {
                if (x.item == null) {
                    unlink(x);
                    return true;
                }
            }
        } else {
            for (Node<E> x = first; x != null; x = x.next) {
                if (o.equals(x.item)) {
                    unlink(x);
                    return true;
                }
            }
        }
        return false;
    }

>clear方法 

      public void clear() {
        // Clearing all of the links between nodes is "unnecessary", but:
        // - helps a generational GC if the discarded nodes inhabit
        //   more than one generation
        // - is sure to free memory even if there is a reachable Iterator
        for (Node<E> x = first; x != null; ) {
            Node<E> next = x.next;
            x.item = null;
            x.next = null;
            x.prev = null;
            x = next;
        }
        first = last = null;
        size = 0;
        modCount++;
    }
>offer方法

     public boolean offerLast(E e) {
        addLast(e);
        return true;
    }

## 4. 应用
>由于他含有First与Last的增删，因此他既可以应用为队列也可以应用为栈。

### 4.1 应用为队列

    队列方法       等效方法
	add(e)        addLast(e)
	offer(e)      offerLast(e)
	remove()      removeFirst()
	poll()        pollFirst()
	element()     getFirst()
	peek()        peekFirst()

### 4.2 应用为栈 

    栈方法        等效方法
	push(e)      addFirst(e)
	pop()        removeFirst()
	peek()       peekFirst()
