# 2019-12-15
脚手架工具的安装请看：[基于 npm + webpack + babel + react + antd 自己动手搭建脚手架工具](https://github.com/kurryluo/front-end-interview-guide/blob/master/%E5%89%8D%E7%AB%AF%E5%B7%A5%E7%A8%8B%E5%8C%96/%E5%9F%BA%E4%BA%8E%20npm%20%2B%20webpack%20%2B%20babel%20%2B%20react%20%2B%20antd%20%E8%87%AA%E5%B7%B1%E5%8A%A8%E6%89%8B%E6%90%AD%E5%BB%BA%E8%84%9A%E6%89%8B%E6%9E%B6%E5%B7%A5%E5%85%B7.md)

## 安装react-router-dom
之前的项目中没有路由（router），所以只能打开http://localhost:3000/这个界面，如果要打开想http://localhost:3000/two-columns，就得用到react-router-dom

react-router是与react配合使用的路由库，react-router提供多个包可以单独使用:

- react-router
- react-router-dom
- react-router-native
- react-router-config

react-router提供路由的核心功能，react-router-dom基于react-router提供了在浏览器运行环境下的一些功能，
例如Link组件和BrowserRouter组件。react-router-native同样基于react-router，提供了react-native运行环境下的一些功能。

这里我们只需要安装react-router-dom即可。

安装版本：
```
+ react-router-dom@5.1.2
```



静态路由：静态路由是将路由声明在一处，并在应用程序渲染前导入，这个react-router v1、v2、v3 使用的方式。

```
// 路由配置说明（你不用加载整个配置，
// 只需加载一个你想要的根路由，
// 也可以延迟加载这个配置）。
React.render((
  <Router>
    <Route path="/" component={App}>
      <Route path="about" component={About}/>
      <Route path="users" component={Users}>
        <Route path="/user/:userId" component={User}/>
      </Route>
      <Route path="*" component={NoMatch}/>
    </Route>
  </Router>
), document.body)
```

动态路由:不需要将路由声明在一处，而是作为我们编写的react组件内容的一部分，Router组件就像div组件一样被使用，也就是在应用程序渲染时才发生路由。

React Router提供了三种类型的组件：
- router components，路由器组件：创建一个 history 对象
  - <BrowserRouter> ：高级组件，有一个响应请求的服务器情况下使用
  - <HashRouter> ：高级组件，使用的是静态文件的服务器情况下使用
- route components，路由组件
  - <Route>：作用是在location和path属性匹配时在此处渲染React组件。通过component、render和children三个属性来指向渲染组件，component属性通常指向一个现存的组件，render只有在需要传递参数给渲染组件时使用。如Route没有path，会一直与它最近的父级匹配
  - <Switch>：Switch不是必须的，用于将Route组件分组并选择一个与当前地址匹配的第一个Route
- navigation components，导航组件
  - <Link>：会在Html中创建一个a标签，to属性指向一个导航地址
  - <NavLink>
  - <Redirect>

React Router 的 Hooks：

- useHistory：操作 history 对象
- useLocation：操作 location 对象
- useParams：返回的是“键值对”对象，操作当前路由下的 match.params。
- useRouteMatch：代替Route去匹配URL，<Route>也有同样的功能，使用这个hooks的好处是不用渲染一个<Route>即可完成匹配。

三个重要的对象：

- match：match对象中包含了如何匹配URL的信息。
- history：history对象实现对session历史的管理。
- location：location属性代表应用程序现在在哪，你想去哪。

match、history和location对象都会在渲染Route组件时传入component、render和children指向的组件。

然后改造src文件夹中的index.js，如下：


```
import React from "react";
import ReactDom from "react-dom";
import {
    BrowserRouter as Router,
    Switch,
    Route,
    Link
} from "react-router-dom";
import "./index.less";
// 加载图片
const image = require('./image.jpg');
// 原生代码开始
const Div = document.createElement("div");
Div.setAttribute("id", "root");
document.body.appendChild(Div);
// 原生代码结束

class Home extends React.Component{
    render(){
        return (
            <div>
                123
            </div>
        )
    }
}

class About extends React.Component{
    render(){
        return (
            <div>
                456
            </div>
        )
    }
}

class Users extends React.Component{
    render(){
        return (
           <div>
               789
           </div>
        )
    }
}


class App extends React.Component{
    render(){
        return (
            <Router>
                <div>
                    <nav>
                        <ul>
                            <li>
                                <Link to="/">Home</Link>
                            </li>
                            <li>
                                <Link to="/about">About</Link>
                            </li>
                            <li>
                                <Link to="/users">Users</Link>
                            </li>
                        </ul>
                    </nav>

                    {/* A <Switch> looks through its children <Route>s and
            renders the first one that matches the current URL. */}
                    <Switch>
                        <Route path="/about">
                            <About />
                        </Route>
                        <Route path="/users">
                            <Users />
                        </Route>
                        <Route path="/">
                            <Home />
                        </Route>
                    </Switch>
                </div>
            </Router>
        )
    }
}

ReactDom.render( <App/>,
    document.getElementById("root")
);

```


# 2019-12-16 

## 实现单栏布局：header,content和footer等宽的单列布局
  有两种实现：
  - 对header,content,footer统一设置width：1000px;（屏幕小于1000px，出现滚动条）
  - 对header,content,footer统一设置max-width：1000px;（屏幕小于1000px，显示内容和屏幕的宽度一致）

```
.header{
  background-color: yellow;
  //width: 1000px;/* 出现控制条*/
  max-width: 1000px;
  height: 100px;
  margin:0 auto;/* 使得块居中*/
}

.content{
  background-color: blue;
  //width: 1000px;/* 出现控制条*/
  max-width: 1000px;
  height: 100px;
  color: white;
  margin:0 auto;
}


.footer{
  background-color: red;
  //width: 1000px;/* 出现控制条*/
  max-width: 1000px;
  height: 100px;
  color: white;
  margin:0 auto;
}

```


## 实现两栏布局
- 一列定宽，另一列自适应
  ```
  .first{
    background-color: yellow;
    width: 100px;
    height: 100px;
    float: left;
  }
  
  .second{
    background-color: blue;
    height: 100px;
    color: white;
  }
  ```
- 一列由内容撑开，另一列自适应
    ```
  .parent {
    overflow: hidden;
    zoom: 1;
  }

  .first{
    background-color: yellow;
    float: left;
    height: 50px;
  }

  .second{
    background-color: blue;
    overflow: hidden; /* 成为一个BFC，起到隔离作用*/
    height: 100px;
    color: white;
  }
    ```
    
## 实现三栏布局：中间列自适应宽度，旁边两侧固定宽度
  - 圣杯布局
  ```
  
.parent {
  padding-left: 220px;/*为左右栏腾出空间*/
  padding-right: 220px;
}


.left {
  float: left;
  width: 200px;
  height: 400px;
  background: red;
  margin-left: -100%;
  position: relative;
  left: -220px;
}


.center {
  float: left;
  width: 100%;
  height: 500px;
  background: yellow;
}

.right {
  float: left;
  width: 200px;
  height: 400px;
  background: blue;
  margin-left: -200px;
  position: relative;
  right: -220px;
}
  ```
  - 双飞翼:同样也是三栏布局，在圣杯布局基础上进一步优化，解决了圣杯布局错乱问题，实现了内容与布局的分离。而且任何一栏都可以是最高栏，不会出问题。
  ```
  
  ```
