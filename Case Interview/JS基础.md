每个实例对象都有一个私有属性(__proto__)指向它构造函数的原型对象(prototype)。该原型对象也有一个自己的原型对象(__proto__)，层层往上直到一个对象的原型对象为Null。

遵循ECMAScript标准，someObject.[[Prototype]]符号用于执行someObject的原型。从ES6开始，[[Prototype]]可以通过Object.getPrototypeOf()和Object.setPrototypeOf()访问器访问。这个等同于JavaScript的非标准但许多浏览器实现的属性__proto__
但它不应该与构造函数func的prototype属性相混淆。被构造函数创建的实例对象的[[prototype]]指向func的prototype属性。Object.prototype属性表示Object的原型对象


原型链的定义：每个对象拥有一个原型对象，对象以其原型为模板，从原型继承属性和方法。原型对象也拥有原型，并从中继承方法和属性，一层一层，以此类推，这种关系常被称为原型链。

__proto__和prototype:对象的原型__proto__与构造函数的prototype属性之间的区别，前者是每个实例上都有的属性，后者是构造函数的属性。也就是说，Object.getPrototypeOf(new Foobar())和Foobar.prototype指向着同一个对象。

prototype:每个函数都有一个特殊的属性叫做prototype


什么是原型和原型链？
每个对象都有一个私有属性（__proto__）指向它构造函数的原型对象（prototype）。
对象从原型继承属性和方法。原型对象也有自己的原型对象，层层往上直到一个对象的原型对象为null，这种关系被称为原型链。注：__proto__是javascript非标准但浏览器实现的属性

继承方法有哪些？
1.对象冒充
继承ClassA的方法，最后删除对ClassA的引用
function ClassA () {}
function ClassB () {
	this.newMethod = ClassA;
	this.newMethod(args);
	delete this.newMethod;
}

2.call()方法
使用call()方法指定this的指向，实现继承。
function ClassB (color, name) {
	ClassA.call(this, color);
	this.name = args;
	this.sayName = function () {
		return this.name;
	}
}

3.apply()方法
与call()方法继承一样。区别是call和apply的区别，参数传递不一样

4.原型链
function ClassA() {}
ClassA.prototype.color = 'blue';
ClassA.prototype.getColor = function () {
	return this.color;
}
function ClassB () {};
ClassB.prototype = new ClassA();
使用原型链继承，将需要父类实例赋值给子类原型对象，可继承所有的属性和方法。
缺点是：原型链上的属性和方法修改，子类也会被影响，没有传递参数。

5.组合继承
使用构造函数定义属性，用原型定义方法；用对象冒充继承构造函数的属性，用原型链继承prototype对象的方法
function ClassA (color) {
	this.color = color;
}
ClassA.prototype.getColor = function () {
	return this.color;
}
function ClassB (color, name) {
	ClassA.call(this, color);
	this.name = name;
}
ClassB.prototype = new ClassA();
ClassB.prototype.sayName = function () {
	return this.name;
}
缺点：父类的构造函数被调用两次。
a.设置子类原型对象，调用一次；ClassB.prototype = new ClassA();
b.实例化对象时调用一次；ClassA.call(this, color);

6.寄生式继承
在不需要实例化父类构造函数时，也能继承父类原型对象上的方法；
function InheritPrototype (child, father) {
	var prototype = Object.create(father.prototype); // 浅拷贝父类的原型对象
	prototype.constructor = child;	// 指向原型对象的构造器为子类
	child.prototype = prototype; // 将原型对象赋值给子类的原型对象
}

new的作用？
1.创建一个空的对象；
2.设置该对象的构造函数到另一个对象；
3.将新创建的对象作为this的上下文；
4.如果该函数没有返回对象，则返回this;

事件循环机制是什么？
1.运行时概念：
	函数调用形成一个栈帧，放到栈中，根据后进先出原则直到栈清空；
	对象被分配到一个堆中，即用以标识一大块非结构化的内存区域；
	一个javascript运行时包含了一个待处理的消息队列。每一个消息都关联这一个用以处理这个消息的函数。


2.JAVASCRIPT事件循环的运行机制：
	1.所有同步任务都在主线程上执行，形成一个执行栈；
	2.主线程之外，还存在一个任务队列。只要异步任务有了运行结果，就在任务队列之中放置一个事件；
	3.一旦执行栈中的所有同步任务执行完毕，系统就会读取任务队列，那些对应的异步任务于是结束等待状态，进入执行站开始执行；
	4.主线程不断重复上面的第三步；
	JS只有一个主线程，主线程执行完执行栈的任务后去检查异步的任务队列，如果异步事件触发，则将其加到主线程的执行栈，主线程不断重复这个过程；


