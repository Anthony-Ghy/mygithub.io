---
title: React基础学习笔记1
categories: React学习
tags:
- 前端
- React
- 基础
---
 学习接触React已经有一段时间，从刚开始的懵懂到现在稍微明白整体的架构，这里想结合最近的学习React整理一些学习笔记，方便后期进行参考。

**参考资料主要有**：

React官方文档，Github优质React项目。
<!-- more -->
​    首先先总结一些个人在学习过程中觉得十分重要的一些基础知识（不会结合项目，后续结合项目再进行相应的展开）。

#### 1.jsx简介：

​    文档中对于它有了相应的介绍，他是Js的一个语法扩展，刚接触时我觉得它有点像Jsp,就是在Js里面写Html标签，只不过他的使用方式与Html还是有很大的不同的，具体使用还是想结合一些自己写的例子举例子比较好，不然就会觉得在照搬文档。

`const element = <h1>Hello, world!</h1>;`

有一点需要注意的就是他的每一个Dom元素都会使用（小驼峰）的命名法来自定义Dom属性，

例如：JSX 里的 `class` 变成了className,而tabindex则变成了tabIndex。（这在后期使用中是十分必要的）

#### 2.组件简介：

​      由于都是在js中编写相应的组件，所以可以通过定义函数来进行组件定义，但最近由于一直接触class使用方式，所以接下来还是先以es6中的class语法来进行展示举例

```
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

上面这段代码中就是运用的class的方式定义的一个组件

**3.State简介：**个人理解就是初始化定义的一些变量，类似于Vue中的data()函数定义的一些变量

首先它是一个对象，定义的变量都会以key,value的方式进行保存

```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

​      上述代码中在Constructor（生命周期函数）定义的state中有一个data变量，这时我们给他赋值当前的时间，在render函数中通过this.state.data进行获取，注意外面要加{}，这就是上面说的jsx写法，这个括号里面就可以调用相应的state里面定义的变量了。后续使用基本都是这样用。

**4.事件处理简介：**

```
<button onClick={activateLasers}>  Activate Lasers</button>
```

上面这段代码中就是在React中定义事件的一个例子，首先要知道事件定义命名采用上文中提到的小驼峰命名法，即onClick，onChange等都是这种方法，这是规定，就这样使用就好。

​        同时要记住在React使用事件函数时不能通过返回 `false` 的方式阻止默认行为。你必须显式的使用 `preventDefault`（这是个人觉得很重要的一点，所以提炼出来）

```
function ActionLink() {
  function handleClick(e)
  {   
      e.preventDefault();
      console.log('The link was clicked.');  
  }
  return 
  (
    <a href="#" onClick={handleClick}>Click me</a>
  );
}
```

​     例如上面这段代码中你在a标签中定义了一个onClick事件函数handleClick,然后在return上方进行方法定义，关键看 ` e.preventDefault();`这行代码，这里的e是一个合成事件，它里面有一个  `preventDefault()`方法，通过这个方法来进行停止执行函数操作的功能。

```
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};
    // 为了在回调中使用 `this`，这个绑定是必不可少的    
    this.handleClick = 	 this.handleClick.bind(this);  
    }

  handleClick() { 
      this.setState(state => (
             { isToggleOn: !state.isToggleOn    }));  
  }
  render() {
    return (
      <button onClick={this.handleClick}>{this.state.isToggleOn ? 'ON' : 'OFF'}</button>
    );
  }
}
```

​       上面这段代码中涉及到了上文中提到的state,事件函数等一系列知识点，但这些不是重点，重点是要看这里的this绑定，代表是将在Toggle这个类中定义的handleClick与这个Toggle类进行绑定，这是必不可少的，他是为了防止在handleClick这个方法中通过使用this时发生this指向不明确的问题，如果没有绑定this，运行时会报错，提示state不存在，因为这时候的this指向指向了window,但如果写了绑定this的那句话，就可以直接使用this.setState进行变量赋值了。虽然将整体原理已经按照自己的思路讲解了一下，但是现在我要说明自己在使用中是怎么使用的。见下面的代码：

```
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};
    }
  handleClick=()=>{this.setState(state => ({ isToggleOn: !state.isToggleOn}));  }
  render() {
    return (
      <button onClick={this.handleClick}>{this.state.isToggleOn ? 'ON' : 'OFF'}</button>
    );
  }
}
```

​     上面我在函数定义时采用了es6中箭头函数的方式来进行调用，熟悉es6语法的人都知道，使用箭头函数以后可以将内外this指向当前定义的类，并不会出现this指向不明确的情况，运用箭头函数以后，也不用在在constroctor中绑定this了。总结就是一句话，定义方法就用箭头函数绝对没错！

**5.列表,Key简介：**

​	 一般在react中，经常会出现数组这个数据结构，循环遍历这个数组并将这个数组渲染出来是经常在开发过程中进行的操作，但是这里循环遍历采用map的方式，他会生成一个新的数组在进行遍历渲染

