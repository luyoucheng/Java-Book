# ArrayList源码学习  


----------
<!-- MarkdownTOC -->
<!-- TOC -->
- [ArrayList源码学习](#arraylist源码学习)   
 - [ 类的继承实现关系](#1-类的继承实现关系)    
   	 - [ 父类的简介](#11-父类的简介)      
      - [ 常规源码分析](#12-常规源码分析)      
      - [ 特征源码分析](#13-特征源码分析)         
          - [构造函数介绍](#构造函数介绍)           
          - [数组扩容介绍](#数组扩容介绍)            
          - [add remove  clear removeRange方法介绍](#add-remove--clear-removerange方法介绍)           
         - [trimTosize方法介绍](#trimtosize方法介绍)<!-- /TOC --><!-- MarkdownTOC -->

<br>

## 1. 类的继承实现关系


![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1560055606096&di=effc3dbbb5f1ef77e3ea60a7c75fdffe&imgtype=0&src=http%3A%2F%2Fupload-images.jianshu.io%2Fupload_images%2F4430762-10a5c23f340ef88c.png)

<br>


    public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable


<br>

>这个类继承了AbstractList  实现了RandomAcess Clonable Serializable


### 1.1 父类的简介

<br>

> 
> AbstractList: 简单的定义了一些List相关的方法，详细的在上一节介绍过  
> 
> RandomAcess【标记接口】: 一个是否支持快速访问的标志，支持快速访问的话，for循环比for each快一些 
>
> Seralizable【标记接口】: 表示其支持序列化 

>Cloneable【标记接口】: 实现该接口，object.clone才能实现复制  



### 1.2 常规源码分析  

<br>

    public class ArrayList<E> extends AbstractList<E> 
        implements List<E>, RandomAccess, Cloneable, Serializable {
   		 private transient Object[] elementData;
   		 private int size;
  
    	public ArrayList(int initialCapacity) {
      		super();
      			// ...
      		this.elementData = new Object[initialCapacity];
    	}
  
   		public E get(int index) {
    		 rangeCheck(index);
     		 return elementData(index);
  	    }
  
   		public E set(int index, E element) {
     		 rangeCheck(index);
      
     		 E oldValue = elementData(index);
      		elementData[index] = element;
      		return oldValue;
    	}
  
   	   public boolean add(E e) {
     		 ensureCapacity(size + 1);
     		 elementData[size++] = e;
      		return true;
    	}

	}


<br>

>arraylist是利用一个数组来实现的，通过数组索引来实现get set。add则是每次进行扩容判断然后再进行添加，因此可以将其理解他是一个可以【动态调整大小的数组】


<br>

### 1.3 特征源码分析   

<br>

> 首先查看其定义的一些常量，一个默认数组长度，一个默认空的数组，一个空的数组 【为什么要存在两个数组后边会介绍 】


       		private static final long serialVersionUID = 8683452581122892189L;

		    /**
		     * Default initial capacity.
		     */
		    private static final int DEFAULT_CAPACITY = 10;
		
		    /**
		     * Shared empty array instance used for empty instances.
		     */
		    private static final Object[] EMPTY_ELEMENTDATA = {};
		
		    /**
		     * Shared empty array instance used for default sized empty instances. We
		     * distinguish this from EMPTY_ELEMENTDATA to know how much to inflate when
		     * first element is added.
		     */
		    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};




<br>

#### 构造函数介绍

> 接着是构造函数的分析 


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

   
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }

    
    public ArrayList(Collection<? extends E> c) {
        elementData = c.toArray();
        if ((size = elementData.length) != 0) {
            // c.toArray might (incorrectly) not return Object[] (see 6260652)
            if (elementData.getClass() != Object[].class)
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            // replace with empty array.
            this.elementData = EMPTY_ELEMENTDATA;
        }
    }


<br>

>有三种构造函数。

>第一种是传入了参数作为数组的长度，然后创建相应长度的数组，如果是0则使用EMPTY_ELEMENTDATA 

>第二种是默认的构造函数，将数组指向DEFAULTCAPACITY_EMPTY_ELEMENTDATA  

>第三种是传入其他容器，然后对容器进行拷贝  


<br>

#### 数组扩容介绍


>接下来是数组的扩容介绍，因为在add执行之前要进行ensureCapacityInternal进行扩容 


    private void ensureCapacityInternal(int minCapacity) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
        }

        ensureExplicitCapacity(minCapacity);
    }


> 首先判断他是不是使用了默认的构造函数，是的话判断给的最小容量与默认容量（10）哪个大 然后传递给 ensureExplicitCapacity，


		private void ensureExplicitCapacity(int minCapacity) {
		        modCount++;
		
		        // overflow-conscious code
		        if (minCapacity - elementData.length > 0)
		            grow(minCapacity);
    	}
> modCount还是用来进行fast-fail机制的，然后判断选出的最大值是否大于数组长度，大于的话grow扩容


    private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }

>oldCapacity记录旧数组长度，将其扩容为1.5倍。如果扩容长度小于minCapacity(适用于最开始为0的情况)，然后将其赋值给minCapacity 。如果他的长度大于最大长度，传递给hugeCapacity。否则将其重新复制扩容。
      private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
    }
>所以正常情况下他扩容为原来的1.5，如果是初始化，设置为10，如果达到最大值则将其置为最大 


<br>

#### add remove  clear removeRange方法介绍  

<br>

    public void add(int index, E element) {
        rangeCheckForAdd(index);

        ensureCapacityInternal(size + 1);  // Increments modCount!!
        System.arraycopy(elementData, index, elementData, index + 1,
                         size - index);
        elementData[index] = element;
        size++;
    }

>先判断是否需要扩容，然后将目标位置后的数使用arraycopy向后平移，然后插入元素 



    public E remove(int index) {
        rangeCheck(index);

        modCount++;
        E oldValue = elementData(index);

        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        elementData[--size] = null; // clear to let GC do its work

        return oldValue;
    }
>将目标后的前移，将最后一个设为Null,等GC处理。

    public void clear() {
        modCount++;

        // clear to let GC do its work
        for (int i = 0; i < size; i++)
            elementData[i] = null;

        size = 0;
    }
   

     protected void removeRange(int fromIndex, int toIndex) {
        modCount++;
        int numMoved = size - toIndex;
        System.arraycopy(elementData, toIndex, elementData, fromIndex,
                         numMoved);

        // clear to let GC do its work
        int newSize = size - (toIndex-fromIndex);
        for (int i = newSize; i < size; i++) {
            elementData[i] = null;
        }
        size = newSize;
    }


<br>

#### trimTosize方法介绍  

<br>


     public void trimToSize() {
        modCount++;
        if (size < elementData.length) {
            elementData = (size == 0)
              ? EMPTY_ELEMENTDATA
              : Arrays.copyOf(elementData, size);
        }
    }


>这个方法是将array长度变为实际长度，将自动扩容的长度删去  



 

