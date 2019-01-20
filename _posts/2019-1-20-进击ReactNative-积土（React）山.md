---
layout: post
title: 进击ReactNative-积土（React）山
key: 20190120
tags:
  - 拈花指
  - React  
---
<!-- 添加目录 http://blog.csdn.net/hengwei_vc/article/details/47122103 -->
<script src="/javascripts/jquery-2.1.4.min.js" type="text/javascript"></script>
<script src="/javascripts/toc.js" type="text/javascript"></script>
<script type="text/javascript">
$(document).ready(function() {
    $('#toc').toc();
}); </script>
<div id="toc"></div>
**曾经深爱着React，与React里的伙伴们一起渡过的人们，再度献上这重逢的时刻**
{:.info}

<!--more-->

# [进击ReactNative-纳百川](https://shengshuqiang.github.io/2018/12/15/%E8%BF%9B%E5%87%BBReactNative-%E7%BA%B3%E7%99%BE%E5%B7%9D.html)

一✐承蒙大神指点，汇总之前发送的ReactNative相关朋友圈优秀文章，献给有需要的朋友<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-181215.jpg)
{:.success}

# [React 移动 web 极致优化](https://mp.weixin.qq.com/s?__biz=MzA3NTYzODYzMg==&mid=2653577496&idx=1&sn=bec9a376715f5bb24fec5c3f054688e8&mpshare=1&scene=2&srcid=1217wiUUaAg19yNjLLPpsquS&from=timeline&ascene=2&devicetype=android-26&version=2607033d&nettype=WIFI&abtest_cookie=BgABAAgACgALABIAEwAUAAcAnYYeACaXHgBamR4Am5keAJ2ZHgC2mR4A1JkeAAAA&lang=zh_CN&pass_ticket=legFXQzE26ou60CPZ70ahMM0G1RrEDlQElQwwdX4SMbDho8wcLVEBrBkDpbPsp4M&wx_header=1)

一✐React极致优化<br>启✵不是一般人能hold住所有端
渔✐斟酌、分治、迭代<br>鱼✎<br>1）特性☞学一写三、VirtualDOM、组件化<br>2）期待☞完爆一切框架，包括原生<br>3）优化☞构建、工具、数据、性能<br>评✎<br>⑴初恋：对React满怀希望，指意它帮我们做好一切，但随着了解的深入，发现需要做一些额外的事情来达到我们的期待<br>⑵React轻量快捷➢首屏加速ReactServerRender➢高性能ReactNative<br>⑶Redux优势：统一在自己定义的reducer函数里面去进行数据处理，在View层中只需要通过事件去处触发一些action就可以改变地应的数据，这样能够使数据处理和dom渲染更好地分离，而避免手动地去设置state<br>⑷React全家桶☞<br>构建【gulp+webpack】、<br>开发提效【redux-dev-tools+hot-reload】、<br>数据管理【redux】、<br>性能【immutable+purerender】、<br>路由【react-router】<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-181217.jpg)
{:.success}

# [ReactNative iOS源码解析（一）](http://awhisper.github.io/2016/06/24/ReactNative%E6%B5%81%E7%A8%8B%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90/)

一✐上代码之iOS切RN<br>启✵把野心放长远点<br>渔✐深入、好奇<br>鱼✎<br>1）ReactNative融React（前端框架）和Native（原生）为一体。在render环节拦截，切浏览器换原生，桥接OC和JS<br>2）核心☞组件RCTModuleData、通道RCTJSExecutor、脉搏RCTDisplayLink<br>3）require图片：静态、应用、网络
当React.JS开始工作的时候，JS把所有布局好的Component，一层套一层的JS界面数据，通过UIManager，调用createView，updateView，setChildren等接口API，来创建一个个纯iOS native的UIKit的界面<br>评✎<br>⑴因为业务原因，注定不可能以单一RCTRootView去实现整个APP功能，注定了大部分保留现有native功能，个别动态性较强的新功能采用ReactNative去开发<br>⑵APIModule是一种面向过程式的模块调用。JS只需要用一个模块名，一个API名，就能通过bridge找到对应的native的方法进行调用，JS Call OC Method<br>⑶UIModule是一种面向对象式的UI组件调用。每一个React的Component都是一个独立的UI组件，经过React的flexbox排版计算，有自己的大小，形状，样式<br>⑷require(imagepath)的处理过程比较复杂了，涵盖rn的打包，执行<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-181218.jpg)
{:.success}