```
Class NumberList() {
  const numbers = [1, 2, 3, 4, 5];
  const listItems = numbers.map((number) =>
    <li}>{number}</li>
  );
  return (
    <ul>{listItems}</ul>
  );
}
```

​    上面代码就是将numbers进行遍历并最终渲染到ul这个标签里面，基本使用方法就是这个，但是当你开心的运行去看结果的时候他会提醒你这里产生报错问题,a key should be provided for list items 这里是什么意思呢？为什么会产生这个情况，我会介绍一下：由于react在渲染之前都是通过diff算法进行的虚拟Dom树，这个时候需要一个标识来确定每一个节点，当遍历数组时会产生多个节点，设置每一个节点一个唯一标识符是很重要的，他会让react具体识别到相应的节点并进行一系列的操作。好的，理论知识结束，我们的目标是要在每一个遍历节点中进行标识。只需这样即可：

```
<li key={number.toString()}>{number} </li>
```

​       一个元素的 key 最好是这个元素在列表中拥有的一个独一无二的字符串,通常情况下我们可以使用id来进行唯一标识判断，在没有id的情况下我们可以使用每一个数组的索引值进行key的设置，但是这样会出现当节点位置发生改变时会产生指向不明确的问题，综上所述，最好还是用id来进行标记。

**6.受控组件简介：**

​    文档中介绍的话个人觉得有些绕，用我的理解来说就是你经常会使用一些比如input,textarea,select等标签，当你在表单中输入值的时候这时候其实他是有更改数据的情况，这时候你需要一个函数来进行监听这些标签中数据的更改，并对这些监听到的数据来进行整体的渲染操作等等（这是后话）

在受控组件中，个人觉得最重要的一个监听函数就是onChange事件，只要表单数据有了相应的修改了，这个组件就会相应的触发并进行后续的操作，但是总结起来还有最重要的一点，表单数据是与你先前在state中的变量是一致的。

```
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};
  }
  handleChange=(event)=>{ this.setState({value: event.target.value});  }
  render() {
    return (
          名字:
          <input type="text" value={this.state.value} onChange={this.handleChange} /> 
    );
  }
}
```

​     例如上面示例代码你定义了一个input标签，并把它的value与你定义的state绑定到了一起，这个时候你需要设置一个onChange函数来进行相应的动态监听，每当表单输入数据进行更改时你就需要在onChange这个函数里面进行相应的监听并把它实时的在state中进行更改，整体受控组件监听流程就是这样，后续操作可以直接这样进行相应的操作。

**7.props简介：**

​     在 React.js 中，数据是从上自下流动（传递）的，也就是一个父组件可以把它的 state / props 通过 props 传递给它的子组件，但是子组件不能修改 props - React.js 是单向数据流，如果子组件需要修改父组件状态（数据），是通过回调函数方式来完成的。

​    讲解这个问题个人觉得可以理解为父子组件传参进行相应的讨论，这个问题如果深入展开来说就是比较复杂,因为子父组件传参这个事情贯穿到开发项目的始终，当开发人员自定义组件的时候一直在使用props这个属性，自定义组件这里我决定后续结合一些项目例子来进行讲解讨论比较好，先介绍props的基础知识。

`const element = <Welcome name="Sara" />`

```
class NEW(props) {  return <Welcome>Hello,{this.props.name}</Welcome>}
```

这里的例子就是当你设置了一个新的组件Welcome时，你需要在你定义的新的组件中使用Welcome组件时，你可以获取到相应的你在Welcome设置的相应的参数这个时候用this.props.name即可获取到你定义的相应的属性，这些属性都存放在props这个对象里面，用的时候直接使用this.props.属性即可获得，这个操作事非常重要的，后续会继续结合项目例子进行总结提炼。

**8.refS简介：**

对于这个知识点我的理解是就是通过它来定位到一些组件来进行相应的操作，上一个实例代码来进行演示：

```
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    // 创建一个 ref 来存储 textInput 的 DOM 元素
    this.textInput = React.createRef();
  }

  focusTextInput() {
        // 直接使用原生 API 使 text 输入框获得焦点
        // 注意：我们通过 "current" 来访问 DOM 节点
        this.textInput.current.focus();  
  }

  render() {
    // 告诉 React 我们想把 <input> ref 关联到
    // 构造器里创建的 `textInput` 上
    return (
      <div>
        <input
          type="text"
          ref={this.textInput} />
          <input
              type="button"
              value="Focus the text input"
              onClick={this.focusTextInput}
          />
      </div>
    );
  }
}
```

​       通过React.createRef()来定义一个this.textInput这个属性，然后为了建立关联，我们可以在input标签中设置ref这个属性来与textInput进行关联，在调用这个this.focusTextInput方法时可以直接将关联的input框进行相应的聚焦操作。整体理解就是这些，自己前期在初学阶段运用这些知识点去写一个todolist的时候，曾经使用过ref来获取input框这个dom节点进而进行聚焦操作。

###        **总结：**

​       虽然react有很多的东西需要学习，但是个人目前觉得最常使用的一些基础就是这些，下一篇我会重点介绍react组件生命周期这个知识点，因为我觉得它值得我详细的对它进行总结。