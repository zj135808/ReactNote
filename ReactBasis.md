# 该文件介绍了一些React的基本语法，详细如下：

### JSX语法
- HTML 语言直接写在 JavaScript 语言之中，不加任何引号，这就是 JSX 的语法，它允许 HTML 与 JavaScript 的混写，用于描述组件树
- 在JSX语法中，变量的引入使用{}来进行变量的引入
- 遇到 HTML 标签（以 < 开头），就用 HTML 规则解析；遇到代码块（以 { 开头），就用 JavaScript 规则解析

```
    var names = ['Anna','Jhon','Alice'];
    class App extends React.Component {
        render(){
            return <div>
            {
                names.map(function(name){
                    return <div>Hello,{name}</div>
                })
            }
            </div>
        }
    }
```
- 如果是一个数组，可以直接使用变量将数组中的内容都进行显示

```
    render(){
        return <div>{names}</div>
    }
```
- JSX中的注释使用如下语法：

```
    return (
        <h1>Hello JSX,{name}
            {/*
             这里是JSX的注释
             */}
        </h1>
    )

```
---
### 组件样式的定义
- 定义组件样式有两种，一种是引入样式文件，一种是直接在组件内写样式对象

```
    index.css 文件中内容
    .container{background-color: lightgreen;color:#fff;}
    
    index.js 文件中内容
    import "./index.css"      // 通过样式文件引入
    class App extends Component {
        render(){
            var fontStyle = {       // 组件内写样式对象
                fontSize:'30px'
            }
            return <h1 className="container" style={fontStyle}>Hello World</h1>
        }
    }
```
---
### 使用ES5来进行组件定义

```
    var React = require('react');
    var ReactDOM = require('react-dom');
    
    var App = React.createClass({
        render: function () {
            return <h1>Hello World</h1>
        }
    })
    
    ReactDOM.render(<App />,document.getElementById('app'))
```
---
### render方法，用于对页面内容的渲染

```
    render(){
        return (
            <div>
                <h1>11</h1>
                <h1>22</h1>
            </div>
        )
    }
```
---
### 