# [Angular、React、Vue.js 等 6 大主流 Web 框架都有什么优缺点？](https://mp.weixin.qq.com/s?__biz=MjM5MjAwODM4MA==&mid=2650691697&idx=2&sn=6f43b9eacb24c52e78ae325d410fb242&chksm=bea629a289d1a0b411cff60fa7cb0004e9a6b20e68c2f9a20228b63226298c07d06214b05a3f&mpshare=1&scene=2&srcid=12191xfJhF2t6fjPi6n0x4zM&from=timeline&ascene=2&devicetype=android-26&version=2607033d&nettype=WIFI&abtest_cookie=BgABAAgACgALABIAEwAUAAcAnYYeACaXHgBamR4Am5keAJ2ZHgC2mR4A1JkeAAAA&lang=zh_CN&pass_ticket=legFXQzE26ou60CPZ70ahMM0G1RrEDlQElQwwdX4SMbDho8wcLVEBrBkDpbPsp4M&wx_header=1)

一✐Web选型<br>启✵为我所用<br>渔✐优缺、挑战、未来、时机<br>鱼✎<br>1）Angular2+☞快、模型视图、组件库、单页<br>2）React+Redux☞简单、专注、解耦、诚实地驾驭<br>3）Vue☞渐进式构建、简洁合理、激情社区、个人<br>4）Dojo2☞动态组件、生态、潜力股、灵活现代响应式智能<br>5）Ember☞固执、成熟、产品、合理、官二代<br>6）Aurelia☞完美、未交付、个人<br>评✎<br>⑴我是否需要使用框架？虽然无框架也能正常工作，但是，这也是有代价的。如果你是一个有着深厚技术和经验的人，确实可以坦诚的不使用框架。但你团队的其他成员呢？你手下的那些人呢？或者当你的决定把你自己陷入困境的时候呢？<br>⑵React 和 Redux 的最大优势在于它们相对简单和专注。做一件事情并把它做好是非常困难的，但这两个库都很有效地完成了它们的目标。React 和 Redux 最大的弱点不是它们是什么，而是它们不是什么<br>⑶要构建一个功能丰富的 Web 应用程序，你需要许多功能，一旦脱离 React 和 Redux 和其他一些库的核心，你将发现一个非常分散的社区，拥有无数的解决方案和模式，不容易整合在一起。虽然 React 和 Redux 都是非常专注的库，但缺乏经验的团队还是会很容易地生成不可维护的解决方案，而不是意识到他们所做的选择会导致性能不佳或错误<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-181219.jpg)
{:.success}

# [React 生态系统：从小白到大神](https://mp.weixin.qq.com/s?__biz=MzIwNjEwNTQ4Mw==&mid=2651577848&idx=1&sn=563ecde0ec32c75614045d5871eb9e98&chksm=8cd9c31cbbae4a0a22f674998eff7de07f6928355dc37c6ab3126f6ea196cc102ae79840fe68&mpshare=1&scene=2&srcid=1220IIh7I16trj1mQe7YgWRs&from=timeline&ascene=2&devicetype=android-26&version=2607033d&nettype=WIFI&abtest_cookie=BgABAAgACgALABIAEwAUAAcAnYYeACaXHgBamR4Am5keAJ2ZHgC2mR4A1JkeAAAA&lang=zh_CN&pass_ticket=legFXQzE26ou60CPZ70ahMM0G1RrEDlQElQwwdX4SMbDho8wcLVEBrBkDpbPsp4M&wx_header=1)

