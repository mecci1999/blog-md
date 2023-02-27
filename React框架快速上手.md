### 前言

&emsp;&emsp;最近在工作中有遇到react框架搭建的项目，为了能够快速入门并上手进行react项目的需求开发，所以我恶补了一下react框架的知识。为了让大家快速了解并使用react框架，我不打算在该篇博客中讲解react框架的技术原理，同时为了能够加速大家理解，我在该篇文章中大量通过react项目和vue3项目的对比，来加速大家的了解，所以该篇博客适合有一定vue3基础知识的读者阅读。

### 结构图

![结构图](https://api.darwin.fun/images/serve?name=BLOG_08_IMG_01.png)

### react框架的核心思想

&emsp;&emsp;函数式编程，数据不可变，单向数据流

### 组件构建

&emsp;&emsp;我们知道vue和react框架的都有一个共同点就是注重UI，所以都有组件复用的特点，接下来我们来了解如何使用react去构建一个组件。

#### JSX语法

&emsp;&emsp;在react项目中我们采用的是函数式编程，所以在构建组件时遵循的是jsx的编写语法规范。在jsx语法中使用render函数进行html标签的渲染，在vue中我们使用的template模版进行html标签的渲染，当然vue也提供了render函数，所以在vue项目中也是可以使用jsx语法进行组件构建。下面用一个例子来讲解jsx语法的具体使用方法。

React组件

``` typescript
import React, { useState } from "react";
import reactLogo from "./assets/react.svg";
import "./App.css";

const App: React.FC = () => {
  const [count, setCount] = useState(0);

  return (
    <div className="App">
      <div>
        <a href="https://vitejs.dev" target="_blank">
          <img src="/vite.svg" className="logo" alt="Vite logo" />
        </a>
        <a href="https://reactjs.org" target="_blank">
          <img src={reactLogo} className="logo react" alt="React logo" />
        </a>
      </div>
      <h1>Vite + React</h1>
      <div className="card">
        <button onClick={() => setCount((count) => count + 1)}>
          count is {count}
        </button>
        <p>
          Edit <code>src/App.tsx</code> and save to test HMR
        </p>
      </div>
      <p className="read-the-docs">
        Click on the Vite and React logos to learn more
      </p>
    </div>
  );
};

export default App;
```

Vue组件

``` typescript
<script setup lang="ts">
  const count = ref(0)
</script>

<template>
  <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div>
  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>

  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank"
      >create-vue</a
    >, the official Vue + Vite starter
  </p>
  <p>
    Install
    <a href="https://github.com/johnsoncodehk/volar" target="_blank">Volar</a>
    in your IDE for a better DX
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>

```

&emsp;&emsp;从上面两段代码对比中，我们可以很明显的看出来，react使用的是直接return一段html代码，vue使用的是template模板进行渲染。react中给标签进行类命名是用className，在html中使用js语法需要用一层{}进行包裹，才能生效。事件的命名和vue也是有所区别，react中是使用驼峰命名监听dom事件，例如点击事件是onClick={},在vue中是@click=""进行事件的绑定。

#### 响应式数据

&emsp;&emsp;在react项目中对于响应式数据的创建是使用useState，而在vue3中使用的是ref、reactive进行创建，对比代码如下：

``` typescript
const [count, setCount] = useState(0); // react16.8版本后

// react16.8之前的版本
state = {
  count: 0
}

handleAdd = () => {
  const { count } = this.state;
  this.setState({ count: count + 1 });
};

const count = ref(0) // vue3
```

&emsp;&emsp;react的用法是使用hooks方法，setCount方法是用来对count变量进行响应式数据的变化，setCount接受一个参数，该参数可以是变量也可以是一个方法，用来改变数据。（注意：hooks写法是react16.8以后引入的新API,之前的写法是使用）

### 组件通信

&emsp;&emsp;了解了react组件的构建后，我们接下来了解一下组件之间的通信，这也是组件能够完成复用的重要部分。

#### 父子组件通信

&emsp;&emsp;使用props属性通信是父组件与子组件之间的一种通信方式，子组件通过定义相关的属性，利用属性进行显示，父组件则提供数据给子组件。下面来讲解一下在react中如何定义组件的props以及使用方法。

#### 父传子

``` typescript
// 父组件
import React from 'react'
import ClassComponent from './ClassComponent'
import FunctionComponent from './FunctionComponent'

const App: React.FC = () => {
  return (
    <div className="App">
      <ClassComponent title="Class Props Title" />
      <FunctionComponent title="Function Props Title" />
    </div>
  );
};

export default App;

// 子组件, class式组件
import React from 'react'

class ClassComponent extends React.Component {
    const { title } = this.props

    return (
      <div className="class-component">
        {title}
      </div> 
    )
}

// 子组件, 函数式组件
import React from 'react'

const App: React.FC = ({ title }) => {
    return (
      <div className="function-component">
        {title}
      </div> 
    )
}

export default App;
```

#### 子传父

&emsp;&emsp;子传父之间的传值归根结底就是在父组件中定义一个函数,然后传到子组件后进行调用,也是一种父传子的表现。

``` typescript
// 父组件
import React, { useState } from 'react'
import Son from './son'
const Father: React.FC = () => {
      const [num, setNum] = useState(0)
      //   console.log(setNum)
      const fn = () => { //定义一个函数
        setNum(num + 1)  //调用修改数据的方法
      }
      return (
            <div>
                父组件
                <Son num={num} fn={fn}></Son> //将函数传入子组件
            </div>
      )
    }

export default Father;

// 子组件
import React from 'react'

const Son: React.FC = (props) => {
      const { num, fn } = props
      return (
            <div>
                子组件:{num}
                <button onClick={fn}>+1</button> //子组件内调用
                // 注意!!!  如果需要传入参数 onClick={()=>{fn(参数)}}
                //避免出现加载就触发
            </div>
      )
    }

export default Son;
```

&emsp;&emsp;props可以传递：数字， 字符串， 布尔类型， 数组， 对象， 函数， jsx。

#### 兄弟组件通信

在react中其实不存在兄弟组件之间的传值,涉及到的兄弟组件之间的传值都会状态提升

将共享状态提升到最近的公共父组件中，由公共父组件管理这个状态

思想：状态提升

公共父组件职责：

提供共享状态
提供操作共享状态的方法

要通讯的子组件只需通过 props 接收状态或操作状态的方法

![兄弟通信](https://api.darwin.fun/images/serve?name=BLOG_08_IMG_02.png)

#### 跨组件通信

类跨组件之间的传值主要有三个步骤:

``` typescript
// 导入并调用createContext方法，从结果中解构出 Provider, Consumer 组件
import { createContext } from 'react'
const { Provider, Consumer } = createContext()

// 使用 Provider 组件包裹根组件，并通过 value 属性提供要共享的数据
return (
 <Provider value={ 这里放要传递的数据 }>
      <根组件的内容/>
 </Provider>
 )

 // 在任意后代组件中，使用第1步中导出的Consumer组件包裹整个组件
return (
  <Consumer>
    {
    （data） => {
      // 这里的形参data 就会自动接收Provider中传入的数据
      // console.log(data)
      return <组件的内容>
      }
    }
  </Consumer>
)
```
函数式组件的跨组件传值

``` typescript
// 导入并调用createContext方法，得到Context对象，导出
import { createContext } from 'react'
export const Context = createContext()

// 使用 Provider 组件包裹根组件，并通过 value 属性提供要共享的数据
return (
 <Provider value={ 这里放要传递的数据 }>
      <根组件的内容/>
 </Provider>
 )

 // 在任意后代组件中，如果希望获取公共数据： 导入useContext；调用useContext(第一步中导出的context) 得到value的值
 import React, { useContext } from 'react'
 import { Context } from './index'
  const App: React.FC = () => {
      const 公共数据 = useContext(Context)
      return ( 函数组件的内容 )
  }
```

### 生命周期

react16.3之后的版本中的生命周期如下

生命周期执行顺序
创建时

constructor()
static getDerivedStateFromProps()
render()
componentDidMount()

更新时

static getDerivedStateFromProps()
shouldComponentUpdate()
render()
getSnapshotBeforeUpdate()
componentDidUpdate()

卸载时

componentWillUnmount()

下面是各生命周期的详解

#### constructor()
constructor() 在React组件挂载之前被调用，在为React.Component子类实现构造函数时，应在其他语句之前调用 super()

super的作用：将父类的this对象继承给子类 (MDN参考)

通常，React构造函数仅用于以下两种情况：

来初始化函数内部 state
为 事件处理函数 绑定实例

如果不初始化 state 或不进行方法绑定，则不需要写 constructor() , 只需要设置 this.state 即可


**不能在 constructor()构造函数内部调用 this.setState(), 因为此时第一次 render()还未执行，也就意味DOM节点还未挂载**

#### static getDerivedStateFromProps(nextProps, state)
getDerivedStateFromProps() **在调用 render方法之前调用，在初始化和后续更新都会被调用**

返回值：返回一个对象来更新 state,  如果返回 null 则不更新任何内容


参数： 第一个参数为即将更新的 props,  第二个参数为上一个状态的 state , 可以比较props 和 state来加一些限制条件，防止无用的state更新


注意：getDerivedStateFromProps 是一个静态函数，**不能使用this**, 也就是只能作一些无副作用的操作
[至于为什么要这样做？请移步 Morgan大佬](https://zhuanlan.zhihu.com/p/38030418)

#### render()
render() 方法是class组件中唯一必须实现的方法，用于渲染dom,  render()方法必须返回reactDOM

**注意： 不要在 render 里面 setState, 否则会触发死循环导致内存崩溃**

#### componentDidMount()
 componentDidMount() 在组件挂载后 (插入DOM树后) 立即调用，componentDidMount() 是**发送网络请求、启用事件监听方法的好时机**，并且可以在 此钩子函数里直接调用 setState()

#### shouldComponentUpdate(nextProps, nextState)
shouldComponentUpdate() 在组件更新之前调用，**可以控制组件是否进行更新**， 返回true时组件更新， 返回false则不更新

包含两个参数，第一个是即将更新的 props 值，第二个是即将跟新后的 state 值，可以根据更新前后的 props 或 state 来比较加一些限制条件，决定是否更新，进行性能优化


不建议在 shouldComponentUpdate() 中进行**深层比较或使用 JSON.stringify()**。这样非常影响效率，且会损害性能


不要 shouldComponentUpdate 中调用 setState()，否则会导致无限循环调用更新、渲染，直至浏览器内存崩溃


可以使用内置 PureComponent 组件替代

#### getSnapshotBeforeUpdate(prevProps, prevState)
getSnapshotBeforeUpdate() 在最近一次的渲染输出被提交之前调用。也就是说，**在 render 之后，即将对组件进行挂载时调用。**

它可以使组件在 DOM 真正更新之前捕获一些信息（例如滚动位置），此生命周期返回的任何值都会作为参数传递给 componentDidUpdate()。如不需要传递任何值，那么请返回 null

#### componentDidUpdate(prevProps, prevState, snapshot)
componentDidUpdate() **会在更新后会被立即调用。首次渲染不会执行**

包含三个参数，第一个是上一次props值。 第二个是上一次state值。如果组件实现了 getSnapshotBeforeUpdate() 生命周期（不常用），第三个是“snapshot” 参数传递


可以进行前后props的比较进行条件语句的限制，来进行 setState() , 否则会导致死循环

#### componentWillUnmount()
componentWillUnmount() 在组件即将被卸载或销毁时进行调用。

**此生命周期是取消网络请求、移除监听事件、清理 DOM 元素、清理定时器等操作的好时机**

### 状态管理工具

在vue中我们可以使用像[vuex](https://vuex.vuejs.org/zh/guide/)、[piana](https://pinia.web3doc.top/introduction.html)这样的状态管理工具来帮助我们进行跨页面的数据管理，在react中也有类似的状态管理工具，这里我建议使用[redux](https://cn.redux.js.org/introduction/getting-started/)来进行跨页面的数据管理。这里就不做过多介绍了，详情请移步官方文档进行了解学习。

### 路由管理

最后我们来了解一下，项目构建必不可少的路由搭建，在react中是通过react-router-dom包，进行基础的路由搭建，下面将举个例子，详细介绍使用方法。

``` typescript
import React from 'react';
import {
  BrowserRouter as Router,
  Switch,
  Route,
} from "react-router-dom";
import Home from './pages/Home';
import Login from './pages/Login';
import Backend from './pages/Backend';
import Admin from './pages/Admin';

const App: React.FC = () => {
  return (
    <Router>
      <Switch>
        <Route path="/login" component={Login}/>
        <Route path="/backend" component={Backend}/>
        <Route path="/admin" component={Admin}/>
        <Route path="/" component={Home}/>
      </Switch>
    </Router>
  );
}

export default App;
```
然后可以在 Home 页面用 Link 加上跳转到其他页面的链接，这样就可以跳转了：

``` typescript
import React from 'react';
import { Link } from 'react-router-dom';

const Home: React.FC = () => {
  return (
    <>
      <h1>首页</h1>
      <ul>
        <li><Link to="/login">登录</Link></li>
        <li><Link to="/backend">后台</Link></li>
        <li><Link to="/admin">管理员</Link></li>
      </ul>
    </>
  );
}

export default Home;
```

上面的例子中，我们可以简单的了解一下在react中路由是怎么完成搭建的，下面我们再简单了解一下react-router-dom中常用组件的定义和使用方法。

**Router组件**：包裹整个应用，一个React应用只需要使用一次
两种常用的Router： **HashRouter**和**BrowserRouter**
**HashRouter：** 使用URL的哈希值实现 （localhost:3000/#/first）
推荐 **BrowserRoute**r：使用H5的history API实现（localhost3000/first）

**Link组件**：用于指定导航链接（a标签）
最终Link会编译成a标签，而to属性会被编译成 a标签的href属性

**Route组件**：指定路由展示组件相关信息
path属性：路由规则，这里需要跟Link组件里面to属性的值一致
component属性：展示的组件
Route写在哪，渲染出来的组件就在哪

**路由的执行过程**

当我们点击Link组件的时候，修改了浏览器地址栏中的url
React路由监听地址栏url的变化
React路由内部遍历所有的Route组件，拿着Route里面path规则与pathname进行匹配
当路由规则（path）能够匹配地址栏中的pathname时，就展示该Route组件的内容

**路由跳转**

1. params形式，路由跳转后，参数会显示在地址栏

跳转的方法是使用**history.push({pathname: '/personal', search: 'test=22222'})**，其中search键对应的值就是**拼接在地址栏的数据**

``` typescript
import React from 'react'
import { useHistory } from 'react-router-dom'

const App: React.FC = () => {
    const history = useHistory()

    // 页面跳转方法
    history.push({ pathname: '/personal', search: 'test=123' })
    // history.goBack()返回上一级路由

    return (123)
}
```

接收的方法。数据都是存储在useLocation中的search获取

``` typescript
import React from 'react'
import { useLocation } from 'react-router-dom'

const App: React.FC = () => {
    const location = useLocation()

    console.log(location, 'props')

    return (123)
}
```

![img_03](https://api.darwin.fun/images/serve?name=BLOG_08_IMG_03.png)

2. 使用state的形式，页面刷新不会丢失数据，并且地址栏也看不到数据

跳转的方法是使用history.push({pathname: '/personal', state: {test: 'dashboard'}})，其中state键对应的值就是拼接在地址栏的数据。

### 结尾
&emsp;&emsp;关于react框架就了解到这里，这篇博客主要是为了让大家快速上手使用react框架进行项目构建，所以并没有过多的介绍react的技术原理，要想真正熟悉并应用react框架还是建议去[官方文档](https://react.docschina.org/docs/getting-started.html)进行详细的学习，并且对框架的技术原理有一定的理解，才能在项目中有更好的应用。最后，感谢大家看到这里，码字不易，若有缺陷，欢迎大伙在评论区指正。

![img_04](https://api.darwin.fun/images/serve?name=BLOG_08_IMG_04.jpg)