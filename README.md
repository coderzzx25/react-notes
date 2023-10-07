# React

- [React](#react)
  - [函数组件与类组件的区别](#函数组件与类组件的区别)
  - [生命周期(万物皆可周期)](#生命周期万物皆可周期)
  - [生命周期和生命周期函数的关系](#生命周期和生命周期函数的关系)
  - [生命周期函数(类)](#生命周期函数类)
  - [为什么使用 setState](#为什么使用-setstate)
  - [setState 的使用](#setstate-的使用)
  - [setState 异步更新](#setstate-异步更新)
  - [setState 一定是异步的吗?(React18 之前)](#setstate-一定是异步的吗react18-之前)
  - [PureComponent](#purecomponent)
  - [高阶组件 memo](#高阶组件-memo)
  - [如何使用 ref](#如何使用-ref)
  - [ref 的转发](#ref-的转发)
  - [认识受控组件](#认识受控组件)
  - [受控组件](#受控组件)
  - [受控组件的基本演练](#受控组件的基本演练)
  - [受控组件的其他演练](#受控组件的其他演练)
  - [非受控组件](#非受控组件)
  - [认识高阶函数](#认识高阶函数)
  - [高阶组件的定义](#高阶组件的定义)
  - [props 增强](#props-增强)
  - [高阶函数的意义](#高阶函数的意义)
  - [portals 的使用](#portals-的使用)
  - [fragment](#fragment)
  - [StrictMode](#strictmode)
  - [严格模式检查的是什么?](#严格模式检查的是什么)
  - [理解 JavaScript 纯函数](#理解-javascript-纯函数)
  - [副作用概念的理解](#副作用概念的理解)
  - [纯函数的案例](#纯函数的案例)
  - [纯函数的作用和优势](#纯函数的作用和优势)
  - [为什么需要 redux](#为什么需要-redux)
  - [Redux 的核心理念 - State](#redux-的核心理念---state)
  - [Redux 的核心理念 - action](#redux-的核心理念---action)
  - [Redux 的核心理念 - reducer](#redux-的核心理念---reducer)
  - [Redux 的三大原则](#redux-的三大原则)
  - [redux 融入 react 代码](#redux融入react代码)

## 函数组件与类组件的区别

**函数组件是使用 function 来定义的函数，只有这个函数会返回和类组件中的 render 函数返回一样的内容**

**函数组件有自己的特点**

1. 没有生命周期，也会被更新并挂载，但是没有生命周期函数
2. this 关键字不能指向组件实例(因为没有组件实例)
3. 没有内部状态(state)

## 生命周期(万物皆可周期)

很多的事物都有从创建到销毁的整个过程,这个过程称之为生命周期
React 组件也有自己的生命周期,了解组件的生命周期可以让我们在最合适的地方完成自己想要的功能

## 生命周期和生命周期函数的关系

**生命周期是一个抽象的概念,在生命周期的整个过程，分成了很多个阶段:**

- 比如装载阶段(Mount),组件第一次在 DOM 树中被渲染的过程;
- 比如更新阶段(Update),组件状态发生变化,重新更新渲染的过程;
- 比如卸载过程(Unmount),组件从 DOM 树中被移除的过程;

**React 内部为了告诉我们当前处于哪些阶段,会对我们组件内部实现的某些函数进行回调，这些函数就是生命周期函数:**

- 比如实现 componentDidMount 函数:组件已经挂在到 DOM 上时,就会回调;
- 比如实现 componentDidUpdate 函数:组件已经发生了更新时,就会回调;
- 比如实现 componentWillUnmount 函数:组件即将被移除时,就会回调;
- 我们可以在这些回调函数中编写自己的逻辑代码,来完成自己的需求功能;

## 生命周期函数(类)

**Constructor**

如果不初始化 state 或不尽兴方法绑定，则不需要为 React 组件实现构造函数。

constructor 中通常只做两件事情:

- 通过给 this.state 赋值对象来初始化内部的 state;
- 为事件绑定实例(this);

**componentDidMount**

componentDidMount()会在组件挂载后(插入 DOM 数中)立即调用;

componentDidMount 中通常进行哪些操作呢？

- 依赖于 DOM 的操作可以在这里进行;
- 在此处发送网络请求就是最好的地方;(官方建议)
- 可以在此处添加一些订阅(会在 componentWillUnmount 取消订阅);

**componentDidUpdate**

componentDidUpdate()会在更新后立即调用,首次渲染不会执行此方法。

- 当组件更新后,可以在此处进行 DOM 操作;
- 如果你对更新前后对 props 进行了比较,也可以选择在此处进行网络请求;(例如:当 prsop 未发生变化时,则不会执行网络请求)

**componentWillUnmount**

componentWillUnmount()会在组件卸载及销毁之前直接进行调用。

- 在此方法中执行必要的清理操作;
- 例如:清除 timer,取消网络请求或清除在 componentDidMount()中创建的订阅等;

## 为什么使用 setState

**开发中我们并不能直接通过修改 state 的值来让界面发生更新:**

- 因为我们修改 state 之后,希望 React 根据新的 State 来重新渲染洁面，但是这种方式的修改 React 并不知道数据发生来变化;
- React 并没有实现类似的于 Vue2 中的 Object.defineProperty 或者 Vue3 中的 Proxy 的方式来监听数据的变化;
- 我们必须通过 setState 来告知 React 数据已发生变化;

**疑惑:在组件中并没有实现 setState 的方法,为什么可以调用呢?**

- 原因很简单,setState 方法是从 component 中继承过来的。

## setState 的使用

1.setState 的基本使用

```javascript
this.setState({
  message: "coderzzx",
});
```

2.setState 可以传入一个回调函数

好处一:可以在回调函数中编写新的 state 的逻辑

好处二:当前的回调函数会将之前的 state 和 props 传递进来

```javascript
this.setState((state, props) => {
  return {
    message: "coderzzx",
  };
});
```

3.setState 在 React 的事件处理中是一个一步调用

如果希望在数据更新之后(数据合并),获取对应的结果执行一些逻辑代码那么可以在 setState 中传入第二个参数:callback

```javascript
this.setState(
  {
    message: "coderzzx",
  },
  () => {
    console.log("最新的message", this.state.message); // coderzzx
  }
);
```

## setState 异步更新

**setState 的更新是异步的?**

```javascript
changeText(){
    this.setState({
        message: 'coderzzx'
    })
    console.log(this.state.message) // Hello World
}
```

最终的打印结果是 Hello World 并不是 coderzzx

可见 setState 是异步的操作,我们并不能在执行完 setState 之后立马拿到最新的 state 的结果

**为什么 setState 设计为异步呢?**

setState 设计为异步其实之前在 Github 上也有很多的讨论:

React 核心成员(Redux 的作者 Dan Abramov)也有对应的回应[why is setState asynchronous?](https://github.com/facebook/react/issues/11527#issuecomment-360199710)

**简单总结:**

setState 设计为异步,可以显著的提升性能;

如果每次调用 setState 都进行一次更新,那么意味着 render 函数会被频繁调用,界面重新渲染，这样效率是很低的;

最好的办法应该是获取到多个更新,之后进行批量更新;

如果同步更新了 state,但是还没有执行 render 函数,那么 state 和 props 不能保持同步;

state 和 props 不能保持一致性,会在开发中产生很多的问题;

## setState 一定是异步的吗?(React18 之前)

在 React18 之前这个是一个同步的

```javascript
changeText(){
    // 在React18之前,setTimeout中setState操作,是同步操作
    // 在React18之后,setTimeout中setState异步操作(批处理)
    setTimeout(()=>{
        this.setState({
            message: 'coderzzx'
        })
        console.log(this.state.message) // react18之前 coderzzx
    },0)
}
```

## PureComponent

**如果所有的类,我们都需要手动来实现 shouldComponmentUpdate,那么会给我们开发者增加非常多的工作量。**

我们来设想一下 shouldComponent 中的各种判断的目的是什么?
props 或者 state 中的数据是否发生了变化,来决定 shouldComponentUpdate 返回 true 或者 false;

**事实上 React 已经考虑到这一点,所以 React 已经默认帮我们实现好了,如何实现呢?**

将 class 继承自 PureComponent。

## 高阶组件 memo

**目前我们针对类组件可以使用 PureComponent,那么函数式组件呢?**

事实上函数组件我们在 props 没有改变时,也是不希望其重新渲染其 DOM 树结构的

**我们需要使用一个高阶组件 memo:**

将之前的一些组件都通过 memo 函数进行一层包裹;

最终的效果时,当数据发生变化时,其他子组件将不会重新执行;

## 如何使用 ref

**在 React 的开发模式中,通常情况下不需要,也不建议直接操作 DOM 原生,但是一些特殊情况,确实需要获取到 DOM 进行某些操作:**

1. 管理焦点,文本选择或媒体播放;
2. 触发强制动画;
3. 集成第三方 DOM 库;

**我们可以通过 refs 获取 DOM，如何创建 refs 来获取对应到 DOM 呢?目前有三种方式:**

方式一: 在 React 元素上绑定一个 ref 字符串

```javascript
class App extends PureComponent {
  constructor() {
    super();
    this.state = {};
  }
  getNativeDom() {
    console.log(this.refs.coderzzx);
  }
  render() {
    return (
      <div>
        <h2 ref="coderzzx">Hello World</h2>
      </div>
    );
  }
}
```

(推荐)方式二: 提前创建好 ref 对象,createRef(),将创建出来的对象绑定到元素

```javascript
class App extends PureComponent {
  constructor() {
    super();
    this.state = {};
    this.titleRef = createRef();
  }
  getNativeDom() {
    console.log(this.titleRef.current);
  }
  render() {
    return (
      <div>
        <h2 ref={this.titleRef}>Hello World</h2>
      </div>
    );
  }
}
```

方式三: 传入一个回调函数,在对应的元素被渲染之后,回调函数被执行,并且将元素传入

```javascript
class App extends PureComponent {
  constructor() {
    super();
    this.state = {};
    this.titleEl = null;
  }
  getNativeDom() {
    console.log(this.titleEl);
  }
  render() {
    return (
      <div>
        <h2 ref={(el) => (this.titleEl = el)}>Hello World</h2>
      </div>
    );
  }
}
```

## ref 的转发

ref 不能应用于函数组件:

因为函数组件没有实例,所以不能获取到应用到组件对象

但是在开发中我们可能想要过去函数组件中某个元素的 DOM,这个时候我们如何操作呢?

方式一: 直接传入 ref 属性(错误做法)

方式二: 通过 forwardRef 高阶函数;

```javascript
class HelloWorld extends PureComponent {
  test() {
    console.log("test");
  }
  render() {
    return <div>Hello World</div>;
  }
}

const Hello = forwardRef(function (props, ref) {
  return <div ref={ref}>Hello</div>;
});

class App extends PureComponent {
  constructor() {
    super();
    this.state = {};
    this.hwRef = createRef();
  }
  getNativeDom() {
    console.log(this.hwRef.current);
    this.hwRef.current.test();
  }
  render() {
    return (
      <div>
        <HelloWorld ref={this.hwRef}></HelloWorld>
        <Hello ref={this.hwRef}></Hello>
      </div>
    );
  }
}
```

## 认识受控组件

**在 React 中,HTML 表单的处理方式和普通的 DOM 元素不太一样:表单元素通常保存在一些内部的 state。**

**比如如下面的 HTML 表单元素:**

这个处理方式是 DOM 默认处理 HTML 表单的行为,在用户点击提交时会提交到某个服务器中,并且刷新页面;

在 React 中,并没有禁止这个行为,它依然是有效的;

但是在通常情况下会使用 javaScript 函数来方便的处理表单提交,同时还可以访问用户填写的表单数据;

实现这种效果的标准方式是使用“受控组件”;

## 受控组件

```javascript
class App extends PureComponent {
  constructor() {
    super();
    this.state = {
      username: "",
    };
  }
  inputChange(event) {
    this.setState({ username: event.target.value }, () => {
      console.log(this.state.username);
    });
  }
  render() {
    const { username } = this.state;
    return (
      <div>
        <input
          type="text"
          value={username} // 如果绑定默认值那么就是受控组件，并且需要写一个onChange事件
          onChange={(e) => this.inputChange(e)}
        />
      </div>
    );
  }
}
```

## 受控组件的基本演练

**在 HTML 中,表单元素(如`<input>`,`<textarea>`和`<select>`)之类的表单元素通常自己维护 state,并根据用户输入进行更新。**

**而在 React 中,可变状态(mutable state)通常保存在组件的 state 属性中,并且只能通过使用 setState()来更新。**

我们将这两者结合起来,使 React 的 state 成为"唯一数据源";

渲染表单的 React 组件还控制着用户输入过程中表单发生的操作;

被 React 以这种方式控制取值的表单输入元素就叫做"受控组件";

由于在表单元素上设置了 value 属性,因此显示的值将始终为 this.state.value,这使得 React 的 state 成为唯一数据源。

由于 onChangeInputValue 在每次按键时都会执行并更新 React 的 state,因此显示的值将随着用户输入而更新。

```javascript
class App extends PureComponent {
  constructor() {
    super();
    this.state = {
      username: "",
      password: "",
    };
  }
  onChangeInputValue(e) {
    const keyName = e.target.name;
    this.setState({
      [keyName]: e.target.value,
    });
  }
  onSubmitClick(e) {
    console.log(e);
  }
  render() {
    const { username, password } = this.state;
    return (
      <div>
        <form onSubmit={(e) => this.onSubmitClick(e)}>
          <label htmlFor="username">
            账户:
            <input
              id="username"
              type="text"
              name="username"
              value={username}
              onChange={(e) => this.onChangeInputValue(e)}
            />
          </label>
          <label htmlFor="password">
            密码:
            <input
              id="password"
              type="password"
              name="password"
              value={password}
              onChange={(e) => this.onChangeInputValue(e)}
            />
          </label>
          <button type="submit">提交</button>
        </form>
      </div>
    );
  }
}
```

## 受控组件的其他演练

```javascript
class App extends PureComponent {
  constructor() {
    super();
    this.state = {
      username: "",
      password: "",
      isAgree: true,
      hobbies: [
        {
          valeu: 1,
          label: "唱",
          isChecked: false,
        },
        {
          value: 2,
          label: "跳",
          isChecked: false,
        },
      ],
      fruit: ["origin"],
    };
  }
  onChangeInputValue(e) {
    const keyName = e.target.name;
    this.setState({
      [keyName]: e.target.value,
    });
  }
  onChangeChecked(e) {
    this.setState({
      isAgree: e.target.checked,
    });
  }
  onSubmitClick(e) {
    console.log(e);
  }
  onChangeHobbies(e, index) {
    const hobbies = [...this.state.hobbies];
    hobbies[index].isChecked = e.target.checked;
    this.setState({ hobbies });
  }
  onChangeSelect(e) {
    // 单选
    // this.setState({ fruit: e.target.value });
    // 多选
    const options = Array.from(e.target.selectedOptions);
    const values = options.map((item) => item.value);
    this.setState({
      fruit: values,
    });
  }
  render() {
    const { username, password, isAgree, hobbies, fruit } = this.state;
    return (
      <div>
        <form onSubmit={(e) => this.onSubmitClick(e)}>
          <label htmlFor="username">
            账户:
            <input
              id="username"
              type="text"
              name="username"
              value={username}
              onChange={(e) => this.onChangeInputValue(e)}
            />
          </label>
          <label htmlFor="password">
            密码:
            <input
              id="password"
              type="password"
              name="password"
              value={password}
              onChange={(e) => this.onChangeInputValue(e)}
            />
          </label>
          {/* checkbox单选 */}
          <label htmlFor="agree">
            <input
              id="agree"
              type="checkbox"
              checked={isAgree}
              onChange={(e) => this.onChangeChecked(e)}
            />
            同意协议
          </label>
          {/* checkbox多选 */}
          <div>
            你的爱好
            {hobbies.map((item, index) => {
              return (
                <label htmlFor="hobbie" key={item.id}>
                  <input
                    type="checkbox"
                    id={item.value}
                    checked={item.isChecked}
                    onChange={(e) => this.onChangeHobbies(e, index)}
                  />
                  {item.label}
                </label>
              );
            })}
          </div>
          {/* select */}
          <select
            value={fruit}
            multiple
            onChange={(e) => this.onChangeSelect(e)}
          >
            <option value="apple">苹果</option>
            <option value="banana">香蕉</option>
            <option value="orange">橘子</option>
          </select>
          <button type="submit">提交</button>
        </form>
      </div>
    );
  }
}
```

## 非受控组件

React 推荐大多数情况下使用受控组件来处理表单数据:

一个受控组件中,表单数据是由 React 组件来管理的;

另一种替代方案是使用非受控组件,这时表单数据将交由 DOM 节点来处理;

如果要使用非受控组件中的数据,那么我们需要使用 ref 来从 DOM 节点中获取表单数据;

```javascript
class App extends PureComponent {
  constructor() {
    super();
    this.state = {
      username: "",
    };
  }
  inputChange(event) {
    this.setState({ username: event.target.value }, () => {
      console.log(this.state.username);
    });
  }
  render() {
    return (
      <div>
        <input type="text" onChange={(e) => this.inputChange(e)} />
      </div>
    );
  }
}
```

## 认识高阶函数

**高阶函数的维基百科定义:至少满足以下条件之一:**

接受一个或多个函数作为输入;

输出一个函数;

**JavaScript 中比较常见的 filter/map/reduce 都是高阶函数。**

**那么什么是高阶组件?**

高阶组件的英文名是 Higher-Order Components,简称 HOC;

官方的定义:高阶组件是参数作为组件,返回值为新组件的函数;

**我们可以进行如下的解析:**

首先,高阶组件本身不是一个组件,而是一个函数;

其次,这个函数的参数是一个组件,返回值也是一个组件;

```javascript
function higherOrderComponent(WrapperComponent) {
  class NewComponent extends PureComponent {
    render() {
      return <WrapperComponet name="coderzzx" />;
    }
  }
  NewComponent.dispalyName = "HigherComponent";
  return NewComponent;
}

function Text() {
  return <div>coder</div>;
}

const Component = higherOrderComponent(Text);

class App extends PureComponent {
  render() {
    return <Component />; // 当前组件存在一个name属性值为coderzzx
  }
}
```

## 高阶组件的定义

**高阶组件的调用过程类似于这样:**

```javascript
const Component = higherOrderComponent(Text);
```

**高阶函数的编写过程类似于这样:**

```javascript
function higherOrderComponent(WrapperComponent) {
  class NewComponent extends PureComponent {
    render() {
      return <WrapperComponet name="coderzzx" />;
    }
  }
  NewComponent.dispalyName = "HigherComponent";
  return NewComponent;
}
```

**组件名称的问题:**

在 ES6 中,类表达式中类名是可以省略的;

组件的名称都可以通过 displayName 来修改;

**高阶组件并不是 React API 的一部分,它是基于 React 的组合特性而形成的设计模式;**

**高阶组件在一些 React 第三方库中非常常见:**

比如 redux 中的 connect;

比如 react-router 中的 withRouter;

## props 增强

```javascript
// 定义组件
function EnhancedUserInfo(OriginComponent) {
  class NewComponent extends PureComponent {
    constructor() {
      super();
      this.state = {
        userInfo: {
          name: "coderzzx",
          level: 99,
        },
      };
    }
    render() {
      // 如果组件传入一些其他数据
      return <OriginComponent {...this.props} {...this.state.userInfo} />;
    }
  }
  return NewComponent;
}

// 使用组件
const Home = EnhancedUserInfo(function (props) {
  return (
    <h1>
      {props.name}-{props.level}
    </h1>
  );
});

const About = EnhancedUserInfo(function (props) {
  return (
    <h1>
      {props.name}-{props.level}
    </h1>
  );
});

// 渲染效果
class App extends PureComponent {
  render() {
    return (
      <div>
        {/* 传入其他数据 */}
        <Home banner={"banner"} />
        <About />
        <Me />
      </div>
    );
  }
}

// 不在同一文件中使用
// page/Me.jsx
class Me extends PureComponent {
  render() {
    return <div>{this.props.name}</div>;
  }
}
export default EnhancedUserInfo(Me);
```

## 高阶函数的意义

**我们会发现利用高阶组件可以针对某些 React 代码进行更加优雅的处理。**

**其实早期的 React 有提供组件之间的一种复用方式是 mixin,目前已经不在建议使用:**

Mixin 可能会相互依赖,相互耦合,不利于代码维护;

不同的 Mixin 中的方法可能会相互冲突;

Mixin 非常多时,组件处理起来会比较麻烦,甚至还要为其做相关处理,这样会给代码造成滚雪球式的复杂性;

**当然,HOC 也有自己的一些缺陷:**

HOC 需要在原组件上进行包裹或者嵌套,如果大量使用 HOC,将会产生非常多的嵌套,这让调试变的非常困难;

HOC 可以劫持 props,在不遵守约定的情况下也可能造成冲突;

**Hooks 的出现,是开创性的,它解决了很多 React 之前存在的问题:**

比如 this 指向问题,比如 hoc 的嵌套复杂度问题等等;

## portals 的使用

**某些情况下,我们希望渲染的内容独立于父组件,甚至是独立于当前挂载到的 DOM 元素中(默认都是挂载到 id 为 root 的 DOM 元素上的)。**

**Portal 提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的优秀的方案:**

第一个参数(child)是任何可渲染的 React 子元素,例如一个元素,字符串或 fragment;

第二个参数(container)是一个 DOM 元素;

```javascript
ReacatDOM.createPortal(child, container);
```

**通常来讲,当你从组件的 render 方法返回一个元素时,该元素被挂载到 DOM 节点中离其最近的父节点:**

**然而,有时候将子元素插入到 DOM 节点中的不同位置也是有好处的:**

```javascript
render() {
  // React挂载了一个新的div,并且把子元素渲染其中
  return (
    <div>
      {this.props.children}
    </div>
  )
}

render(){
  // React并没有创建一个新的div,它只是把子元素渲染到domNode中
  return ReactDom.createPortal(
    this.props.children
  )
}
```

## fragment

**在开发中,我们总是在一个组件中返回内容时包裹一个 div 元素:**

**我们又希望可以不渲染这样一个 div 应该如何操作呢?**

使用 Fragment

Fragment 允许呢将子列表分组,而无需向 DOM 添加额外节点;

**React 还提供了 Fragment 的短语法:**

它看起来像空标签<></>;

但是,如果我们需要在 Fragment 中添加 key,那么就不能使用短语法;

## StrictMode

**StrictMode 是一个用来突出显示应用程序中潜在问题的工具:**

与 Fragment 一样,StrictMode 不会渲染任何可见的 UI;

它为其后代元素出发额外的检查和警告;

严格模式检察仅在开发模式下运行;它们不会影响生产构建;

**可以为应用程序的任何部分启用严格模式:**

不会对 Header 和 Footer 组件运行严格模式检查;

但是,ComponentOne 和 componentTwo 以及它们的所有后代元素都将进行检查;

```javascript
<Header />
<React.StrictMode>
  <div>
    <ComponentOne/>
    <ComponentTwo/>
  </div>
</React.StrictMode>
<Footer />
```

## 严格模式检查的是什么?

**但是检测,到底是检测什么?**

1. 识别不安全的生命周期
2. 使用过时的 ref API
3. 检查意外的副作用
   - 这个组件的 constructor 会被调用两次;
   - 这是严格模式下故意进行的操作,让你来查看这里写的一些逻辑代码被调用多次时,是否会产生一些副作用;
   - 在生产环境中,是不会被调用两次的;
4. 使用废弃的 findDOMNode 方法
   - 在之前的 React API 中,可以通过 findDOMNode 来获取 DOM,不过已经不推荐使用了。
5. 检测过时的 context API

   - 早期的 Context 是通过 static 属性声明 Context 对象属性,通过 getChildContext 返回 Context 对象等方式来使用 Context 的;

   - 目前这种方式已经不推荐使用

## 理解 JavaScript 纯函数

**函数式编程**中有一个非常重要的概念叫**纯函数**,JavaScript 符合**函数式编程的范式**,所以也有**纯函数的概念**;

在 **react 开发中纯函数是被多次提及**的;

比如**react 中组件就被要求像是一个纯函数**(为什么是像,因为还有 class 组件),**redux 中有一个 reducer 的概念**,也是要求必须是一个纯函数;

所以**掌握纯函数对于理解很多框架的设计**是非常有帮助的;

**纯函数的维基百科定义:**

在程序设计中,若一个函数符合以下条件,那么这个函数被称为纯函数:

此函数在相同的输入值时,需要产生相同的输出。

函数的输出和输入值以外的其他隐藏信息或状态无关,也和由 I/O 设备产生的外部输出无关。

该函数不能有语义上可观察到函数副作用,如"触发事件",使输出设备输出,或更改输出值以外物件的内容等。

**当然上面的定义过于的晦涩,所以简单总结一下:**

确定的输入,一定会产生确定的输出;

```javascript
function foo(num) {
  return num + 10;
}
// 在传入的num同样的情况下，输出的值永远不会变
foo(10);
foo(10);
```

函数在执行过程中,不能产生副作用;

```javascript
const info = { name: "coderzzx" };
// 传入的值仅仅只能使用不能进行修改
function foo(info) {
  info.name = "zzx";
}
foo(info);
```

## 副作用概念的理解

**什么是副作用呢?**

**副作用(side effect)** 其实本身是一个医学概念,比如我们经常说吃什么药本来是为了治病,可能会引发一些其他的副作用;

在计算机科学中,也引用了副作用的概念,表示在执行一个函数时,除了返回函数值之外,还对调用函数产生了附加的影响,比如修改了全局变量,修改了参数或者改变外部的存储;

**纯函数在执行过程中就是不能产生这样的副作用:**

副作用往往是产生 bug 的 "温床"。

## 纯函数的案例

**对数组操作的两个函数:**

slice:slice 截取数组时不会对原数组进行任何操作,而是生成一个新的数组;

splice:splice 截取数组,返回一个新的数组,也会对原数组进行修改;

**slice 就是一个纯函数,不会修改数组本身,而 splice 函数不是一个纯函数;**

```javascript
var names = ["abc", "cba", "nba", "dna"];
// slice截取数组时不会对原数组进行任何操作,而是生成一个新的数组
var newNames = names.slice(0, 2);
console.log(newNames);
// splice截取数组,会返回一个新的数组,也会对原数组进行修改
var newNames2 = names.splice(0, 2);
console.log(newNames2);
console.log(names);
```

## 纯函数的作用和优势

**为什么纯函数在函数式编程中非常重要呢?**

因为你可以安心的编写和安心的使用;

你在写的时候保证了函数的纯度,只是单纯实现自己的业务逻辑即可,不需要关系传入的内容是如何获取的或者依赖其他的外部变量是否已经发生了修改;

你在用的时候,你确定你输入内容不会被任意篡改,并且自己确定的输入,一定会有确定的输出;

React 中就要求我们无论是函数还是 class 声明一个组件,这个组件都必须像纯函数一样,保护他们的 props 不被修改:

React 非常灵活,但它也有一个严格的规则:所有 React 组件都必须像纯函数一样保护它们的 props 不被更改。

在 redux 中,reducer 也被要求是一个纯函数。

## 为什么需要 redux

**JavaScript 开发的应用程序,已经变的越来越复杂了:**

JavaScript 需要管理的状态越来越多,越来越复杂;

这些状态包括服务器返回的数据,缓存数据,用户操作产生的数据等等,也包括一些 UI 的状态,比如某些元素加载动效,当前分页;

**管理不断变化的 state 是非常困难的:**

状态之间相互会存在依赖,一个状态的变化会引起另一个状态的变化,View 页面也有可能会引起状态的变化;

当应用程序复杂时,state 在什么时候,因为什么原因而发生了变化,发生了怎样的变化,会变得非常难以控制和追踪;

**React 是在试图层帮助我们解决了 DOM 的渲染过程,但是 State 依然是留给我们自己来管理:**

无论是组件定义自己的 state,还是组件之间的通信通过 props 进行传递;也包括通过 Context 进行数据之间的共享;

React 主要负责帮助我们管理试图,state 如何维护最终还是我们自己来决定;

Redux 就是一个帮助我们管理 State 的容器:Redux 是 JavaScript 的状态容器,提供了可预测的状态管理;

Redux 除了和 React 一起使用之外,它也可以和其他界面库一起来使用(比如 Vue),并且它非常小(包括依赖在内,只有 2kb)

## Redux 的核心理念 - State

**Redux 的核心理念非常简单。**

**比如我们有一个朋友列表需要管理:**

比如我们没有定义统一的规范来操作这段数据,那么整个数据的变化就是无法跟踪的;

比如页面的某处通过 friends.push 的方式增加了一条数据;

比如另一个页面通过 friends[0].age = 25 修改了一条数据;

**整个应用程序错综复杂,当出现 bug 时,很难跟踪到底哪里发生的变化;**

```javascript
const initialState = {
  friends: [
    { name: "coderzzx", age: 18 },
    { name: "lilei", age: 18 },
    { name: "hanmeimei", age: 18 },
  ],
};
```

## Redux 的核心理念 - action

**Redux 要求我们通过 action 来更新数据:**

所有数据的变化,必须通过派发(dispatch)action 来更新;

action 是一个普通的 JavaScript 对象,用来描述这次更新的 type 和 content;

**比如下面就是几个更新 friends 的 action:**

强制使用 action 的好处是可以清晰的知道数据到底发生了什么样的变化,所有的数据变化都是可跟踪,可预测的;

```javascript
const action1 = { type: "Add_FRIEND", info: { name: "lucy", age: 20 } };
const action2 = { type: "INC_AGE", index: 0 };
const action3 = {
  type: "CHANGE_NAME",
  playload: { index: 0, newName: "kobe" },
};
```

## Redux 的核心理念 - reducer

**但是如何将 state 和 action 联系在一起呢?答案是 reducer**

reducer 是一个纯函数;

reducer 做的事情就是将传入的 state 和 action 结合起来生成一个新的 state;

```javascript
// store/index.js
const { createStore } = require("redux");
const initialState = {
  name: "coderzzx",
  count: 100,
};

// 定义reducer函数:纯函数
/**
 * 参数一:store中目前保存的state
 * 参数二:本次需要更新的action(dispatch传入的action)
 * 返回值:它的返回值会作为store之后存储的state
 */
function reducer(state = initialState, action) {
  // if写法
  // if (action.type === "change_name") {
  //   return { ...state, name: action.name };
  // } else if (action.type === "add_number") {
  //   return { ...state, count: state.count + action.count };
  // }
  // return state;
  // switch写法
  switch (action.type) {
    case "change_name":
      return { ...state, name: action.name };
    case "add_number":
      return { ...state, count: state.count + action.count };
    default:
      return state;
  }
}

const store = createStore(reducer);
```

```javascript
// 使用store中的数据
const sotre = require("./store");

console.log(store.getState()); // {name:'coderzzx',count:100}
```

```javascript
// 修改store中的数据
const sotre = require("./store");

const nameAction = { type: "change_name", name: "kobe" };
store.dispatch(nameAction);
console.log(store.getState()); // {name:'kobe',count:100}

// 修改count
const countAction = { type: "add_number", count: 10 };
store.dispatch(countAction);
console.log(store.getState()); // {name:'kobe',count:110}
```

```javascript
// 订阅store中的数据
const sotre = require("./store");

store.subscribe(() => {
  console.log("订阅数据的变化:", store.getState());
  // {name:'kobe',count: 100}
  // {name:'kobe',count: 110}
});

store.dispatch({ type: "change_name", name: "kobe" });

// 修改count
store.dispatch({ type: "add_number", count: 10 });
```

```javascript
// 动态生成action
const sotre = require("./store");

store.subscribe(() => {
  console.log("订阅数据的变化:", store.getState());
  // {name:'kobe',count: 100}
  // {name:'kobe',count: 110}
});

// actionCreators:帮助我们创建action
// 实际情况不应该写在此处，需要放在单独文件中，导入后进行使用
const changeNameAction = (name) => ({
  type: "change_name",
  name,
});

store.dispatch(changeNameAction("kobe"));

const AddNumberAction = (count) => ({
  type: "add_number",
  count,
});
store.dispatch(AddNumberAction(10));
```

```javascript
// 最终写法
// index.js
const { createStore } = require("redux");
const reducer = require("./reducer.js");
const store = createStore(reducer);

module.exports = store;

// reducer.js
const { CHANGE_NAME, ADD_NUMBER } = require("./constants.js");
const initialState = {
  name: "coderzzx",
  count: 100,
};

function reducer(state = initialState, action) {
  switch (action.type) {
    case CHANGE_NAME:
      return { ...state, name: action.name };
    case ADD_NUMBER:
      return { ...state, count: state.count + action.count };
    default:
      return state;
  }
}

module.exports = reducer;

// constants.js
const CHANGE_NAME = "change_name";
const ADD_NUMBER = "add_number";

module.exports = {
  CHANGE_NAME,
  ADD_NUMBER,
};

// actionCreators.js
const { CHANGE_NAME, ADD_NUMBER } = require("./constants.js");
const changeNameAction = (name) => ({
  type: CHANGE_NAME,
  name,
});
const AddNumberAction = (count) => ({
  type: ADD_NUMBER,
  count,
});

module.exports = {
  changeNameAction,
  AddNumberAction,
};

// test.js 使用页面
const store = require("./index.js");
const { changeNameAction, AddNumberAction } = require("./actionCreators.js");

store.dispatch(changeNameAction("hanmeimei"));
store.dispatch(AddNumberAction(200));
```

## Redux 的三大原则

**单一数据源**

整个应用程序的 state 被存储在一颗 object tree 中,并且这个 object tree 只存储在一个 store 中;

Redux 并没有强制让我们不能创建多个 Store,但是那样做并不利于数据的维护;

单一的数据源可以让整个应用程序的 state 变得方便维护,追踪,修改;

**State 是只读的**

唯一修改 State 的方法一定是触发 action,不要试图在其他地方通过任何的方式来修改 State;

这样就确保了 View 或网络请求都不能直接修改 state,它们只能通过 action 来描述自己想要如何修改 state;

这样可以保证所有的修改都被集中化处理,并且按照严格的顺序来执行,所以不需要担心 race condition(竟态)的问题;

**使用纯函数来执行修改**

通过 reducer 将 state 和 action 联系在一起,并且返回一个新的 state;

随着应用程序的复杂度增加,我们可以将 reducer 拆分成多个小的 reducers,分别操作不同的 state tree 的一部分;

但是所有的 reducer 都应该是纯函数,不能产生任何副作用;

## redux 融入 react 代码
