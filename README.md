# react-redux
# React总结

props属性

每个组件对象都会有props属性

组件标签内的所有属性都会保存在props中

读取某个属性值：this.props.propertyName

通过标签属性从组件外向组件内传递数据

对props属性值类型限制和必要性限制

<Person {...person}/> 将对象的所有属性通过props传递

Person.defultProps ={  }设置默认属性

组件类的构造函数 constructor（props）{

super（props）；

}

设置npm代理为淘宝代理：

npm config set registry https://registry.npm.taobao.org 

或者

编辑 `~/.npmrc` 加入下面内容

registry = https://registry.npm.taobao.org

## 组件间传值

父组件->子组件 ：通过props

子组件->父组件 :  通过函数  子组件中绑定fufun 示例：<button onClick={this.props.fufun.bind(this,this.state.ziText)}>点击向父组件发送数据</button> 

父组件中：       <News name="xiaoming" fufun={this.datafun}> </News>

定义 this.state = {

​      text:"我是默认值"

​    } 

定义：datafun= (text)=> {

​    console.log(text)

​    this.setState({

​      text

​    }



同级组件之间传值：

通过pubsub-js

安装 npm i pubsub-js --save

import PubSub from 'pubsub-js'

例如new组件与phone组件 

new组件中： <button onClick={this.pubsub.bind(this)}>点击同级组件发送数据</button>

  pubsub(){

​    PubSub.publish("evt",this.state.num)

  }    pubsub发布一个名为evt 的内容为this.state.num 的数据

phone组件中:在构造函数中

constructor(props) {

​    super(props)

​    PubSub.subscribe("evt",(msg,data)=>{

​      console.log("phone"+data)

​    })

  }  订阅名为evt数据





import React, { PureComponent } from 'react'

import News from './News'

import Phone from './Phone'

class Home extends PureComponent {

  constructor(props) {

​    super(props)



​    this.state = {

​      text:"我是默认值"

​    }

  }

  datafun= (text)=> {

​    console.log(text)

​    this.setState({

​      text

​    }

​      

​    )

  }



  render() {

​    return (

            <div>

             <p>Home-------{this.state.text}</p>

​       <News name="xiaoming" fufun={this.datafun}> </News>

​       <Phone></Phone>

​      </div>

  }

}

export default Home

## 异步请求：

npm i json-server -g

npm i axios --save

import axios from 'axios'

模拟数据

data.json

{

  "arr":[

​    {"id":"1","name":"小明"},

​    {"id":"2","name":"小明"},

​    {"id":"3","name":"小明"},

​    {"id":"4","name":"小明"},

​    {"id":"5","name":"小明"}

  ]

}

启动 json-server

json-server data.json --port 4000

json-server json文件名  -port 端口号

导入axios

import axios from 'axios'

钩子函数中调用

 componentDidMount(){

​    this.ajaxfun()

  }

回调函数中获取数据

ajaxfun=()=>{

​    axios.get("http://localhost:4000/arr").then((ok)=>{

​      console.log(ok)

​      this.setState({

​        arr:ok.data

​      })

​    })

  }



示例：

import React, { PureComponent } from 'react'

import axios from 'axios'

class Home extends PureComponent {

  constructor(props) {

​    super(props)



​    this.state = {

​      arr:[]

​    }

  }

  componentDidMount(){

​    this.ajaxfun()

  }

  ajaxfun=()=>{



​    axios.get("http://localhost:4000/arr").then((ok)=>{

​      console.log(ok)

​      this.setState({

​        arr:ok.data

​      })

​    })

  }

  render() {

​    return (

            <div>

​      Home---{this.state.arr.map((v,i)=>{

​        return <p key={i}>{v.name}</p>

​      })}

​      </div>

​    )

  }

}

export default Home



## 解决跨域问题：

正向代理--开发环境     客户端-> 代理服务器为->目标服务器

反向代理--上线环境     网络请求->代理服务器 ->网络服务器 请求客户端



## react路由

1.安装： cnpm i  react-router-dom --save 

react-router 核心api  react-router-dom 更多 

hash  HashRouter hash模式 带#号 刷新不会丢失

browser BrowserRouter Browser模式 不带#好 历史记录模式 刷新会丢失 本地模式不会丢失

2.index.js引入路由

3.路由模式包裹根组件

Browser模式

ReactDOM.render(<BrowserRouter><App/></BrowserRouter>,document.getElementById('root'));

引入router  import { Route } from 'react-router-dom';

配置Route

Link导航<Link>  <NavLink> 可以动态的给选中的类名添加active类名  设置选中的样式

包裹<Switch></Switch> 多个匹配时只渲染一个

<Redirect from="/" to="/home" exact> 如果是/ 则精准匹配到/home

路由进阶与高阶组件 ：

withRouter（高阶组件  参数为组件返回也是组件）

就是让没有路由切换的组件也具备三个属性：location match history

作用：1.监控路由变化

2.Link.pathname 切换路径

3.编程式导航 props.history.push("url")

4.路由传参   <Route path="/Phone/:id" component={Phone}/>   <Link to="/phone/我是参数">点我去phone</Link>  接受参数：componentDidMount(){

​    console.log(this.props.match.params.id)

  }

5.query方式传参   <NavLink to={{pathname:"/news",query:{name:"xiaoming"}}}>news</NavLink>

接受参数： componentDidMount() {

​    console.log(this.props.location.query.name)

  }



## hook使用

##### 让无状态组件有状态:使用useState

let [val,setVal] = useState(0)

使用修改状态

<div>{val}
    <button onClick={()=>{setVal(val+1)}}>点击修改数据</button>

## redux

##### js提供的可预测性的状态容器：（一个固定的输入，必定可以等到一个结果）

##### 集中管理react中多个组件的状态

##### redux 状态管理库 

#### 场景：

##### 某个组件的状态需要共享的时候。

##### 一个 组件需要改变另一个组件状态的时候。

##### 组件中的状态需要在任何地方都可以拿到的时候。

1.安装redux

cnpm i redux --save

2.创建redux目录 创建reducer.js    store.js  action.js 

3.reducer.js中定义数据 并向外暴露数据data  并定义对数据操纵的行为

var obj = [{

  name:"小明",age:18

}]

export function data(state=obj[0].age,action){

  switch (action.type) {

​    case "ADD":

​      return state+action.data

​    case "DEL":

​      return state-action.data  

​    default:

​      return state

  }

}

action.js中定义对数据的操作

指定暴露方法名    返回操作类型  操作数据类型

export const add=(num)=>{

  return{type:"ADD",data:num }

}

export const del=(num)=>{

  return{type:"DEL",data:num }

}

store.js  中放置reduce.js中暴露的数据data

import {createStore} from 'redux'

import {data} from './reducer'

export var store=createStore(data) 



在需要数据的组件中：

​	import {store} from '../redux/store'

​    import * as action from "../redux/action"

挂载

componentDidMount(){

​    store.subscribe(()=>{

​      this.setState({

​        num:store.getState()

​      })

​    })

  }

在触发函数中调用

 	<button onClick={()=>{store.dispatch(action.add(1))}}>点我加1</button>

​      <button onClick={()=>{store.dispatch(action.del(1))}}>点我减1</button>

