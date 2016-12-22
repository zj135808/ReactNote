# ���ļ�������һЩReact�Ļ����﷨����ϸ���£�

### JSX�﷨
- HTML ����ֱ��д�� JavaScript ����֮�У������κ����ţ������ JSX ���﷨�������� HTML �� JavaScript �Ļ�д���������������
- ��JSX�﷨�У�����������ʹ��{}�����б���������
- ���� HTML ��ǩ���� < ��ͷ�������� HTML �����������������飨�� { ��ͷ�������� JavaScript �������

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
- �����һ�����飬����ֱ��ʹ�ñ����������е����ݶ�������ʾ

```
    render(){
        return <div>{names}</div>
    }
```
- JSX�е�ע��ʹ�������﷨��

```
    return (
        <h1>Hello JSX,{name}
            {/*
             ������JSX��ע��
             */}
        </h1>
    )

```
---
### �����ʽ�Ķ���
- ���������ʽ�����֣�һ����������ʽ�ļ���һ����ֱ���������д��ʽ����

```
    index.css �ļ�������
    .container{background-color: lightgreen;color:#fff;}
    
    index.js �ļ�������
    import "./index.css"      // ͨ����ʽ�ļ�����
    class App extends Component {
        render(){
            var fontStyle = {       // �����д��ʽ����
                fontSize:'30px'
            }
            return <h1 className="container" style={fontStyle}>Hello World</h1>
        }
    }
```
---
### ʹ��ES5�������������

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
### render���������ڶ�ҳ�����ݵ���Ⱦ

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
### ���������
- props  �������������ݴ���
- state  ���ڹ�������Լ��ڲ�������
- props:һ�����ڸ�����������ͨ�ţ������֮��ͨ��ʹ��
- state:һ����������ڲ���״̬ά���������齨�ڲ������ݣ�״̬�������������props��

- ������������ݴ��ݸ�����������������ݴ��ݣ�

```
    index.js
    
    class App extends Component {
        render(){
            var data = {
                "name": "zj135808",
                "id": 19897852,
                "avatar_url": "https://avatars.githubusercontent.com/u/19897852?v=3"
            };
            return (
                 <Profile name={data.name} id={data.id} url={data.avatar_url}/>
            )
        }
    }


    Profiles.js
    
    export default class Profile extends Component{
        render (){
            let data = this.props;
            return (
                <div>
                    <img src={this.props.url} alt=""/>
                    <h1>{this.props.name}</h1>
                    <h2>{this.props.id}</h2>
                </div>
            )
        }
    }
```
- Ϊ��������ô������ݵ�����
- ע����Ҫ��PropTypes��������

```
    import React, {Component,PropTypes} from 'react'
    
    Profile.propTypes = {
        url:PropTypes.string,
        name:PropTypes.string,
        id:PropTypes.number
    }
```
- Ϊ���������Ĭ�ϲ���

```
    Profile.defaultProps = {      // ���ڶ���Ĭ�ϲ���
        name:'xxx',
        id:0,
        url:''
    };

```
---
### �����״̬
- �����״̬ state ���ڹ�������ڲ�������
- ����ע������£�
    - ��constructor�б���Ҫ��super()�������Ǽ̳У����û�еĻ�constructor�е�this����ָ��ǰ��������һ��б���
    - ��updateData������������ü�ͷ�����Ļ���Ҫ�ı���thisָ�����ʹ���˼�ͷ�����Ͳ��ã�����ʹ�ü�ͷ�����Ļ���Ҫ��װbabel-preset-stage-0���ģ��

```
    class App extends Component {
        constructor (){
            super();
            this.state = {
                val:'DefaultValue'
            }
        }
        updateData = (e) => {
            this.setState({
                val:e.target.value
            })
        }
        render(){
            return (
                <div>
                    <input type="text" onChange={this.updateData}/>
                    <h1>{this.state.val}</h1>
                </div>
            )
        }
    }
```
---
### ʹ��refs����DOM
- ref��React�е�һ�����ԣ���render��������ĳ�������ʵ��ʱ�����Ը�render�е�ĳ������DOM�ڵ����һ��ref���ԣ�ͨ��ref���ԣ����ǻ������õ�������DOM��Ӧ����ʵDOM�ڵ�
- �����ַ�ʽ�����õ���ʵ��DOM�ڵ�

```
    <input type="text" ref="username" />  
       
    var usernameDOM = this.refs.username.getDOMNode();  
    var usernameDOM = React.findDOMNode(this.refs.username);  
```
- ʵ����ʵ�ֹ���Ϊ��input���������Ӧ�����֣��ͻ���������ֶ�Ӧ��input��
```
    class App extends Component {
    
        changeHandle = (e) => {
            let index = Number(e.target.value);
            if(!index){
                return false;
            }
            if(index<1 || index>11){
                return false;
            }
            var refsName = 'input'+index;
            let curNode = findDOMNode(this.refs[refsName]);
            console.log(refsName,curNode);
            curNode.focus()
        }
    
        render(){
            let inputs = [];
            for(let i=1;i<=10;i++){
                inputs.push(<div key={i}><li><input type="text"  ref={"input" + i}/></li><br/></div>)
            }
    
            return (
                <div>
                    <label htmlFor="input" >�����������������������������������Զ���λ��������ڣ�</label>
                    <input type="text" id="input" onChange={this.changeHandle}/>
                    <ol>
                        {inputs}
                    </ol>
                </div>
            )
        }
    }
```
---
### ��ȡ�����
- this.props.children    ��   React.Children
- React.Children ��һ�����������������count��forEach��map��only��toArray�ȷ���
- this.props.children �ǵ�ǰ����µ���Ԫ�أ����Ի�ȡ����Ԫ�أ������������Ӧ����
- ע��  Chidren��Ҫ����һ��ģ������

```
    class App extends Component {
    
        render(){
            return (
                <div>
                    <NodeList>
                        <a href="http://www.baidu.com">BaiDu</a>
                        <a href="http://www.qq.com">QQ</a>
                    </NodeList>
                </div>
            )
        }
    }
    class NodeList extends Component{
        render (){
            let liList = Children.map(this.props.children,(item) => {
                return  <li>{item}</li>
            })
            return (
                <ul>
                    {liList}
                </ul>
            )
        }
    }
```
---
### ���React�е�thisָ��
```
    class App extends Component{
        constructor() {
            super();  // �̳У����û�� super() �Ļ��������this����App������ᱨ��
            // ��ʼ��state  ��  defaultProps��������һ����
            this.state = {name: 'react course'};
            // this.update = this.update.bind(this);
        }
        update = (e) => {  // ʹ�ü�ͷ�����Ļ���Ҫ��װbabel-preset-stage-0������
            console.log(this);
            //  �ı� state ��ֵ
            this.setState({
                name:e.target.value
            })
        }
        render(){
          return (
              <div>
                  <input type="text" onChange={this.update}/>
                  <h1>Hello, {this.state.name}</h1>
              </div>
          )
        }
    }
```
---
###  reatc����������
![](img/react-component-life.png)
- <a href="http://www.codeceo.com/article/reactjs-life-circle-event.html">ReactJS �������ڡ����������¼�</a>
- �״�ʵ������

```
    getDefaultProps
    getInitialState
    componentWillMount
    render
    componentDidMount
```
- ������

```
    componentWillReceiveProps
    shouldComponentUpdate
    componentWillUpdate
    render
    componentDidUpdate
```
- ������

```
    componentWillUnmount
```
- React����������֮����

```
    import React, {Component,Children} from 'react';
    import ReactDom, {render,findDOMNode} from 'react-dom'
    
    class App extends Component{
        constructor(){
            super()
            this.state = {
                val:0
            }
        }
        componentWillMount(){
            console.log('Will Mount');
        }
        update = (e) => {
            this.setState({
                val:this.state.val+1
            })
        }
        render(){
            console.log('render~~')
            return (
                <button onClick={this.update}>{this.state.val}</button>
            )
        }
        componentDidMount(){
            console.log('did mount~')
        }
    }
    
    render(<App />,document.getElementById('app'))

```
- React����������֮����

```
    index.js

    import React, {Component,Children} from 'react';
    import ReactDom, {render,findDOMNode} from 'react-dom'
    import Handler from './components/Handler.js'
    
    class App extends Component{
        render(){
            return <Handler />
        }
    }
    
    render(<App />,document.getElementById('app'))
    
    
    
    Messgae.js
    
    import React,{ Component } from 'react'
    
    export default class Message extends Component{
        componentWillReceiveProps(nextProps){
            console.log('receive props');
        }
        shouldComponentUpdate(nextProps,nextState){
            console.log('shouldComponentUpdate')
            //return false;  // ���Ϊfalse��ô�������������µĻ��Ͳ��ٽ�����Ⱦ
            return true
        }
        componentWillUpdate(){
            console.log('componentWillUpdate')
        }
        componentDidUpdate(){
            console.log('componentDidUpdate')
    
        }
        componentWillMount(){
            console.log('Will Mount');
        }
        render(){
            console.log(this.props);
            console.log('render~~~')
            return (
                <span>{this.props.val}</span>
            )
        }
        componentDidMount(){
            console.log('did mount~')
        }
        componentWillUnmount(){
            console.log('componentWillUnMount')
        }
    }
    
    
    
    Handler.js
    
    import React, { Component } from 'react'
    import ReactDOM from 'react-dom'
    
    import Message from './Message.js'
    
    export default class Handler extends Component{
        constructor(){
            super()
            this.state = {
                val:0,
                destoryed:false
            }
        }
        update = (e) => {
            this.setState({
                val:this.state.val+1
            })
        }
        destory = (e) => {
            this.setState({
                destoryed:true
            })
        }
        remove = () => {
            // remove method2
            // �Ӹ�Ԫ���Ƴ�����DOM
            ReactDOM.unmountComponentAtNode(document.getElementById('app'));
        }
        render(){
            // remove method1
            // �������������Ⱦ
            if(this.state.destoryed){
                return null
            }
    
            return (
                <div id="container">
                    <Message val={this.state.val}/>
                    <button onClick={this.update}>{this.state.val}</button>
                    <button onClick={this.destory}>remove Component</button>
                </div>
            )
        }
    }

```