一✐React江湖<br>启✵有责【划分越明确】无界【能做的事越多】<br>渔✐高瞻【体系】远瞩【架构】落地【工程化】<br>鱼✎<br>1）前后端分离分层架构图☞面向服务、解耦、有责无界、微服务<br>2）需求☞开发、共享【代码、工具方法、组件库】、性能、部署<br>3）特性☞同构渲染、完全组件化、生态圈<br>评✎<br>⑴React生命周期二问：<br>React 在初始化和更新的时候会触发的钩子函数？<br>父组件在更新状态的时候父组件与子组件的生命周期顺序是怎么执行的？<br>⑵如何学习RN呢？<br>可以跟着 git 上的 awesome 系列去进阶。<br>从源码角度看RN，主要需要了解RN如何做到JS和OC交互、RN的启动流程是怎样的、如何加载 JS 源码、UI 控件的渲染流程、事件处理流程以及 RN 与 iOS 之间的通信方式等。<br>⑶「当你唯一的工具是锤子时，什么在你眼中都是钉子」<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-181220.jpg)
{:.success}

# [【第1392期】React从渲染原理到性能优化（二）-- 更新渲染](https://mp.weixin.qq.com/s?__biz=MjM5MTA1MjAxMQ==&mid=2651229841&idx=1&sn=d8f1b4feaf9298522daf8f9543080df3&chksm=bd4957158a3ede033693e0b3e499837fbaefd2166c703e71c1313299fbc9f4d32cb302ddd786&mpshare=1&scene=2&srcid=1221Rd1fXNtNm9Bb2rW84JUQ&from=timeline&ascene=2&devicetype=android-26&version=2607033d&nettype=WIFI&abtest_cookie=BgABAAgACgALABIAEwAUAAcAnYYeACaXHgBamR4Am5keAJ2ZHgC2mR4A1JkeAAAA&lang=zh_CN&pass_ticket=legFXQzE26ou60CPZ70ahMM0G1RrEDlQElQwwdX4SMbDho8wcLVEBrBkDpbPsp4M&wx_header=1)

一✐入原理出优化<br>启✵源码为王<br>渔✐知其所以然<br>鱼✎<br>⑴创建☞JSX◑Babel➢React.createElement表达式◑render➢虚节点（标签、属性、子节点）➢真节点<br>⑵更新☞props/state变化◑脏标记➢批处理◑render➢shouldComponentUpdate◑Diff算法➢增量<br>⑶优化☞结构稳定、key<br>评✎<br>⑴[2018 React Conf腾讯IMWeb](https://img.w3ctech.com/%E4%BB%8E%E6%B8%B2%E6%9F%93%E5%8E%9F%E7%90%86%E5%88%B0%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96%E4%BF%AE%E6%94%B9%E7%89%88.pptx)@黄琼分享<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-181221.jpg)
{:.success}

# [漫谈前端性能 突破 React 应用瓶颈](https://segmentfault.com/a/1190000016008108)

一✐React实现突破性能瓶颈<br>启✵突破极限➢能做的事越来越多<br>渔✐预知探索聚焦<br>鱼✎<br>⑴解药：耗时任务分段、工作线程&批处理<br>⑵调度策略【虚拟DOM构建➢DOM diff➢渲染补丁】升级☞stack reconcile➢Fiber reconcile<br>⑶解析☞React core【复杂DOM计算✧切入点】、React-Dom【真实DOM交互】<br>评✎<br>⑴社区上关于 React 性能的内容往往聚焦在业务层面，主要是使用框架的“最佳实践”。这里我们不去谈论“使用shoulComponentUpdate 减少不必要的渲染”、“减少 render 函数中 inline-function”等已经“老生常谈”的话题，本文主要✧从React框架实现层面分析其性能瓶颈和突破策略✧<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-181224.jpg)
{:.success}

# [[译] 图解 React](https://zhuanlan.zhihu.com/p/39658720)

一✐【译】漫画React<br>启✵就看你想不想了<br>渔✐白话涂鸦<br>鱼✎<br>⑴React☞开发用户界面JS库<br>⑵jQuery☞操纵DOM更简单的JS工具库<br>⑶核心科技☞响应式UI【数据改变自动刷新界面】、虚拟DOM【记录细节整理更新】、组件【分类组合】<br>评✎<br>⑴插图大爱、生动有趣、视角独到<br>⑵原始社会☞开发者【人】调用NativeAPI完成页面开发
奴隶社会☞开发者【奴隶主】完成构想和图纸，指挥jQuery【奴隶】完成页面开发，释放开发者时间，做更有意义的事<br>资本社会☞开发者【老板】完成构想和图纸，交给React【秘书】高效完成页面开发，进一步释放开发者时间<br>共产社会☞开发者【真身】提出构想，AI【影分身】规划落地，实现最终理想✧商君虽死，秦法不灭pkj<br>⑶✨ 🎄 圣诞快乐(✪▽✪)<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-181225.jpg)
{:.success}