ES6的新特性有哪些？
1. let和const
2. 字符串模板
3. 变量的解耦赋值
4. 箭头函数
5. Array.from Array.of
6. Set数据结构(类似数组，但是成员的值都是唯一的，没有重复的值)
7. Proxy 作用是对目标对象提供拦截行为
8. Promise对象
9. async 函数
10. module语法

ES6模块和CommonJS模块的差异：
* CommonJS模块输出的是一个值的拷贝，ES6模块输出的是值的引用
	CommonJS模块输出一个值，模块内部的变化不会影响该值，除非写一个函数才能获取内部改动后的值。
	ES6输出的值，如果原始值改变，那输出的值也会跟着变。
* CommonJS模块是运行时加载，ES6模块是编译时输出接口
	CommonJS记载的是一个对象，该对象只有在脚本运行完才会生成。
	ES6模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。


Webpack3和Webpack4有什么区别？
1.webpack4新增mode选项。根据配置类型将其设置为生产或开发。
2.弃用部分插件，包括压缩js的UglifyJsPlugin，DefinePlugin创建一个在编译是可以配置的全局常量的插件；
压缩插件改用Optimization选项，DefinePlugin内置到new Webpack里面


webpack4常用Loading有哪些？
babel-loader：允许使用babel和webpack转译js文件
css-loader
postcss-loader
sass-loader
style-loader
less-loader
url-loader
file-loader

webpack4常用的plugin有哪些？
webpack.Defineplugin：允许创建一个在编译时可以配置的全局常量
webpack.HotModuleReplacementPlugin：热替换模块
webpack.optimize.LimitChunkCountPlugin：可以通过合并的方式，处理你的chunk，以减少请求数
UglifyjsWebpackPlugin：压缩JS代码
htmlWebpackPlugin
CleanWebpackPlugin

vue和react的哪里不同？
1.React严格上只针对MVC的view层，Vue则是MVVM模式
2.virtual DOM不一样。vue会跟踪每一个组件的依赖关系，不需要重新渲染整个组件树
而对React而言，每当应用的状态被改变时，全部组件都会重新渲染，如要避免不必要的子组件重新渲染需要用shouldComponentUpdate.
3.组件写法不一样。React推荐使用JSX。Vue推荐用webpack + vue-loader的单文件组件格式，即html,css,js写在同一个文件。
4.数据绑定：vue实现了数据的双向绑定，react数据流动是单向的
5.state对象在react应用中不可变，需要使用setState方法更新状态；
vue在state对象不是必须的，数据由data属性在vue对象中管理

常见Http请求头
1. Accept 可接受的响应内容类型 Accept: text/plain
2. Accept-Charset 可接受的字符集 Accept-Charset: utf-8
3. Accept-Encoding 可接受的响应内容的编码方式 Accept-Encoding: gzip, deflate
4. Accept-Language 可接受的响应内容语言列表 Accept-Language: en-US
5. Accept-Datetime 可接受的按照时间来表示的响应内容版本 Accept-Datetime: xxx
6. Authorization 用于表示HTTP协议中需要认证资源的认证信息 Authorization: Basic OSdjJGRpbjpvcGVuIANlc2SdDE==
7. Cache-Control 用来指定当前的请求/回复中的，是否使用缓存机制 Cache-Control: no-cache
8. Connection 客户端想要优先使用的连接类型 Connection: keep-alive
9. Cookie 由之前服务器通过Set-Cookie设置的一个HTTP协议Cookie
10. Content-Length 以8进制表示的请求体的长度 Content-Length: 348
11. Content-MD5 请求体的内容二进制MD5散列值，以Base64编码的结果
12. Content-Type 请求体的MIME类型 Content-Type: application/x-www-form-urlencoded
13. Date 发送该消息的日期和时间
14. Expect 表示客户端要求服务器做出特定的行为
15. From 发起次请求的用户的邮件地址
16. Host 表示服务器的域名以及服务器所监听的端口号
17. Origin 发起一个针对跨域资源共享的请求
18. User-Agent 浏览器的身份标识字符串

promise、async有什么区别?
1. Async是基于Promise实现的，它不能用于普通的回调函数
2. Async使得异步代码看起来像同步代码
3. Async函数会返回一个Promise对象
4. Async让 try/catch可以同时处理同步和异步错误
5. Async函数让代码更简洁，更符合语义

