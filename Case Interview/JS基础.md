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
3.将新创建的对象作用this的上下文；
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