# [[译] React Native vs. Cordova、PhoneGap、Ionic，等等](https://zhuanlan.zhihu.com/p/45356420)

一✐软件深渊和巅峰<br>启✵梦中梦<br>渔✐是什么➢为什么➢价值<br>鱼✎<br>⑴软件是控制大量晶体管和电路 (统称硬件) 的指令集合【开关序列＋记忆】<br>⑵软件是金字塔式结构【阶级】，层层相扣，距硬件越近越原生<br>⑶原生的机遇【自由意志✧我说了算】和挑战【战龙于野✧啥都要干】并存，做到优秀和烂到渣渣一样容易【全凭本事】<br>评✎<br>⑴以模拟的角度来看，Cordova 应用的 UI 就是运行在 Web 浏览器中的模拟世界，而浏览器又是运行在原生框架里的另一个模拟世界。相比之下，React Native 的 UI 要比 WebView 框架低一个层级，它直接运行在原生框架里<br>⑵Hardware↬Native↬Webview↬Cordova<br>⑶React Native 直接使用了原生 UI 组件，而 WebView 框架是使用 HTML/CSS 的 Web UI 来模拟原生 UI 。真和假，你更喜欢哪个？<br>⑷根据经验，识别出一个应用是否是使用 WebView 框架开发的并不难。通过一些小测试，比如滚动加速、键盘操作、导航和 UI 的流畅性。如果这些操作达不到原生般的效果，那么累积后的效果将导致糟糕的用户体验<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-181226.jpg)
{:.success}

# [[译] 图解 React Native](https://zhuanlan.zhihu.com/p/40398439)

一✐RN承上【React】启下【Native】<br>启✵范式＝问题思考方式＋问题描述方式＋解决方案<br>渔✐我是谁，从哪来，到哪去<br>鱼✎<br>⑴ 角色分离☞React范式 【专注于上层业务、核心共享库】、渲染器ReactDOM【支撑各平台交互】<br>⑵RN是用同样的思维构建不同平台的原生应用解决方案，从Web来，到X平台【iOS、Android、VR……】去<br>评✎<br>⑴特朗普用英语在推特上说一句话，全世界都知道美国想干啥<br>特朗普的推特就是React，<br>而我们Too Young，Too Native<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-181227.jpg)
{:.success}

# [【第1448期】深入理解 React 高阶组件](https://mp.weixin.qq.com/s?__biz=MjM5MTA1MjAxMQ==&mid=2651230344&idx=1&sn=023025f5d31ef1d6c7ae3af7bfdac22e&chksm=bd49490c8a3ec01a19f1f51226ee7900da80af6b6fafed7d28cec3b6fb761e45635d01f36f86&mpshare=1&scene=2&srcid=1228eB6AFiYIdl7bvgizlK6g&from=timeline&ascene=2&devicetype=android-26&version=2607033d&nettype=WIFI&abtest_cookie=BgABAAgACgALABIAEwAUAAcAnYYeACaXHgBamR4Am5keAJ2ZHgC2mR4A1JkeAAAA&lang=zh_CN&pass_ticket=legFXQzE26ou60CPZ70ahMM0G1RrEDlQElQwwdX4SMbDho8wcLVEBrBkDpbPsp4M&wx_header=1)

一✐是否此组件最高<br>启✵DRY【Don’t Repeat Yourself】就是不凑合<br>渔✐层次递进<br>鱼✎<br>⑴函数是JS一等公民✧变量、参数、返回值<br>⑵高阶函数☞是一个函数，有一个回调函数作参数，返回一个新函数，返回函数会触发传入的回调函数<br>⑶高阶组件☞是一个组件，有一个组件做为参数，返回一个新组件，返回组件会渲染传入的组件<br>评✎<br>⑴偏函数应用(Partial Application)☞通过一个多参数的函数来返回一个具有较少参数的函数的模式<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-181228.jpg)
{:.success}

