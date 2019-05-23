# JVM

<!-- MarkdownTOC -->
 <!-- TOC -->
- [JVM](#jvm)  
   - [1. 你对Java的理解？](#1-你对java的理解)    
   - [2. 平台无关性如何实现？](#2-平台无关性如何实现)     
	     - [2.1 如何查看字节码（Javap）](#21-如何查看字节码javap)     
	      - [2.2 流程图](#22-流程图)    
	       - [2.3 为什么JVM不直接将源码解析成机器码去执行？](#23-为什么jvm不直接将源码解析成机器码去执行)   
	- [3. JVM](#3-jvm)      
		- [3.1 JVM虚拟机](#31-jvm虚拟机)       
		  - [3.2 JVM结构](#32-jvm结构)       
		   - [3.3 Jvm如何加载.class文件](#33-jvm如何加载class文件)  
	- [4. 反射](#4-反射)      
		  - [4.1 反射的概念](#41-反射的概念)       
		  - [4.2 反射的实例](#42-反射的实例)       
		  - [4.3 反射的流程（类从编译到执行）](#43-反射的流程类从编译到执行) 
 - [5. classloader](#5-classloader)       
		  - [5.1 简介](#51-简介)      
	  - [5.2 classloder的种类](#52-classloder的种类)        
	  - [5.3 classloder的加载原理](#53-classloder的加载原理)     
	  - [5.4 双亲委派机制](#54-双亲委派机制)          
			  -      [5.4.1  为什么使用双亲委派机制？](#541--为什么使用双亲委派机制)auto  
			  -               [5.4.2  java能不能自己写一个类叫java.lang.System](#542--java能不能自己写一个类叫javalangsystem) 
  - [6. Java类的装载过程](#5-java类的装载过程)     
	  -   [6.1  loadClass与Class.forName 区别](#61--loadclass与classforname-区别)
	-	[7. JVM内存模型](#7-jvm内存模型)      
         - [7.1 线程私有部分](#71-线程私有部分)           
	         -  [7.1.1  局部变量表与操作栈的区别](#711--局部变量表与操作栈的区别)auto       	
	         -  [7.1.2  递归为什么会引发java.lang.StackOverflowErrow异常？](#712--递归为什么会引发javalangstackoverflowerrow异常)           
	         -  [7.1.3  虚拟机栈过多会引发java.lang.OutOfMemoryError异常？](#713--虚拟机栈过多会引发javalangoutofmemoryerror异常)        
	     - [7.2 线程共有部分](#72-线程共有部分)         
		     -  [7.2.1 方法区](#721-方法区)            	
		     -  [7.2.2 堆](#722-堆)       
		 - [7.3 JVM三大调优参数含义](#73-jvm三大调优参数含义)      
	  - [7.4 内存分配策略](#74-内存分配策略)      
	  - [7.5 堆与栈的联系](#75-堆与栈的联系)      
	  -   [7.6 堆与栈的区别](#76-堆与栈的区别)         
		  -    [7.6.1 栈的动态分配与静态分配](#761-栈的动态分配与静态分配)        
		 - [7.7 内存之间的联系](#77-内存之间的联系)       
		 -  [7.8 各个内存中存放的内容](#78-各个内存中存放的内容)<!-- /TOC -->

<!-- /MarkdownTOC -->

首先我们通过一个问题引入对Java底层知识JVM的学习。
## 1. 你对Java的理解？
   

> 这是一个很笼统，很开放的问题。这就涉及到很多考察角度，比如基础知识，主要模块，运行原理等，而且往往会根据你的答案深入展开，因此答好这个问题是很重要的，
> 
> 首先我们汇总一下Java的语言特性：
>     
> 1.平台无关性
> 
> 2.GC
> 
> 3.语言特性：泛型 反射 lam 
> 
> 4.面向对象：封装 继承 多态
> 
> 5.类库 IO NIO 并法库
> 
> 6.异常处理


然后我将从以上这几个角度对问题进行展开学习与分析。


<BR>

## 2. 平台无关性如何实现？

<BR>
   Compire once run anywhere(一处编译，多处运行)
> 
> java我们一般分为两个时期，编译期与运行期
> 
> 编译就是 javac直接操作java源码将其存储到.class文件中，具体展示如下：

首先先创建一个java类

<BR>


    	public  class Demo{
				public static void main(String []args){
					int x =1; int y=5;
					x++;  y++;
					System.out.print(x);
					System.out.print(y);
				}	
		}
> 进入到该文件路径 然后执行Javac Demo.java 命令， 同级目录下就会创建一个相应的class文件（JAVA字节码文件）这里边包含了变量，函数等等以及.class信息等等 我们打开来看。

![](https://s2.ax1x.com/2019/05/22/VpwKVH.png)


> 实际上这都只是一些乱码，这是我们就需要利用**Java自带的反编译指令**查看

<BR>

### 2.1 如何查看字节码（Javap）
<BR>
> 他可以将查看javac编译文件生成的字节码，用于分解.class文件
> 
> 执行javap指令得到分解后的文件就查看到了分解后的内容

<BR>


    Compiled from "Demo.java"
	public class Demo {
 	 public Demo();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  	public static void main(java.lang.String[]);
    Code:
       0: iconst_1
       1: istore_1
       2: iconst_5
       3: istore_2
       4: iinc          1, 1
       7: iinc          2, 1
      10: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
      13: iload_1
      14: invokevirtual #3                  // Method java/io/PrintStream.print:(I)V
      17: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
      20: iload_2
      21: invokevirtual #3                  // Method java/io/PrintStream.print:(I)V
      24: return
	}

<BR>

> 首先系统自动为我们创建了一个构造函数，然后Code 0与1显示调用了父类object的构造函数
> 然后后边执行Main方法执行我们自己编写的代码，基本流程就是压栈弹栈等等，我们只做大概了解


### 2.2 流程图

![](https://s2.ax1x.com/2019/05/22/VpB0g0.png)

<Br>

> javac先编译Java文件生成Java字节码文件然后存放到class文件中，
> 
> 不同平台JVM在解析class文件然后生成相应平台的执行指令 【不同平台有不同的JVM】

*这就实现了一处编译，到处执行*

<BR>

### 2.3 为什么JVM不直接将源码解析成机器码去执行？

<BR>

> 在之前我们学习了java的跨平台特性，这里边有一个步骤是生成java字节码文件。这时我们假如不生成java字节码，直接将java文件传给jvm去跨平台解析也是可以的，但是为什么不这样呢？
> 
> 原因1:  提高系统性能。 缺少这一过程，导致每次转码都要检查语法，句法，语意等等，比如这份文件我会在win ios都多次使用，并且文件有一个BUG，那么在WIN解析发现BUG但是不会记录BUG，IOS端不会知道也要自己解析才会发现，共解析了多次。引入中间的java字节码文件，出错的话记录错误改错重新编译只有一次，因此引入字节码问价极大的提高了性能。
> 
> 原因2： 提高兼容性。如果其他语言（Ruby）生成字节码文件也可以被JVM解析使用。

<bR>

## 3. JVM


### 3.1 JVM虚拟机

> 他是虚拟的计算机，通过软件仿真模拟计算机功能。他有完善的硬件架构，如处理器，堆栈等，他屏蔽了底层操作系统原理 这是一个内存中的虚拟机，写的所有类，变量，方法都在内存中，JVM本质上就是一个进程。

<br>

### 3.2 JVM结构

![](https://s2.ax1x.com/2019/05/22/V9pkxH.png)

<br>
>
>它主要是由四部分组成  
>
> 1.class loader :将符合要求的class文件添加到内存中（以字节数组的形式加载）将其转换为Class对象
> 
> 2.Runtime Data Area: JVM内存空间结构模型
> 
> 3.Execution Engine:解析字节码提交给操作系统执行
> 
> 4.Native Interface:融合不同开发语言原生库给JAVA库使用 Class.ForName()底层就是用的本地方法

### 3.3 Jvm如何加载.class文件


> 
> 简单的过程就是classloader将符合格式要求的class文件加载到内存，Execution Engine对字节码进行解析
> 交给操作系统执行
> 

<br>

> 较为详细的解释应该是classLoader根据双亲委派机制，调用loadClass选择出合适的类加载器，类加载器先调用findClass方法将其转换为二进制字节码文件传递给defineClass，他调用本地方法生成Class对象加载到内存中去。
> 
> 
> 




## 4. 反射 

  <br>

### 4.1 反射的概念 

<br>

> 
> 反射就是在程序运行状态下，可以将一个类拆分为Methon filed class等几个对象，从而获取类的属性，方法。
> 也可以对一个对象，调用他的任意方法与属性。这种动态获取与动态调用的情况较为反射。


### 4.2 反射的实例 

> 实例类


    public class DemoClass implements Runnable{
   	 	 private  int   privatefield=1;
   		 public  int publicfield =2;
   		 public  void  publicMethod(){

        	System.out.println("this is publicMethod");
   		 }
    	 private void  privateMethod(){
       		 System.out.println("this is privateMethod");
   		 }

   		 @Override
    	public void run() {
        	System.out.println("this is run");
   		 }

    	public static  void staticMethod(){
       	 	System.out.println("this is staticMethod");
   		 }
	}

>应用类

    public class reflectDemo {
    public static void main(String[] args) throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchMethodException, InvocationTargetException, NoSuchFieldException {
        /**
         *
         * 创建对象 动态调用方法与属性
         */
      
        Class c = Class.forName("base_code.reflectDemo.DemoClass");

        //创加对象
        DemoClass dc =(DemoClass)c.newInstance();
        //调用方法
        dc.publicMethod();
        System.out.println(dc.publicfield);
       //由于他们不在同一类 所以是无法调用private的方法与属性的

        /**
         *
         *  动态获取方法与属性
         */


       //    继承方法的调用
        Method  extendsMethod = c.getMethod("run");
        extendsMethod.invoke(dc);

        //    私有方法的调用
        Method  privateMethod = c.getDeclaredMethod("privateMethod");

        //必须有这个设置 否则报错 can not access a member of class base_code.reflectDemo.DemoClass with modifiers "private"
        privateMethod.setAccessible(true);
        privateMethod.invoke(dc);

        //      属性的调用
        Field publicField =c.getField("publicfield");
        publicField.set(dc,10);

        //    私有属性的调用
        Field privateField =c.getDeclaredField("privatefield");
        privateField.setAccessible(true);
        privateField.set(dc,10);


    	}
	}

**获得类的属性就必须获取类的Class对象，想要获得Class对象就必须先获取class字节码文件，从字节码文件到class对象的过程就是classloader运行的过程**


### 4.3 反射的流程（类从编译到执行）

> 
> classLoader调用loadClass找到合适的类加载器，然后调用当前类加载器的findClass()将class字节码文件生成二进制文件，然后由defineClass调用本地代码生成Class对象，然后由CLass对象生成相应类的对象实例，然后去相关属性 方法。
> 
> 

 
## 5. classloader 

### 5.1 简介

<br>

> 
>  他主要工作在Class装载的加载阶段，他的主要作用是从系统外部获得class二进制数据流，然后装载进系统给Java虚拟机进行连接，初始化等操作 所有的class都是由classloader加载的
> 

<br>

### 5.2 classloder的种类




![](https://s2.ax1x.com/2019/05/23/VCQYtO.png)


>
>Bootstrap :c++编写  加载核心库 java*
>
>Extension : Java编写 加载拓展库 javax.*
>
>app：       Java编写 加载程序所在目录
>
> custom:   Java编写，用户自己定义的加载器，他加载可以不是class文件，是由用户自己定制的 


<br>

### 5.3 classloder的加载原理 
    
> 只是单纯的考虑一个类加载器，他大概的加载过程主要是由两个函数完成的
> 
> 第一个是findClass: 他的作用是找到class文件，并将其转换为二进制字节码文件 传递给defineClass
> 
>第二个就是defineClass:他将二进制字节码文件转换成了Class对象载入内存中 
>
> 下边将编写一个自定义的类加载器


**1. 先在外部环境创建一个Java文件，并通过javac将其编译为.class文件**

<br>
		
		//java文件
    	public class test{
			public static void main(String []args){
				System.out.println("你好");
			}
		}



<br>

**2.创建一个自定义的类加载器，主要是使用findclass与超类定义的final类型的defineClass**

`			

	import java.io.*;
    public  class MyClassLoader extends  ClassLoader{
    	private String path;  //目标路径
		private String classLoaderName ;  //当前类加载器名称

   		 public MyClassLoader(String path,String classLoaderName) {
       	 	this.path=path;
        	this.classLoaderName=classLoaderName;
   		 }

    	/**
    	 *
    	 * findclass用来生成二进制字节码文件，将结果给父类中的defineClass执行 返回Class对象
    	 */
   		 @Override
   		 protected Class<?> findClass(String name) throws ClassNotFoundException {
       		 byte [] b = loadClassDate(name);
        	return defineClass(name,b,0, b.length);
   		 }

    	/**
   		  * 
   		  * 利用IO 将文件读入，将文件以二进制字节码输出，并返回字符串数组
    	 * 
    	 */
   		 private byte[] loadClassDate(String name){
      		  String url =path+name+".class";
      		  File file =new File(url);
      		  InputStream is=null;
      		  ByteArrayOutputStream bos=null;
      		  try {
      			     is =new FileInputStream(file);
          		 bos =new ByteArrayOutputStream();
          		 int i =0;
          		 while((i=is.read())!=-1){
            	   bos.write(i);
           		}
        	} catch (FileNotFoundException e) {
         		   e.printStackTrace();
       		 } catch (IOException e) {
         		   e.printStackTrace();
       		 }finally {
        		    try {
            		    bos.close();
             		   is.close();
            		} catch (IOException e) {
               		 e.printStackTrace();
           			 }
       		 }
       			 return bos.toByteArray();
   		 }

	}

<br>

**3. 验证类加载器**


    public class MyClassLoaderChecker {
   		 public static void main(String[] args) throws ClassNotFoundException, IllegalAccessException, InstantiationException {
       		 MyClassLoader mc =new MyClassLoader("B:/","myclassloder");
       		 Class c =  mc.findClass("test");
        	 c.newInstance();
        	System.out.println(c.getClassLoader());
   		 }
	}`

<br>

**4. 结果验证**

    base_code.MyClassLoaderChecker.MyClassLoader@677327b6



> 可以看出自定义的类加载器起到了作用
> 
> 这在实际应用中也是很有作用的，根据应用我们知道，只要是符合要求的二进制字节码文件，我们就可以加载进内存。比如我们可以远程记载一个字节码文件，或者是将一个字节码文件进行加密，findclass的时候解密，亦或者是修改传入findclass的二进制文件，增强或者更改其功能。

<br>

### 5.4 双亲委派机制

> 5.3介绍的是单个类加载器加载class字节码文件到内存的原理，但是JVM中是有多个类加载器的。假如传进来一个class文件，该用哪个类加载器去加载他呢，Java的规定是双亲委派机制

![](https://s2.ax1x.com/2019/05/23/VCQYtO.png)



>  两个原则：
> 
>  1.自底向上检查类是否已经加载
> 
>  2.自顶向下尝试类的加载 
> 
> 大概意思就是一个class文件，从子加载器到父加载器依次检查其是否已经被加载。如果被加载，返回，如果没被加载，尽量去使用父级加载器去加载，否则再由子级加载器加载。

**直接看源码：**

    ` protected Class<?> loadClass(String name, boolean resolve)
        throws ClassNotFoundException
     {
        synchronized (getClassLoadingLock(name)) {
            // First, check if the class has already been loaded
            Class<?> c = findLoadedClass(name);
            if (c == null) {
                long t0 = System.nanoTime();
                try {
                    if (parent != null) {
                        c = parent.loadClass(name, false);
                    } else {
                        c = findBootstrapClassOrNull(name);
                    }
                } catch (ClassNotFoundException e) {
                    // ClassNotFoundException thrown if class not found
                    // from the non-null parent class loader
                }

                if (c == null) {
                    // If still not found, then invoke findClass in order
                    // to find the class.
                    long t1 = System.nanoTime();
                    c = findClass(name);

                    // this is the defining class loader; record the stats
                    sun.misc.PerfCounter.getParentDelegationTime().addTime(t1 - t0);
                    sun.misc.PerfCounter.getFindClassTime().addElapsedTimeFrom(t1);
                    sun.misc.PerfCounter.getFindClasses().increment();
                }
            }
            if (resolve) {
                resolveClass(c);
            }
            return c;
        }
     }`

> 首先是一个同步锁，防止出现多个线程同时去请求加载一个class文件的情况。
> 然后执行finLoadedClass 调用的是一个本地的方法判断当前class是否已经加载进内存中了已加载的话直接返回了
> 
> 没有加载的话就执行这段代码

    `if (c == null) {
            long t0 = System.nanoTime();
            try {
                if (parent != null) {
                    c = parent.loadClass(name, false);
                } else {
                    c = findBootstrapClassOrNull(name);
                }
            } catch (ClassNotFoundException e) {
                // ClassNotFoundException thrown if class not found
                // from the non-null parent class loader
            }`

> if(parent!=null)与else其实都是在找他父类加载器，只不过是else是ext类加载器要往上找到时候，他的父类加载器是C++写的，需用本地方法去找，别的类加载器直接parent.loadClass就可以。大概含义就是当前加载器没加载该class的话就让父类去加载，然后一直递归到父加载器也就是BootStrap加载器 。接着执行：

   		 if (c == null) {
                // If still not found, then invoke findClass in order
                // to find the class.
                long t1 = System.nanoTime();
                c = findClass(name);

                // this is the defining class loader; record the stats
                sun.misc.PerfCounter.getParentDelegationTime().addTime(t1 - t0);
                sun.misc.PerfCounter.getFindClassTime().addElapsedTimeFrom(t1);
                sun.misc.PerfCounter.getFindClasses().increment();
            }

>如果父类加载器findBootstrapClassOrNull执行结果还是为Null，那么返回结果交由EXT去加载，EXT findClass像我们之前自定义类的findClass那样去相应的地方找，有则加载没有交给子加载器，一直向下，
>最后都没有找到的话 返回classNotFoundException

*这就是双亲委派机制的大概流程，app与ext都继承自urlCLassLoader,但是代码中设定ext是app的父加载器*


<br>


#### 5.4.1  为什么使用双亲委派机制？
 
> 
> 1. 避免重复的字节码加载 
>    比如说System 我们要输出文字，我们就要加载System到系统,假如我们有多个文件要调用System,那么不使用双亲委派的话可能就会出现多个加载器加载了system,导致不唯一 
> 
> 2. 保证自己的核心API不被修改
> 如果没有双亲委派机制，而是每个类加载器加载自己的话就会出现一些问题，比如我们编写一个称为 java.lang.Object 类的话，那么程序运行的时候，系统就会出现多个不同的 Object 类。




<br>

#### 5.4.2  java能不能自己写一个类叫java.lang.System

> 
> 不可以 ，根据双亲委派机制，即使写了，也是只是执行父类加载的System, 如果自己写一个类加载器，并将目录放在父类加载器目录以外也不可以，会提示SecurityException，因为要求不可以使用java.开头的类

<br>

## 6. Java类的装载过程
![](https://s2.ax1x.com/2019/05/23/VCq6dU.png)

> 
> 其中需要注意的是准备过程是给类变量赋默认值，在初始化时候才会给其赋真正的值

### 6.1  loadClass与Class.forName 区别 
  
> 
> loadClass只会执行到装载过程的第一步：加载 
> 
> Class.forName 会加载class文件字节码，也会执行链接初始化，于是他会初始化静态代码块
> 
> 两者的应用：
> 
> 加载数据库的驱动，Driver类中有一段静态代码块 Driver.registDriver()这就需要用Class.forName去初始化他。 在spring ioc 读取bean的话如果延时加载 使用loadClass只需要加载进去，提高加载速度。

<br>

## 7. JVM内存模型 
JVM内存模型就是JVM架构中的 RUNTIME DATA AREA区域 
![](https://s2.ax1x.com/2019/05/23/VCXILq.png)


### 7.1 线程私有部分

>
> 程序计数器：
> 
> 他是用来记录当前线程下一条指令执行的行号，是线程私有的，如果执行的是Java方法，他会记录，如果执行的是本地方法 它显示undefined ,不会出现内存泄漏问题，他是逻辑计数器，不是物理计数器
>
> 虚拟机栈：
> 
> 它用来存储栈帧，每一个方法都会创建一个栈帧，并压入栈顶。一个栈帧中存储了方法的局部变量表，操作栈，动态链接，返回地址，方法调用结束栈帧销毁。
> 


#### 7.1.1  局部变量表与操作栈的区别
> 
> 一个方法的所有局部变量存储在局部变量表中，包括Int String boolean等等各种类型
> 
> 操作栈则是进行的压栈 入栈操作，执行的是交换，四则运算等操作，产生消费变量

#### 7.1.2  递归为什么会引发java.lang.StackOverflowErrow异常？

> 
> 这个错误表示虚拟机栈的栈帧太多了，执行一个方法就压入一个栈帧，递归次数太多的话压入的栈帧就太多了。到达一定的限度就超过了最大允许的深度。如果是代码错误，检查是否是缺少判定条件，导致递归一直进行，如果代码没错，可以考虑将递归改成for循环
> 


#### 7.1.3  虚拟机栈过多会引发java.lang.OutOfMemoryError异常？

> 
> java大致等于 = 堆+ 虚拟机栈*线程数 
>  
> 每创建一个线程就会分配一个虚拟机栈，那么我们无限的去创建线程，并且线程都一直在执行，最后就可能会出现占满整个内存，然后出现OutOfMemoryError异常

**相关实例代码（不要随便运行 会死机）**

    `public class OutOfMemoryDemo {
  		  public static void main(String[] args) {
       		 while(true){
          		  new Thread(){
             		   @Override
               		 public void run() {
                  		  while(true){
                        		System.out.println("running");
                   		 	}
           		   	  }
            	}.start();
        	}
   	 	}
	}`



### 7.2 线程共有部分


#### 7.2.1 方法区

> 
> 在1.7以后，常量池由方法区移动到了堆中，8之后元空间替代了永久代成为方法区的标准。
> 
> 两者区别：
> 
>元空间使用本地内存，永久代使用的是JVM内存，这就解决了空间不足的问题。 
>永久代不利于GC的回收 类与方法信息大小难以确定 给永久代大小指定带来困难 
>方便其他技术的集成，有的技术是使用永久代的。


#### 7.2.2 堆

> 
> 用来存放对象的实例
> 这是GC管理的主要区域
> 

### 7.3 JVM三大调优参数含义 

> 
> -Xss : 规定了每个线程虚拟机栈的大小 一般256K 他会影响并发线程数大小
> 
> -Xms:  堆的初始值，如果后来由于使用超过了堆 他就会扩容 直到达到最大值
> 
> -Xmx: 堆能达到的最大值  通常情况下 令xms==xmx 因为扩容会发生内存抖动，影响性能
   

### 7.4 内存分配策略

> 
> 静态存储：编译时候就能确定需要的内存大小，如静态数据、全局 static 数据和常量
> 
> 栈式存储：方法体内的局部变量（其中包括基础数据类型、对象的引用）都在栈上创建
>
> 堆式存储：程序运行时直接 new 出来的内存



### 7.5 堆与栈的联系
![](https://s2.ax1x.com/2019/05/23/VPKymq.png)
> 
> 引用对象或数组的时候，栈中存储的是堆中目标的首地址，作为一个引用变量指向堆地址。如果是对象，堆还会指向方法区中存储的class相关信息
> 
> 
 
### 7.6 堆与栈的区别

> 
> 管理方式：栈自动释放 堆需要GC
> 
> 空间大小：栈比堆大 因为堆需要存储很多对象
> 
> 碎片相关：栈产生的碎片小于堆，栈帧使用完就会释放 而且结构是连续的，堆垃圾可能会堆积一阵才会回收
> 
> 分配方式：栈支持静态和动态分配，堆仅支持动态分配 
> 
> 效率：栈效率比堆高，内存本身就是堆栈结构与栈更相符，并且栈的操作更简单 只有进栈 出栈，但是他的灵活性低于堆，因为堆底层是双向链表

#### 7.6.1 栈的动态分配与静态分配

> 
> 
> 
> 静态分配是编译器完成的，比如局部变量的分配。动态分配由函数malloc进行分配,比如可变数组。不过栈的动态分配和堆不同，他的动态分配是由编译器进行释放，无需我们手工实现
> 


### 7.7 内存之间的联系  
> 
> 书写下面一段代码来分析内存之间的关系 

    public class HelloWorld{

		pirvate String name;
		public void sayHello(){
			System.out.println("hello"+name);
		}
		public void setName(String name){
			this.name =name;
		}

		public static void main(String[] args){
			int a =1;
			HelloWorld hw =new HelloWorld();
			hw.setName("test");
			hw.sayHello();
		}

	}


>方法区 ：Class类信息： ~HelloWordl ~System  Method ~sayHello ~setName ~Main   
>
>虚拟机栈： a  hw  
>
> 堆：  1（常量池） String("test") (常量池) Object: Helloword


### 7.8 各个内存中存放的内容

> 
> 虚拟机栈：存放局部变量，对象引用等等，基础数据类型的对象 方法调用完自动回收 
> 
> 方法区： 类信息 方法信息 属性信息 
> 
> 堆： 静态变量 对象本身   需要GC回收

 





