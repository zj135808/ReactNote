## let �� const
- const����ı�����ֻ��������ʱ��ֵ�����Ҳ����޸�
- let�Ƕ���Ŀ鼶������

---
## ģ���ַ���

```
    `${name}is${age} years old.`
```
---
## ��ͷ����
- ���ں������������Ĳ�ͬ����ͷ����������д��

```
    () => { �� } // ��������� () ��ʾ�� 
    x => { �� } // һ����������ʡ�� ()�� 
    (x, y) => { �� } // ���������ʡ�� ()��
```
- ���ں����弰����ֵ���֣������²ο�

```
    x => { return x * x }; // �������� x * x
    x => x * x; // ͬ��һ��
    x => return x * x; // SyntaxError ��������ʡ�� {}
    x => { x * x }; // �Ϸ���û�ж��巵��ֵ������ undefined
```
- ����ͨ����һ������ͷ����֧��ʣ������Լ�Ĭ�ϲ��������£�

```
    var func1 = (x = 1, y = 2) => x + y;
    func1(); // �õ� 3
    
    var func2 = (x, ...args) => { console.log(args) };
    func2(1,2,3); // ��� [2, 3]
```
- ��ͷ����û��constructor������Ҳû��prototype����˲�֧��new����
- ��ͷ��������ͨ������һ���������ڼ�ͷ�����е�thisʼ��ָ��������ʱ��this
- ��ͷ�����е�this���ܸı�ָ���������֪��apply��call���Ըı�thisָ�򣬵��Ƕ��ڼ�ͷ�����ǲ���Ч��

```
    var fn = {
		name:'Anna',
		testFn:function(){
			setTimeout(function(){
				console.log(`My name is ${this.name}`);
			},1000);
		}
	}	
	fn.testFn();     // �����Ϊ��ͷ���������Ϊ  My name is
	
    var fn = {
		name:'Anna',
		testFn:function(){
			setTimeout(() => {
				console.log(`My name is ${this.name}`);
			},1000);
		}
	}	
	fn.testFn();     // �����Ϊ��ͷ���������Ϊ  My name is Anna
```
---
## ��
- ES5�й���ԭ�͵�˵��
- 1��ÿһ�������������ͣ��������Դ�һ�����ԣ���prototype������һ������
- 2��prototype��������������Դ�һ������constructor,��ָ��ǰ�����ࣻ
- 3��ÿ�������϶���һ������__proto__,��ָ��ǰʵ���������ԭ�ͣ�
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
- ES6�й�����ļ̳�

```
    class WebProject extends Project {
        constructor(name, technologies) {
            super(name);   // ����ʵ�ּ̳У����ø���Ĺ��캯����������������������ʹ��this����֮ǰ,���򽫱���
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
- ˽�е����Ժͷ���д��constructor�У����е����Ժͷ���д�����棬��constructorƽ����
- ES6�еļ̳�������Ի�ȡ�������constructor�е����Ե�����������У�����̳и���ķ�����ͨ��ԭ�͵�ԭ�ͽ��л�ȡ�ģ����磬����ʵ����project��webJournal��ӡ�����Ľṹ���£�

```
    Proect --> name:"Journal" --> __proto__ --> start()
    WebProject --> name:"FrontEndJournal";technologies:"javascript" --> __proto__ --> info() --> __proto__ -->start()
```
---
## �⹹��Destructing��
- �⹹��ֵ������ʹ�����������������������﷨������Ͷ�������Ը������ֱ���

#### ����⹹
- ���£���1��������first��2��������second��3��������third
```
    var ary = [1,2,3];
    var [first, second, third] = ary;
```
- ����ͨ���ڶ�Ӧλ���������������⹹�����ĳЩԪ��

```
    var [,,third] = ["foo", "bar", "baz"];
    console.log(third);
    // "baz"
```
- ����ͨ��������������ģʽ���������е�����β��Ԫ��

```
    var [head, ...tail] = [1, 2, 3, 4];
    console.log(tail);
    // [2, 3, 4]
```
#### ����⹹
- ��ʵ��

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
- �����������������ͬʱ����������д����

```
    var { foo, bar } = { foo: "lorem", bar: "ipsum" };
    console.log(foo);
    // "lorem"
    console.log(bar);
    // "ipsum"
```
- ͨ���⹹����ģ����������Ľӿ���

```
    import React,{Component} from 'react'
```
---
## ģ��ĵ����뵼��
#### ���� export
- �������������£�

```
    export function foo() {  
        // ..  
    }  
    export var awesome = 42;  
    var bar = [1,2,3];  
    export { bar };  
```
- ����ʱ������

```
    function foo() { .. }  
    export { foo as bar };  
```
#### ���� import
```
    1��import name from 'my-module.js' ;
����    ��������ģ�鵽��ǰ������name��Ϊ���ո�ģ��Ķ�������
    2��import {moduleName} from 'my-module.js';
    ��������ģ���еĵ�����ԱmoduleName
    3��import {moduleName1,moduleName2} from 'my-module';
    ��������ģ���еĶ����ԱmoduleName1��moduleName2
    4��import {moduleName as moduleAlias} from 'my-module';
    ��������ģ���еĵ�����ԱmoduleName,moduleAliasΪ�ó�Ա����
    5��import myDefault,{moduleName1,moduleName2} from 'my-module';
    ����myDefaultΪmy-module.js�ļ�default������
```
---
## ES6�е�Ĭ�ϲ����������������չ�����
- Ĭ�ϲ���

```
    function(name="you",age="18"){}
```
- �������

```
    function agrv(obj,...keys){}
    agrv({name:"name"},"title","name")
    // title��name������keys����
```
- ��չ���������������������ķ���

```
    var arr=[1,2,3,4,5,6]
    Math.max(...arr)
    var newArr=[...arr,100,200]
```