# [Hummingbird: Web 里的 Flutter](https://mp.weixin.qq.com/s?__biz=MzAwODY4OTk2Mg==&mid=2652048000&idx=1&sn=2f546834b19bbc44bc6c966cea596971&chksm=808caac5b7fb23d322c72150bb26f604fb0b1f554c6abaea2fae6a6bafdb94222ea29bd0867c&mpshare=1&scene=2&srcid=0102ziIoYcT30aYQYTlemobK&from=timeline&ascene=2&devicetype=android-26&version=2607033d&nettype=WIFI&abtest_cookie=BgABAAgACgALABIAEwAUAAcAnYYeACaXHgBamR4Am5keAJ2ZHgC2mR4A1JkeAAAA&lang=zh_CN&pass_ticket=legFXQzE26ou60CPZ70ahMM0G1RrEDlQElQwwdX4SMbDho8wcLVEBrBkDpbPsp4M&wx_header=1)

一✐将Flutter带进Web<br>启✵预测未来不如创造未来<br>渔✐尝试可能➢实验对比➢跨平台移植权衡➢不断探索<br>鱼✎<br>⑴Flutter架构☞框架【widget、物理效果、动画或布局 (文本布局除外)】、引擎【dart:ui】<br>⑵Web运行Flutter条件☞编译Dart代码、Web运行的Flutter子集、支持Web功能子集<br>⑶Widget：构建➢布局➢绘制<br>评✎<br>⑴Flutter高扩展性：是一个多层系统，可以在更高的层级上用很少的代码就开发出很丰富的内容，也可以深入到较低的层去进行控制更多的系统行为，但相应的，复杂度也会更高一些<br>⑵目标是尽可能多地将框架移植到 Web 上。但是，这并不意味着任何 Flutter 应用都能无需更改代码就在 Web 上运行。Flutter Web 应用仍然是一个 Web 应用，它在浏览器中被沙箱化，只能执行 Web 浏览器允许的操作<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-190102.jpg)
{:.success}

# [40 行代码内实现一个 React.js](https://zhuanlan.zhihu.com/p/25398176)

一✐40行纯JS代码手写React<br>启✵回到起点✧简单粗暴<br>渔✐一心一意组件化<br>鱼✎<br>⑴原理☞状态改变➢render方法➢构建新DOM元素➢页面更新<br>⑵Diff算法☞批处理合并状态+更新补丁<br>⑶实现☞专注于状态变化的render方法、抽象出Component类【setState、生成DOM元素&绑定事件】和mount方法【页面更新DOM】<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-190103.jpg)
{:.success}

# [React 是怎样炼成的](https://segmentfault.com/a/1190000013365426)

一✐React进化论<br>启✵以史为鉴✧管窥脸书<br>渔✐步步高<br>鱼✎<br>⑴字符拼接时代✧2004➢XHP标签时代✧2010➢JSX✧2013➢React【DOM、Diff、key➢虚拟DOM、批处理、裁剪】<br>⑵脸书自研Diff和虚拟DOM✧开源社区贡献批处理和裁剪<br>评✎<br>⑴You need to be right before being good<br>为了验证迁移方案的可行性，开发者必须快速实现一个可用版本，暂时不考虑性能问题<br>⑵React 的开源可谓是一石激起千层浪，社区开发者都被这种全新的 Web 开发方式所吸引，React 因此迅速占领了 JS 开源库的榜首<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-190104.jpg)
{:.success}

# [进击ReactNative-疾如风](https://shengshuqiang.github.io/2019/01/07/%E8%BF%9B%E5%87%BBReactNative-%E7%96%BE%E5%A6%82%E9%A3%8E.html)

一✐儒门有志羁风雨，失鹿山河散若星<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-190107.jpg)
{:.success}