防抖和节流？
防抖：函数在频繁需要触发情况时，只有等足够空闲的时间才去执行一次。
节流：预定一个函数只有在大于等于执行周期的时候才去执行

介绍观察者模式
意图：定义对象间一种一对多的依赖关系，当一个对象状态发生改变时，所有依赖它的对象都会得到通知并被自动更新
主要解决：一个对象状态改变给其他对象通知的问题，而且要考虑到易用和低耦合，保证高度的协作

介绍中介者模式
意图：用一个中介对象来封装一系列的对象交互，中介者使各对象不需要显式的相互引用，从而使其耦合松散，而且可以独立的改变它们之间的交互。
主要解决：对象与对象之间存在大量的关联关系，这样势必会导致系统的结构变得很复杂，同时若一个对象发生改变，我们也需要跟踪与之相关联的对象，同时做出相应的处理。

观察者和订阅-发布的区别，各自用在哪里
观察者由具体目标调用，订阅-发布是统一由调度中心调用。
观察者模式的订阅者与发布者存在依赖关系，而订阅-发布模式不会。

介绍http2.0新特性
1. 在应用层(HTTP)和传输层(TCP)之间增加一个二进制分帧层
二进制分帧：在二进制分帧层上，HTTP2.0会将所有传输信息分割为更小的消息和帧，并对它们采用二进制格式的编码将其封装
2. 首部压缩：所有首部键必须全部小写
3. 流量控制
4. 多路复用
5. 请求优先级
6. 服务器推送

通过什么做到并发请求
Promise处理并发请求

介绍css3中position:sticky
粘性定位可以被认为是相对定位和固定定位的混合。
该元素定位不对后续元素造成影响，当元素被粘性定位时，后续元素的位置仍按照该元素未定位时的位置来确定

浏览器事件流向
DOM事件传播包括三个阶段：
1. 捕获
2. 事件目标处理函数
3. 冒泡

介绍事件代理以及优缺点
事件委托原理：事件冒泡机制
优点：
1. 大量节省内存占用，减少事件注册
2. 可以实现当新增子对象时，无需再对其进行事件绑定
缺点：
上诉1中的需求才会使用，使用场景少。如果把所有事件都用事件委托，可能会出现事件误判

tcp3次握手
第一次握手：建立连接时，客户端发送syn包到服务器，并进去SYN_SENT状态，等待服务器确认；(SYN: 同步序列编号)
第二次握手：服务器收到syn包，必须确认客户端的syn，同时自己也发送一个syn包，此时服务器进入SYN_RECV状态；
第三次握手：客户端收到服务器的SYN+ACK包，向服务器发送确认包ACK，此包发送完毕，客户端和服务器进入established状态，完成三次握手


bind、call、apply的区别
相似之处：
1. 都是用来改变函数的this指向
2. 第一个参数都是this指向的对象
3. 都可以利用后续参数传参
区别：
call和apply都是对函数的直接调用
call后续参数是单独一个一个穿
apply第二个参数是一个数组
bind方法返回的仍然是一个函数

闭包的定义和作用
闭包是有权访问另一个函数作用域中变量的函数。
特性：
1. 函数外部的代码无法访问函数体内部的变量，而函数体内部的代码可以访问函数外部的变量
2. 即使函数已经执行完毕，在执行期间创建的变量也不会销毁，因此每运行一次函数就会在内存中留下一组变量。

介绍下跨域
跨域是指协议、域名、端口不一致，出于安全考虑，跨域的资源之间无法交互。

如何实现跨域？
最常用的三种方法：JSONP、CORS、postMessage
跨域资源共享(CORS)是一种机制，他使用额外的HTTP头来告诉浏览器让运行在一个origin上的Web应用被准许访问来自不同源服务器上的指定的资源。当一个资源从与该资源本身所在的服务器不同的域、协议或端口请求一个资源时，资源会发起一个跨域HTTP请求。
跨域资源共享(CORS)机制允许Web应用服务器进行跨域访问控制，从而使跨域数据传输得以安全进行。
特别是GET以外的HTTP请求，或者搭配某些MIME类型的POST请求，浏览器必须首先使用OPTIONS方法发起一个预检请求，从而获知服务器是否允许该跨域请求。服务器确认允许后，才发起实际的HTTP请求。在预检请求的返回中，服务器端可以通知客户端，是否需要携带身份凭证(包括Cookies和HTTP认证相关数据);