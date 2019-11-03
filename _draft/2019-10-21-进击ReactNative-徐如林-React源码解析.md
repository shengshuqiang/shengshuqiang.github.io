---
layout: post
title: 进击ReactNative-徐如林-React源码解析
key: 20191021
tags:
  - 蜻蜓切
  - ReactNative
  - 源码解析
---
<!-- 添加目录 http://blog.csdn.net/hengwei_vc/article/details/47122103 -->
<script src="/javascripts/jquery-2.1.4.min.js" type="text/javascript"></script>
<script src="/javascripts/toc.js" type="text/javascript"></script>
<script type="text/javascript">
$(document).ready(function() {
    $('#toc').toc();
}); </script>
<div id="toc"></div>

<img style="border-radius: 15px;box-shadow: darkgrey 5px 5px 10px 5px" src="http://img.mp.itc.cn/upload/20170718/89520d891b0441a885f129366a70d190_th.jpg"/>


![进击ReactNative疾如风](https://shengshuqiang.github.io/assets/%E5%BE%90%E5%A6%82%E6%9E%97logo.png)<br>有的人可能会不理解，大前端平台化的战火为谁而燃，吾辈何以为战？<br>专注于移动互联网大前端致富，一直是我们最崇高的理想，而ReactNative是一个碉堡。<br>纵观行业风向，有作壁上观者，有磨刀霍霍者，有入门到放弃者，有大刀阔斧者，但是缺乏深潜微操者。<br>哈，是时候该我出手了。<br>祭出“**大海航术**”，经过一年来不懈钻研，基于React Developer Tools**研发插件**，实时绘制运行时三张图--**Fiber双树图**、**Native View树图**、**React方法调用树图**，在上帝视角和时间旅行的引领下，冲破波诡云谲的算法迷航，日照大海现双龙。
{:.success}
<!--more-->

// TODO 演进图

本篇文章主要针对React源码分析，先说清楚开发者接触到的API，然后再深挖对应底层实现逻辑。如果有对ReactNative不太熟悉的朋友，建议看一下[《进击ReactNative-疾如风》](https://shengshuqiang.github.io/2019/01/07/%E8%BF%9B%E5%87%BBReactNative-%E7%96%BE%E5%A6%82%E9%A3%8E.html)热热身，该文从“原理+实践，现学现做”的角度手写石器时代ReactNative，粗线条描述跨平台套路，迂回包抄，相对比较轻松！本文则正面刚React源码，略显烧脑。

话说，做大事，就要用大斧头。先用[阿里“三板斧”](https://baijiahao.baidu.com/s?id=1609462546639223406&wfr=spider&for=pc)撼动一下。

# 定目标

## 传道（攻坚方法论）

近几年的移动互联网北漂生涯，给我结结实实的上了一课：人生，就是不断探索、抽象、践行、强化自己的方法论，过程呈螺旋式上升，成长快乐的秘诀在于**复盘**。

我攻坚ReactNative的原动力，就是借假修真，跨平台技术最终王者也许花落Flutter或者小程序（还有很多人在纠结到底哪家强，耽误了学习，其实这好比考清华还是考北大，Top2高校有那么难选么，真正难选的是Top3高校），但这不重要，我能举一，必能反三，这就是道。我旨在强化出一套无界王者的方法论，如何从零将ReactNative技能练到比肩高阶Android的熟练度，并且同样适用于进击Flutter和小程序。

## 授业（懂算法）

现在市面上高水准解析ReactNative文章太少（老外写的硬核文章居多），而且大多停留在理论层面，只给出源代码片段，根本无法深入实操，只能作者说啥就是啥，反正不明觉厉。也罢，唯一的出路只有自力更生啃源码了。

我一直坚信，只有源码才是唯一的真相，不二的注释，思想的火花，王者的农药。后来，终于在眼泪中明白，源码大法好啊，得到的比想要的多得多（贫穷限制了我的想象）。往小的说，技术成长。往大的说，核心竞争力。

本文和你分享的是如何通过**先进生产力**相对轻松地看懂代码，区别于呆板的流水式英文阅读，尽量做到：

1. **承上**（用户态--上层API怎么用）
	* 组件中方法（constructor、setState、forceUpdate、render）的作用是什么？
	* 生命周期调用时机是什么？
	* PureComponent比Component好在哪里，怎么能做得更好？
1. **启下**（内核态--底层原理怎么玩）
	* 各种概念的含义，对应数据结构是什么？
	* 深入浅出Fiber双树算法？
	* Diff算法在哪？
	* Native操作指令从哪来？

## 解惑（考考你）

聪明的童靴往往都会有一些亟需亲自操刀的疑问，我也不能免俗。问题来了，现在就差一个满意的答案。

### 组件

1. 明明只写了几个组件，通过React Developer Tools看到的却是一堆布局，而且还有Context.Consumer，这些都是干啥的？
2. React组件和Native View看起来不是一一对应的，那么映射关系是什么？
3. [组件普通API](https://reactjs.org/docs/react-component.html#other-apis)调用时机、作用和最佳实践？

{% highlight javascript linenos %}
// 组件类
class Component<P, S> {
	// 变量
	props;
	state;
	// 方法
	constructor(props, context);
	setState(state, callback): void;
	forceUpdate(callBack): void;
	render(): ReactNode;
}

{% endhighlight %}

### 生命周期

1. 区分哪些方法只会调用一次，哪些可能会调用多次？哪些方法中能使用setState，哪些不能？
1. 区分每个方法调用条件，是props改变还是state，是初始化，更新还是都有？
1. React16.3开始废弃和新增的方法是哪些，补位策略是什么？废弃方法现在还能不能用，新旧方法混用又怎样？
2. [组件生命周期API](https://reactjs.org/docs/react-component.html#the-component-lifecycle)调用时机、作用和最佳实践？

{% highlight javascript linenos %}
// 静态生命周期
interface StaticLifecycle {
    getDerivedStateFromProps?: GetDerivedStateFromProps;
}
// 新生命周期
interface NewLifecycle<P, S, SS> {
    getSnapshotBeforeUpdate?(prevProps, prevState): SS | null;
    componentDidUpdate?(prevProps, prevState, snapshot): void;
}
// 废弃生命周期
interface DeprecatedLifecycle<P, S> {
    componentWillMount?(): void;
    UNSAFE_componentWillMount?(): void;
    componentWillReceiveProps?(nextProps, nextContext): void;
    UNSAFE_componentWillReceiveProps?(nextProps, nextContext): void;
    componentWillUpdate?(nextProps, nextState, nextContext): void;
    UNSAFE_componentWillUpdate?(nextProps, nextState, nextContext): void;
}
// 组件生命周期（继承新和废弃生命周期）
interface ComponentLifecycle<P, S, SS> extends NewLifecycle<P, S, SS>, DeprecatedLifecycle<P, S> {
    componentDidMount?(): void;
    shouldComponentUpdate?(nextProps, nextState, nextContext): boolean;
    componentWillUnmount?(): void;
    componentDidCatch?(error, errorInfo): void;
}
{% endhighlight %}


### 数据结构

2. 区分Element、Instance、DOM、Component、Fiber的不同含义以及之间关系？
3. Fiber节点数据结构中各属性含义？

### Virtual DOM

1. Virtual DOM遇到了哪些假问题，又解决了哪些真问题？
2. React有棵DOM树，树在哪，怎么看，怎么操作对应Native View树？

### Diff算法

2. Diff算法的策略是什么，能得出哪些最佳实践？
3. 都说React有个Diff算法（Tree Diff 分层求异；Component Diff 同类同树，异类异树；Element Diff 增删移复用key），代码在哪里，怎么比较的？

### 原理

4. React高效在哪？
7. React工作流程？
8. 如何关联Native自定义组件？
9. Fiber双树是干啥的？

<img style="border-radius: 10px;box-shadow: darkgrey 0px 0px 10px 5px" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1571497262025&di=4ae4817071de66ff8d666ece3b484ece&imgtype=jpg&src=http%3A%2F%2Fimg0.imgtn.bdimg.com%2Fit%2Fu%3D3424028830%2C393276537%26fm%3D214%26gp%3D0.jpg"/>


# 追过程

## 学习

我们不是一个人在战斗，有必要站在巨人的肩膀上，集思广益，事半功倍。网上一顿关键字搜索，给点耐心，妥妥滴数十篇深度文章以上，你的感觉才能上来。这里给大家安利我的[博客主页](https://shengshuqiang.github.io/)和[微信朋友圈](https://shengshuqiang.github.io/about.html)，我会阶段性将看到的ReactNative优秀文章汇总起来。发盆友圈，我是认真的，停是不可能停下来的，天天上班天天发财。

本着[”**坚定看多，数量堆死力量**“](https://new.qq.com/omn/20191006/20191006A0FQJK00.html)，经过不间断的阅读输出，自己的方法论姿势见涨，比方说通过XMind自由缩放源码地图帮助理解、手写ReactNative寻求理论加实践、抽象伪代码表述助力说清楚等。

**Fiber架构里程碑**

**Why：**一路狂奔式地更新，无暇处理用户响应，引发界面咔咔咔。

**What：**Fiber，纤维，是比线程控制得更精密的并发处理机制。更新过程碎片化，化整为零，允许紧急任务插队，可中断恢复。本质上，它还是一个工具，用来帮助开发者操纵 DOM ，从而构建出页面。

**How Much：**纵享丝滑。

**硬核资料：**Lin Clark在2017 React大会的演讲[Lin Clark - A Cartoon Intro to Fiber - React Conf 2017](https://www.bilibili.com/video/av40427580/)。这个内容太棒啦，墙裂建议大家看一看。网上大部分Fiber算法分析都引用了她的卡通图。

**术语**

[***Component***](https://zh-hans.reactjs.org/docs/glossary.html#components)：组件，是可复用的小的代码片段，它们返回要在页面中渲染的 React 元素。分为[类组件](https://reactjs.org/docs/react-api.html#components)（继承[Component](https://reactjs.org/docs/react-api.html#reactcomponent)的普通组件和继承[PureComponent](https://reactjs.org/docs/react-api.html#reactpurecomponent)的纯组件）和函数式组件（返回Element的函数）。

```
// 普通组件
class App extends React.Component {
    render() {
        return <Text style={{color: 'black'}}>{'点击数0'}</Text>;
    }
}
```
```
// 纯组件
class App extends React.PureComponent {
    render() {
        return <Text style={{color: 'black'}}>{'点击数0'}</Text>;
    }
}
```
```
// 函数式组件
const App = function () {
    return <Text style={{color: 'black'}}>{'点击数0'}</Text>;
}
```

[*JSX*](https://zh-hans.reactjs.org/docs/glossary.html#jsx)：是类Html标签式写法转化为纯对象Element函数调用式写法的语法糖。Babel 会把 JSX 转译成一个名为 [React.createElement](https://reactjs.org/docs/react-api.html#createelement) 函数调用.

```
React.createElement(
	// 类型type
	{$$typeof: Symbol(react.forward_ref), displayName: "Text", propTypes: {…}, render: ƒ},
	// 属性props
	{style: {color: "black"}, __source: {…}},
	// 子节点children
	"点击数0"
);
```
***Instance***：组件实例，组件类实例化的结果，ref指向组件实例（函数式组件不能实例化）。在生成Fiber节点时会调用new Component()创建。

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
[***Element***](https://zh-hans.reactjs.org/docs/glossary.html#elements)：元素，描述了你在屏幕上想看到的内容。是DOM节点的一种纯对象描述，即虚拟DOM，对应组件render方法主要返回值。详见[React.createElement](https://github.com/shengshuqiang/AdvanceOnReactNative/blob/master/AwesomeProject/node_modules/react/cjs/react.development.js#L753)。

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

***FiberNode***：碎片化更新中可操作的细粒度节点，用于存储中间态计算结果，为“**可紧急插队、可中断恢复**”的页面刷新提供技术支持。详见[ReactNative.createFiberFromElement](https://github.com/shengshuqiang/AdvanceOnReactNative/blob/master/AwesomeProject/node_modules/react-native/Libraries/Renderer/oss/ReactNativeRenderer-dev.js#L6021)。

```
// FiberNode
{
	actualDuration: 175.7499999985157,
	actualStartTime: 9793.884999999136,
	// 候补树，在调用render或setState后，会克隆出一个镜像fiber，diff产生出的变化会标记在镜像fiber上。而alternate就是链接当前fiber tree和镜像fiber tree, 用于断点恢复
	alternate: null,
	// 第一个子节点
	child: FiberNode {id: 12, tag: 11, key: null, elementType: {…}, type: {…}, …},
	// TODO
	childExpirationTime: 0,
	contextDependencies: null,
	// 副作用，增删改操作。Placement=2;Update=4;PlacementAndUpdate=6;Deletion=8;
	effectTag: 5,
	// 描述了它对应的组件。对于复合组件，类型是函数或类组件本身。对于宿主组件（div，span等），类型是字符串。定义此 Fiber 节点的函数或类。对于类组件，它指向构造函数，对于 DOM 元素，它指定 HTML 标记。我经常使用这个字段来理解 Fiber 节点与哪个元素相关。
	// ClassComponent对应为函数，如APPContainer()。ForwardRef、ContextConsumer、ContextProvider对应为对象，如{$$typeof: Symbol(react.forward_ref), render: ƒ, displayName: "View"}。HostComponent对应为字符串，如“RCTView”。HostText对应为null。
	elementType: ƒ App(props),
	// TODO
	expirationTime: 0,
	// 用来保存中断前后 effect 的状态，用户中断后恢复之前的操作。这个意思还是很迷糊的，因为 Fiber 使用了可中断的架构
	firstEffect: FiberNode {id: 13, tag: 1, key: null, elementType: ƒ, type: ƒ, …},
	// 我添加的Fiber节点唯一标识（采用id自增生成），用于生成Fiber双树
	id: 11,
	index: 0,
	// 复用标识
	key: null,
	// 参考firstEffect
	lastEffect: FiberNode {id: 13, tag: 1, key: null, elementType: ƒ, type: ƒ, …},
	// 在前一个渲染中用于创建输出的 Fiber 的 props
	memoizedProps: {rootTag: 191},
	// 用于创建输出的 Fiber 状态。处理更新时，它会反映当前在屏幕上呈现的状态
	memoizedState: null,
	mode: 4,
	// workInProgress tree上每个节点都有一个effect list，用来存放需要更新的内容。此节点更新完毕会向子节点或邻近节点合并 effect list
	nextEffect: FiberNode {id: 10, tag: 5, key: null, elementType: "RCTView", type: "RCTView", …},
	// props是函数的参数。一个 fiber 的pendingProps在执行开始时设置，并在结束时设置memoizedProps。已从 React 元素中的新数据更新并且需要应用于子组件或 DOM 元素的 props
	pendingProps: {rootTag: 191},
	ref: null,
	// 父节点
	return: FiberNode {id: 10, tag: 5, key: null, elementType: "RCTView", type: "RCTView", …},
	selfBaseDuration: 28.63000000070315,
	// 兄弟节点
	sibling: null,
	// 保存组件的类实例、DOM 节点或与 Fiber 节点关联的其他 React 元素类型的引用。总的来说，我们可以认为该属性用于保持与一个 Fiber 节点相关联的局部状态。(HostRoot对应{containerInfo}；ClassComponent对应为new的函数对象实例；HostComponent对应为ReactNativeFiberHostComponent，包含_children和_nativeTag；HostText对应为nativeTag）
	stateNode: hookClazz {props: {…}, context: {…}, refs: {…}, updater: {…}, _reactInternalFiber: FiberNode, …},
	// 它在协调算法中用于确定需要完成的工作。如前所述，工作取决于React元素的类型
	tag: 1,
	treeBaseDuration: 155.36499999871012,
	// 同elementType
	type: ƒ App(props),
	// state更新队列。状态更新、回调和 DOM 更新的队列
	updateQueue: null,
	_debugID: 12,
	_debugIsCurrentlyTiming: false,
	_debugOwner: null,
	_debugSource: {fileName: "/Users/shengshuqiang/dream/AdvanceOnReactNative/Aw…native/Libraries/ReactNative/renderApplication.js", lineNumber: 38},
	__proto__: Object
}
```

***DOM***：文档对象模型（Document Object Model），简单说就是界面控件树（对应Html是DOM树，对应Native是View树）的节点。

```
UIManager.createView	[3,"RCTRawText",1,{"text":"点击数0"}]
UIManager.createView	[5,"RCTText",1,{"ellipsizeMode":"tail","allowFontScaling":true,"accessible":true,"color":-16777216}]
UIManager.setChildren	[5,[3]]
UIManager.createView	[7,"RCTView",1,{"flex":1,"pointerEvents":"box-none","collapsable":true}]
UIManager.setChildren	[7,[5]]
UIManager.createView	[9,"RCTView",1,{"pointerEvents":"box-none","flex":1}]
UIManager.setChildren	[9,[7]]
UIManager.setChildren	[1,[9]]
```

[<img style="border-radius: 10px;box-shadow: darkgrey 0px 0px 10px 5px" src="https://shengshuqiang.github.io/assets/Component-Instance-Element-FiberNode.svg"/>](https://shengshuqiang.github.io/assets/Component-Instance-Element-FiberNode.svg)

## 运行（Playground）

搭一个自己的专属实验室--本地可运行环境（开发平台macOS，目标平台Android）。

1. 安装软件：Webstorm（前端开发环境）、AndroidStudio（Android开发环境，送Android模拟器）。
2. 安装依赖：安装XCode（iOS开发环境，送iPhone模拟器）就顺带解决了。
2. 使用 React Native 命令行工具来创建一个名为"AwesomeProject"的新项目：`react-native init AwesomeProject`。
3. 欧了，[简单Demo](https://github.com/shengshuqiang/AdvanceOnReactNative/blob/master/AwesomeProject/App.js)(页面一个红色按钮，初始显示点击数n，点击切换为“汽车”图标)测试一下。该Demo主要用于观察初始渲染和用户点击渲染。<br><img style="border-radius: 10px;box-shadow: darkgrey 0px 0px 10px 5px;padding: 3px" src="https://km.meituan.net/210298964.gif?contentType=1&contentId=211293023&attachmentId=211323365&originUrl=https://km.meituan.net/210298964.gif&token=eAHjYBRYt4xZYeu5RZ8f6xpJJefn6hUn5mWXJmbqZZbopSam6CVnliSmpOZYKRgaGacYmiUmJltaGJkkW5haWqaaaCWZmhgbpaWYmxomOVkorLmype-5rgaTEUHFFkBbHVg8bi-4cPaRbpRCcpKxkaF5ipmJqZahiUGqAdASc8tUC5PENAMTgxTDJAAVXzT2**eAEVyMkRwDAIBLCWzHKZcsBA_yVkoqfoSTq1McojcRbPLQMqZ_8jcO1Oj01JdvHG3JFtvQZW_QA8NhHl&template=0&isDownload=false&isNewContent=true"/>
5. 更多配置详见[React Native 中文网-搭建开发环境](https://reactnative.cn/docs/getting-started.html)。

## 源码

我们来读源码（16.8.3 react，0.59.8 react-native）吧！

* ReactNative上层JS代码主要实现在[ReactNativeRenderer-dev.js](https://github.com/shengshuqiang/AdvanceOnReactNative/blob/master/AwesomeProject/node_modules/react-native/Libraries/Renderer/oss/ReactNativeRenderer-dev.js)这一个文件，代码行数21194（区区2W，好像压力也没辣么大）。
* [react.development.js](https://github.com/shengshuqiang/AdvanceOnReactNative/blob/master/AwesomeProject/node_modules/react/cjs/react.development.js)：纯JS侧React相关定义和简单实现。
* [react.d.ts](https://github.com/shengshuqiang/AdvanceOnReactNative/blob/master/AwesomeProject/react.d.ts)：接口定义，详见本地目录/Applications/WebStorm.app/Contents/plugins/JavaScriptLanguage/jsLanguageServicesImpl/external/react.d.ts。

```
#  源码目录/Users/shengshuqiang/dream/AdvanceOnReactNative/AwesomeProject/node_modules
.
├── react
│   └── cjs
│       └── react.development.js # 纯JS侧React相关定义和简单实现
└── react-native
    ├── LICENSE
    └── Libraries
        ├── Components # 官方提供的各种组件，如View、ScrollView、Touchable等
        └── Renderer
            └── oss
                ├── GreateNavigationArt.js # “大海航术”核心实现，主要hook调用，打印调用栈日志和dump Fiber双树信息，约600行
                └── ReactNativeRenderer-dev.js # ReactNative上层JS代码核心实现，约2W行

```

## 迷航

我给自己的读码方法论命名为“**海航术**”，是通过运行时日志分析为辅，断点调试分析为主，匹配自己野兽般的想象力（悟），努力做到自圆其说，能唬住不懂的人（包括我自己），假装懂了的术（套路）。

对付简单的算法，这招基本够用，否则我也混不下去了。但是，Fiber算法，忒难了。第一个回合硬着头皮看下来，只知道一堆乱七八糟的调用，混杂着各种光怪陆离的Fiber属性，而且用到了复杂的双树数据结构。这些，小本子根本记不过来。来张我的笔记感受一下（不用细看，我也没打算讲这张图），一波操作下来，差不多要2天闭关专注的投入，要是被打断了，都找不到北。

[<img style="border-radius: 10px;box-shadow: darkgrey 0px 0px 10px 5px" src="https://km.meituan.net/212321734.png?contentType=1&contentId=207390791&attachmentId=212321735&originUrl=https://km.meituan.net/212321734.png&token=eAHjYBRYt4xZYeu5RZ8f6xpJJefn6hUn5mWXJmbqZZbopSam6CVnliSmpOZYKRgaGacYmiUmJltaGJkkW5haWqaaaCWZmhgbpaWYmxomOVkorLmype-5rgaTEUHFFkBbHVg8bi-4cPaRbpRCcpKxkaF5ipmJqZahiUGqAdASc8tUC5PENAMTgxTDJAAVXzT2**eAEVyMkRwDAIBLCWzHKZcsBA_yVkoqfoSTq1McojcRbPLQMqZ_8jcO1Oj01JdvHG3JFtvQZW_QA8NhHl&template=0&isDownload=false&isNewContent=false"/>](https://shengshuqiang.github.io/assets/深入ReactNative.xmind)

按这个套路，**连**日志**加**调试**带**瞎猜，发现装不下去了，我太难了。一度跌入绝望之谷，挣扎着把源码看了三遍（毕竟指望这一波发财），仍然没什么收获，等着顿悟吧。

## 微光

直到那一天，我终于等到了这个变数--如果能可视化Fiber双树在运行时的状态变化，是否有望突破React技术壁垒？

脑子再活一点的我就想：“可不可以写个脚本把Fiber双树画出来”，随后的问题就是“能不能写个插件实时绘制运行时Fiber双树”，进一步“绘制实时方法调用树（看着有点像抽象语法树），有问题吗？”能有啥问题，没问题，那就干。

[<img style="border-radius: 10px;box-shadow: darkgrey 0px 0px 10px 5px" src="https://km.meituan.net/210460288.png?contentType=1&contentId=211293023&attachmentId=211323368&originUrl=https://km.meituan.net/210460288.png&token=eAHjYBRYt4xZYeu5RZ8f6xpJJefn6hUn5mWXJmbqZZbopSam6CVnliSmpOZYKRgaGacYmiUmJltaGJkkW5haWqaaaCWZmhgbpaWYmxomOVkorLmype-5rgaTEUHFFkBbHVg8bi-4cPaRbpRCcpKxkaF5ipmJqZahiUGqAdASc8tUC5PENAMTgxTDJAAVXzT2**eAEVyMkRwDAIBLCWzHKZcsBA_yVkoqfoSTq1McojcRbPLQMqZ_8jcO1Oj01JdvHG3JFtvQZW_QA8NhHl&template=0&isDownload=false&isNewContent=true"/>](https://shengshuqiang.github.io/assets/DrawFiber/Drawfiber.1.1.html)

说到底，“**海航术**”通过日志和调试阅读源码的方向是没有问题的，有问题的是仅通过分析上万条日志信息，过程枯燥乏味，很难通过想象串联这么大量级的信息。如果借助工具提高生产力，可视化图像具象日志信息，那就能攻守易势。特别对于这种抽象的树形结构，没有什么比画图更通俗易懂了。

本着**DRY（Dont Repeat Yourself）**原则，一步步迭代插件。当然，过程是艰辛的，无法一蹴而就。能想到接入React Developer Tools插件，是因为李阳大牛推荐过该工具帮助分析Virtual DOM树，恰巧彼时团队内部也有童靴在扩展该工具。接入插件当时并没有把握，表面上是扩大战果，但也可能被拖入新的泥潭，舍本逐末。幸好运气不错，在瓶颈期通过董思文和陈卓双大牛的点拨下，灰常顺利的搞出来了。

![](https://shengshuqiang.github.io/assets/ReactDeveloperToolsDemo.png)

这里必须给React Developer Tools点32个赞，这是我迄今见过最好的架构，我就一JS倔强青铜的水平，竟然看着文档能把源码跑起来（过程中编译相关小问题找大牛给解了），进一步把自己的脚本集成进去，模仿已有脚本一顿Ctrl+F、Ctrl+C、Ctrl+V就成了，延展性可见一斑，不服不行。

## 大海航术

“**海航术**”的大方向（日志、调试、想象）是正确的，这个想象操作空间太大，是个非标品。“**大海航术**”的大就在可视化放飞想象力。

1. 以**React方法调用树图**为主线，监控每一个方法调用，不轻易放过任何一个细节，弄清楚他是谁、从哪来、到哪去。同时以Fiber节点操作为里程碑，dump出当前Fiber树（Fiber双树图数据源），衍生出可供**时间旅行**的慢动作回放，便于步步为营式探索。<br>![](https://shengshuqiang.github.io/assets/React方法调用树图.png)
2. 以**Fiber双树图**为小因果，讲清楚Fiber树的每次变化。Fiber算法的核心就是分段式操作Fiber树计算出副作用（DOM操作），然后一次提交（刷新页面）。带着问题去阅读是一种怎样的体验？<br>![](https://shengshuqiang.github.io/assets/Fiber双树图.png)
3. 以**Native View树图**为大因果，说明白Native View树的每次变化。Fiber算法的目标就是生成操作Native View树的一系列指令。<br>![](https://shengshuqiang.github.io/assets/NativeView树图.png)

让我们一起感受一下大海航术的视觉盛宴吧。


[![](https://shengshuqiang.github.io/assets/大海航术动图.gif)](https://shengshuqiang.github.io/assets/DrawFiber/Drawfiber.2.0.html)

[![](https://shengshuqiang.github.io/assets/大海航术动图2.gif)](https://shengshuqiang.github.io/assets/DrawFiber/Drawfiber.2.0.html)

更多详见[Html Demo 页面](https://shengshuqiang.github.io/assets/DrawFiber/Drawfiber.2.0.html)。

## 用户态（浅水区）

[React官方文档](https://zh-hans.reactjs.org/docs/getting-started.html)和简单Debug能解决大部分问题（前提你得知道问题是什么），剩下的交给时间（时间是一种解药，也是一种毒药）。

[**组件API**](https://zh-hans.reactjs.org/docs/react-component.html#other-apis-1)

组件变量/方法 | 概念 | 调用时机 | 作用 | 最佳实践
--- | --- | --- | --- | --- 
[props](https://zh-hans.reactjs.org/docs/glossary.html#props) | 属性 | 使用时设置属性<br>this.props读取 | 存储父组件传递的信息 | 1. UI组件，根据属性纯展示，没有内部逻辑
[state](https://zh-hans.reactjs.org/docs/glossary.html#state) | 状态 | setState设置<br>this.state读取 | 存储自身维护的状态 | 1. 容器组件，纯逻辑处理，通过组合UI组件完成渲染<br>2. 构造函数直接this.state赋值。否则setState替代<br>3. [反面模式: 直接复制 prop 到 state](https://zh-hans.reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#anti-pattern-unconditionally-copying-props-to-state)
[constructor](https://zh-hans.reactjs.org/docs/react-component.html#constructor) | 构造函数 | 在React组件挂载之前 | 用于创建组件实例 | 1. 初始化state或进行方法绑定，否则不需要实现<br>2. 不是props传递的唯一入口（仅初始化调用，后续props更新不调用）<br>3. super(props)必须在使用this.props之前调用
[setState](https://zh-hans.reactjs.org/docs/react-component.html#setstate) | 设置状态 | 用户主动调用 | 将更改排入队列并通知重新渲染 | 1. 控制影响组件粒度，避免大规模刷新（性能杀手）<br>2. 区分哪些生命周期中不能调用setState，避免死循环<br>3. 仅影响组件显示状态的数据放在state里面，其他数据可用成员变量存储<br>4. 不总是立即更新组件，会批量推迟更新
[forceUpdate](https://zh-hans.reactjs.org/docs/react-component.html#forceupdate) | 强制更新 | 用户主动调用 | 跳过shouldComponentUpdate直接触发render | 1. 谨慎使用<br>2. 如render方法依赖于其他数据，则可调用forceUpdate强制刷新
[render](https://zh-hans.reactjs.org/docs/react-component.html#render) | 渲染<br>唯一必须实现| Diff比较前 | 描述当前组件的颜值 | 1. 合理通过组件进行封装，确保可读性和可维护性<br>2. 减少inline-function<br>3. 养成良好的编程习惯（可扩展性、鲁棒性、可靠性、易用性、可移植性等）

[**生命周期**](https://zh-hans.reactjs.org/docs/react-component.html#the-component-lifecycle)

每个组件都包含“生命周期方法”，你可以重写这些方法，以便于在运行过程中特定的阶段执行这些方法。

生命周期 | 概念 | 类型 | 调用时机 | 调用次数 | 调用setState | 作用 | 最佳实践
--- | --- | --- | --- | --- | --- | --- | --- 
[static getDerivedStateFromProps](https://zh-hans.reactjs.org/docs/react-component.html#static-getderivedstatefromprops) | 从属性获取状态 | 常规方法 | 在调用render方法之前调用（初始挂载及后续更新时都调用） | 多次 | 不支持（无法持有引用） | 返回一个对象来更新 state（返回null则不更新任何内容） | 适用于state值在任何时候都取决于props
[getSnapshotBeforeUpdate](https://zh-hans.reactjs.org/docs/react-component.html#getsnapshotbeforeupdate) | 更新前获取快照回调 | 新增方法 | 在最近一次渲染输出（提交到 DOM 节点）之前调用 | 多次 | 支持 | 使得组件能在发生更改之前从 DOM 中捕获一些信息 | 不常见，可能出现在 UI 处理中（如滚动位置）
[componentDidUpdate](https://zh-hans.reactjs.org/docs/react-component.html#componentdidupdate) | 组件已更新回调 | 新增方法 | 在更新后会被立即调用（首次渲染和shouldComponentUpdate返回false时不会调用） | 多次 | 支持 | 当组件更新后，可以在此处对 DOM 进行操作 | 对更新前后的 props 进行了比较判断后触发逻辑（props变化时触发网络请求）
componentWillMount<br>[UNSAFE_componentWillMount](https://zh-hans.reactjs.org/docs/react-component.html#unsafe_componentwillmount) | 组件待挂载回调 | 废弃方法 | 在挂载之前被调用 | 一次 | 支持 | 用于触发挂载前逻辑 | 避免在此方法中引入任何副作用或订阅（改用componentDidMount）
componentWillReceiveProps<br>[UNSAFE_componentWillReceiveProps](https://zh-hans.reactjs.org/docs/react-component.html#unsafe_componentwillreceiveprops) | 组件待接收属性回调 | 废弃方法 | 在已挂载的组件接收新的 props 之前被调用（初始渲染和setState不调用） | 多次 | 支持 | 用于触发接收属性前逻辑 | 1. 不建议用（使用通常会出现 bug 和不一致性）<br>2. 更新状态以响应 prop 更改
componentWillUpdate<br>[UNSAFE_componentWillUpdate](https://zh-hans.reactjs.org/docs/react-component.html#unsafe_componentwillupdate) | 组件待更新回调 | 废弃方法 | 当组件收到新的 props 或 state 时，在渲染之前调用（初始渲染和shouldComponentUpdate返回false不会调用） | 多次 | 不支持（避免循环调用） | 用于触发组件更新前逻辑 | 不建议用（改用componentDidUpdate）
[componentDidMount](https://zh-hans.reactjs.org/docs/react-component.html#componentdidmount) | 组件已挂载回调 | 常规方法 | 在组件挂载后（插入 DOM 树中）立即调用 | 一次 | 支持 | 用于触发挂载后逻辑 | 依赖于 DOM 节点的初始化（添加订阅、网络请求）应该放在这里
[shouldComponentUpdate](https://zh-hans.reactjs.org/docs/react-component.html#shouldcomponentupdate) | 组件是否更新 | 常规方法 | 当props或state发生变化时，会在渲染执行之前被调用（首次渲染或使用forceUpdate时不会调用） | 多次 | 不支持（避免循环调用） | 减少不必要的渲染（性能优化） | 1.考虑使用内置的PureComponent组件 <br>2. 不建议深比较（性能杀手）
[componentWillUnmount](https://zh-hans.reactjs.org/docs/react-component.html#componentwillunmount) | 组件待卸载回调 | 常规方法 | 在组件卸载及销毁之前直接调用 | 一次 | 不支持（该组件将永远不会重新渲染和挂载） | 用于触发卸载前逻辑 | 执行必要的清理操作（清除timer、取消网络请求、注销订阅等）
[componentDidCatch](https://zh-hans.reactjs.org/docs/react-component.html#componentdidcatch) | 子组件出错回调 | 常规方法 | 在子组件抛出错误后被调用 | 多次 | 支持 | 用于记录错误 | 上报错误日志 

[![](https://shengshuqiang.github.io/assets/生命周期图谱.png)](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)


![](https://shengshuqiang.github.io/assets/生命周期调用.png)

**备注：**

* 新增和废弃生命周期混用时，只会知悉新的生命周期
* 生命周期中是调用setState的前提是：没有循环调用风险（shouldComponentUpdate和componentWillUpdate中调用会导致循环调用）、受限（必须包裹在条件语件里）条件下运行调用、有意义（componentWillUnmount调用无意义）、能调用（static getDerivedStateFromProps里面无法调用），详见[React setState 实现机制及循环调用风险](http://www.ptbird.cn/react-state-setState.html)

### 内核态（深水区）

![循序渐进](http://ddrvcn.oss-cn-hangzhou.aliyuncs.com/2019/7/7NJRve.jpg)

终于到了压轴环节，上大海航术动图。



受限于屏幕大小，无法鸟瞰全貌，后续考虑直接生成一个网页。

React源码解析，需要牢记：React组件是数据的函数，v = f(d)。抓住输入和输出，才能有的放矢。本次解析分为二段，初始渲染时间线（用户进入页面Fiber算法干啥类）、用户点击渲染时间线（用户点击按钮切换文本为图标，Fiber算法又干啥类）。这两个场景是所有Fiber算法行为的本源，万变不离其宗。然后再用简单伪代码回顾一下。

### 初始渲染时间线

**初始化页面布局**(里面有一堆组件，远比我们写的要多)

![](https://shengshuqiang.github.io/assets/初始化页面布局.png)

**初始化JS2Native通信**(通信主要是通过桥UIManager调用createView创建、setChildren关联（增删改）和updateView更新)

	1. invoke    UIManager.createView    [3,"RCTRawText",11,{"text":"点击数0"}]
	2. invoke    UIManager.createView    [5,"RCTText",11,{"ellipsizeMode":"tail","allowFontScaling":true,"accessible":true,"fontSize":30,"color":-1,"textAlignVertical":"center","textAlign":"center"}]
	3. invoke    UIManager.setChildren    [5,[3]]
	4. invoke    UIManager.createView    [7,"RCTView",11,{"backgroundColor":-65536,"height":150,"width":300,"accessible":true}]
	5. invoke    UIManager.setChildren    [7,[5]]
	6. invoke    UIManager.createView    [9,"RCTView",11,{"flex":1,"pointerEvents":"box-none","collapsable":true}]
	7. invoke    UIManager.setChildren    [9,[7]]
	8. 
	9. invoke    UIManager.createView    [13,"RCTView",11,{"pointerEvents":"box-none","flex":1}]
	10. invoke    UIManager.setChildren    [13,[9]]
	11. 
	12. invoke    UIManager.setChildren    [11,[13]]


**初始化Fiber树**

![](https://shengshuqiang.github.io/assets/初始化Fiber树.jpg)

**初始化NativeView树**

![](https://shengshuqiang.github.io/assets/初始化NativeView树.png)

**手机横过来看**

[![](https://shengshuqiang.github.io/assets/React算法初始渲染时间线.png)	](https://shengshuqiang.github.io/assets/React算法初始渲染时间线-横版.png)

### 用户点击渲染时间线

**用户点击页面组件布局**

![](https://shengshuqiang.github.io/assets/点击页面组件布局.png)

**用户点击JS2Native通信**，

    1. invoke    UIManager.measure    [7,27]
    1. invoke    UIManager.playTouchSound    []
    2. invoke    UIManager.createView    [15,"RCTImageView",11,{"loadingIndicatorSrc":null,"defaultSrc":null,"src":[{"uri":"http://demo.sc.chinaz.com/Files/pic/icons/5918/c12.png"}],"shouldNotifyLoadEvents":false,"opacity":0.85,"overflow":"hidden","height":100,"width":100}]
    3. invoke    UIManager.manageChildren    [7,[],[],[],[],[0]]
    4. invoke    UIManager.manageChildren    [7,[],[],[15],[0],[]]
    5. invoke    UIManager.updateView    [7,"RCTView",{"backgroundColor":-16777216}]
    1. invoke    UIManager.updateView    [15,"RCTImageView",{"opacity":null}]
    2. invoke    UIManager.updateView    [7,"RCTView",{"backgroundColor":-65536}]

**用户点击Fiber树**

![](https://shengshuqiang.github.io/assets/用户点击渲染Fiber双树图.jpg)

**用户点击NativeView树**

![](https://shengshuqiang.github.io/assets/用户点击NativeView树图.png)

**手机横过来看**

[![](https://shengshuqiang.github.io/assets/React算法用户点击渲染时间线.png)](https://shengshuqiang.github.io/assets/React算法用户点击渲染时间线-横版.png)

### 小结

“render” 阶段生命周期和 “commit” 阶段生命周期

#### 简约伪代码

// TODO highlight.js使用
[](https://facebook.github.io/react-native/docs/performance.html#common-sources-of-performance-problems)
可编辑表格

Talk is cheap. Show me the code.

```
function ReactNativeRenderer_render() {
    const ClassComponent = 1;
    const HostComponent = 5;
    const HostText = 6;
    const Snapshot = 256;
    const Placement = 2;
    const Update = 4;
    const PlacementAndUpdate = 6;
    const Deletion = 8;

    const root = {};
    const {current, workInProgress, rootContainerInstance} = root;
    let {nextUnitOfWork, nextEffect} = root;
    const {oldProps, newProps, oldState, newState, oldContext, newContext} = workInProgress;
    /** performWorkOnRoot */
    {
        /** renderRoot */
        {
            // 深度优先遍历完所有Fiber节点
            /** workLoop */
            {
                while (nextUnitOfWork !== null) {
                    /** performUnitOfWork */
                    {
                        /** beginWork */
                        {
                            // 判断数据是否变化（属性相关）
                            const hasDataChanged = {}
                            // 数据没有变化，则直接当前Fiber节点克隆出工作Fiber节点，详见bailoutOnAlreadyFinishedWork
                            const bailoutOnAlreadyFinishedWork = function (workInProgress) {
                            };
                            if (!hasDataChanged) {
                                nextUnitOfWork = bailoutOnAlreadyFinishedWork(workInProgress);
                            } else {
                                // 数据变化，重新创建Fiber节点
                                /** updateXXX */
                                switch (workInProgress.tag) {
                                    case ClassComponent: {
                                        /** updateClassComponent
                                         * 调用生命周期，新旧取决于用于在类里面增加的方法是新还是旧，
                                         * 如果都有则只调用新的，新生命周期对应construct->getDerivedStateFromProps->render，
                                         * 旧生命周期对应construct->componentWillMount->UNSAFE_componentWillMount->render。
                                         * nextChildren = instance.render()
                                         * */
                                        {
                                            let instance = workInProgress.stateNode;
                                            // 根据是否有新生命周期方法判断是否要调用旧生命周期
                                            const ctor = workInProgress.type;
                                            const getDerivedStateFromProps = ctor.getDerivedStateFromProps;
                                            const hasNewLifecycles = ctor.getDerivedStateFromProps && instance.getSnapshotBeforeUpdate;
                                            if (instance === null) {
                                                // 初始创建
                                                /** constructClassInstance */
                                                {
                                                    // 调用construct实例化组件
                                                    instance = new ctor();
                                                }
                                                /** mountClassInstance */
                                                {
                                                    /** applyDerivedStateFromProps */
                                                    {
                                                        // 调用新生命周期getDerivedStateFromProps
                                                        getDerivedStateFromProps(newProps, oldState);
                                                    }

                                                    if (!hasNewLifecycles) {
                                                        // 调用旧生命周期
                                                        /** callComponentWillMount */
                                                        {
                                                            // 调用旧生命周期componentWillMount
                                                            instance.componentWillMount();
                                                            instance.UNSAFE_componentWillMount();
                                                        }
                                                    }
                                                }
                                            } else {
                                                // 已存在，则Diff更新(为了简化，忽略resumeMountClassInstance)
                                                /** updateClassInstance */
                                                {
                                                    // 更新实例
                                                    let shouldUpdate;
                                                    const hasPropsChanged = oldProps !== newProps || oldContext !== newContext;
                                                    if (!hasNewLifecycles && hasPropsChanged) {
                                                        // 无新生命周期且属性变化
                                                        /** callComponentWillReceiveProps */
                                                        {
                                                            // 调用旧生命周期componentWillReceiveProps
                                                            instance.componentWillReceiveProps(newProps, newContext);
                                                            instance.UNSAFE_componentWillReceiveProps(newProps, newContext);
                                                        }
                                                    }

                                                    /** applyDerivedStateFromProps */
                                                    {
                                                        // 调用新生命周期getDerivedStateFromProps
                                                        getDerivedStateFromProps(newProps, oldState);
                                                    }

                                                    /** checkShouldComponentUpdate */
                                                    {
                                                        if (instance.shouldComponentUpdate) {
                                                            // 刷新逻辑交个用户控制，也就是大家说的高性能操作
                                                            shouldUpdate = instance.shouldComponentUpdate(newProps, newState, newContext);
                                                        } else if (ctor.prototype.isPureReactComponent) {
                                                            // 纯组件，进行浅比较判断是否刷新
                                                            const shallowEqual = function () {
                                                            };
                                                            shouldUpdate = !shallowEqual(oldProps, newProps) || !shallowEqual(oldState, newState);
                                                        } else {
                                                            // 普通组件，直接刷新
                                                            shouldUpdate = true;
                                                        }
                                                    }

                                                    if (shouldUpdate) {
                                                        if (!hasNewLifecycles) {
                                                            // 调用旧生命周期componentWillUpdate
                                                            instance.componentWillUpdate(newProps, newState, newContext);
                                                            instance.UNSAFE_componentWillUpdate(newProps, newState, newContext);
                                                        }
                                                    }
                                                }
                                            }
                                            /** finishClassComponent */
                                            {
                                                if (!shouldUpdate) {
                                                    nextUnitOfWork = bailoutOnAlreadyFinishedWork(workInProgress);
                                                } else {
                                                    const nextChildren = instance.render();
                                                    /** reconcileChildFibers
                                                     * 硬核Diff算法
                                                     * */
                                                    {
                                                        const isObject = typeof nextChildren === "object" && nextChildren;
                                                        if (isObject) {
                                                            /** reconcileSingleElement */
                                                            {
                                                                if (workInProgress) {
                                                                    // 判断key是否相等
                                                                    const isKeyEquals = workInProgress.key === nextChildren.key;
                                                                    if (isKeyEquals) {
                                                                        // 判断类型是否相同
                                                                        const isTypeEquals = child.elementType === nextChildren.type;
                                                                        if (isTypeEquals) {
                                                                            // Diff算法:类型相同,复用子节点树&删除子节点兄弟树
                                                                            (function deleteRemainingChildren(sibling) {
                                                                            })(workInProgress.sibling);
                                                                            workInProgress.child = (function useFiber(workInProgress) {
                                                                            })(workInProgress);
                                                                        } else {
                                                                            // Diff算法:类型不相同,删除全部子节点树
                                                                            (function deleteRemainingChildren(sibling) {
                                                                            })(workInProgress);
                                                                            // Diff算法:新建子节点
                                                                            workInProgress.child = (function createFiberFromElement(nextChildren) {
                                                                            })(nextChildren);
                                                                        }
                                                                    } else {
                                                                        // Diff算法:key不同,删除子节点树
                                                                        (function deleteChild(sibling) {
                                                                        })(workInProgress);
                                                                        // Diff算法:新建子节点
                                                                        workInProgress.child = (function createFiberFromElement(nextChildren) {
                                                                        })(nextChildren);
                                                                    }
                                                                }
                                                            }
                                                        } else {
                                                            /** 暂时忽略 */
                                                            // string、number
                                                            // array
                                                            // iterator
                                                            // undefined
                                                            // deleteRemainingChildren(returnFiber, currentFirstChild)
                                                        }
                                                    }
                                                    nextUnitOfWork = workInProgress.child;
                                                }
                                            }
                                        }
                                        break;
                                    }
                                    default:
                                        /** 忽略，不重要 */
                                        break;
                                }
                            }
                            nextUnitOfWork = beginWork(current$$1, workInProgress, nextRenderExpirationTime);
                        }
                        if (nextUnitOfWork === null) {
                            {
                                /** completeUnitOfWork
                                 *  深度优先遍历回溯，调用桥UIManager创建&连接Native View。
                                 *  同时生成副作用链表。
                                 *  */
                                while (true) {
                                    // nextUnitOfWork = completeWork(
                                    /** completeWork */
                                    {
                                        switch (workInProgress.tag) {
                                            case HostComponent: {
                                                // 是否已实例化
                                                const hasInstance = current && workInProgress.stateNode != null;
                                                if (hasInstance) {
                                                    /** updateHostComponent$1 */
                                                    {
                                                        if (oldProps !== newProps) {
                                                            const updatePayload = (function prepareUpdate(workInProgress) {
                                                            })(workInProgress);
                                                            workInProgress.updateQueue = updatePayload;
                                                        }
                                                    }
                                                } else {
                                                    let instance;
                                                    const tag = (function allocateTag() {
                                                    })();
                                                    const ReactNativeViewConfigRegistry = function () {
                                                    };
                                                    const viewConfig = ReactNativeViewConfigRegistry.get(workInProgress);
                                                    const updatePayload = create(props, viewConfig.validAttributes);
                                                    /** createInstance */
                                                    {
                                                        UIManager.createView(
                                                            tag, // reactTag
                                                            viewConfig.uiViewClassName, // viewName
                                                            rootContainerInstance, // rootTag
                                                            updatePayload // props
                                                        );

                                                        function ReactNativeFiberHostComponent() {
                                                        };
                                                        instance = new ReactNativeFiberHostComponent(tag, viewConfig)
                                                    }
                                                    /** appendAllChildren */
                                                    {
                                                        (function appendAllChildren(workInProgress) {
                                                        })(workInProgress);
                                                    }
                                                    /** finalizeInitialChildren */
                                                    {
                                                        const {parentInstance, nativeTags} = workInProgress;
                                                        UIManager.setChildren(
                                                            parentInstance._nativeTag, // containerTag
                                                            nativeTags // reactTags
                                                        );
                                                    }
                                                }
                                            }
                                                break;
                                            case HostText: {
                                                // 是否已实例化
                                                const hasInstance = current && workInProgress.stateNode != null;
                                                const {oldText, newText} = [oldProps, newProps];
                                                if (hasInstance) {
                                                    /** updateHostText$1 */
                                                    {
                                                        if (oldText !== newText) {
                                                            /** createTextInstance */
                                                            {
                                                                const tag = (function allocateTag() {
                                                                })();

                                                                UIManager.createView(
                                                                    tag, // reactTag
                                                                    "RCTRawText", // viewName
                                                                    rootContainerInstance, // rootTag
                                                                    {text: newText} // props
                                                                );
                                                                workInProgress.stateNode = tag;
                                                            }
                                                        }
                                                    }
                                                } else {
                                                    /** createTextInstance */
                                                    {
                                                        const tag = (function allocateTag() {
                                                        })();

                                                        UIManager.createView(
                                                            tag, // reactTag
                                                            "RCTRawText", // viewName
                                                            rootContainerInstance, // rootTag
                                                            {text: newText} // props
                                                        );
                                                        workInProgress.stateNode = tag;
                                                    }
                                                }
                                            }
                                                break;
                                            default:
                                                /** 忽略 */
                                                break;
                                        }
                                    }
                                    if (nextUnitOfWork !== null) {
                                        return nextUnitOfWork;
                                    }
                                    /** 自底向上归并有效副作用节点，连接成副作用链表 */
                                }
                                nextUnitOfWork = completeUnitOfWork(workInProgress);
                            }
                        }
                    }
                }
            }
        }
        const {finishedWork} = root;
        const {firstEffect} = finishedWork;
        if (finishedWork !== null) {
            /** completeRoot */
            {
                const instance = finishedWork.stateNode;
                /** commitRoot */
                {
                    nextEffect = firstEffect;
                    while(nextEffect) {
                        /** commitBeforeMutationLifeCycles */
                        {
                            switch (finishedWork.tag) {
                                case ClassComponent:
                                    {
                                        if (finishedWork.effectTag & Snapshot) {
                                            // 调用新生命周期getSnapshotBeforeUpdate
                                            instance.getSnapshotBeforeUpdate(oldProps, oldState);
                                        }
                                    }
                                    break;
                                default:
                                    // 忽略
                                    break;
                            }
                        }
                        nextEffect = nextEffect.nextEffect;
                    }

                    nextEffect = firstEffect;
                    while(nextEffect) {
                        /** commitAllHostEffects */
                        {
                            var {effectTag} = nextEffect;
                            var primaryEffectTag = effectTag & (Placement | Update | Deletion);
                            switch (primaryEffectTag) {
                                case Placement:
                                    {
                                        /** commitPlacement */
                                        {
                                            const {containerID, moveFromIndices, moveToIndices, addChildReactTags, addAtIndices, removeAtIndices} = finishedWork;
                                            UIManager.manageChildren(containerID, moveFromIndices, moveToIndices,addChildReactTags,addAtIndices,removeAtIndices);
                                        }
                                    }
                                    break;
                                case PlacementAndUpdate:
                                    {
                                        /** commitPlacement */
                                        {
                                            const {containerID, moveFromIndices, moveToIndices, addChildReactTags, addAtIndices, removeAtIndices} = finishedWork;
                                            UIManager.manageChildren(containerID, moveFromIndices, moveToIndices,addChildReactTags,addAtIndices,removeAtIndices);
                                        }
                                        /** commitWork */
                                        {
                                            switch (finishedWork.tag) {
                                                case HostComponent:
                                                    {
                                                        /** commitUpdate */
                                                        const {reactTag, viewName, props} = finishedWork;
                                                        UIManager.updateView(reactTag, viewName, props);
                                                    }
                                                    break;
                                                case HostText:
                                                    {
                                                        /** commitTextUpdate */
                                                        const {reactTag, props} = finishedWork;
                                                        UIManager.updateView(reactTag, "RCTRawText", props);
                                                    }
                                                    break;
                                                default:
                                                    // 忽略
                                                    break;
                                            }
                                        }
                                    }
                                    break;
                                case Update:
                                    {
                                        /** commitWork */
                                        {
                                            switch (finishedWork.tag) {
                                                case HostComponent:
                                                    {
                                                        /** commitUpdate */
                                                        const {reactTag, viewName, props} = finishedWork;
                                                        UIManager.updateView(reactTag, viewName, props);
                                                    }
                                                    break;
                                                case HostText:
                                                    {
                                                        /** commitTextUpdate */
                                                        const {reactTag, props} = finishedWork;
                                                        UIManager.updateView(reactTag, "RCTRawText", props);
                                                    }
                                                    break;
                                                default:
                                                    // 忽略
                                                    break;
                                            }
                                        }
                                    }
                                    break;
                                case Deletion:
                                    {
                                        /** commitDeletion */
                                        {
                                            /** commitUnmount */
                                            {
                                                let {node} = root;
                                                while(true) {
                                                    switch (node.tag) {
                                                        case ClassComponent:
                                                            {
                                                                instance.componentWillUnmount();
                                                            }
                                                            break;
                                                        default:
                                                            // 忽略
                                                            break;
                                                    }
                                                    node = node.sibling;
                                                    if (!node.return) {
                                                        break;
                                                    }
                                                }
                                            }
                                        }
                                    }
                                    break;
                                default:
                                    // 忽略
                                    break;
                            }
                        }
                        nextEffect = nextEffect.nextEffect;
                    }

                    root.current = finishedWork;

                    nextEffect = firstEffect;
                    while(nextEffect) {
                        /** commitAllLifeCycles */
                        {
                            const current$$1 = nextEffect.alternate;
                            switch (finishedWork.tag) {
                                case ClassComponent:
                                    {
                                        if (current$$1 === null) {
                                            // 调用生命周期componentDidMount
                                            instance.componentDidMount();
                                        } else {
                                            // 调用新生命周期componentDidUpdate
                                            instance.componentDidUpdate(oldProps, oldState);
                                        }
                                    }
                                    break;
                                default:
                                    // 忽略
                                    break;
                            }
                        }
                        nextEffect = nextEffect.nextEffect;
                    }
                }
            }
        }
    }
}
```

#### 方法调用图

[![](https://shengshuqiang.github.io/assets/React源码解析.png)](./React源码解析.png)


# 拿结果

## QA

**问：**明明只写了几个组件，通过React Developer Tools看到的是一堆布局，而且还有Context.Consumer，这些都是干啥的？

**答：**查看View.js源码，发现里面会再次render出Context.Consumer。<br>![](https://shengshuqiang.github.io/assets/view_render.png)<br>![](https://shengshuqiang.github.io/assets/text_render.png)<br>我们写的组件其实外面会被包裹一层，比方显示yellowbox提示啥的。<br>![](https://shengshuqiang.github.io/assets/renderApplication.png)

**问：**React的组件和Native看起来好像不是一一对应的，这个映射策略是什么？

**答：**只有HostComponent和HostText会映射到Native View，其他类型不会，只是用于运算和记录状态。Fiber中的tag表示类型，创建NativeView时（createInstance和createTextInstance）的tag是组件唯一标识，从数字3开始累积2生成。<br>![](https://shengshuqiang.github.io/assets/fiber_tag.png)<br>![](https://shengshuqiang.github.io/assets/get_fiber_tag.png)<br>![](https://shengshuqiang.github.io/assets/text_fiber_tag.png)<br>![](https://shengshuqiang.github.io/assets/allocateTag.png)

**问：**Element、Instance、DOM之间关系？

**答：**![](https://shengshuqiang.github.io/assets/element_instance_dom_relation.png)<br>![](https://shengshuqiang.github.io/assets/element_instance_dom.png)<br>![](https://shengshuqiang.github.io/assets/element_instance_dom2.png)

**问：**都说React有个Diff算法，这个在代码哪里，怎么比较的，文案变了会设计Diff算法吗？

**答：**Diff算法在[reconciliation模块](https://zh-hans.reactjs.org/docs/reconciliation.html)里面，对应函数为ChildReconciler。<br>![](https://shengshuqiang.github.io/assets/reconcileSingleElement.png)<br>文本节点和数组见reconcileSingleTextNode和reconcileChildrenArray。更多可以参考[React 源码剖析系列 － 不可思议的 react diff](https://zhuanlan.zhihu.com/p/20346379)。

**问：**浅比较shouldComponentUpdate的正确姿势是啥？

**答：**判断组件是否更新时调用，优先调用shouldComponentUpdate方法，无该该方法是判断是否是纯组件，是则浅比较（判断对象props和state前后是否改变，只对比一级属性是否严格相等===）<br>![](https://shengshuqiang.github.io/assets/shouldComponentUpdate.png)<br>![](https://shengshuqiang.github.io/assets/shallowEqual.png)

**问：**React有棵DOM树，树在哪，怎么看，怎么操作对应Native View树？

**答：**在我扩展的插件上看。

**问：**setState到底干啥了？

**答：**触发Fiber双树重新Diff渲染，具体调用可以使用方法调用树追踪。

**问：**React高效在哪？

**答：**基于优先级的可中断的树遍历算法，且Diff算法复杂度O（n）。

**问：**React工作流程？

**答：**文章中有。

**问：**如何关联Native自定义组件？

**答：**这是个好问题，留给读者自行解答。

**问：**Fiber节点数据结构中各属性含义？

**答：**

1. return, child, sibling：<br>![](https://pic2.zhimg.com/80/v2-453e1f48a4f53356bee021c90ee00bed_hd.jpg)
2. key: 复用标识。
3. tag：它在协调算法中用于确定需要完成的工作。如前所述，工作取决于React元素的类型。
4. stateNode：保存组件的类实例、DOM 节点或与 Fiber 节点关联的其他 React 元素类型的引用。总的来说，我们可以认为该属性用于保持与一个 Fiber 节点相关联的局部状态。
	1. HostRoot对应{containerInfo}。
	2. ClassComponent对应为new的函数对象实例。
	3. HostComponent对应为ReactNativeFiberHostComponent，包含_children和_nativeTag。
	4. HostText对应为nativeTag。
5. elementType/type: 描述了它对应的组件。对于复合组件，类型是函数或类组件本身。对于宿主组件（div，span等），类型是字符串。定义此 Fiber 节点的函数或类。对于类组件，它指向构造函数，对于 DOM 元素，它指定 HTML 标记。我经常使用这个字段来理解 Fiber 节点与哪个元素相关。
	1. ClassComponent对应为函数，如APPContainer()。
	2. ForwardRef、ContextConsumer、ContextProvider对应为对象，如{$$typeof: Symbol(react.forward_ref), render: ƒ, displayName: "View"}。
	3. HostComponent对应为字符串，如“RCTView”。
	4. HostText对应为null。
6. memoizedProps：在前一个渲染中用于创建输出的 Fiber 的 props。
7. memoizedState：用于创建输出的 Fiber 状态。处理更新时，它会反映当前在屏幕上呈现的状态。
8. pendingProps：props是函数的参数。一个 fiber 的pendingProps在执行开始时设置，并在结束时设置memoizedProps。已从 React 元素中的新数据更新并且需要应用于子组件或 DOM 元素的 props。
9. updateQueue: state更新队列。状态更新、回调和 DOM 更新的队列。
10. firstEffect 、lastEffect 等玩意是用来保存中断前后 effect 的状态，用户中断后恢复之前的操作。这个意思还是很迷糊的，因为 Fiber 使用了可中断的架构。
11. effectTag：副作用，增删改操作。
12. alternate：在调用render或setState后，会克隆出一个镜像fiber，Diff产生出的变化会标记在镜像fiber上。而alternate就是链接当前fiber tree和镜像fiber tree, 用于断点恢复。workInProgress tree上每个节点都有一个effect list，用来存放需要更新的内容。此节点更新完毕会向子节点或邻近节点合并 effect list。


## 生命周期调用

![](https://shengshuqiang.github.io/assets/生命周期调用.png)

## 高性能实践

让浏览器休息好，浏览器就能跑得更快

留到下期。

## 问题定位利器

基于上面插件，同理研发。

## 方法钩子

我的数据映射关系怎么来的，这个插件是怎么写的，这又可以再写一篇。

# 长歌

1. 永远不要只满足于世界的表象，要敢于探寻未知的可能。--《守望先锋》
2. 天不生我李淳罡，剑道万古长如夜。剑来！--《雪中悍刀行》
4. 在本帅眼里没有圣女，也无所谓蛊王。--《画江湖之不良人》
5. 世间万事，风云变幻，苍黄翻覆，纵使波谲云诡，但制心一处，便无事不办，天定胜人，人定兮胜天！李淳风，霸道如何，天道又如何？我，不在乎。--《画江湖之不良人》
6. 三界唯心，万法唯识。唯心所变，唯识所现。--《佛法》
6. 谋逆？！哈哈哈哈哈！我陆危楼何惧谋逆叛教之说，不过从头再来罢了！--《圣焰暝影》
6. 当其他人盲目追随真理的时候，记住，万物皆虚；当其他人被道德和法律束缚的时候，记住，万事皆允。我们躬耕于黑暗，服侍着光明。--《刺客信条》
7. 天生万物以养人，世人犹怨天不仁。--《七杀碑》
8. 好男儿，别父母，只为苍生不为主。-- 《红巾军军歌》

# 结语

感谢岳母大人和媳妇大人的默默付出，感谢士兴大佬、朝旭大神、车昊大哥、张杰大哥、思文大拿、陈卓大牛的技术支持和迷津指点。

曾经在知乎看到一个问题，“[能魔改react-native源码的是什么水平的前端？](https://www.zhihu.com/question/269731127)”我挑战了这个水平。

Airbnb摇了摇头，说“ ReactNative太难了”，然后倒下了。但是我们，必须，站到台前，领导大家～

谨以此文，献给(那些)曾经热爱互联网技术，并和并肩作战的伙伴们一同度过时光的人们，呈现这重逢的此刻。

# 参考

1. [React16源码之React Fiber架构](https://github.com/HuJiaoHJ/blog/issues/7#)
2. [React Fiber架构](https://zhuanlan.zhihu.com/p/37095662)
2. [「译」React Fiber 那些事: 深入解析新的协调算法](https://juejin.im/post/5c052f95e51d4523d51c8300)
3. [浅析React Diff 与 Fiber](https://zhuanlan.zhihu.com/p/58863799)
3. [200行代码实现简版react](https://juejin.im/post/5c0c7304f265da613e22106c)
2. [React 源码剖析系列 － 不可思议的 react diff](https://zhuanlan.zhihu.com/p/20346379)
3. [React源码分析](https://juejin.im/post/5abe05ea5188255c61631d6c)
4. [[react] React Fiber 初探](https://www.cnblogs.com/qingmingsang/articles/9131512.html)
5. [Virtual DOM 的实现和 React Fiber 简介](https://www.jianshu.com/p/b189b2949b33)
6. [React中一个没人能解释清楚的问题——为什么要使用Virtual DOM](https://www.zcfy.cc/article/the-one-thing-that-no-one-properly-explains-about-react-why-virtual-dom-hashnode-1211.html)
6. [深度剖析：如何实现一个 Virtual DOM 算法 #13](https://github.com/livoras/blog/issues/13)
6. [React Fiber 架构【译】](https://blog.yongyuan.us/articles/2017-04-10-react-fiber/)
7. [React Fiber是什么](https://zhuanlan.zhihu.com/p/26027085)
5. [浅谈React16框架 - Fiber](https://blog.csdn.net/P6P7qsW6ua47A2Sb/article/details/82322033)
6. [如何理解 React Fiber 架构？](https://www.zhihu.com/question/49496872)
7. [React 16 架构研究记录（文末有彩蛋）](https://zhuanlan.zhihu.com/p/36926155)
8. [对React生命周期的理解](https://blog.csdn.net/WonderGlans/article/details/83479577)