# [【第1477期】想写好前端，先练好内功](https://mp.weixin.qq.com/s?__biz=MjM5MTA1MjAxMQ==&mid=2651230706&idx=1&sn=e96555bdf9b8251852928f4a3c5193e0&chksm=bd4948768a3ec16057c55ff79157b0c2b586748f1bb845ab288f41f33a4281efacd40d0d58ab&mpshare=1&scene=2&srcid=0108f7FMQzeri9gbTHXCZ62x&from=timeline&ascene=2&devicetype=android-26&version=2607033d&nettype=WIFI&abtest_cookie=BgABAAgACgALABIAEwAUAAcAnYYeACaXHgBamR4Am5keAJ2ZHgC2mR4A1JkeAAAA&lang=zh_CN&pass_ticket=legFXQzE26ou60CPZ70ahMM0G1RrEDlQElQwwdX4SMbDho8wcLVEBrBkDpbPsp4M&wx_header=1)

一✐三套内功口诀遇见毕生所盼望完美之境<br>启✵大敌当前✧淡然自若<br>渔✐掌握技术背后的设计理念和工程思想<br>鱼✎<br>⑴开闭原则☞对扩展开放【增加抽象并统一接口的实现】，对修改封闭【核心逻辑稳定性✧牵一发而动全身】<br>⑵函数式编程☞高复用性、易测试性、健壮性、简洁<br>⑶消息机制☞模块间安全解耦、驱动模块间协作<br>评✎<br>⑴SOLID 五大原则的出发点也是软件工程的终极目标：“高内聚、低耦合”<br>⑵上周刚发布的 XXX 新版本文档还没看，今天 YYY 公司又发布了新框架，到底先学哪个？
其实，无论是哪种框架哪项技术都是解决实际业务需求的手段、方法，和武林中各门各派的武功招式是一样的，各有所长，各有各的独到之处。<br>大道至简，殊途同归，关键是先能“举一”，才能“反三”<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-190108.jpg)
{:.success}

# [React Native 从入门到原理](https://mp.weixin.qq.com/s?__biz=MzAxMzE2Mjc2Ng==&mid=2652155925&idx=2&sn=26dd67e06f62f8e0c3fa658cdecbc20d&chksm=8046d074b73159629ad39dd4da257728749790ead9e6dc97af2dee36eda24c20d1a3afacd036&mpshare=1&scene=2&srcid=0109p2GzM1Sd8JlAioeJVzqq&from=timeline&ascene=2&devicetype=android-26&version=2607033d&nettype=WIFI&abtest_cookie=BgABAAgACgALABIAEwAUAAcAnYYeACaXHgBamR4Am5keAJ2ZHgC2mR4A1JkeAAAA&lang=zh_CN&pass_ticket=legFXQzE26ou60CPZ70ahMM0G1RrEDlQElQwwdX4SMbDho8wcLVEBrBkDpbPsp4M&wx_header=1)

一✐RN入门到破门<br>启✵面向新手迈步高阶<br>渔✐背景✧问题➢源码✧机制<br>鱼✎<br>⑴React☞用简洁【只用JS构造页面】的语法高效【独创VirtualDOM增量刷新】绘制DOM的框架<br>⑵RN☞基于JS，具备动态配置和运行调试能力，面向前端开发者的移动端开发框架<br>评✎<br>⑴前端☞HTML构建布局+CSS控制样式+JS处理事件并刷新<br>⑵JavaScript是大哥（决策者），它提供了配置信息和逻辑的处理结果，指挥Native小弟（执行者）怎么做，而不是取代Native<br>⑶作者对ReactNative定位☞
利用脚本语言进行原生平台开发的一次成功尝试，降低了前端开发者入门移动端的门槛，一定业务场景下具有独特的优势，几乎不可能取代原生平台开发<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-190109.jpg)
{:.success}

# [React的虚拟DOM与diff算法的理解](https://mp.weixin.qq.com/s?__biz=MzI2MTM3MTg3Nw==&mid=2247483671&idx=1&sn=cdacdb3abe1924234969da89b3e179a0&chksm=ea5a239ddd2daa8b9eb589e295f3a1a4dd4965f1bbf0b1eb19b941bb20eb5c455ee5f7b189ae&mpshare=1&scene=2&srcid=0110vtxKBWgJ0V6cxOvLLOOy&from=timeline&ascene=2&devicetype=android-26&version=2607033d&nettype=WIFI&abtest_cookie=BgABAAgACgALABIAEwAUAAcAnYYeACaXHgBamR4Am5keAJ2ZHgC2mR4A1JkeAAAA&lang=zh_CN&pass_ticket=legFXQzE26ou60CPZ70ahMM0G1RrEDlQElQwwdX4SMbDho8wcLVEBrBkDpbPsp4M&wx_header=1)

