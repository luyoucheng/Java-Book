# j2ee


----------


<br>


## servlet 

<br>

## servlet的层级结构 


<br>

![](https://s2.ax1x.com/2019/08/05/e2mBQA.png)





我们自己创建的类是要继承HttpServlet，然后HttpServlet如图示形成继承机构，下边也会依次介绍其结构 




##  ServletConfig与ServletContext

<br>


>ServletContext在tomcat一启动就会创建，在tomcat运行结束销毁，整个过程只会创建一次，用来获得一些全局配置以及存储一些所有servlet公共用的数据 



>ServletConfig对于一个Servlet只有一个，也就是说一个Servlet对应一个ServletConfig，存储获取该servet特有的信息，比如该servlet的名字等，他在创建servlet之前，通过父类GenericServlet写好的init方法将信息传递给SerletConfig信息中。所以他在创建servlet之前创建 。


<br>

>也就是说他们与Servlet的创建顺序为，ServletContext ----  ServletConfig-----Servlet



<br>


## Servlet的声明周期 


<br>


1. 初始化： 有请求过来时，服务器从web.xml中找到相应的servlet。如果创建了直接调用，如果没创建，并调用init方法初始化（整个生命周期只调用一次）

2. 服务： 请求经过service方法判断请求类型调用相应的doGet doPost等方法处理请求 


3. 销毁：在web服务停用前调用servlet的destory方法（整个生命周期也只调用一次）


<br>

##  Servlet生命周期相关的一些方法定义位置


<br>



>Servlet接口中定义了基本的方法 如init service destory方法 


<br>


>GenericServlet主要是实现了Init方法，将servletConfig中传入进来，然后添加了一些关于读取servlet配置信息的方法 


![](https://s2.ax1x.com/2019/08/06/eWJSv6.png)


>HttpServlet则多实现了service方法，可以通过不同请求调用不同的方法 




<br>

## get与post区别


<br>


① get方法主要是用于查询（因为他符合幂等性和安全性），post这种是用于传递数据


② get方法会将参数直接添加到url后边，相当于明文传输，post则是放到请求体中

③ get方法有最大传输限制，post没有

④ get可以被缓存，可以被保留在历史记录，可以被收藏，post不行



<br>

## 转发与重定向区别

<br>

>两者都是将页面信息转到新的页面，转发是服务器内部进行转换，重定向则是通知浏览器要跳转到新的地址。所以首先是 转发浏览器只发出一次请求，重定向浏览器发出了两个请求 


<br>

>转发只请求一次，所以他的URL地址不会发生变化。 重定向URL地址会转换成心底

<br>

>转发可以将当前页面数据的直接传递给新界面实现数据共享，但是重定向不可以 

<br>

>转发一般用于登录后跳转到新界面，重定向用于注销或挑战到新的网站  


<br>

>转发由于只请求一次，所以效率要比重定向高  


<br>

## servlet是否是线程安全的 

<br>


>servlet是单例多线程的，每一个请求都交给相应的唯一的servlet处理，但是可能会多个线程访问，因此要使用变量的话尽量不要使用全局变量 


<br>

# jsp


----------

<br>




## jsp与servlet


>两者其实都是特殊的java文件，他们都是编译为class文件。只不过servlet是先编译再传入到tomcat，jsp是传入到服务器当第一次使用在编译，并且tomcat会监控他的变化，jsp修改后，他会重新编译，如果不变化就一直用同一个jsp编译后的class


<br>


## jsp中的内置对象 


<br>


· request: 将客户端的请求封装  

· response:将服务器响应的信息封装 

· pageContext:上下文对象，通过该对象获得其他对象 

· session: 封装会话信息

· out:用于输出的对象

· config: 配置信息的对象

· page:jsp页面的本身 

· application:封装服务器运行环境的对象 

· exception:封装抛出异常的对象




<br>

## getAttribute与getParameter区别

<br>

getParameter用于接收从服务器传递来的参数，返回的是String 


getParamaer相当于是一个Map，是一个容器用于各个两个页面之间的参数传递，setAttribute(key,value) 
getAttribute获取值，返回的是Object



<br>

## jsp的四种作用域 

<br>

· page 是当前页面的对象属性等 只维持在当前界面

· request 是一次请求的封装，只维持在这一个界面或者传递到另一个界面

· session是一次会话，维持一次会话，一般是一个用户使用一个session 


· application: 是整个web的相关的存储，是一个全局作用域 



<br>

## 实现会话跟踪的四种方法 

<br>


1. cookie ,存储在客户端不占用服务器内存 但是有的浏览器可能会禁用 


2. url重写 他可以将相关识别信息添加到URL末尾，在cooke被禁用可以使用，但是相对繁琐 

3. 隐藏表单域，他可以将数据隐式的传递给服务器，但是只能用在表单提交过程中 

4. session 他可以将数据存储在服务器， 缺点就是会占用内存，当然也可以将数据持久化，比如存储搭到Redis中。


<br>

##  session与cookie的区别 

两者最主要的区别就是保存的位置不同，cookie保存在客户端，session保存在服务器，session相对安全一些 





