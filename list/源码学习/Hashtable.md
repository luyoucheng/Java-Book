# Hashtable 



----------
<!-- TOC -->autoauto- [Hashtable](#hashtable)auto    - [1. 常量](#1-常量)auto    - [2. 构造函数](#2-构造函数)auto    - [3. put方法](#3-put方法)auto- [4. rehash](#4-rehash)auto- [5. 遍历](#5-遍历)auto    - [6. 与HashMap的比较](#6-与hashmap的比较)auto        - [6.1 为什么HashMap中的&位必须为奇数（Length - 1)?](#61-为什么hashmap中的位必须为奇数length---1)autoauto<!-- /TOC -->
    public class Hashtable<K,V>
    extends Dictionary<K,V>
    implements Map<K,V>, Cloneable, java.io.Serializable
>他是直接继承自Dictionary 而HashMap是继承自AbstractMap ,两者的主要区别在于一个使用iterator迭代，Hashtable使用Enumeration【版本较老，为了兼容】

<br>


## 1. 常量
    private transient Entry<?,?>[] table;
    private transient int count;
    private int threshold;
    private float loadFactor;
    private transient int modCount = 0;

>他的常量基本上与HashMap是一样的

<br>

## 2. 构造函数 

<br>

     public Hashtable(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal Load: "+loadFactor);

        if (initialCapacity==0)
            initialCapacity = 1;
        this.loadFactor = loadFactor;
        table = new Entry<?,?>[initialCapacity];
        threshold = (int)Math.min(initialCapacity * loadFactor, MAX_ARRAY_SIZE + 1);
    }

    public Hashtable(int initialCapacity) {
        this(initialCapacity, 0.75f);
    }

    public Hashtable() {
        this(11, 0.75f);
    }

    public Hashtable(Map<? extends K, ? extends V> t) {
        this(Math.max(2*t.size(), 11), 0.75f);
        putAll(t);
    }

>可以看出他与HashMap的区别在于 HashMap的table数组在使用的时候才会进行实例化，而HashTable在构造函数就进行了实例化 ，此外HashMap默认数组长度为16，Hashtable默认为11


<br>
## 3. put方法  

<br>

    public synchronized V put(K key, V value) {
        // Make sure the value is not null
        //不允许value为空 
        if (value == null) {
            throw new NullPointerException();
        }
>他的value是显示的不允许为NULL，如果为NULL则直接抛出异常

        // Makes sure the key is not already in the hashtable.
        Entry<?,?> tab[] = table;
        int hash = key.hashCode();   //直接使用底层的hashCode方法
>他的key为隐式不可以为Null，因为为Null不能调用hashCode()，否则会报空指针异常

> HashMap中存在对null的判断所以不存在该问题  【return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);】

        int index = (hash & 0x7FFFFFFF) % tab.length;
        @SuppressWarnings("unchecked")
        Entry<K,V> entry = (Entry<K,V>)tab[index];
        for(; entry != null ; entry = entry.next) {
            if ((entry.hash == hash) && entry.key.equals(key)) {
                V old = entry.value;
                entry.value = value;
                return old;
            }
        }

        addEntry(hash, key, value, index);
        return null;
    }

>总结下来，这里与HashMap的区别为。

>区别1 ： 他的键值都不能为NULL，而HashMap中键有一个可以为NULL，值可以都为NULL  

>区别2 ： Hashtable是直接调用的HashCode得到hash值，而HashMap是调用HashCode然后前16与后16异或才得到hash值， 极大的降低碰撞可能性  

>区别3 ：Hashtable方法是同步的，因此他是线程安全的


>区别4 ：Hashtable不满足双驼峰规定 

<br>

# 4. rehash 

<br>

     protected void rehash() {
        int oldCapacity = table.length;
        Entry<?,?>[] oldMap = table;

        // overflow-conscious code
        int newCapacity = (oldCapacity << 1) + 1;

>扩容时候，变为原来的二倍+1   HashMap是变为原来的2倍  

        if (newCapacity - MAX_ARRAY_SIZE > 0) {
            if (oldCapacity == MAX_ARRAY_SIZE)
                // Keep running with MAX_ARRAY_SIZE buckets
                return;
            newCapacity = MAX_ARRAY_SIZE;
        }
        Entry<?,?>[] newMap = new Entry<?,?>[newCapacity];

        modCount++;
        threshold = (int)Math.min(newCapacity * loadFactor, MAX_ARRAY_SIZE + 1);
        table = newMap;

        for (int i = oldCapacity ; i-- > 0 ;) {
            for (Entry<K,V> old = (Entry<K,V>)oldMap[i] ; old != null ; ) {
                Entry<K,V> e = old;
                old = old.next;

                int index = (e.hash & 0x7FFFFFFF) % newCapacity;
                e.next = (Entry<K,V>)newMap[index];
                newMap[index] = e;
            }
        }
    }
>他的重新扩容是按次序计算每条链上的每一个元素的hash然后存入新的,而HashMap是计算链上的元素与新数组长度取余为0或不为0分为两条链，为0原位置不动，不为0原位置基础+增加的长度 





<br>

# 5. 遍历 

<br>

    Hashtable<Integer,String>  ht =new Hashtable();

        ht.put(1,"111");
        ht.put(2,"111");
        ht.put(3,"111");
        ht.put(4,"111");
        System.out.println(ht);
          Set<Integer> set =ht.keySet();
         for(Integer s :set){
              if(s==1){
                  ht.remove(s);
              }
         }
        System.out.println(ht);

>Hashtable可以边遍历边删除【添加不行】，HashMap想要实现就需要借助迭代器来实现 




<br>

## 6. 与HashMap的比较  

<br>


>  从安全角度： Hashtable线程安全  HashMap线程不安全  

>  从效率角度:Hashtable效率低  HashMap线程效率高  

>从应用角度 ：Hashtable不能为null  HashMap可以 

>从底层角度 ：
>
>Hashtable 继承dirctionary在构造函数时就初始化了数组，并且默认长度为11 插入数据直接调用本地hashCode方法，需要扩容时变为2倍+1 并且扩容时一个一个的计算hash插入，遍历时候用Enumeration

>HashMap 继承自abstractMap 在使用的时候数组，并且默认长度为16 插入数据调用本地hashCode方法再前16后16异或，需要扩容时变为2倍 并且扩容时每一个桶先计算出两条链再分到新的桶     

>最重要的： HashMap用到了红黑树

>因此无论是hash的计算方法，还是红黑树，HashMap对碰撞的处理都比Hashtable要好很多  



        

<br>

###  6.1 为什么HashMap中的&位必须为奇数（Length - 1)?  

<br>

>2的幂次方-1 2 进制后 后几位都为11111这样与大量key算出的hash与的时候，冲突的概率小，降低了hash碰撞的可能性