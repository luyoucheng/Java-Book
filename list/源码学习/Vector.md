# Vector源码学习 


----------

<!-- MarkdownTOC -->
<!-- TOC -->
- [Vector源码学习](#vector源码学习)    
 - [1. 构造函数](#1-构造函数)   
 - [2. 扩容方法](#2-扩容方法)   
 - [3. Stack栈](#3-stack栈)<!-- /TOC -->
<!-- /MarkdownTOC -->
<br>

>总体来说，Vector与ArrayList实现原理是很接近的，只是Vector在函数中都添加了synchronized关键字，所以他是线程安全的，这是他与arraylist的最大区别。此外，stack也是基于Vector实现的  


<br>

    public class Vector<E>
    extends AbstractList<E>
    implements List<E>, RandomAccess, Cloneable, java.io.Serializable


>他的继承关系也与arraylist是一样的   


    protected Object[] elementData;
    protected int elementCount;
    protected int capacityIncrement;
    private static final long serialVersionUID = -2767605614048989439L;


>也与arraylist基本一样，数组实现，elementCount表示容器中元素的个数，capacityIncrement表示每次扩容增加的长度 





## 1. 构造函数 



<br>

    public Vector(int initialCapacity, int capacityIncrement) {
        super();
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        this.elementData = new Object[initialCapacity];
        this.capacityIncrement = capacityIncrement;
    }

>该构造方法会初始化数组长度与默认增长个数 

    public Vector(int initialCapacity) {
        this(initialCapacity, 0);
    }
>该构造方法只会初始化数组长度，增长个数默认为0
    public Vector() {
        this(10);
    }
>默认构造函数认为他初始化长度为10，增长个数默认为0

    public Vector(Collection<? extends E> c) {
        elementData = c.toArray();
        elementCount = elementData.length;
        // c.toArray might (incorrectly) not return Object[] (see 6260652)
        if (elementData.getClass() != Object[].class)
            elementData = Arrays.copyOf(elementData, elementCount, Object[].class);
    }
>该构造方法表示通过其他的容器来初始化，使用较少


<br>

## 2. 扩容方法 

<br>

     private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                         capacityIncrement : oldCapacity);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        elementData = Arrays.copyOf(elementData, newCapacity);
    }

>别的方法基本只是在arraylist的基础上将方法改为同步的就不重复介绍，这里介绍一下扩容方法。 如果设置了扩容的长度，那么每次增加这么长，如果没有的话，则默认翻倍，而arrylist默认是增加1.5倍。


<br>

## 3. Stack栈 


<br>

>说的栈 第一时间想到的应该都是后进先出，他其实是基于vector实现的   


    public
	class Stack<E> extends Vector<E> {
    
    public Stack() {
    }

   
    public E push(E item) {
        addElement(item);

        return item;
    }

>push方法【不是同步方法】 调用了vector方法，实际就是在数组最后一位添加元素 

    public synchronized void addElement(E obj) {
        modCount++;
        ensureCapacityHelper(elementCount + 1);
        elementData[elementCount++] = obj;
    }
>pop方法也是取到最后一个元素下标，然后移除并返回
    public synchronized E pop() {
        E       obj;
        int     len = size();

        obj = peek();
        removeElementAt(len - 1);

        return obj;
    }


>接下来是peek方法，也是取到最后一个元素下标返回

   
    public synchronized E peek() {
        int     len = size();

        if (len == 0)
            throw new EmptyStackException();
        return elementAt(len - 1);
    }

>empty方法直接让size与0判断
    
    public boolean empty() {
        return size() == 0;
    }
>search调用查找方法找到最后一次该元素的下边 

    public synchronized int search(Object o) {
        int i = lastIndexOf(o);

        if (i >= 0) {
            return size() - i;
        }
        return -1;
    }

    /** use serialVersionUID from JDK 1.0.2 for interoperability */
    private static final long serialVersionUID = 1224463164541339165L;
	}
