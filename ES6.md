## let 和 const
- const定义的变量，只能在声明时赋值，并且不可修改
- let是定义的块级作用域

---
## 模板字符串

```
    `${name}is${age} years old.`
```
---
## 箭头函数
- 对于函数参数个数的不同，箭头函数有以下写法

```
    () => { … } // 零个参数用 () 表示； 
    x => { … } // 一个参数可以省略 ()； 
    (x, y) => { … } // 多参数不能省略 ()；
```
- 对于函数体及返回值部分，有如下参考

```
    x => { return x * x }; // 函数返回 x * x
    x => x * x; // 同上一行
    x => return x * x; // SyntaxError 报错，不能省略 {}
    x => { x * x }; // 合法，没有定义返回值，返回 undefined
```
- 和普通函数一样，箭头函数支持剩余参数以及默认参数，如下：

```
    var func1 = (x = 1, y = 2) => x + y;
    func1(); // 得到 3
    
    var func2 = (x, ...args) => { console.log(args) };
    func2(1,2,3); // 输出 [2, 3]
```
- 箭头函数没有constructor方法，也没有prototype，因此不支持new操作
- 箭头函数与普通函数不一样，是由于箭头函数中的this始终指向函数定义时的this
- 箭头函数中的this不能改变指向对象，我们知道apply和call可以改变this指向，但是对于箭头函数是不生效的

```
    var fn = {
		name:'Anna',
		testFn:function(){
			setTimeout(function(){
				console.log(`My name is ${this.name}`);
			},1000);
		}
	}	
	fn.testFn();     // 如果改为箭头函数，输出为  My name is
	
    var fn = {
		name:'Anna',
		testFn:function(){
			setTimeout(() => {
				console.log(`My name is ${this.name}`);
			},1000);
		}
	}	
	fn.testFn();     // 如果改为箭头函数，输出为  My name is Anna
```
---
## 类
- ES5中关于原型的说明
- 1）每一个函数数据类型，都天生自带一个属性，叫prototype，它是一个对象
- 2）prototype这个对象上天生自带一个属性constructor,他指向当前所属类；
- 3）每个对象上都带一个属性__proto__,他指向当前实例所属类的原型；
```
    class Project {
        constructor(name) {
          this.name = name;
        }
     
        start() {
          return "Project " + this.name + " starting";
        }
    }    
    var project = new Project("Journal");
    project.start(); // "Project Journal starting"
```
- ES6中关于类的继承

```
    class WebProject extends Project {
        constructor(name, technologies) {
            super(name);   // 这里实现继承，调用父类的构造函数，必须存在且需放在子类使用this对象之前,否则将报错
            this.technologies = technologies;
        }
    
        info() {
           return this.name + " uses " + this.technologies;
        }
    }
    
    var webJournal = new WebProject("FrontEnd Journal", "javascript");
    console.log(webJournal.start()); // "Project FrontEnd Journal starting"
    console.log(webJournal.info()); // "FrontEnd Journal uses javascript"
    
    console.log(project);
	console.log(webJournal);
```
- 私有的属性和方法写到constructor中，公有的属性和方法写在外面，和constructor平级，
- ES6中的继承子类可以获取到父类的constructor中的属性到自身的属性中，子类继承父类的方法是通过原型的原型进行获取的，例如，上面实例的project和webJournal打印出来的结构如下：

```
    Proect --> name:"Journal" --> __proto__ --> start()
    WebProject --> name:"FrontEndJournal";technologies:"javascript" --> __proto__ --> info() --> __proto__ -->start()
```
---
## 解构（Destructing）
- 解构赋值允许你使用类似数组或对象字面量的语法将数组和对象的属性赋给各种变量

#### 数组解构
- 如下：将1赋给变量first，2赋给变量second，3赋给变量third
```
    var ary = [1,2,3];
    var [first, second, third] = ary;
```
- 可以通过在对应位置留空来跳过被解构数组的某些元素

```
    var [,,third] = ["foo", "bar", "baz"];
    console.log(third);
    // "baz"
```
- 可以通过“不定参数”模式捕获数组中的所有尾随元素

```
    var [head, ...tail] = [1, 2, 3, 4];
    console.log(tail);
    // [2, 3, 4]
```
#### 对象解构
- 简单实例

```
    var robotA = { name: "Bender" };
    var robotB = { name: "Flexo" };
    var { name: nameA } = robotA;
    var { name: nameB } = robotB;
    console.log(nameA);
    // "Bender"
    console.log(nameB);
    // "Flexo"
```
- 当属性名与变量名相同时，可做如下写法：

```
    var { foo, bar } = { foo: "lorem", bar: "ipsum" };
    console.log(foo);
    // "lorem"
    console.log(bar);
    // "ipsum"
```
- 通过解构导入模块所需所需的接口名

```
    import React,{Component} from 'react'
```
---
## 模块的导入与导出
#### 导出 export
- 基本导出，如下：

```
    export function foo() {  
        // ..  
    }  
    export var awesome = 42;  
    var bar = [1,2,3];  
    export { bar };  
```
- 导出时重命名

```
    function foo() { .. }  
    export { foo as bar };  
```
#### 导入 import
```
    1、import name from 'my-module.js' ;
　　    导出整个模块到当前作用域，name作为接收该模块的对象名称
    2、import {moduleName} from 'my-module.js';
    　　导出模块中的单个成员moduleName
    3、import {moduleName1,moduleName2} from 'my-module';
    　　导出模块中的多个成员moduleName1、moduleName2
    4、import {moduleName as moduleAlias} from 'my-module';
    　　导出模块中的单个成员moduleName,moduleAlias为该成员别名
    5、import myDefault,{moduleName1,moduleName2} from 'my-module';
    　　myDefault为my-module.js文件default导出项
```
---
## ES6中的默认参数、任意参数、扩展运算符
- 默认参数

```
    function(name="you",age="18"){}
```
- 任意参数

```
    function agrv(obj,...keys){}
    agrv({name:"name"},"title","name")
    // title和name都将被keys接受
```
- 扩展运算符（类似于任意参数的反向）

```
    var arr=[1,2,3,4,5,6]
    Math.max(...arr)
    var newArr=[...arr,100,200]
```











