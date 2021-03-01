---
title: React基础学习笔记2
categories: React学习
tags:
- 前端
- React
- 基础
---
组件生命周期这个概念无论在React，还是Vue中都是十分重要的，但是由于版本更新问题，新版与旧版的生命周期在很多方面发生了改变，所以个人还是觉得有必要拿出来区分一下，为后续方便开发做准备。
<!-- more -->
​    **先介绍一下老版本的生命周期函数：**

​    先上图：

![](https://img10.360buyimg.com/imagetools/jfs/t1/92485/10/17877/49856/5e8ee9f9E16f123fe/9e95709a5ce6bfd4.jpg)

### **挂载阶段**（实例化）

​        挂载阶段这些函数只会执行一次，也就是说这些组件在创建过程中需要执行的函数，但是，一但组件被真正渲染到页面以后就不会再进行。

####  1.constructor() 

​       类的构造函数，也是组件初始化函数，一般情况下，我们会在这个阶段做一些初始化的工作这个函数一般是在组件挂载之前进行调用，就是最开始的时候，这个构造函数里面有一个十分重要的super(props),应在其他语句调用之前就使用这个方法，否则有可能会出现一些bug。该阶段可以给事件绑定this(如果给事件使用箭头函数就不用再绑定)，创建初始化state，根据props获取到父组件传来的一些属性，通过React.createdRef()这种方式创建ref准备渲染以后与DOM节点进行绑定。 

#### 2.componentWillMount()

​      这是你最后一次机会在挂载前对state进行修改

####  3.render()

​      这个函数是必须存在的，不必多说

####  4.componentDidMount() 

​     组件已经渲染完成，发出通知。

------



### 更新阶段 （存在期）

####   **this.props变更**

##### 1.componentWillReceiveProps(nextProps)

​       如果组件收到新的属性（props），才会触发。这句话刚开始看的时候有些懵，什么叫组件后来变化？？？自己仔细想了一下，你把props通过与state结合不就可以了吗！网上说这叫派生状态，通过setState进行修改属性以后才会修改这个props,这个方法才会进行调用。

##### 2.shouldComponentUpdate(nextProps, nextState) 

​        当组件接收到新的属性和状态改变的话，都会触发调用 shouldComponentUpdate，输入参数 nextProps 和上面的 componentWillReceiveProps 函数一样， nextState 表示组件即将更新的状态值。这个函数的返回值决定是否需要更新组件，如果 true 表示需要更新，继续走后面的更新流程。否则，则不更新，直接进入等待状态。默认情况下，这个函数永远返回 true 用来保证数据变化的时候 UI 能够同步更新。在大型项目中，你可以自己重载这个函数，通过检查变化前后属性和状态，来决定 UI 是否需要更新，能有效提高应用性能。（这个函数我最近在学习过程中发现它是一个十分重要的函数，比如menu列表切换，nav头部切换------结合if语句判断相应的props是否进行了更改）

##### 3.componentWillUpdate(nextProps, nextState)

​      上面的 shouldComponentUpdate 返回为 true以后，就可以直接更新组件了，在这里你可以进行一系列更新界面之前要做的事情，需要特别注意的是，在这个函数里面，你就不能使用 this.setState 来修改状态。  

##### 4.render()

​	  必须存在的，不必多说

##### 5.componentDidUpdate(prevProps, prevState)

​      组件更新完成，两个参数是更新之前的参数。

**this.state变更  （函数具体功能参考上方）**

shouldComponentUpdate(nextProps, nextState)       

componentWillUpdate(nextProps, nextState）

render(）

componentDidUpdate(prevProps, prevState) 

------

### 卸载阶段

#### 1.componentWillUnmount()

​    当组件要被从界面上移除的时候，就会调用。在这个函数中，可以做一些组件相关的清理工作，例如取消计时器、网络请求等。

**代码演示**

为了方便 查看每一个生命周期，我决定将child分为挂载阶段和更新阶段两个部分，分别进行查看。

父组件App.js

```
import React,{Component} from 'react';
import Child from './child';
class App extends Component {
    state={
        name: "123",
        isShow: true
    }
    setName=(name)=>{
        this.setState({
            name
        });
    }
    render(){
      let {isShow} = this.state;
      return <div>
          {isShow?<Child name={this.state.name} setName={this.setName} />:"组件被卸载了"}
          <button onClick={()=>{
              this.setState({
                isShow: !isShow
              });
          }}>卸载</button>
      </div>;
    
}
export default App;


```

首先先展示一下初始渲染页面

​      ![](https://img10.360buyimg.com/imagetools/jfs/t1/88156/25/17940/8617/5e8ff631Ec208799b/789a98164c88cc7c.png)

##### 挂载阶段

```
import React,{Component} from 'react';
class Child extends Component {
    constructor(props){
        super(props);
        this.state = {
            age: 8
        };
        console.log(1,"初始化");
    }
    componentWillMount(){
        console.log(2,"组件即将被渲染到DOM");
    }
    componentDidMount(){
        console.log(4,"组件被渲染到DOM了");
    }
    render(){
      console.log(3,"向DOM中渲染");
      let {name,setName} = this.props;
      let {age} = this.state;
      return <div>
            <p>姓名: <input 
                        type="text" 
                        placeholder="请输入新名字" 
                        value={name} 
                        onChange={(e)=>{
                            setName(e.target.value);
                        }}
                    /></p>
             <p>{name}</p>       
            <p>年龄: <input 
                 type="text" 
                 placeholder="请输入年龄"
                 value = {age}
                 onChange={(e)=>{
                    this.setState({
                        age: e.target.value
                    });
                }}
            /></p>
      </div>;
    }
}
export default Child;	
```

控制台打印结果：

​        ![](https://img10.360buyimg.com/imagetools/jfs/t1/109449/3/11907/10583/5e8f100cEe901f994/85de8aa360a597a4.png)

#### 更新阶段

```
import React,{Component} from 'react';
class Child extends Component {
    state = {
        age: 8
    };
    componentWillReceiveProps(nextProps){
        console.log(0,"父组件更新引起子组件更新",nextProps,this.props);
    }
    // nextProps 新的props nextState 新的state
    shouldComponentUpdate(nextProps, nextState){
        console.log(1,"组件更新",nextProps, nextState,this.props,this.state);
        return true;// true 接着向下执行生命周期,并更新视图，false 打断执行,不在更新视图
    }
    componentWillUpdate(nextProps, nextState){
        console.log(2,"组件即将更新");
    }
    componentDidUpdate(prevProps, prevState){
        console.log(4,"组件更新完成",prevProps, prevState,this.props,this.state);
    }
    render(){
      let {name,setName} = this.props;
      let {age} = this.state;
      console.log(3,"正在更新");
      return <div>
            <p>姓名: <input 
                        type="text" 
                        placeholder="请输入新名字" 
                        value={name} 
                        onChange={(e)=>{
                            setName(e.target.value);
                        }}
                    /></p>
             <p>{name}</p>       
            <p>年龄: <input 
                 type="text" 
                 placeholder="请输入年龄"
                 value = {age}
                 onChange={(e)=>{
                    this.setState({
                        age: e.target.value
                    });
                }}
            /></p>
      </div>;
    }
}
export default Child;
```

   两种情况：

   1. 当姓名输入框中输入数据时

      打印结果为: ![](https://img14.360buyimg.com/imagetools/jfs/t1/109862/27/11814/26611/5e8f100aE0e3b87a3/2856191e064c44f1.png)

   2.在年龄输入框输入数据时

​       打印结果为：

![](https://img14.360buyimg.com/imagetools/jfs/t1/98787/39/17925/19233/5e8ff65eEf26d4659/7871527aa8859b74.png)

​       同是更新，为什么两个打印结果不一致，这是因为componentWillReceiveProps这个函数值会监听props发生变化时才会执行，而根据上方代码，姓名是绑定在props上面动态进行更改的，而年龄是在child里面设置的state属性，所以才会出现上面的情况。

##### 卸载阶段：

```
import React,{Component} from 'react';
class Child extends Component {
    state = {
        age: 8
    };
    componentWillUnmount(){
        console.log("组件即将被卸载");
    }
    render(){
        let {name,setName} = this.props;
        let {age} = this.state;
        return <div id="box">
            <p>姓名: <input
                type="text"
                placeholder="请输入新名字"
                value={name}
                onChange={(e)=>{
                    setName(e.target.value);
                }}
            /></p>
            <p>{name}</p>
            <p>年龄: <input
                type="text"
                placeholder="请输入年龄"
                value = {age}
                onChange={(e)=>{
                    this.setState({
                        age: e.target.value
                    });
                }}
            /></p>
        </div>;
    }
}
export default Child;
```

​	控制台打印结果：

​       ![](https://img14.360buyimg.com/imagetools/jfs/t1/99661/36/17944/3946/5e8f119aEeadc3063/f821884e31e25667.png)

------

现在我总结一下新版本（^16.4版本以上）：

再上一张图：

![](https://img10.360buyimg.com/imagetools/jfs/t1/116565/12/778/80490/5e90118fE41775c5a/08bb398f2b07d3e5.png)

##### 新版本挂载阶段（实例化）

 1.constructor()

 2.static getDerivedStateFromProps(props, state) 

​       替代了componentWillMount()

 3.render()

 4.componentDidMount()

##### 新版本更新阶段（存在期）

1.static getDerivedStateFromProps(props, state) 

​           替代了componentWillReceiveProps(nextProps)，componentWillUpdate(nextProps, nextState)

 2.shouldComponentUpdate(nextProps, nextState)

3.render()

4.componentDidUpdate(prevProps, prevState) 

##### 新版本卸载阶段

1.componentWillUnmount(）

------

**代码演示**

为了方便 查看每一个生命周期，我决定将child分为挂载阶段和更新阶段两个部分，分别进行查看。

父组件App.js

```
import React,{Component} from 'react';
import Child from './child';
class App extends Component {
    state = {
        name: "123",
        isShow: true
    };
    setName = (name) => {
        this.setState({
            name
        });
    };
    render() {
        let {isShow} = this.state;
        return <div>
            {isShow ? <Child name={this.state.name} setName={this.setName}/> : "组件被卸载了"}
            <button onClick={() => {
                this.setState({
                    isShow: !isShow
                });
            }}>卸载
            </button>
        </div>;
    }
}
export default App;
```

挂载阶段

```
import React,{Component} from 'react';
class Child extends Component {
    constructor(props){
        super(props);
        this.state = {
            age: 8
        };
        console.log(1,"初始化");
    }
    // 利用getDerivedStateFromProps替代了componentWillMount
    static getDerivedStateFromProps(props,state){
        console.log(props,state,"组件即将挂载");
        return false;
    }
    componentDidMount(){
        console.log(4,"组件被渲染到DOM了");
    }

    render(){
        console.log(3,"向DOM中渲染");
        let {name,setName} = this.props;
        let {age} = this.state;
        return <div id="box">
            <p>姓名: <input
                type="text"
                placeholder="请输入新名字"
                value={name}
                onChange={(e)=>{
                    setName(e.target.value);
                }}
            /></p>
            <p>{name}</p>
            <p>年龄: <input
                type="text"
                placeholder="请输入年龄"
                value = {age}
                onChange={(e)=>{
                    this.setState({
                        age: e.target.value
                    });
                }}
            /></p>
        </div>;
    }
}
export default Child;
```

挂载结果：

   ![](https://img12.360buyimg.com/imagetools/jfs/t1/117378/29/687/15522/5e8ff739E045c2a55/86b5b8df23464301.png)

------

更新阶段 ：   

```
import React,{Component} from 'react';
class Child extends Component {
    constructor(props){
        super(props);
        this.state = {
            age: 8
        };
        console.log(1,"初始化");
    }
    // 利用getDerivedStateFromProps 替代了 componentWillReceiveProps
    static getDerivedStateFromProps(props,state){
        console.log(2,props,state,"组件即将更新");
        return false;
    }
    shouldComponentUpdate(nextProps, nextState){
        console.log(3,"组件更新",nextProps, nextState,this.props,this.state);
        return true;// true 接着向下执行生命周期,并更新视图，false 打断执行,不在更新视图
    }
    componentDidUpdate(prevPorps,prevState,prevInfo){
        let box = document.querySelector("#box");
        console.log(5,'组件更新完毕');
        console.log(box.innerHTML);
    }
    render(){
        console.log(4,"向DOM中渲染");
        let {name,setName} = this.props;
        let {age} = this.state;
        return <div id="box">
            <p>姓名: <input
                type="text"
                placeholder="请输入新名字"
                value={name}
                onChange={(e)=>{
                    setName(e.target.value);
                }}
            /></p>
            <p>{name}</p>
            <p>年龄: <input
                type="text"
                placeholder="请输入年龄"
                value = {age}
                onChange={(e)=>{
                    this.setState({
                        age: e.target.value
                    });
                }}
            /></p>
        </div>;
    }
}
export default Child;
```

更新结果：

​     姓名，年龄发生更改时：![](https://img14.360buyimg.com/imagetools/jfs/t1/113826/10/715/32256/5e8ff816Ec1fd3c3b/5dbcd9c99328ec92.png)



### 总结

1.React16.4以上版本新的生命周期弃用了componentWillMount、componentWillReceivePorps，componentWillUpdate，这三个后期会被删除掉。

2.getDerivedStateFromProps来代替弃用的三个钩子函数（componentWillMount、componentWillReceivePorps，componentWillUpdate

3.React16.4以上版本并没有删除这三个componentWillMount、componentWillReceivePorps，componentWillUpdate钩子函数，但是不能和新增的钩子函数getDerivedStateFromProps混用。