# HashMap源码学习 

----------

![](https://s2.ax1x.com/2019/06/11/Vg36sI.png)

    public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable 

>他的父类是AbstractMap,他实现了Map,Cloneable，Serializable接口 


<br>

### 1 常量   


>第一个常量是HashMap的桶的数量，如上图的0-15 他最开始默认是16个桶的

    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
>  桶的最大容量为2的30次方

    static final int MAXIMUM_CAPACITY = 1 << 30;
>乘积因子，后面再详细讲解 

    static final float DEFAULT_LOAD_FACTOR = 0.75f;

>表示节点到达多少，由链表转换为树状结构 
    static final int TREEIFY_THRESHOLD = 8;
>表示节点减少到多少，由树状结构变回链表结构 

    static final int UNTREEIFY_THRESHOLD = 6;

    static final int MIN_TREEIFY_CAPACITY = 64;
>一个桶类型的数组，用于存放桶
    Node<K,V>[] table;

<br>



### 2 Node结构【内容补充】


<br>

>Node是Entry的子类，他具体的实现了KEY-VALUE的结构以及如果查看键值，设置键值

    static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;

        Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }

        public final K getKey()        { return key; }
        public final V getValue()      { return value; }
        public final String toString() { return key + "=" + value; }

        public final int hashCode() {
            return Objects.hashCode(key) ^ Objects.hashCode(value);
        }

        public final V setValue(V newValue) {
            V oldValue = value;
            value = newValue;
            return oldValue;
        }

        public final boolean equals(Object o) {
            if (o == this)
                return true;
            if (o instanceof Map.Entry) {
                Map.Entry<?,?> e = (Map.Entry<?,?>)o;
                if (Objects.equals(key, e.getKey()) &&
                    Objects.equals(value, e.getValue()))
                    return true;
            }
            return false;
        }
    }








<br>

### 3 构造函数 


<br>


	     public HashMap(int initialCapacity, float loadFactor) {
	        if (initialCapacity < 0)
	            throw new IllegalArgumentException("Illegal initial capacity: " +
	                                               initialCapacity);
	        if (initialCapacity > MAXIMUM_CAPACITY)
	            initialCapacity = MAXIMUM_CAPACITY;
	        if (loadFactor <= 0 || Float.isNaN(loadFactor))
	            throw new IllegalArgumentException("Illegal load factor: " +
	                                               loadFactor);
	        this.loadFactor = loadFactor;
	        this.threshold = tableSizeFor(initialCapacity);
	    }
	
	    public HashMap(int initialCapacity) {
	        this(initialCapacity, DEFAULT_LOAD_FACTOR);
	    }
	
	    public HashMap() {
	        this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
	    }
	
	    public HashMap(Map<? extends K, ? extends V> m) {
	        this.loadFactor = DEFAULT_LOAD_FACTOR;
	        putMapEntries(m, false);
	    }

>主要看第一个构造函数，后边两个实际都是间接的调用第一个构造函数。先判断初始化的他那个的长度，如果超过最大长度，就按照最大长度初始，如果加载因子也没有问题，交给tableSizeFor处理，theSold表示他所能承受的最大容量，超过则需要扩容。所以实际上执行完构造函数之后，桶数组仍旧为空，这里可以认为他是懒加载。


<br>


###  4 get 方法 

<br>

>查找一个键就是先定位到他所在桶的位置，然后再判断他是树结构还是链表结构，进行查找 

    public V get(Object key) {
	    Node<K,V> e;
	    return (e = getNode(hash(key), key)) == null ? null : e.value;
	}

	final Node<K,V> getNode(int hash, Object key) {
	    Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
	    // 1. 定位键值对所在桶的位置
	    if ((tab = table) != null && (n = tab.length) > 0 &&
	        (first = tab[(n - 1) & hash]) != null) {
	        if (first.hash == hash && // always check first node
	            ((k = first.key) == key || (key != null && key.equals(k))))
	            return first;
	        if ((e = first.next) != null) {
	            // 2. 如果 first 是 TreeNode 类型，则调用黑红树查找方法
	            if (first instanceof TreeNode)
	                return ((TreeNode<K,V>)first).getTreeNode(hash, key);
	                
	            // 2. 对链表进行查找
	            do {
	                if (e.hash == hash &&
	                    ((k = e.key) == key || (key != null && key.equals(k))))
	                    return e;
	            } while ((e = e.next) != null);
	        }
	    }
	    return null;
	}

>关于(n - 1) & hash就相当与hash对length取余，因为计算出的hash值可能非常大，想要判断他在哪个桶中就需要根据hash计算出的key的值【hash(key)】来进行判断，取余运算之后，他的范围就在0-length-1长度中就知道他在哪个桶了。

