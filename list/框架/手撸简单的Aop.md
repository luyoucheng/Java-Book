# 手撸简单的AOP容器 



----------



<br>

在前边已经详细的学习了动态代理的知识，spring中aop容器就是利用动态代理这个原理来进行面向切面的编程，这里我们写一短简单的代码演示一下aop容器。



<br>


>首先我们简单梳理一下aop的结构，首先我们要有一个被进行代理的类，也就是切点。 我们主要演示JDK动态代理，那么这个类一定要实现一个接口,所以首先需要一个接口，一个实现类。  

<br>

>然后则是我们要让他增强添加什么功能，一个类单独实现一个方法，表示增强的功能，我们称为通知，所以还要有一个通知类 

<br>

>其次就是作为通知会有前置通知，后置通知等等，我们要定义一个类来确定我们是哪种通知 

<br>

>最后就是Aop类，根据这些条件生成代理对象 



综上：

1.  IDemoMethod 切点接口 DemoMethodImpl 切点实现类  

2.  通知类

3.  Advice 通知接口 BeforeAdvice前置通知类 

4.  Aop生成代理对象类  

5.  Test测试类



<br>

>切点接口 

		public interface IDemoMethod {
    		void printHello();
		}

>切点实现类 

		public class DemoMethodImpl implements IDemoMethod {
		    @Override
		    public void printHello() {
		        System.out.println("Hello world");
		    }
		}

>通知类接口，添加拓展功能的话就实现这个方法 

		public interface MethodInvoke {
    			void invoke();
		}
>通知类型接口，确定哪种通知类型就实现这个接口 

		public interface Advice  extends InvocationHandler {
		}
>前置接口类，要求将切面与通知传入 

		public class BeforeAdvice  implements Advice{
			    private Object object;
			    private MethodInvoke methodInvoke;
			
			    public BeforeAdvice(Object object, MethodInvoke methodInvoke) {
			        this.object = object;
			        this.methodInvoke = methodInvoke;
			    }
			
			    @Override
			    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
			        methodInvoke.invoke();
			        return method.invoke(object,args);
			    }
		 }
>生成代理对象类 

		public class Aop {

		    public static Object getProxy(Object obj,Advice advice){
		       return  Proxy.newProxyInstance(Aop.class.getClassLoader(),obj.getClass().getInterfaces(),advice);
		    }
		}
>测试类

		public class Test {

			    public static  void main(String[] args) {
			        DemoMethodImpl dmi =new DemoMethodImpl();
			        BeforeAdvice ba =new BeforeAdvice(dmi, new MethodInvoke() {
			            @Override
			            public void invoke() {
			                System.out.println("Hello!");
			            }
			        });
			        IDemoMethod proxy = (IDemoMethod) Aop.getProxy(dmi, ba);
			        proxy.printHello();
			    }
		 }

>执行结果 

		Hello!
		Hello world

可以看到实现了简单的AOP前置通知的功能



PS： 后置通知就是将通知代码放在切面代码执行的后边，环绕通知就是在前后都添加方法，异常通知就是添加try catch语句块，将通知代码放在catch语句块中，返回通知就是在try中return 然后在finally中执行通知
		