一✐React，妙啊<br>启✵WhatWhyHow<br>渔✐追根溯源<br>鱼✎<br>⑴操作DOM☞原生慢【每一次操作均触发浏览器完整的渲染流程】✧虚拟快【批处理缓存操作一次性提交、Diff增量更新】<br>⑵境界☞开发者任性自由➢虚拟DOM和Diff算法保证高效渲染<br>⑶Diff算法☞传统【完全、最小、o(n³)】✧React【分层求异、同类同树、元素标识、o(n)】<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-190110.jpg)
{:.success}

# [React 源码分析(1)：调用ReactDOM.render后发生了什么](https://mp.weixin.qq.com/s?__biz=MzAxODE2MjM1MA==&mid=2651554047&idx=1&sn=47f989d86c1b2095f766ccd1f46f1c01&chksm=8025573eb752de28acc1ecfd1e74e2b94986e654a66e40bd42b859ea21f7df65df286d2f6119&mpshare=1&scene=2&srcid=0111acUR2GaeognuaKh7lia9&from=timeline&ascene=2&devicetype=android-26&version=2607033d&nettype=WIFI&abtest_cookie=BgABAAgACgALABIAEwAUAAcAnYYeACaXHgBamR4Am5keAJ2ZHgC2mR4A1JkeAAAA&lang=zh_CN&pass_ticket=legFXQzE26ou60CPZ70ahMM0G1RrEDlQElQwwdX4SMbDho8wcLVEBrBkDpbPsp4M&wx_header=1)

一✐React渲染源码<br>启✵敢想敢做<br>渔✐泛读理头绪➢调试精简Demo➢精读源码<br>鱼✎<br>⑴元素☞由React.createElement创建【JSX声明】，是描述DOM树的JS对象<br>⑵ReactDOM.render（元素， DOM容器）<br>⑶批处理☞避免重复渲染策略【何时存储更新、何时批量更新】<br>评✎<br>⑴事务☞wrapper【initialize➢perform【原逻辑】➢close】<br>⑵作者心路历程☞<br>硬着头皮一遍一遍看, <br>结合文章云里雾里的慢慢摸索, <br>不断更正认知.<br>看多了, 有大彻大悟的感觉, <br>零碎的认知开始连通起来, <br>逐渐摸清了来龙去脉<br>值得, 学到了不少<br>⑶[React 源码分析(2)：组件的初始渲染](https://mp.weixin.qq.com/s?__biz=MzAxODE2MjM1MA==&mid=2651554048&idx=2&sn=8e0e587a528efc0d7784c0a7c0e297ed&chksm=802556c1b752dfd783d5d28064754e095db4ea2e633bb21e0e5dc143e384c3e3790fce9ae53b&scene=7&ascene=0&devicetype=android-26&version=2607033d&nettype=WIFI&abtest_cookie=BQABAAgACgALABMAFAAGAJ2GHgAmlx4AWpkeAJuZHgCdmR4AtpkeAAAA&lang=zh_CN&pass_ticket=nlVXcWBLTK0PoeiT8CU4Iih7eNNiabXY7hae%2BilLV2MWix7NTJ182uOzvdQDt4ke&wx_header=1)<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-190111.jpg)
 {:.success}
 
# [【第1493期】 React 的今天和明天（图文版）第一部分](https://mp.weixin.qq.com/s?__biz=MjM5MTA1MjAxMQ==&mid=2651230857&idx=1&sn=5b13d8fb7f47b7e43bf0fd2955467350&chksm=bd494b0d8a3ec21b9587797214b018c6dd08f6c294d6c2575a8f3af967a5e7e6466954938af8&mpshare=1&scene=2&srcid=0114EbUIagA2nlJMFOP1ZnIa&from=timeline&ascene=2&devicetype=android-26&version=2607033d&nettype=WIFI&abtest_cookie=BgABAAgACgALABIAEwAUAAcAnYYeACaXHgBamR4Am5keAJ2ZHgC2mR4A1JkeAAAA&lang=zh_CN&pass_ticket=legFXQzE26ou60CPZ70ahMM0G1RrEDlQElQwwdX4SMbDho8wcLVEBrBkDpbPsp4M&wx_header=1)

一React今天<br>启✵正确的方向✧撒欢地奔跑<br>渔✐正反面<br>鱼✎<br>⑴手段☞简化、性能提升、开发者工具➢使命☞让开发者更容易地构建好的UI<br>⑵复用☞高阶组件、渲染属性<br>⑶副作用☞复用引来的包装地狱、庞大杂乱的组件、令人困惑的Class<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-190114.jpg)
{:.success}

