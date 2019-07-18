# String相关问题 


<br>

## 传值还是传址问题 

<br>
>问题： 输出的值为多少？ 答案 lisi 

    `public class String01 {
		    public static void change(String name){
		        name="zhangsan";
		    }
		
		    public static void main(String[] args) {
		        String str ="lisi";
		        change(str);
		        System.out.println(str);
		    }

	}

<br>

>根据JVM内存模型（后边单独的文章讲了），每次执行一个方法都会创建一个栈帧，并且执行结束之后释放栈帧。
main方法创建一个栈帧，其中声明了str常量

![](https://s2.ax1x.com/2019/07/16/Z78JCq.png)

然后开始执行change方法，并将str的地址传递给该方法形成如下图的情况


![](https://s2.ax1x.com/2019/07/16/Z78t2V.png)

change执行name ="zhangsan" 由于string是不可变的，他需要新创建一个字符串重新指向 

![](https://s2.ax1x.com/2019/07/16/Z78aKU.png)

change方法执行完毕释放栈帧又回到最初始的状态，所以他的值仍为lisi

![](https://s2.ax1x.com/2019/07/16/Z78JCq.png)

<h4> 知识点</h4>

> jvm内存结构     &nbsp;&nbsp;&nbsp;  string是不可变的 


<h3>拓展</h3>

    public class String01 {
		    public static void change(int id){
		        id =4;
		    }
		
		    public static void main(String[] args) {
		        int id =10;
		        change(id);
		        System.out.println(id);
		    }
	}

答案为10  
>他的原理与string不同,由于他是基本数据类型，直接创建在自己的栈帧中,所以他整个过程都没有影响到main中的id值 

![](https://s2.ax1x.com/2019/07/16/Z78fqe.png)  



<br>

<br>


    `
	public class String01 {
		    String str;
		    public static void change(String01 str){
		        str.str ="liming";
		    }
		
		    public static void main(String[] args) {
		       public static void main(String[] args) {
			        String01 str01 =new String01();
			        str01.str= "lisi";
			        change(str01);
			        System.out.println(str01.str);
    
		   	  }
			}

	}`

答案 ：lisi 

![](https://s2.ax1x.com/2019/07/16/Z7J2He.png)


总结： String是引用数据类型，传递的是地址。  如果是如下string a =""这样声明创建在常量池，new出来的话全部在自己的对象堆中。





<br>

##  String str =new String("aaa")创建了几个对象

<br>

string在new的时候如果发现常量池中无该字符串就在常量池中创建一个字符串也在相应的对象堆中创建一个字符串，一共创建两个对象 

如果常量池中已经有了则只创建一个内存对象。所以他可能创建一个或者两个对象，但是他无论创建几个，他都返回的是堆内存中的地址。



<br>

##  String str1 =new String("A"+"B")创建多少个对象   

在常量池中没有A串  B串 AB串 创建三个 堆中没有AB串 创建 然后创建一个对象引用指向堆中的AB 一共5个 


##  String str1 =new String("AAA"）+"AAA"创建了多少个对象 

字符串 堆中创建了 AAA 常量池创建AAA 常量与变量相加转为StringBuilder append添加在堆中 	AAAAAA 一共3个




<br>

##  两个字符串是否相等

<br>


    	String str1 ="aaa";
        String str2 =new String("aaa");
        System.out.println(str1==str2);
 		System.out.println(s1.equals(s2));

>答案为false true   第一个直接在常量池中创建，第二个在堆内存中创建， 两者返回的引用地址不同，所以返回false。两者的值相同，所以返回的是true



		String str1 =new String("aaa")
        String str2 =new String("aaa")  
        System.out.println(str1==str2);

>这段一段代码一共创建了三个对象，第一个创建了两个分别在堆和常量池中，第二个在堆中创建一个
>答案为false,因为他们各自指向了自己堆中的地址 


		String str1 =new String("aaa");
        String str2 =new String(str1);
        System.out.println(str1==str2);

>答案为false 因为将str1传递给str2.构造函数


		public String(String original) {
	        this.value = original.value;
	        this.hash = original.hash;
    	}

>他只是将str1的值传递进去了，并有没将地址传递过去，所以仍然是创建在自己的内存中，两者地址仍然不同 返回false

<br>

##  两个字符串是否相等

<br>


		String str1 ="aaa";
        String str2 =new String("aaa");
        String str3 =str2.intern();
        System.out.println(str1==str3);

>intern会返回该对象在常量池中的引用与str1指向一致，所以返回true 


<br>

##  字符串的拼接问题 


**常量的拼接**

String str = "hello" + "world";  他在编译的时候就会自动合并为helloworld然后在常量池中查看是否有该字符串，如果有就指向，没有就创建。



**final修饰的string**

		final String str1 = "hello";
        final String str2 = "world";
        String str3 = str1 + str2;
        String str4 ="helloworld";
        System.out.println(str3==str4);

在底层实现中 如果string被final修饰 那么他会直接当做常量合并 所以返回true


**两个string的拼接**

		String str1 = "hello";
        String str2 = "world";
        String str3 = str1 + str2;
        String str4 ="helloworld";
        System.out.println(str3==str4);
在底层实现中，如果string没有被final修饰那么他会创建一个stringBuilder然后append数据 所以应该返回false 因为str3指向堆中的stringBuilder对象 str4指向常量池 




<b4>练习题</h4>

    	public class StringLiteral {

			    public static class Other {
			        public static String hello = "Hello";
			    }
			
			    public static void main(String[] args) {
			        String hello = "Hello", lo = "lo";
			        System.out.println(hello == "Hello");
			        System.out.println(Other.hello == hello);
			        System.out.println(hello == "Hel"+ "lo");
			        System.out.println(hello == ("Hel"+ lo));
			        System.out.println(hello == ("Hel"+ lo).intern());
			    }

		}

 答案： true true true false true

第一个是直接在常量池中比较 第二个同理 第三个常量合并 第四个常量变量使用stringBuilder append添加  第五个将相应的常量池的内容地址传递与hello地址一样，返回true




		String s = new String("1");
        s.intern();
        String s2 = "1";
        System.out.println(s == s2);
        System.out.println(s.intern() == s2);

        String s3 = new String("1") + new String("1");
        s3.intern();
        String s4 = "11";
        System.out.println(s3 == s4);


答案： false  true true  第一个new的时候发现常量池中没有1 创建1 常量池中已经存在了 所以s.intern直接指向   第一个为false  s.intern()指向的是常量池中的值与s2一样 返回true  第三个常量池中没有11 s3.intern令 常量池中创建指向s3的引用 ， s4看到常量池有11指向s3 所以为true    
**因为1.7之后Intern不是向常量池中拷贝值 而是地址值，执行方法如果发现常量池中没有该值，将地址值拷贝进去，如果发现有该值，直接指向**  


<br>

## String、StringBuilder和StringBuffer


他们三者都是基于char[]数组实现的，但是String较其他两者而言是不可变的，因为他的char数组定位为final，就是一旦赋值就无法修改

StringBuilder与StringBuffer相比 stringBuffer是线程安全的 stringBuilder是线程不安全的 但是效率更高 



<br>

>此外上边也提到了我们在将两个变量相加赋值给字符串的时候其实底层是通过stringBuilder的append方法实现的

所以 当我们进行 
			String str="";
			for(int i=0;i<10;i++){
				str+=i;
			}
的时候 底层实际进行的操作是 

			String str="";
			for(int i=0;i<10;i++){
				new StringBuilder(String.valueof(str)).append(i)
			}
这样他每次都要新创建一个StringBuilder非常损耗性能 

			StringBuilder sb =new StringBuilder();
			for(int i=0;i<10;i++){
				sb.append(i);
			}
            System.out.print(sb.toString());


#UNDO  VALUEOF的区别 