<br>

>再大概的捋一遍就是用户输出要得到的KEY，HashMap利用Hash算法得到他的Hash值，然后利用与数组长度的取余运算得到他所在的桶，然后交给链表或者树结构去查找得到相应的节点。因此Hash是一个很重要的方法



<br>


###  4.1 hash 方法 

<br>

    /**
	 * 计算键的 hash 值
	 */
	static final int hash(Object key) {
	    int h;
	    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
	}


>Hash函数的算法就是得到Key的hashCode值然后再领其高16位与低16位异获，得到相应的值，这样做有两个好处 。

> 1. 增加低位的复杂性 变相的让高位参与到计算中 

> 2.增加Hash的复杂性，有效的避免Hash冲突，我们都知道HashMap是根据计算出的Hash散列的将数据分布在不同 的桶的，Hash复杂了就降低了不同的Key分布到一个桶的情况，使得他的离散型提高。


<br>


###  5.  遍历  

<br>

>在对HashMap进行遍历的时候，我们一般是先得到他的key的映射或者是他key-value映射，然后再利用foreach来进行遍历


	
	 HashMap<String,String> hash  =new HashMap();
          Set set =hash.keySet();
          for(Object temp:set ){
              System.out.println(temp);
          }

        for(HashMap.Entry entry :hash.entrySet() ){
            System.out.println(entry);
        }` 

>如图我们一般是通过这样的方式进行遍历，在编译的时候他其实会帮我们转成迭代器的形式进行遍历 

		Set keys = map.keySet();
		Iterator ite = keys.iterator();
		while (ite.hasNext()) {
		    Object key = ite.next();
		    // do something
		}

>多次遍历之后，我们发现每次遍历的结果都是一样的，但是与插入的顺序是一定差距的，他的遍历模式如下图

![](https://s2.ax1x.com/2019/06/12/V2oq1K.png)



>先自定向下找到不是空的桶，然后根据桶中的链表关系进行遍历 




<br>


###  6.  插入【重点】

<br>

>插入其实并不是简单的计算Hash然后得到桶的位置，然后进行相应的插入，他还涉及到扩容，树化等一系列操作 


    public V put(K key, V value) {
   		 return putVal(hash(key), key, value, false, true);
	}
>put操作实际是调用了PutVal操作，传入计算出的hash值，以及key -value 


	final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
	    Node<K,V>[] tab; Node<K,V> p; int n, i;
	    // 初始化桶数组 table，table 被延迟到插入新数据时再进行初始化
	    if ((tab = table) == null || (n = tab.length) == 0)
	        n = (tab = resize()).length;
>这里在插入数据时才第一次初始化Node数组 

	    // 如果桶中不包含键值对节点引用，则将新键值对节点的引用存入桶中即可
	    if ((p = tab[i = (n - 1) & hash]) == null)
	        tab[i] = newNode(hash, key, value, null);

>当前桶中不存在键值对，直接新建 

	    else {
	        Node<K,V> e; K k;
	        // 如果键的值以及节点 hash 等于链表中的第一个键值对节点时，则将 e 指向该键值对
	        if (p.hash == hash &&
	            ((k = p.key) == key || (key != null && key.equals(k))))
	            e = p;
>如果里边以及存在键值对，并且要插入的正好与以及存在的第一个键一样，令其指向第一个键值对 
        
	        // 如果桶中的引用类型为 TreeNode，则调用红黑树的插入方法
	        else if (p instanceof TreeNode)  
	            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
>如果桶中是红黑树形式 将其插入 

	        else {
	            // 对链表进行遍历，并统计链表长度
	            for (int binCount = 0; ; ++binCount) {
	                // 链表中不包含要插入的键值对节点时，则将该节点接在链表的最后
	                if ((e = p.next) == null) {
	                    p.next = newNode(hash, key, value, null);
	                    // 如果链表长度大于或等于树化阈值，则进行树化操作
	                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
	                        treeifyBin(tab, hash);
	                    break;
	                }
	                
	                // 条件为 true，表示当前链表包含要插入的键值对，终止遍历
	                if (e.hash == hash &&
	                    ((k = e.key) == key || (key != null && key.equals(k))))
	                    break;
	                p = e;
  					  }
	        }
>对整个链表进行遍历，如果包含则直接指向目标节点，如果不包含，则遍历到最后将其插入。插入之后判断是否达到树化条件，得到的话进行树化操作

	        
>最后判断是否E有指向的节点，指向的话则更新VALUE值,返回旧的值 	        
	        // 判断要插入的键值对是否存在 HashMap 中
	        if (e != null) { // existing mapping for key
	            V oldValue = e.value;
	            // onlyIfAbsent 表示是否仅在 oldValue 为 null 的情况下更新键值对的值
	            if (!onlyIfAbsent || oldValue == null)
	                e.value = value;
	            afterNodeAccess(e);
	            return oldValue;    
	        }
	    }
>最后整体节点数量增加  如果数量超过阈值扩容 
	    ++modCount;
	    // 键值对数量超过阈值时，则进行扩容
	    if (++size > threshold)
	        resize();
	    afterNodeInsertion(evict);
	    return null;
	}


<br>

>所以主要是分为以下几种情况 

>第一种:是否第一次加入数据，第一次的话进行resieze扩容初始化   

>第二种：插入数据的桶是否为空，为空直接令其为头结点  

>第三种：要插入的数据已经存在，然后根据条件是否覆盖更新【onlyIfAbsent 表示是否仅在 oldValue 为 null 的情况下更新键值对的值】

>第四种：插入数据所在桶不为空，如果结构为树，将其插入，为链表则遍历链表插入末尾，然后判断是否达到树化条件 

>第五种：插入完毕，然后得到扩容条件，进行扩容


<br>


###  6.1  插入流程介绍 

<br>

>插入一个数据，如果HashMap第一次进行插入数据，那么他需要调用resize进行扩容初始化，然后根据Hash计算出的Hash值，然后利用Hash与桶长度做取余运算得到他所在的桶，然后进行插入判断。如果该桶为空，他就作为该桶的第一个节点，如果该桶不为空，判断该桶结构。如果是红黑树，遍历红黑树是否存在该节点，如果存在则临时变量指向该节点，如果不存在将其插入，如果结构为链表，遍历链表查看是否存在，存在则临时变量指向该节点，不存在则插入到末尾。插入后判断链表长度是否得到树化长度8，达到则树化。如果临时变量不为空则说明已经存在该节点，那么根据条件决定是否更新VALUE【由onlyIfAbsent决定，onlyIfAbsent为false则不为空也可以更新数据，为true则表示只有原数据为空才能更新】,最后插入数据成功长度增加，然后判断是否需要扩容，需要则扩容，不需要则直接结束。

<br>


###  7.  扩容【重点】

<br>


    final Node<K,V>[] resize() {
>将原数组复制给oldTab然后根据长度判断他是否已经初始化过 
    Node<K,V>[] oldTab = table;
    int oldCap = (oldTab == null) ? 0 : oldTab.length;
    int oldThr = threshold;
    int newCap, newThr = 0;
    // 如果 table 不为空，表明已经初始化过了
>如果已经初始化了并且数组长度已经大于最大值，则不扩容

    if (oldCap > 0) {
        // 当 table 容量超过容量最大值，则不再扩容
        if (oldCap >= MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return oldTab;
        } 
>如果扩容后没达到最大值并且原数组大于16则都扩容为2倍

        // 按旧容量和阈值的2倍计算新容量和阈值的大小
        else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                 oldCap >= DEFAULT_INITIAL_CAPACITY)
            newThr = oldThr << 1; // double threshold
>这个判断分支表示使用了带参构造函数创建，然后利用threshold存储initialCapacity

    } else if (oldThr > 0) // initial capacity was placed in threshold
        /*
         * 初始化时，将 threshold 的值赋值给 newCap，
         * HashMap 使用 threshold 变量暂时保存 initialCapacity 参数的值
         */ 
        newCap = oldThr;

> 这个分支表示使用了无参构造函数，则按默认的初始化 

    else {               // zero initial threshold signifies using defaults
        /*
         * 调用无参构造方法时，桶数组容量为默认容量，
         * 阈值为默认容量与默认负载因子乘积
         */
        newCap = DEFAULT_INITIAL_CAPACITY;
        newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
    }
    
>如果新的阈值因子没有被赋值则给其赋值，【可能原因为中间计算溢出】

    // newThr 为 0 时，按阈值计算公式进行计算
    if (newThr == 0) {
        float ft = (float)newCap * loadFactor;
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                  (int)ft : Integer.MAX_VALUE);
    }
    threshold = newThr;

>然后创建桶数组 

    // 创建新的桶数组，桶数组的初始化也是在这里完成的
    Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
    table = newTab;
    if (oldTab != null) {
        // 如果旧的桶数组不为空，则遍历桶数组，并将键值对映射到新的桶数组中
        for (int j = 0; j < oldCap; ++j) {
            Node<K,V> e;
            if ((e = oldTab[j]) != null) {
                oldTab[j] = null;

>在桶上的键值对后边没有其他键值对与之相连 

                if (e.next == null)
                    newTab[e.hash & (newCap - 1)] = e;
>后边有树与之相连 拆分并重新映射 

                else if (e instanceof TreeNode)
                    // 重新映射时，需要对红黑树进行拆分
                    ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);

>后边有链表，对这一条链的节点求余运算，得到一条仍未0的链与一条不是0的链，然后将链放到新的桶的位置

                else { // preserve order
                    Node<K,V> loHead = null, loTail = null;
                    Node<K,V> hiHead = null, hiTail = null;
                    Node<K,V> next;
                    // 遍历链表，并将链表节点按原顺序进行分组
                    do {
                        next = e.next;
                        if ((e.hash & oldCap) == 0) {
                            if (loTail == null)
                                loHead = e;
                            else
                                loTail.next = e;
                            loTail = e;
                        }
                        else {
                            if (hiTail == null)
                                hiHead = e;
                            else
                                hiTail.next = e;
                            hiTail = e;
                        }
                    } while ((e = next) != null);
                    // 将分组后的链表映射到新桶中
                    if (loTail != null) {
                        loTail.next = null;
                        newTab[j] = loHead;
                    }
                    if (hiTail != null) {
                        hiTail.next = null;
                        newTab[j + oldCap] = hiHead;
                    }
                }
            }
        }
    }
    return newTab;
	}


>与新的容量取模，因为其扩大了一倍，所以取模的位数扩大1，与1取模要么为0要么为1 所以要么他的位置要么不变，要么增加一哥位置长度。1.7时是一个一个的Put插入进去，而1.8是一串链分成两部分直接插入，效率大大提高。




<br>


###  8  删除 

<br>

    public V remove(Object key) {
    Node<K,V> e;
    return (e = removeNode(hash(key), key, null, false, true)) == null ?
        null : e.value;
	}

	final Node<K,V> removeNode(int hash, Object key, Object value,
                           boolean matchValue, boolean movable) {
    Node<K,V>[] tab; Node<K,V> p; int n, index;
    if ((tab = table) != null && (n = tab.length) > 0 &&
        // 1. 定位桶位置
        (p = tab[index = (n - 1) & hash]) != null) {
        Node<K,V> node = null, e; K k; V v;
        // 如果键的值与链表第一个节点相等，则将 node 指向该节点
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            node = p;
        else if ((e = p.next) != null) {  
            // 如果是 TreeNode 类型，调用红黑树的查找逻辑定位待删除节点
            if (p instanceof TreeNode)
                node = ((TreeNode<K,V>)p).getTreeNode(hash, key);
            else {
                // 2. 遍历链表，找到待删除节点
                do {
                    if (e.hash == hash &&
                        ((k = e.key) == key ||
                         (key != null && key.equals(k)))) {
                        node = e;
                        break;
                    }
                    p = e;
                } while ((e = e.next) != null);
            }
        }
        
        // 3. 删除节点，并修复链表或红黑树
        if (node != null && (!matchValue || (v = node.value) == value ||
                             (value != null && value.equals(v)))) {
            if (node instanceof TreeNode)
                ((TreeNode<K,V>)node).removeTreeNode(this, tab, movable);
            else if (node == p)
                tab[index] = node.next;
            else
                p.next = node.next;
            ++modCount;
            --size;
            afterNodeRemoval(node);
            return node;
        }
    }
    return null;
}






<br>


###  9  补充【重点】

<br>

#### 9.1 树化条件 
>树化是有两个条件的，一个是链表长度达到树化的临界长度，即8。另一个条件是桶的容器大于默认最小的长度64。
因为Hash碰撞率较高导致链的长度很大，这个时候树化确实可以解决一定的问题。但是碰撞率高很大程度是因为桶太少造成的。桶太少后来还要不断的扩容，扩容就需要对二叉树拆分等等一些列操作，因此如果桶容量较少先进行的是扩容而不是树化

<br>

#### 9.2 插入的数据要实现equal与hashCode方法吗？
>需要，HashMap实现的方法是为了将元素放入到相应的桶中，元素与其他元素是否相等的判断还要依靠他们自身的equal与hashCode方法来判断。 

<br>

#### 9.3 各变量默认值 


> onlyIfAbsent :只有原来的值为空时，新值才能覆盖 默认为false  

>evict ：是否根据缓存条件判断是否删除元素 ，默认为true 


### 9.4 为什么树化条件设为8?  

树化的Node空间是普通Node的二倍 ，得到阈值才才树化，如果hashCode符合泊松分布那么小概率达到8。

链表的查询长度为n/2 树为log(n) 为8时候 数为3  链表为4 优于链表，所以转为树，为6时候 分别为3和2.3 虽然树效果更好但是树转化也需要消耗一定时间空间所以没必要转换。至于为什么不是7，主要是为了防止频繁转换，作为过渡，假如一个操作频繁在8之间增删，但是低于8就链化高于就树化，效率就很低。
 
