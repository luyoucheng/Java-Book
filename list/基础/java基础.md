# Java基础

<!-- TOC -->
-  [Java基础](#java基础)  
 - [JVM JRE JDK的区别](#jvm-jre-jdk的区别)  
  - [跨平台原理](#跨平台原理)  
  - [编译型语言与解释型语言的区别](#编译型语言与解释型语言的区别)
   [Undo](#undo)
 - [java的数据类型](#java的数据类型)  
 - [整数类型](#整数类型)      
     - [备注：](#备注)
 - [浮点类型](#浮点类型)  
 - [自动装箱拆箱](#自动装箱拆箱)   
 - [Integer的valueOf方法与parseInt方法以及Number类的intValue](#integer的valueof方法与parseint方法以及number类的intvalue)   
  - [有时候不用int声明而用integer声明？](#有时候不用int声明而用integer声明)   
  - [byte i=1 i+=1 与i =i+1有什么区别？](#byte-i1-i1-与i-i1有什么区别)  
  - [i=i++值为多少](#ii值为多少) 
     - [备注：](#备注-1)
       
 - [Object有哪些方法](#object有哪些方法)  
 - [clone 深克隆浅克隆](#clone-深克隆浅克隆)
 - [什么是序列化与反序列化](#什么是序列化与反序列化)
 - [serialVersionUID作用](#serialversionuid作用)  
 - [不使变量序列化有几种方法](#不使变量序列化有几种方法)    
 - [==与equals的区别](#与equals的区别) 
 - [equals方法与hashCode方法的关系](#equals方法与hashcode方法的关系)
 - [关键词](#关键词) 
 - [Java特性](#java特性)   
 - [面向对象与面向过程](#面向对象与面向过程)   
   	 - [关于继承的笔试题](#关于继承的笔试题)   
        - [笔试题2](#笔试题2)    
 - [静态绑定与动态绑定 UNDO](#静态绑定与动态绑定-undo)  
 - [抽象类与接口的异同](#抽象类与接口的异同)   
 - [Override与OverWrite的区别](#override与overwrite的区别)  
 - [进程线程](#进程线程)       
 - [进程与线程](#进程与线程)       
 - [创建进程的方法](#创建进程的方法)
 -  【UNDO】 callable实现原理  线程池(#undo-callable实现原理--线程池)
                    -<!-- /TOC -->

<br>

## JVM JRE JDK的区别

JVM是java虚拟机，他可以将辨识java字节码文件，然后调用相应的底层api完成操作 

JRE是java运行时环境，他包括jvm与一些基础类库，他可以让我们在本地进行java代码的调用

jdk是java开发工具包，是java开发的核心 他包含了编译 反编译等一些工具，他是为开发者提供的




<br>

## 跨平台原理


java的一大特点就是跨平台，他可以一处编译到处执行，这主要得益于jvm 他编译之后形成Java字节码文件，jvm识别字节码文件之后再将其解释为对应操作系统的指令，调用操作系统的api完成跨平台的调用





<br>

## 编译型语言与解释型语言的区别

编译型语言是在程序全部书写完成之后然后生成相应的执行文件，因为他只需要编译一次所以他的执行效果较高。但是修改时候相对繁琐，需要重新编译，同时他的平台移植性较差 

解释型语言就是编写一句代码，解释器解释一句，实时的对结果显示。他的效率较低，但是他的平台移植性很高

他们就好比编译型语言写了一个软件更新的时候我们需要卸掉旧的程序，下载安装新的程序，但是解释型语言，我们直接一刷新界面比如网页，新跟新的东西就可以显示出来，所以解释型语言更新性更好。

<br>

# Undo 
servlet是先编译后部署，修改完以后，MyEclipse进行编译，然后部署.class文件到servlet容器中。如果web服务器已启动，则之前class已被servlet容器加载，可能修改后的class文件不会被servlet容器执行。
而jsp是web服务器进行编译。加载时当场编译的，而不是预先编译好，tomcat可以设置为监视jsp文件的改动，改动之后则重新编译、执行。所以jsp是改动时，不需要重启服务器。

<br>

## java的数据类型

<br>


![](https://s2.ax1x.com/2019/07/15/ZT1jgO.jpg)

>java数据类型分为基本数据类型与引用数据类型两种

>基本数据类型又分为8种，如图上分类。


## 整数类型

<table border="1px" align="center" bordercolor="black" width="80%" height="100px">
    <tr align="center">
        <td>类型</td>
        <td>字节数</td>
        <td>二进制位数</td>
        <td>范围</td>
		<td>默认值</td>
       
    </tr>
    <tr align="center">
        <td>byte</td>
        <td>1</td>
       <td>8位</td>
        <td>-128-127</td>
		<td>0</td>
    </tr>
 <tr align="center">
        <td>short</td>
        <td>2</td>
       <td>16位</td>
        <td>-32768-32767</td>
		<td>0</td>
    </tr>
 <tr align="center">
        <td>int</td>
        <td>4</td>
       <td>32位</td>
        <td>-2147483648-2147483647【10位数】</td>
		<td>0</td>
    </tr>
 <tr align="center">
        <td>long</td>
        <td>8</td>
       <td>64位</td>
        <td>-263～263-1</td>
		<td>0L</td>
    </tr>
</table>

<br>

### 备注： 

>1. 我们确定这么多种数据类型是为了保证合理的利用空间，就好比我们要到一杯水，没必要用一个喷子来承装
>2. 数据从低到高可以隐式转换类型，从高到低需要显示转换并且可能会导致溢出
>3. 负数的存储一般以补码的形式存储，补码转换为原码则为-1然后除了符号位取反，以byte为例。最大值为0111 1111是127，最小值为1000 0000 先减去1 变为1111 1111 然后取反得到1000 0000 -128 -0也是这个二进制码 
。根据存储规则，-1为 1111 1111
>4. 当我们要存储的值Long也存储不下的时候我们可以使用BigDecimal，下边会详细讲


补充：  

> 内存溢出与内存泄漏，内存溢出是指申请内存的时候没有足够的内存可以使用。比如我们用int存一个只有long才能存下的数据就造成了内存溢出。 内存泄漏则是指一段申请了的内存在使用完之后无法释，堆积太多就会导致系统崩溃



<br>

## 浮点类型

<table border="1px" align="center" bordercolor="black" width="80%" height="100px">
  <tr align="center">
        <td>类型</td>
        <td>字节数</td>
        <td>指数位</td>
        <td>位数位</td>
        <td>范围</td>
        <td>精度</td>
		<td>默认值</td>	
       
   </tr>
  <tr align="center">
        <td>float</td>
        <td>32</td>
        <td>8</td>
        <td>23</td>
        <td>-2^128 ~ +2^128</td>
        <td>2^23 = 8388608  6-7位有效数字</td>
		<td>0.0f</td>
 </tr>
 <tr align="center">
        <td>double</td>
        <td>64</td>
        <td>11</td>
        <td>52</td>
        <td>-2^1024 ~ +2^1024</td>
        <td>2^52 = 4503599627370496   15-16位有效数字</td>
		<td>0.0</td>
 </tr>

</table>

<br>


<br>

## 自动装箱拆箱 


自动装箱的大概意思就是将基本类型自动转换为包装器类型， 拆箱就是将包装器类型转换为基本类型 

![](https://s2.ax1x.com/2019/07/16/ZHEK5F.png)


自动装箱的实现方式为调用valueOf方法 

 	 int i = 20;
    Integer integer = Integer.valueOf(i);

 	public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
可以看出他提前创建好了一个数组存放-128-127的数据，如果超过才会new一个Integer出来 


自动拆箱则是调用intValue方法 直接返回value值


>他的实际应用：比如我们要进行四则运算的时候，我们要使用基本数据类型进行运算。但是我们进行equals或者存入到HashMap中时，我们需要装箱令其成为包装类调用相应的方法。


<br>

## Integer的valueOf方法与parseInt方法以及Number类的intValue


parseInt是将字符串转换为基本数据类型int  

valueOf是将字符串转换为包装类型Integer，并且valueOf底层调用了parseInt

intValue返回基本数据类型，是对象调用 其他两个是Interger调用静态方法，intValue相当于强制转换 

float double也都继承了这个方法他们会直接转换精度转换为int Long也是直接强制转换(int)

parseInt的效率更高


<br> 

## 有时候不用int声明而用integer声明？

因为int默认返回值为0，Integer返回值为Null，如果用int就导致分不清是默认的0还是传入的0

其次，泛型也是不支持基本类型的  

<br>

## byte i=1 i+=1 与i =i+1有什么区别？

第一个 i的类型仍为byte 第二个i的类型变为了int 因为byte与其他数做操作默认转换为int类型操作 

## i=i++值为多少 

值为1 反编译发现 操作数栈在变量进行自加之后，将1返回给变量 导致i假自增





<h4>面试题： </h4>

        int a =100;
        int b =100;
        System.out.println(a==b);

        Integer c =100;
        Integer d =100;
        System.out.println(c==d);

        Integer e =new Integer(100);
        Integer f =new Integer(100);
        System.out.println(e==f);

        c=200;
        d=200;
        System.out.println(c==d);

答案: true true false false  

>第一个由于是基础类型直接比较数值是否一致。第二个是包装类型，自动装箱，由于在范围内，不创建新的integer。第三个是new出来的，每一个值都单独放在自己实例对象中，他们地址不一样。第四个也是由于超出了范围，所以每一个都需要新创建，比较地址不一样。

<br>


###  备注： 

>1. 小数默认是浮点类型
>2. float和double不可以直接参与四则运算，因为其范围问题可能会出错。如果使用小数参与运算要使用BigDecimal


假如0.1+0.5 直接运算可能得到的是0.600001。那么实际应用中就可能会出现问题，余额还剩0.6元却买不了0.1+0.5的东西，所以计算的时候使用使用BigDecimal  

BigDecimal a =new BigDecimal("0.1")

BigDecimal b =new BigDecimal("0.5")
a.add(b) 这里需要注意两点 一个是要将其转换为字符串，其次是他俩不可以直接运算，需要调用方法。


<h3>补充：  </h3>
转换原则：从低精度向高精度转换byte->(short、char)->int->long->float->double

两个char,short,byte型运算时，自动转换为int型；当char,short,byte与别的类型运算时，也会先自动转换为int型的，再做其它类型的自动转换






<br>

## Object有哪些方法 


equals hashCode wait notify notifyall clone toString getClass


<br>

## clone 深克隆浅克隆

浅克隆就是复制一个类，然后类中的基本类型复制过来，引用类型只将地址复制过来也就是复制前后，两者类中的引用类型指向相同，修改一个另一个也会变化，实现浅复制只需要类继承Clonable接口，然后继承父类的clone方法默认就是浅克隆。实现深克隆的话将类中的引用类型在进行克隆，但是如果有多层引用，那么用序列化的方法可以实现深克隆【每个类都要实现序列化】

浅克隆只需要继承Clonable接口，实现父类默认的clone方法 并且转换为public
深克隆继承Clonable接口，然后重写父类方法 这里是在Teacher类中有一个Studenet属性， 他们都实现clone方法 

    ` @Override
    public Object clone() throws CloneNotSupportedException {

        Object obj=super.clone();
        Student str =((Teacher)obj).getStudent();
        ((Teacher)obj).setStudent((Student) str.clone());
        return obj;
    }`

	
多层的克隆问题使用序列化 

			public Teacher deepclone() throws IOException, ClassNotFoundException {
			        ByteArrayOutputStream bos = new ByteArrayOutputStream();
			        ObjectOutputStream oos = new ObjectOutputStream(bos);
			
			        oos.writeObject(this);
			       oos.flush();
			        // 反序列化
			        ByteArrayInputStream bis = new ByteArrayInputStream(bos.toByteArray());
			        ObjectInputStream ois = new ObjectInputStream(bis);
			
			        return (Teacher) ois.readObject();
    	}

注意： 所有的类都要实现序列化 


<br>

##  什么是序列化与反序列化 

序列化就是将对象转为可保存或可传输的状态，比如用于将其写入数据库硬盘或者远距离传输调用，他需要对象实现serializable接口

反序列化就是将其从保存 传输状态转换回对象的状态 

<br>

## serialVersionUID作用 

它用来判断类是否被修改，反序列化时候会与传来的值与类中的ID比较如果不相等则说明被修改不反序列化，我们可以显示设置他的值，如果不设置，他默认设置为1L

transient可以使变量不被序列化，反序列化是其默认值为0  父类如果不实现serializable接口 子类实现，那么他必须有一个默认的构造函数，因为创建子类要求先创建父类，如果为空构造函数，那么父类的变量都为默认值 

<br>

## 不使变量序列化有几种方法 

1. transient关键字 
2. 将变量放在父类
3. static关键字 因为他属于类不属于对象








<br>

## ==与equals的区别

对于基本数据类型 ==比较的是值是否相等 对于引用类型来说比较的是地址是否相同 

equals默认情况下与==相同，但是有一些类重写的方法比如String Integer他们都只是比较数值是否相同，我们自定义类的时候也可以重写object的equals方法，必须同时重写hashCode方法。



## equals方法与hashCode方法的关系  

hashCode返回的值相同 equals方法不一定返回true 

equals方法返回true  hashCode返回值一定相同 

equals方法返回false hashCode返回值不一定不同 

hashCode返回值不同 equals一定返回false  

重写equals方法一定要重写hashCode方法。很多时候，比如在HashMap中插入数据是，我们首先判断他们的hashCode是否相等，然后再进行equals判断，不重写可能会导致如上四条推论出问题 

一个类对象要作为一个hashMap的key,那该类一定要重写hashCode与equals方法 



<br>

## 关键词 

>throw与throws

thorw跟在方法声明后边 表示抛出该异常由调用者处理 可以添加多个异常 由逗号隔开，不一定会出现异常知识表示可能 

throw 在方法体中表示抛出异常，只能连接一个异常 


>final finally finalize

final修饰变量时表示他不可修改 修饰方法时候表示他不可以被重写但可以重载 、

finally用在try catch语句块最后表示不论是否发生异常 都会执行finally语句块 

finalize是object的方法，是垃圾回收时如果没有指向该对象的引用就先调用finalize方法然后再判定是否回收，当然他的优先级很低在回收之前不一定会被执行到 


>



##  Java特性 


<br>

java的三大特性  封装 继承 多态

>封装：将对象的属性与实现细节封装起来，使其模块化 更加安全

>继承：将一些共同的模块抽象出来，提高了代码的复用率

>多态：一个实体在不同的情况下有不同的表现形式，他提高了拓展性同时也更加容易维护【更新的话添加一个新的子类，令其指向父类，这样不用修改太多代码】。分为编译时多态【重载 @Override】 与 运行时多态【重写 @OverWrite】 


多态根据实际应用又分为传参多态和赋值多态： 

传参多态是父类饮用指向子类对象 赋值多态是指将子类作为参数传递到需要父类参数的类中


向上转型与向下转型  ：  向上转型是父类对象指向子类引用 向下转型是强制将父类对象转换为子类对象






<br>

## 面向对象与面向过程

>面向过程是一种自顶向下的编程，他不需要实例化，性能比面向对象高

>面向对象是对事物进行抽象，可以设计低耦合的系统。使系统更易于维护，但是性能会比较低


## 关于继承的笔试题


    public class Son  extends  Father{

		    static  int x =SonStatic();
		    static{
		        System.out.println("调用了子类的静态代码块");
		    }
		
		    {
		        System.out.println("调用子类的非静态代码块");
		    }
		    public  static int SonStatic(){
		
		        System.out.println("调用了子类的静态变量");
		        return 1;
		    }
		
		    public Son() {
		        System.out.println("调用了子类的构造函数");
		    }
		
		    @Override
		    public void extMethod() {
		        System.out.println("调用了子类的函数");
		    }
		
		    public static void main(String[] args) {
		        Father father= new Son();
		        father.extMethod();
		    }
	}
	class Father{
		    public Father() {
		        System.out.println("调用了父类的构造函数");
		    }
		
		
		    static{
		        System.out.println("调用了父类的静态代码块");
		    }
		
		    public static int fatherStaic(){
		        System.out.println("调用了父类的静态变量");
		        return 1;
		    }
		    static  int y =fatherStaic();
		    public void  extMethod(){
		        System.out.println("调用了父类的方法");
		    }
		    {
		        System.out.println("调用了父类的非静态代码块");
		    }
	}

最终的输出：

    调用了父类的静态代码块
	调用了父类的静态变量
	调用了子类的静态变量
	调用了子类的静态代码块
	调用了父类的非静态代码块
	调用了父类的构造函数
	调用子类的非静态代码块
	调用了子类的构造函数
	调用了子类的函数

>规则：先调用父类的静态方法和静态变量【他俩谁在前边先调用谁】，然后调用子类的静态变量和方法。然后调用父类的非静态代码块和构造函数，然后调用子类的非静态代码块和构造函数，最后调用子类的函数

为什么子类加载之前一定要加载父类，否则的话子类不知道该继承什么方法，哪些属性 





## 笔试题2 


    public class Test1  extends Father{

	    int x =10;
	    int y =1000;
	    public  static void show(){
	        System.out.println("子类的show");
	    }

	    public static void main(String[] args) {
	        Father father =new Test1();
	        System.out.println(father.x+ " "+father.y);
	        father.show();
	    }

	}

	class Father{
	    public int  x =0;
	    public static int y=10;
	    public static void show(){
	        System.out.println("父类的show");
	    }
	}

>答案：0 10 父类的show  
虽然发生了多态，但是方法的调用时动态绑定的过程，但是静态方法与常量都已经静态编译完成了，



			class Person{
			 		String name = "person";
			}
			 
			class Son extends Person {
			 		String name = "son";
			}
			public class Test {
					 public static void main(String[] args){ 
					  Person p = new Son();
					  System.out.println(p.name);
					 } 
			}

答案： person 静态绑定，p是Person类型 所以.name找的就是person的name

<br>  

## 静态绑定与动态绑定 UNDO

静态绑定就是程序执行前就能确定该方法属于哪个类，通常由编译器实现，一般static private final 修饰以及构造方法的都是静态绑定。

而动态绑定比如父类引用指向子类对象，他在子类对象新创建的时候在方法区中出创建一个方法列表，包含父类的方法，他们在常量表中的index相同，这样再根据他们相应的类型去调用相应的方法。




<br>

## 抽象类与接口的异同

接口主要是强调功能上的规范，他表示一个类应该具有什么功能，抽象类则是强调逻辑上的规范，他表示的一种从属关系以及该逻辑类型所具有的特性。 

从代码角度：

1.  接口不可以有构造函数 但是抽象类可以有构造函数（用来初始化一些字段）
2.  一个类只能继承一个抽象类 但是可以实现多个接口   
3.  接口中只能声明常量关键词为public static final  抽象类中可以声明常量也可以声明变量也可以使用private关键字 
4.  接口1.8之后可以有实现方法但是必须为default 方法名 ，抽象类既可以有抽象方法也可以有实现方法 


相同点： 

1.一个非抽象类类无论是继承接口或者实现抽象类 都必须完全实现所有抽象方法

2.接口与抽象类都可以没有抽象方法



##  Override与OverWrite的区别 


<br>

>重载是静态类型确定 属于编译时多态 是同一个类中方法的方法名相同 形参的个数，顺序，类型不同。 *注意：只与参数有关，与返回值，修饰符无关*


>重写是动态类型确定 属于运行时多态 是指子类继承父类，只将父类的方法的方法体重写，原则是两同两小一大 

两同： 参数列表相同 方法名相同 
两小： 抛出的异常比父类小 返回的类型小于父类的返回类型【即可以返回返回父类方法返回值的子类】
一大： 子类的访问权限大于父类的访问权限


    public class String01 extends  demo1{

	    @Override
	    public  Integer   nihao() {
	        System.out.println("1111");
	        return 1;
	    }

	}
	class  demo1{

	    protected  Object nihao(){
	        System.out.println("nihao");
	        return 1;
	    }
	}







<br>



##  进程线程 

<br>

###   进程与线程

进程的概念：进程就是指正在运行的程序

线程的概念： 线程是进程的执行过程 

进程是资源分配的最小单元，线程是程序调度的最小单元。线程属于进程，一个进程最少拥有一个线程。多个线程共同一个进程的资源，
  

###  创建进程的方法 


1. 继承thread方法 实现run方法  



    	public class MyThread extends  Thread{

   		 @Override
   		 public void run() {
        	super.run();
   		 }

    	public static void main(String[] args) {
       		 new MyThread().start();
    	}}

2. 实现runable接口，实现run方法  
  
   

		public class MyThread  implements  Runnable{

   			 @Override
   			 public void run() {
       			 System.out.println();
    		}

    		public static void main(String[] args) {
       			 Thread t =new Thread(new MyThread());
   		   }
		}
或者直接使用匿名内部类创建 


	       public class MyThread {
    				public static void main(String[] args) {
    	   			 Thread t =new Thread(new Runnable() {
    		        @Override
    		        public void run() {
    		            System.out.println("nihao");
    		        }
    		    	});
     			   t.start();
    			}
		}









3. 继承callable方法 



    `	
	
		public class MyThread implements Callable {
	
   			 @Override
   			 public Integer call() throws Exception {
       			 return 1;
   		 	}
	
    		public static void main(String[] args) {
       		 FutureTask ft =new FutureTask(new MyThread());
        		try {
          		  	int x =(int)ft.get();
            		System.out.println(x);
        		} catch (InterruptedException e) {
            		e.printStackTrace();
        		} catch (ExecutionException e) {
           			 e.printStackTrace();
        		}

    		}
		}



<br>

三种方式的比较：

thread方法创建相对简单，查看当前线程直接this就可以。而runable则需要Thread.currentThread.getName才可以看

runable数据与操作分离，非常适合多线程调用同一份资源的场景，比如多个售票站卖同一批火车票

callable可以返回数值，并且可以抛出异常

# 【UNDO】 callable实现原理  线程池 


 


