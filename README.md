# ES6-promise的基本用法
#### [作者] 董爽
#### [邮箱] dongshuang@haomo-studio.com
#### [版本] v1.0.0
#### [标签] 
* promise


## 1.什么是promise：

Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。它由社区最早提出和实现，ES6将其写进了语言标准，统一了用法，原生提供了promise对象。
所谓promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

没有异步，就没有promise，
没有异步，就不需要promise。
Promise不是实现异步，而是我们在编写异步应用时的一种写代码的方式。  


## 2.promise A+规范：

异步的回调地狱一直都存在。所以就有很多大牛，想了一些办法，将这个嵌套的写法，变成同步的写法。基于此，就有多种实现。于是，promise用法就有些乱套了，导致没有一个规范，使用的时候千奇百怪。
此时，就制定了promise的相关规范。
在众多的规范中，有一个比较好的规范---Promise A+规范。
es6中的promise，就是基于Promise A+规范的。

### 目录结构：

>
一句话概括：4个规范  
1：一个promise对象  
2：两个事件，resolve事件和reject  
3：三大状态，pending、resolved、rejected  
4：四大术语


### (1).四大术语

一定要结合异步来理解。
异步操作是需要有过程的，当操作开始的时候，就进入了一个状态。等等执行的结果。

解决（fulfill）：指一个 promise 成功时进行的一系列操作，如状态的改变、回调的执行。虽然规范中用 fulfill 来表示解决，但在后世的 promise 实现多以 resolve 来指代之。

拒绝（reject）：指一个 promise 失败时进行的一系列操作。

终值（eventual value）：所谓终值，指的是 promise 被解决时传递给解决回调的值，由于 promise 有一次性的特征，因此当这个值被传递时，标志着 promise 等待态的结束，故称之终值，有时也直接简称为值（value）。

据因（reason）：也就是拒绝原因，指在 promise 被拒绝时传递给拒绝回调的值。

终值和解决是一组，据因和拒绝是一组。


### (2).三大状态  
一个 Promise 的当前状态必须为以下三种状态中的一种：  

	等待态（Pending）  
	执行态（Fulfilled）  
	拒绝态（Rejected）  

每一个状态，都需要满足一定的条件

**等待态（Pending）**
处于等待态时，promise 需满足以下条件：
可以迁移至执行态或拒绝态

**执行态（Fulfilled）**
处于执行态时，promise 需满足以下条件：
不能迁移至其他任何状态
必须拥有一个不可变的终值

**拒绝态（Rejected）**
处于拒绝态时，promise 需满足以下条件：
不能迁移至其他任何状态
必须拥有一个不可变的据因

三大状态，最终只有两条路可选。
### (3).两个事件（then方法）： 

在状态变化的过程中，如图：
只有两条路：  


	Pending --> fullfilled
	Pending --> rejected
 

这就相当于有两个事件触发了。
从**Pending --> fullfilled**，触发了成功的事件，通常用resolve来表示。
从**Pending -->rejected**，触发了失败的事件，通常用reject来表示。

需要注册这两个事件。
此时，就需要使用then方法。
只有两条路：


	Pending --> fullfilled
	Pending --> rejected


这就相当于有两个事件触发了。
从**Pending --> fullfilled**，触发了成功的事件，通常用resolve来表示。
从**Pending -->rejected**，触发了失败的事件，通常用reject来表示。

需要注册这两个事件。
此时，就需要使用then方法。

	promise.then(onFulfilled,onRejected)

并且，在onFullfilled，必须拥有一个value，作为终值，即成功时返回的结果
在onRejected中，必须拥有一个reason，作为据因，即失败时返回的原因。
### (4).1个对象：

	就是promise对象


## 3.promise的基本用法：

前面，我们站在使用者的角度，直接使用别人提供的promise对象。
现在，我们站在开发者的角度，需要自己定义promies对象，供自己和别人使用。

[声明]：我们现在学习的基于es6的promise对象
### (1).实例化Promise对象
在es6中，就原生的提供了一个Promise构造器。  

	>console.dir(Promise)

