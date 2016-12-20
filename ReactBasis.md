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
### 














