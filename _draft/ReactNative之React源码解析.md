<!--# ReactNative之React源码解析-->

# 为什么（Why）

有的人可能会不理解，大前端跨平台战火为谁而燃，吾辈何以为战？

专注于移动互联网大前端致富，一直是我们最崇高的理想，为了完成大前端转型，ReactNative（以下简称 RN）首当其冲。

纵观行业风向，有磨刀霍霍者，有作壁上观者，有从入门到放弃者，有一把梭者，但缺乏深潜微操者。

哈，是时候该我出手了。

很多同学在纠结到底哪家强，耽误了学习，其实这好比考清华还是考北大，Top2高校有那么难选么，真正难选的是Top3高校。

跨平台技术最终王者也许花落 Flutter 或者小程序，但这不重要。举一反三，王者无界。如何从零将RN技能练到和高阶 Android 一样强，并且同样适用于进击 Flutter 和小程序，这才是我的最终幻想。

现在市面上高水准的 RN 解析文章太少了（老外写的硬核文章居多），而且大多停留在理论层面，只给出算法理论和源码片段，根本无法深入实操，看完后不明觉厉，然后默默地收藏。

那么，自力更生啃源码吧。

# 是什么（What）

让我们来看看官方文档的定义。

[**React**](https://www.reactjscn.com/)：用于构建用户界面的 JavaScript 库。

[**React Native**](https://reactnative.cn/)：使用JavaScript和React编写原生移动应用。

![](https://pic1.zhimg.com/80/v2-152a1797b7ee8c4f0f04b1d81c7eb81c_hd.jpg)

[**Component**](https://www.reactjscn.com/docs/react-api.html#components)：React 组件可以让你把UI分割为独立、可复用的片段，并将每一片段视为相互独立的部分。组件从概念上看就像是函数，它可以接收任意的输入值props，并返回一个需要在页面上展示的React元素。

```
// 普通组件（继承React.Component的类组件）
class App extends React.Component {
    render() {
        return <Text style={{color: 'black'}}>{'点击数0'}</Text>;
    }
}

// 纯组件（继承React.PureComponent的类组件）
class App extends React.PureComponent {
    render() {
        return <Text style={{color: 'black'}}>{'点击数0'}</Text>;
    }
}

// 函数式组件（直接返回Element的函数）
const App = function () {
    return <Text style={{color: 'black'}}>{'点击数0'}</Text>;
}
```

[*JSX*](https://www.reactjscn.com/docs/introducing-jsx.html)：类似`<h1>Hello, world!</h1>`这种是看起来可能有些奇怪的标签语法既不是字符串也不是 HTML。它被称为 JSX， 一种 JavaScript 的语法扩展。JSX 用来声明 React 当中的元素。Babel 转译器会把 JSX 转换成一个名为 [React.createElement()](https://www.reactjscn.com/docs/react-api.html#createelement) 的方法调用。

```
// 上述JSX写法的普通组件转换如下
class App extends React.Component {
	render() {
		// Babel转换JSX后
		return React.createElement(
			// 类型type
			{$$typeof: Symbol(react.forward_ref), displayName: "Text", propTypes: {…}, render: ƒ},
			// 属性props
			{style: {color: "black"}, __source: {…}},
			// 子节点children
			"点击数0"
		);
	}
}
```

**Instance**：组件实例，组件类实例化的结果，ref指向组件实例（函数式组件不能实例化）。在生成Fiber节点时会调用new Component()创建。

```
// App
{
	forceUpdate: ƒ (),
	isReactComponent: ƒ (),
	setState: ƒ (),
	componentDidMount: ƒ (),
	componentWillUnmount: ƒ (),
	constructor: ƒ App(props),
	isMounted: (...),
	render: ƒ (),
	replaceState: (...),
	__proto__: Component
}
```

[**Element**](https://www.reactjscn.com/docs/rendering-elements.html)：元素是构成 React 应用的最小单位，用来描述界面上的任何东西。

```
// App
{
	// React Element唯一标识
	$$typeof: Symbol(react.element),
	// 开发者指定唯一标识，用于复用
	key: null,
	// 属性
	props: {rootTag: 241},
	// 引用
	ref: null,
	// 类型
	type: ƒ App(props),
	_owner: null,
	_store: {validated: true},
	_self: null,
	_source: {fileName: "/Users/shengshuqiang/dream/AdvanceOnReactNative/Aw…native/Libraries/ReactNative/renderApplication.js", lineNumber: 38},
	__proto__: Object
}
// Text
{
	$$typeof: Symbol(react.element),
	key: null,
	props: {style: {color: "black"}, children: "点击数0"},
	ref: null,
	type: {$$typeof: Symbol(react.forward_ref), displayName: "Text", propTypes: {…}, render: ƒ},
	_owner: FiberNode {id: 11, tag: 1, key: null, elementType: ƒ, type: ƒ, …},
	_store: {validated: false},
	_self: null,
	_source: {fileName: "/Users/shengshuqiang/dream/AdvanceOnReactNative/AwesomeProject/App.js", lineNumber: 213},
	__proto__: Object
}
```

[**虚拟DOM**](https://www.reactjscn.com/docs/faq-internals.html#%E4%BB%80%E4%B9%88%E6%98%AF%E8%99%9A%E6%8B%9Fdom%EF%BC%88virtual-dom%EF%BC%89)：Virtual DOM，简写VDOM。是一种编程概念，是指虚拟的视图被保存在内存中，并通过诸如ReactDOM这样的库与“真实”的DOM保持同步。

[**ReactFiber**](https://www.reactjscn.com/docs/faq-internals.html#%E4%BB%80%E4%B9%88%E6%98%AFreact-fiber%EF%BC%9F)

[React Fiber Architecture (原文)](https://github.com/xxn520/react-fiber-architecture-cn)

![](https://learnreact.design/static/06-middleman-cecc053c8ade6d7012817193ce68b690-c86e7.png)
![](https://learnreact.design/static/rn-2e243f87a6a3bcfc143739ba8c6f90db-9160a.png)
![](https://learnreact.design/static/1-react-summary-28be1df2fed9962a09c159ded7e14881-d47ca.png)

[What Is React?](https://learnreact.design/2017/06/08/what-is-react)
[What Is React Native?](https://learnreact.design/2017/06/20/what-is-react-native/)

![](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/DOM-model.svg/1920px-DOM-model.svg.png)
[文档对象模型](https://zh.wikipedia.org/wiki/%E6%96%87%E6%A1%A3%E5%AF%B9%E8%B1%A1%E6%A8%A1%E5%9E%8B)

![XML DOM 教程](https://www.w3school.com.cn/i/ct_nodetree1.gif)

[XML DOM 教程](https://www.w3school.com.cn/xmldom/index.asp)

[网上都说操作真实 DOM 慢，但测试结果却比 React 更快，为什么？](https://www.zhihu.com/question/31809713)

[React Virtual DOM、Ember Glimmer和Incremental DOM技术哪家强](https://www.w3ctech.com/topic/1609)
[DOM概述](https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model/Introduction)

![](https://www.zendevs.xyz/wp-content/uploads/2019/03/dom-html-2-render.png)

[理解DOM到底是什么](https://juejin.im/post/5c01e2b051882518eb1f785a)

![](http://storage.360buyimg.com/mtd/home/sdsdsd1514185541265.jpg)
[virtual-dom](https://nervjs.github.io/docs/guides/virtual-dom.html)

## 当我们在谈论RN原理时，到底在说什么



Fiber（纤维）算法：是比线程控制更精密的并发处理机制。支持更新过程碎片化，化整为零，允许紧急任务插队，可中断恢复。为解决一路狂奔式地更新，无暇处理用户响应，引发界面咔咔咔。

## 概念

# 怎么做（How）

# 价值

## [不可变数据的力量](https://zh-hans.reactjs.org/docs/optimizing-performance.html)


# 参考

1. [React](https://zh-hans.reactjs.org/)
2. [ReactNative](https://reactnative.cn/)
2. [Virtual DOM 的实现和 React Fiber 简介](https://www.jianshu.com/p/b189b2949b33)
2. [](https://react.docschina.org/docs/reconciliation.html)
2. [GETTING STARTED WITH REACT](https://ryanclark.me/getting-started-with-react/)
3. [diff之React:Virtual DOM](https://www.jianshu.com/p/278fcd3e9301)
2. [颠覆式前端 UI 开发框架：React](https://www.infoq.cn/article/subversion-front-end-ui-development-framework-react/)
3. [深入浅出 React（一）：React 的设计哲学 - 简单之美](https://www.infoq.cn/article/react-art-of-simplity/?utm_source=tuicool)
4. [深入浅出 React（二）：React 开发神器 Webpack](https://www.infoq.cn/article/react-and-webpack/)
4. [深入浅出 React（三）：理解 JSX 和组件](https://www.infoq.cn/article/react-jsx-and-component/)
3. [深入浅出 React（四）：虚拟 DOM Diff 算法解析](https://www.infoq.cn/article/react-dom-diff/)
4. [图解 React Virtual DOM](https://segmentfault.com/a/1190000010924023)
5. [深入理解react中的虚拟DOM、diff算法](https://www.cnblogs.com/zhuzhenwei918/p/7271305.html)