使用new的方式来实例化一个对象，如下：

	let p1 = new Promise((resolve,reject) => {
		//直接从pending-->fulfilled
		resolve('ok');
	})
	p1.then(response => console.log(response))
这个代码的意思是，我实例化了一个promise对象。Promise对象在实例化的那一刻，其状态一定是pending。
  
实际上，在实例化Promise对象的时候，必须要传递一个回调函数，并且该函数拥有如下两个参数：
```
resolve    
reject
```  
这两个参数的作用，就是用于转换状态。
其中，resolve的作用，就是将状态 从pending --> fullfilled
reject的作用，就是将状态从 pending --> rejected
转换的时候，就是直接调用resolve方法和reject方法。同时，可以通过设置参数，作为resolve的终值和reject的据因。
当调用resolve函数的时候，就将promise对象的状态 从pending --> fullfilled，同时我将’ok’作为promise对象的终值。

请站在事件的角度来理解，一旦执行了resolve方法，意味着触发了一个事件 ---- onFulfilled

需要注册onFullFilled事件，然后才可以获取到这个终值。

同理，如果我触发了rejected事件，就需要注册onReject事件。代码如下：

	let p1 = new Promise((resolve,reject) => {
		//直接从pending-->rejected
		resolve('ok');
	})
	p1.then(response => console.log(response),err => console.log(err))

在当前的这个用法中，每次都是直接将pending置为fullfilled或者rejected，在实际开发中，不是这样的。
通常，需要定义一个方法，然后返回一个promise对象，需要根据条件，来执行resolve和reject方法。

一定要结合异步来进行的。  

	function random(){
		return new Promise((resolve,reject)=>{
				setTimeout(()=>{
					let number = Math.random();
					if (number>0.5) {
						resolve("ok"+number);
					}else{
						reject("error"+number);
					}
				},100);
			})
		}
		let p1 = random();
		p1.then(res=>console.log(res),err=>console.log(err))
then方法作用，就是用于注册事件。并且是注册两个事件。作为then方法的两个参数来实现的。

第一个参数，onFullFilled，表示pending->fullfilled，会触发的事件。  
第二个参数，onRrejected，表示pending->rejected，会触发的事件。	  

	let p1 = random();
	p1.then(res=>console.log(res),err=>console.log(err))
### (3).catch方法:
在刚才的写法中，


p1.then(response => console.log(response),err => console.log(err));

如果我只想注册onRejected事件，该怎么做？  

	p1.then(null,err=>console.log(err))

针对这个写法，我们可以直接使用catch方法来完成。

	p1.then(null,err=>console.log(err))

所以，catch（err => {}），就是 then（null，err => {}）的简写方式。
由于catch的语义更好，一般都建议使用catch来注册onRejected。

### (4).then方法的链式调用
then方法，必须返回一个promise对象。所以，我们可以链式调用,如下:

	p1.then(res=>console.log(res)).catch(err=>console.log(err))
then方法返回的是一个全新的promise对象，不是原来的那个。
Then方法还可以使用return指定返回值，它不会影响promise的返回值（仍然会返回一个全新的promise对象），同时会将return指定的返回值作为下一个then方法的参数。


### (5).all和race方法
如果有多个promise对象，我们可以使用all和race来实现一些特定的需求：
比如，有多个promise，进行异步操作，只有当所有的请求都成功，才能进行下一步操作。
此时就可以使用all。

有多个promise，进行异步操作，只要有一个请求成功，就可以进行下一步操作，就可以使用race。

刚才的这个读取文件，就可以使用all来实现。

编写代码如下：
all和race都是Promise构造器的静态方法。

all使用如下：

	const p2 = new Promise((resolve,reject)=>{
		resolve('hello')
		})
		.then(result=>result)
		.catch(err=>err);
	const p3 = new Promise((resolve,reject)=>{
		throw new Error('报错了');
		})
		.then(result=>result)
		.catch(err => err);
	Promise.all([p2,p3])
		.then(result =>console.log(result))
		.catch(err =>console.log(err));
同理，race的使用同上