# [[译] React 的今天和明天（图文版） —— 第二部分](https://juejin.im/post/5bfccbf8f265da61407e97b5)

一✐Hook代表React未来的期许<br>启✵事实上，我可以更进一步<br>渔✐问题➢提案➢对比<br>鱼✎<br>⑴包装地狱、庞大组件、困惑Class➢React没有原生提供一个比类组件更简单、更小型、更轻量级的方式来添加状态或生命周期<br>⑵Hook提供了除class外使用React众多特性新选择，是一个钩子函数<br>评✎<br>⑴Mixins是有害的✧实验证明☞带来的问题远比它解决的问题多<br>⑵RFC（request for comments），意味着无论是我们还是其他人想要对 React 做出大量变化或者添加新特性时,都需要撰写一个提案，提案里面需要包含动机的详情和该提案如何工作的详细设计<br>⑶React的Logo含义☞原子模型【由原子核和绕核运动的电子组成】、基于反应的（reactions）<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-190115.jpg)
{:.success}

# [React - Mixins 是“有害”的（Mixins Considered Harmful） #53](https://github.com/tcatche/tcatche.github.io/issues/53)

一✐【译】Mixins在代码里面下毒<br>启✵知错能改<br>渔✐问题+解决方案<br>鱼✎<br>⑴组件间代码共享➢组件组合<br>⑵毒☞隐式依赖、名称冲突、滚雪球式复杂性<br>⑶药☞嵌入导出函数、高阶组件、提取渲染逻辑组件<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-190116.jpg)
{:.success}

# [React 源码全方位剖析](http://www.sosout.com/2018/08/12/react-source-analysis.html)

一✐React台前幕后<br>启✵混个眼熟<br>渔✐一探细节究竟<br>鱼✎<br>⑴React只涉及组件定义、状态、生命周期、更新等跨平台一致处理，具体组件渲染由环境决定，故能衍生出ReactX（DOM、Native、字符串...）<br>⑵标签首字母大小写☞Babel将JSX转为createElement调用，大写直接作参数，小写转成字符串参数<br>⑶调和器渲染☞全量Stack➢ 分段Fiber<br>评✎<br>⑴ReactDOM.render（渲染入口） => legacyRenderSubtreeIntoContainer（把虚拟的dom树渲染到真实的dom容器中） => DOMRenderer.updateContainer（更新容器内容） => scheduleRootUpdate（开始更新） => scheduleWork（处理更新） => commitWork（提交更新）<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-190117.jpg)
{:.success}

# [React Native 三端同构实战](https://www.ibm.com/developerworks/cn/web/wa-universal-react-native/index.html)

一✐React举一反三<br>启✵抽象中间层的想象空间<br>渔✐架构✧对比<br>鱼✎<br>⑴React负责UI抽象和组件状态管理➢虚拟DOM中间层➢多端渲染<br>⑵原理☞对外暴露的API或组件映射到Web平台<br>⑶reactxp【微软、多平台、抹平差异】✧react-native-web【推特、RN+Web、灵活】<br>评✎<br>⑴场景☞Web页降级兜底、Web页分享到社交网络<br>⑵React Native 三端同构虽然无法实现 100% 和 React Native 环境运行一致，但能快速简单的转换大多数场景，以低成本的方式为你的项目带来收益<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积土（React）山-190118.jpg)
{:.success}
