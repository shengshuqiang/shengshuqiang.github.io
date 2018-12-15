---
layout: post
title: 进击ReactNative-纳百川
key: 20181215
tags:
  - 拈花指
  - ReactNative
  - React
  - 渲染引擎
  - 动态化框架
  - Weex
  - Flutter
---
<!-- 添加目录 http://blog.csdn.net/hengwei_vc/article/details/47122103 -->
<script src="/javascripts/jquery-2.1.4.min.js" type="text/javascript"></script>
<script src="/javascripts/toc.js" type="text/javascript"></script>
<script type="text/javascript">
$(document).ready(function() {
    $('#toc').toc();
}); </script>
<div id="toc"></div>
**承蒙大神指点，汇总之前发送的ReactNative相关朋友圈优秀文章，献给有需要的朋友**
{:.info}

<!--more-->
# [ReactNative For Android 框架启动核心路径剖析](https://cloud.tencent.com/developer/article/1004392)

一✐上代码之RN启动<br>三✐模块、流<br>启✵渲染突破口CoreModulesPackage<br>渔✐航海图<br>鱼✎<br>1）本地【运行时➢上下文】➢JS【运行时➢布局】➢本地渲染<br>2）CatalystInstance通过ReactBridge的JNI实现Java与JS通信<br>评✎<br>1）Builder(NativeModuleRegistry、JavaScriptModuleRegistry)➢ReactApplicationContext➢ReactPackage（CoreModulesPackage、MainReactPackage）➢CatalystInstance（ReactBridge、JsQueueThread）ReactContext➢JS Application<br>2）ReactPackage分为framework的CoreModulesPackage（如UIManagerModule，控制JS层DOM到Native View）和业务方可选的基础MainReactPackage
{:.success}

# [【Dev Club分享】React Native项目实战总结](https://mp.weixin.qq.com/s/poL4ZUxdpnolJq6gjm81mQ)

一✐拥抱并亲一口RN<br>三✐优劣势、踩填坑、新启发<br>渔✐黄沙百战穿金甲<br>鱼✎<br>1）原理☞内核JSC解析业务JSBundle，调用Native渲染页面<br>2）坑☞包大小、稳定性、安全性、兼容性、性能<br>3）优化☞分包、独立进程操作so<br>评✎<br>1）╭( ′• ㉨ •′ )╭☞警察叔叔！就是这个人！腾讯mango王☞拼理想，抱变化，走巅峰<br>2）前端写的标签怎么转换到终端的一个view?很简单，js层会将控件标签转换成js对终端UI模块的一次调用，如比像这种UIManager.creaeView或UIManager.removeView。无论是java到js还是js到java，中间都必须经过我们的jsc进行桥接。<br>3）掌握源码才有真正的主动权。很多问题，我们也都是去阅读源码发现的。其实源码并不复杂，里面很多知识沉淀，我个人是非常建议去读源码的。<br>4）rn目前是开源的。目前开发者社区都是高活跃，应该不会存在kpi项目之类的问题，并且动态更新肯定是趋势，我觉得可能会有其他方案，但暂时来看，rn还是相当优秀的解决方案的
{:.success}

# [深入源码探索 ReactNative 通信机制](https://mp.weixin.qq.com/s/F6bP2OtqHhdPFQ4dKL9Zmg)

一✐上RN源码<br>三✐模块、通信<br>渔✐源码剖析<br>鱼✎<br>1）Java➢CatalystInstance➢ReactBridge JNI➢Onload callFunction➢JSC➢BatchedBridge➢JS<br>2）JS➢MessageQueue➢【等待】➣Java<br>评✎<br>1）JS模块继承JavascriptModule，通过动态代理实现调用JS模块。由JavaScriptModuleRegistry统一管理。Java模块继承BaseJavaModule，在JS层存在同名文件识别为可调用的 Native。由NativeModuleRegistry统一暴露。<br>2）Java -> Js ：Java通过注册表调用到CatalystInstance实例，透过ReactBridge的jni，调用到Onload.cpp中的callFunction，最后通过javascriptCore，调用BatchedBridge.js，根据参数｛moduleID,methodID｝require相应Js模块执行。<br>3）JS -> Java：JS不主动传递数据调用Java。在需要调用调Java模块方法时，会把参数｛moduleID,methodID｝等数据存在MessageQueue中，等待Java的事件触发，再把MessageQueue中的｛moduleID,methodID｝返回给Java，再根据模块注册表找到相应模块处理。
{:.success}

# [从Android到React Native开发](https://www.jianshu.com/p/97692b1c451d)

一✐安卓端RN进阶<br>三✐通信、自定义、打包<br>渔✐老司机飙车<br>鱼✎<br>1）核心：封装组件，组件组成App<br>2）ReactInstanceManager【注册】ReactPackage【关联】NativeModule实现类【注解】被JS端调用<br>3）应用：继承ReactActivity或布局加入ReactRootView<br>评✎<br>1）Why？
因为现在许多主流的应用都有React Native的影子，它对比原生开发更为灵活，对比H5体验更为高效，而且跨平台的支持特性。<br>2）优劣势：
作为原生开发，因为React Native的特殊性，在入门的时候会比前端开发更慢一些，除非你会基础的javascript，ES6语法，React相关基础知识，不然这一层面确实相对会缺乏优势。
原生开发在React Native的优势是后期，React Native随着业务的增加，单纯的React Native时时无法满足需求，那时候熟悉原生、又掌握了React Native的你，一定能给出更好的解决方案。<br>3）红屏恐慌：
刚刚接触React Native，运行不起来是时有的事情，百度谷歌一个一个解决就好了，大部分时候都是忘记npm install，react-native link，ip不对，node服务没有重新启动等等，相信我，React Native会让从此讨厌上红色！
{:.success}

# [React Native For Android 架构初探](https://zhuanlan.zhihu.com/p/20259704?utm_source=wechat_timeline&utm_medium=social)

一✐安卓条理切入RN<br>三✐架构、过程、通信<br>渔✐大处着眼，小处着手<br>鱼✎<br>1）Java（开源库，封装）⇄C++（JSCore，桥）⇄JS（React，组件，生命周期、布局）<br>2）通信☞双端存模块配置表，通信指令为{moduleID，methodID，callbackID，args}，处理端匹配注册模块并调用<br>评✎<br>1）核心类☞ReactInstanceManager（平台）、ReactRootView（载体）、CatalystInstance（中枢）<br>2）React将UI分解成组件，废弃了模板，统一视图逻辑标签，使构建的视图更容易扩展和维护，Vitual Dom更是其提高性能的亮点，React 中的Dom并不保证马上影响真实的Dom，React会等到事件循环结束，利用diff算法，通过当前新Dom树与之前的Dom树作比较，计算出最小的步骤更新真实的DOM
{:.success}

# [Weex原理之带你去蹲坑](https://www.jianshu.com/p/ae1d7a2b0a8a)

一✐Weex入门到放弃<br>三✐原理、入门、坑<br>渔✐一知半解（十分钟入门）足以胜任跨界<br>鱼✎<br>1）Weex：Render（UI线程，渲染）⇄DOM（工作线程，解析、映射、添加、通知UI更新）⇄JS Bridge（工作线程，通信）<br>2）老大哥Cordova：提供丰富原生插件和打包，依旧WebView渲染，在JS和Native间搭建通信桥梁（访问端能力）<br>3）坑：语法、多页面、样式<br>评✎<br>1）作者在与RN对比表格中引擎弄反了，应该是：RN是JScore，Weex是V8。<br>2）作者在性能对比上，认为RN较好，Weex较弱，和之前看到文章（Weex踩在RN肩膀上做了很多优化，主打性能）不一样，有空考证一下。<br>3）直击Weex社区门可罗雀，开发者遇到问题三过门而进不去，可能是阿里要赚足专利费（时间）再普惠大众，希望不用等太久→_→
{:.success}

# [移动端跨平台开发的深度解析](https://www.jianshu.com/p/7e0bd4708ba7)

一✐跨平台一统天下<br>三✐ReactNative、Weex、Flutter<br>渔✐原理、现状与未来<br>鱼✎<br>1）RN：脸书、JS、JSCore、React、原生、JS⇄C++⇄Java/OC<br>2）Weex：阿里、JS、V8、Vue、原生、JS⇄C++⇄Java/OC<br>3）Flutter：谷歌、Dart、Engine、响应式、原生、Dart⇄Skia<br>跨平台目标：省钱、偷懒<br>评✎<br>1）框架选择标准：成熟度+生命力<br>2）RN优化方向：线程模型、异步渲染、桥接<br>3）Airbnb 在宣布放弃的文中，也对 react native 的表示了很大量的肯定，而使得他们放弃的理由，其实主要还是集中于项目庞大之后的维护困难，第三方库的良莠不齐，兼容上需要耗费更多的精力导致放弃<br>4）weex 比起react native，主要是在JS V8的引擎上，多了 JS Framework 承当了重要的职责，使得上层具备统一性，可以支持跨三个平台。总的来说它主要负责是：管理Weex的生命周期；解析JS Bundle，转为Virtual DOM，再通过所在平台不同的API方法，构建页面；进行双向的数据交互和响应
{:.success}

# [weex&ReactNative对比](https://zhuanlan.zhihu.com/p/21677103)

一：Weex✧ReactNative<br>三：可扩展、轻量级、高性能<br>渔：内涵♧实用<br>鱼：<br>1）组件化&数据绑定&VirtualDOM框架：Vue（小巧轻量，2w+☆）✧React（革命性2w+☆）<br>2）开发：浏览器playground✧native工程<br>3）政策：生态系统✧性能<br>评：<br>1）Weex的门槛相对比较低，更适合业务开发同学上手，简单就是不简单<br>2）阿里优势：像WebKit一样构建高质量的Weex内核<br>3）动态框架两极：菜单（拿来主义，人家有啥你用啥）▽ 改造（耗费巨大人力物力财力支持，各大派军备竞赛，需极有远见的战略眼光）<br>4）使用ReactNative确实也可以做到不错，但是最终我发现，自己其实是在做weex团队已经做的事情。<br>5）要想肩并肩或者超越，必须站在巨人的肩膀上造更牛B的轮子，Weex也是这么干的
{:.success}

# [Weex & ReactNative & JSPatch](http://awhisper.github.io/2016/07/22/Weex-ReactNative-JSPatch/)

一：动态框架PK<br>三：Weex、ReactNative、JSPatch<br>渔：大气势、超视距<br>鱼：<br>1.RN牛B灵活、模块扩展设计，天然支持扩展RN不足<br>2.Weex站在RN的肩膀上，贴心<br>3.JSPatch依托OC runtime jsbridge（动态、不需预埋还能修改预埋），normal jsbridge不可同日而语<br>评：<br>1.Weex和RN一脉相承，与JSPatch大相径庭<br>2.三端复用：Android，iOS，Wap。<br>3.Weex是真正三端复用，可以直接在浏览器开发预览<br>4.大胆的想法：以客户端的思路，依然用js脚本驱动native绘制☞私人定制无尽列表
{:.success}

# [【译】Google V8 引擎](https://blog.csdn.net/xiangzhihong8/article/details/74996757)

一：窥豹（V8）一斑<br>三：渲染、项目、V8♡JSCore<br>渔：一图胜千言<br>鱼：<br>1.WebKit：OS、WebCore、JS引擎（JSCore、V8）、WebKit Ports、WebKit嵌入式接口、第三方库（图形、网络、视频、数据）<br>2.渲染：Html—JS→DOM树—CSS→RenderObject树、RenderLayer树—绘图→图像
{:.success}

# [前端知识 - JavaScript 运行原理 & V8 引擎分析](https://mp.weixin.qq.com/s/xL_IXNHVhMDrnunQD_uNHg)

一：JS文韬&V8武略<br>三：寻址、栈帧、Event Loop、V8算法<br>渔：画龙点睛、高屋建瓴<br>鱼：<br>1. JS单线程，用户交互&操作DOM<br>2. 引擎=Memory Heap+Call Stack<br>3. V8更快：编译到机器代码、运行时观察记忆，预见未来优化点、提前内联、隐藏类、内联缓存、去优化保护、增量式标记回收<br>评：<br>1. 9+引擎：V8（Google系）、JScore（Safari）、Chakra（IE）、JerryScript（物联网）...<br>2. Google不愧是业界扛把子(︶︹︺)哼<br>3. 让我想起大学时光中热血沸腾的《黑客之道：漏洞发掘的艺术》 （和栈帧玩耍，游走系统边界）和第一个病毒（数组溢出栈帧，注入病毒）
{:.success}

# [JNI官方规范中文版——JNI程序设计总结](https://blog.csdn.net/a345017062/article/category/1256568)

一：JNI规范中文版<br>三：思想概述、原理浅析、策论讨论<br>渔：理论实践，举一反三<br>鱼：<br>1. JVM对象对native透明，操作依赖JNI函数<br>2. native/java耗时是java/java10倍<br>3. 启动器解析命令、加载JVM、Invocation API运行JAVA<br>评：<br>1. HelloWorld Demo：<br>HelloWorld.java（含native函数原型）→HelloWorld.class,（helloworld.h→helloworld.c（函数原型实现）→hello.so）→“Hello World !”<br>2. 博主☞农场♡老马☜，访问量 177万+ ，原创 167 ，你值得拥有！<br>3. 我空我得把Demo敲一遍[奋斗]
{:.success}

# [JNI技术规范](https://www.jianshu.com/p/88fbe27621fc)

一：JNI手册<br>三：机制、数据、函数<br>渔：名正（官）言顺（融）<br>鱼：<br>1. JNI统一、标准✧二进制兼容、高效、功能<br>2. JNI标准，有能力、机会在JVM运行本地库<br>3. Native和Java桥梁☞接口函数表JNIEnv<br>评：<br>1. 彩蛋：阿里热修复框架andFix原理和锁原理<br>2. JNI作为Java虚拟机的接口，衔接了Java层和native层。并且涉及到ClassLoader和JVM的内部实现等知识，全面了解JNI可以作为一把钥匙来打开JDK和JVM底层研究的大门。<br>3. 结合代码使用示例，严谨而不枯燥，达到了手册给人留下好印象的目的
{:.success}

# [使用 Java Native Interface 的最佳实践](https://www.ibm.com/developerworks/cn/java/j-jni/index.html)

一：JNI SOP☞知错就改<br>三：性能、正确性、方法<br>渔：MECE（相互独立、完全穷尽）<br>鱼：<br>1. 性能：缓存、数组、通信、界限、引用<br>2. 正确性：JNIEnv、异常、返回、数组、引用<br>3. 方法：规范、跟踪、-verbose:jni、转储、审查<br>评：<br>1. JNI双刃剑：牺牲Java安全，换取灵活和强大。<br>2. 有效集成已有代码资源（C/C++、Java、JavaScript……）的能力对于面向对象架构（SOA）和基于云的计算这两种技术的成功至关重要，这正是JNI的价值之一。
{:.success}

# [使用 Google V8 引擎开发可定制的应用程序](https://www.ibm.com/developerworks/cn/opensource/os-cn-v8engine/index.html)

一：用C++说“Hello Google V8”<br>三：概念、应用、原型<br>渔：有条不紊，循序渐进<br>鱼：<br>1. V8是C++编写，实现ECMAScript第五版<br>2. 对象指针handle、上下文context、数据Data及模板Template<br>3. 手写计算器原型（搭载V8引擎），轰~<br>评：<br>1. Node.js：使用 V8 引擎的基于事件的高效网络框架。<br>2. JS引擎支持脚本（JS）和宿主（C/C++）相互调用
{:.success}

# [Java中JNI的使用详解](http://www.520monkey.com/archives/128)

名：纯Java环境JNI入门<br>睛：数据转化、方法调用、引用<br>渔：Demo教学、生死看淡，不服就干<br>鱼：<br>1. Java调C说HelloWorld<br>2. Java和C相互调用，修改数据<br>3. C创建Java对象<br>评：<br>1. 共六篇，字字珠玑。详情http://www.520monkey.com/archives/category/java技术篇/page/3<br>2. 我跳过的舞，没有人能跳第二遍，包括我自己。——尼古拉斯•赵四（亚洲舞王）<br>3. 这正是我想要的纯Java环境JNI教科书式教学，而且我竟然看懂了。
{:.success}

# [JNI开发之实践篇](https://mp.weixin.qq.com/s/xhGnPpvuVFDbLo2I6GXVqA)

核：JNI开发之旅带你飞<br>点：C基础、Demo<br>渔：站如松（基础）、行如风（教科书式教学）<br>鱼：<br>1. 场景：驱动硬件、高性能、安全、C轮子<br>2. 编写：Java接口映射C实现（.java->.class->.h、.c->.so/.dll）<br>3. Java是入口，C等待吊起<br>评：<br>1. native ：本地语言 ，即开发本操作系统的语言。Windows是C/C++ 、Linux是C/C++，Android（Linux儿子）也是C/C++。<br>2. C程序员的传说：C语言在手，天下我有，妥妥的干到退休（如果你还活着）。几乎所有底层的东西都是用C写的,不仅能让你活的更好,还能让你吃嘛嘛香（彻底了解计算机底层原理）
{:.success}

# [深入理解JNI](https://juejin.im/entry/575588ff207703006c0487f1)

核：JNI抓重点（笔记）<br>点：JNI加载、函数映射、数据结构<br>渔：写作使人精确<br>鱼：<br>1. 注册：静态（函数名）、动态（JNINativeMethod数据结构）。<br>2. JNIEnv线程相关，通过进程JavaVM对象跨线程，能访问JVM数据结构和函数表。<br>3. JNI是连接底层Native世界和java世界的桥梁。<br>评：<br>1. 动态库文件格式：Linux是.so，Windows是.dll，OSX（苹果）是.jnilib。<br>2. String编码转换：Java是Unicode，C/C++是UTF。
{:.success}

# [NDK探究之旅《一》——对jni和NDK的认识](https://mp.weixin.qq.com/s/jePJ6aUoWRKtWImHPUTEOA)

核：大哥JNI和小弟NDK的那些事<br>点：JNI、NDK<br>渔：对于每一个细节都不想放过<br>鱼：<br>1. JNI（Java Native Interface，本地开发接口）实现Java和C/C++互相调用。<br>2. NDK（Native Delelop Kits，本地开发工具包）用作C/C++开发，提供动态库。<br>3. JNI三步：会C语言；懂JNI规范；熟NDK。<br>评：<br>1. 凭什么IPhone比Android丝滑：没有中间商赚性能差价。<br>2. Java（Android编程语言）由Dalvik虚拟机进行解释执行（优点能跨平台，缺点不能直接操作硬件）。<br>3. ObjectC（iPhone编程语言）能直接操作硬件，效率比较高。<br>4. Java执行流程：.java源文件—「Java编译器编译」—〉.class字节码文件—「Java虚拟机解释」—〉机器识别码。<br>5. Java语言面向对象，把状态和方法封装到对象里面，好比个体户，能独立处理各种输入输出；C语言面向过程，任何时候都是一步一步来，好比流水线上的工人，只用重复做一件事。
{:.success}

# [JNI初探](https://mp.weixin.qq.com/s/xu1wM2c7mCNRK8xI1M7P4A)

核：极简命令行实操JNI Demo<br>点：javac、javah、gcc<br>渔：简化操作、步步为营<br>鱼：<br>1. javac JNIDemo.java<br>2. javah JNIDemo<br>3. gcc -dynamiclib JNIDemo.c -o libNativeLibrary.jnilib<br>4. java JNIDemo<br>评：<br>1. 唯一比较纠结的是（浪费我2小时）：在Mac机器上，看到Android中JNI对应的是so文件，但命令行实测生成so文件运行报错“UnsatisfiedLinkError: no XXX in java.library.path”，各种尝试无果。后来换成 libNativeLibrary.jnilib没有问题。<br>2. 恍然大悟：上面命令是运行在我电脑上，是OSX系统，动态链接库文件要用jnilib格式；Android工程是运行在手机上，是Linux系统，要用so格式。[捂脸][耶]
{:.success}

# [【第1219期】从浏览器多进程到JS单线程，JS运行机制最全面的一次梳理](https://mp.weixin.qq.com/s/vIKDUrbuxVNQMi_g_fiwUA)
￼
核：强化浏览器知识体系钢甲<br>点：浏览器内核、JS引擎、Event Loop<br>渔：系统梳理、简单说、一张图总结<br>鱼：<br>1. 浏览器内核（渲染进程）作业：页面渲染，JS执行，事件循环。<br>2. GUI渲染线程与JS引擎线程互斥（因共享DOM操作）。<br>3. WebWorker，JS的“多线程”：运行JS的子任务，因为是render进程的线程，所有JS仍是单线程。<br>评：<br>1. 温馨提示：本文适合有一定经验的前端人员，新手请规避，避免受到过多的概念冲击。可以先存起来，有了一定理解后再看，也可以分成多批次观看，避免过度疲劳。<br>2. “此乃大唐之土，朝廷无官之处我便是官” — 《天地英雄》
{:.success}

# ￼[【第1293期】浏览器之美，你知道多少？](https://mp.weixin.qq.com/s/8NtfuoVFBp1LszXjVMB_Zw)

小弟初来乍到，路经宝地，希望留下一片云彩。其实我有个宏伟的，并为之付出一生的目标：我要成为现代化一枚最具有气质，最骚气的程序猿。。。
{:.success}

# ￼[【第1431期】图解浏览器的基本工作原理](https://mp.weixin.qq.com/s/cb8VJOmAB1Yrv-ct4jJ3JQ)

核：浏览器，你真的懂吗？<br>点：进程和线程、Chrome多进程架构、渲染<br>渔：开篇明义（三个希望）、图文并茂、言简意赅<br>鱼：<br>1. 打开网页流程：处理输入、开始导航、读取响应、查找渲染进程、确认导航、额外的步骤。<br>2. 渲染流程：构建 DOM、加载次级的资源、JS 的下载与执行、样式计算、获取布局、绘制各元素、合成帧。<br>3. 三棵树：模型树（DOM、Layout、 Layer）→呈现树（Presentation ）→渲染树（Render）。<br>评：<br>1. 一个好的程序常常被划分为几个相互独立又彼此配合的模块，浏览器也是如此。<br>2. 进程是独立团队，相互协作做更大的事。线程是队员，共享团队资源，相互配合共同完成团队任务。<br>3. Web事件分发原理是事件冒泡，Android也是。<br>4. 术语：Site Isolation（网站隔离）、非快速滚动区域（non-fast scrollable region）、栅格化、磁贴、光栅化。<br>5. 大前端阮一峰老师无处不在。[强]
{:.success}

# [浏览器的工作原理：新式网络浏览器幕后揭秘](https://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/)

核：浏览器天天在干啥？<br>点：DOM树和呈现树、布局绘制、CSS 框模型<br>渔：深入浅出<br>鱼：<br>1. 布局：增量（异步、dirty 位系统）和全局（同步）<br>2. 绘制：增量（dirty 区域合并）和全局<br>3. CSS 框模型：可视化元素都是矩形框，包含外边距区域、边框区域、内边距区域、内容区域<br>评：<br>1. 由Tali Garsiel 和 Paul Irish 共同创作，已翻译为八国语言（我的链接是中文的）<br>2. 昨天发的是上半部，这个是全集<br>3. Android与浏览器如出一辙（单线程、事件循环、测量、布局、绘制、事件流）<br>4. 学习浏览器的内部工作原理将有助于您作出更明智的决策，并理解那些最佳开发实践的个中缘由
{:.success}

# [【第914期】浏览器的工作原理：新式网络浏览器幕后揭秘（上）](https://mp.weixin.qq.com/s/ux2QduYgN_LMjLhmMg8ToA)

核：浏览器天天在干啥？塔利·加希尔大量的研究成果带你揭开神秘的面纱（上）<br>点：浏览器结构、呈现引擎、解析<br>渔：梳理浏览器结构，从源代码切入，柿子专找硬的（呈现引擎）捏<br>鱼：<br>1. 浏览器 = 用户界面 + 浏览器引擎 + 呈现引擎 + 网络 + 用户界面后端 + JavaScript 解释器 + 数据存储<br>2. 解析是以文档所遵循的上下文无关语法规则（由词汇和语法规则构成）为基础的。人类语言并不属于这样的语言。<br>3. 解析器生成器输入词汇和语法规则就可以帮助您生成解析器。WebKit使用了Flex和Bison。<br>评：<br>1. HTML 不是与上下文无关的语法），用HTML5 规范的算法自定义解析。<br>2. 呃，里面只有数以百万行计的 C++ 代码 …<br>3. 作者深入浅出[强]：让我们试着定义一个简单的数学语言，用来演示解析的过程。
{:.success}

# ￼[第989期】认识 V8 引擎](https://mp.weixin.qq.com/s/e5CwofqIqxbPNLL9AVFa8w)

核：V8引擎凭什么这么强<br>点：渲染、JavaScriptCore、V8<br>渔：抛出渲染原理和诉求、引出V8，对比JavaScriptCore看差距。<br>鱼：<br>1. V8引擎是一个JavaScript引擎实现（JavaScriptCore也是），使用C++开发，使命是更快更强，已达到运行速度媲美二进制程序。<br>2. V8 VS JavaScriptCore：V8引擎较为激进，青睐可以提高性能的新技术，而JavaScriptCore引擎较为稳健，渐进式的改变着自己的性能。<br>评：<br>1. JS引擎工作：源代码→抽象语法树→字节码→JIT(即时编译器)→本地代码。<br>2. JavaScriptCore是WebKit的默认引擎，在谷歌系列产品中被替换为V8引擎。<br>3. 启示：契合引擎原理，能编写高性能SOP。
{:.success}

# ￼[深入理解JavaScript的设计模式](https://mp.weixin.qq.com/s/Jtbsm3NN71BgakoBxmMZ1w)

一言蔽之：适合你的才是最好的。当然，不了解对方，根本谈不上是否合适。对象是这样，设计模式更是这样。<br>关键词：模块模式、单例模式、工厂模式、装饰模式。<br>渔：What、Why、少林三十六绝技<br>鱼：<br>1. 是什么：可重用解决方案、SOP、编程模板。<br>2. 工程师黑话（通用词汇表），帮助快速了解意图，专注解决业务问题，相信模式，有问题都是我姿势不对。<br>评：<br>1. JavaScript 没有访问修饰符，因此模块模式通过使用 iife（即时调用函数表达式）、闭包和函数作用域来模拟封装的概念。<br>2. 装饰模式用于扩展对象的功能，而不修改现有的类或构造函数。该模式可用于向对象添加特性，而不修改使用它们的底层代码。在JavaScript的弱类型语言中用起来特别爽。
{:.success}

# ￼[如何理解虚拟DOM?](https://www.zhihu.com/question/29504639?utm_source=wechat_timeline&utm_medium=social)

一言蔽之：Virtual DOM，Talk is cheap，Show me the code！<br>关键词：Virtual DOM算法实现<br>渔：抓住核心问题，将解决做到极致。写一个就知道了。<br>鱼：<br>1. 复杂应用的核心问题：维护状态，更新视图。<br>2. Virtual DOM 算法：<blockquote>a. JS构建DOM节点对象（类型、属性、子节点），生成JS对象状态树。<br>b. 状态变更时，构造一个新的状态树；比对新旧状态树，记录差异（REPLACE、REORDER、PROPS、TEXT）。<br>c. 将记录的差异映射到DOM树并且刷新（增量刷新）</blockquote>3. 复杂度O(n)：已知新旧顺序，求最小的插入、删除操作（移动=删除+插入）。<br>评：<br>1. 现实中的极致：适合的才是最好的。<br>2. Virtual DOM算法抽象是字符串的最小编辑距离问题（Edition Distance），最常见的解决算法是 Levenshtein Distance，通过动态规划求解，时间复杂度为 O(M * N)。<br>3. 但是Virtual DOM算法并不需要真的达到最小的操作，<br>4. 只需要优化一些比较常见的移动情况，牺牲一定DOM操作，让算法时间复杂度达到线性的（O(max(M, N))。这就是大局观[强]
{:.success}

# ￼[一起理解 Virtual DOM](https://www.jianshu.com/p/bef1c1ee5a0e)

一言蔽之：聪明的前端工程师拥抱变化，勇攀“极简够懒”的技术高峰<br>关键词：前端变化、Virtual DOM<br>渔🎣：从“数据变化与UI同步更新”这个角度来理解 Virtual DOM，坚持怀疑思辨，大胆整理求证。<br>鱼🐟：<br>1. 进化史：静态页面（RD手写Html）=》全量刷新动态页面（RD手动触发）=》增量刷新动态页面（RD手动触发）=》MV\*增量刷新动态页面（RD要按模板开发，MV\*自动触发）=》React全量比对，增量刷新动态页面（RD想怎么开发怎么开发，React自动触发）。<br>2. Virtual DOM数据结构：元素类型、元素属性、元素的子节点。<br>3. 价值观：技术本身不是目的，能够更好地解决问题才是王道。<br>评：<br>1. React🐮🐝 之处：回到起点，回到那个简单而美好的时候，在提供给开发者简单的开发模式的情况下，借助 Virtual DOM 实现了性能上的优化，以致于敢说自己“不慢”。[强]
{:.success}

# ￼[深入研究 Virtual DOM](https://juejin.im/entry/5857482a61ff4b0063c80d4d?)

一言蔽之：工业机器😱✨The Inner Workings Of Virtual DOM<br>关键词：Babel和JSX、Virtual DOM算法<br>渔🎣：举个🌰，一步一步的来分析每一个过程，所以不要觉得太复杂。<br>鱼🐟：<br>1. JSX：打破用代码写布局的噩梦，能在JS里面写Html，还能在Html里面这JS。<br>2. Virtual DOM秀操作（对界面元素的增删改查）。<br>3. 格局：没有找到任何一篇深入浅出的解释VDOM文章或者文档，所以决定自己写一篇。<br>4. 愿景：希望读者能更好的理解React和Preact，也为他们的发展做出一点贡献。
{:.success}

# [理解 Virtual DOM](https://github.com/y8n/blog/issues/5)

一言蔽之：手工制作Virtual DOM<br>关键词：状态管理、Diff算法、 性能对比。<br>渔🎣：抓住核心、基准测试。<br>鱼🐟：<br>1. 什么是Virtual DOM:一个对象、两个前提、三个步骤。<br>2. Virtual DOM的优势不在于单次的操作，而是在大量、频繁的数据更新下，能够对视图进行合理、高效的更新。（统计学范畴）<br>3. 对于Virtual DOM的思考远远没有结束。<br>评：<br>1. 通过手写业界巅峰框架，不仅能深入理解，而且明白，原来我也可以[胜利]
{:.success}

# [深入理解React、Redux](https://www.jianshu.com/p/0e42799be566)

React+Redux非常精炼，良好运用将发挥出极强劲的生产力。但最大的挑战来自于函数式编程（FP）范式。<br>关键词：React理解、Redux理解、FP🆚OO。<br>渔🎣：了解其独特的东西，置新技术于上下文中，从对比看到优势，直面挑战，挖掘极强劲的生产力。<br>鱼🐟：<br>1. React大胆的通过Virtual DOM想要构建一个理想的前端模型（听起来有点扯），不可思议的是Facebook真做到了（扛把子，你好！请收下我的膝盖）。<br>2. Redux大量应用FP，是MVC完美实现体，应用最大的挑战更多来自设计层面。<br>3. 通过架构设计，FP在生产力有着一定的优势（顶层设计做好，代码复用度极高，代码量少，否则难以演进）。同时对付复杂系统，能更好调测、定位问题。在新时代下，值得尝试。<br>评：<br>1. 软件领域里没有银弹，有好处一定有挑战。<br>2. React不走寻常路（模块化，如Html、CSS、JavaScript文件分离），让尔等知道什么才是高内聚（JSX在一个文件完美混合Html、CSS、JavaScript）。<br>3. 没先学会走就学跑从来不是问题，先问问自己是不是天才，如果不是，就要一步步来！--《寒战2》<br>4. 有FP经验的或者架构能力比较强，团队人员比较少、能力强，较强适合用react+redux。<br>5. 我们团队正在使用，值得借鉴[机智]
{:.success}

# [第637期】看漫画，学 Redux](https://mp.weixin.qq.com/s/hAICtTcbRttlG6h87IFGtw)

一言蔽之：Redux和他爸爸的故事——不破不立<br>关键词：区别Flux、角色介绍、数据流。<br>概要：<br>1. Redux作者格局：他想要的开发者工具包含了代码热替换（Hot Reload）和时间旅行（Time Travel）功能。<br>2. 容器型组件（Container component，专注管理）和展示型组件（Presentational component，专注搬砖）各司其职。<br>3. 视图层绑定，各组件通过Provider提供的connect直接绑定Store中的状态进行间接交互。<br>4. 数据牛：<blockquote>1. 用户和界面交互。<br>2. 界面发出一个Action指令（由ActionCreator 创建的格式化信息）。<br>3. Action触发Store自动Dispatch。<br>4. 根Reducer收到当前状态树和Actoin,切分状态树并且分流Action到子Reducer。<br>5. 子Reducer结合Action信息修改小块状态树副本，回传。<br>6. 根Reducer将收到的全部回传信息组合成新状态树，回传。<br>7. Store收到最新状态树重置状态，发更新通知。<br>8. 界面收到通知，获取更新的状态并重新渲染。<br>9. 用户看到更新后的界面。</blockquote>评：<br>1. Redux 解决的问题和Flux一样，但 Redux 能做的还有更多。
{:.success}

# [【第1329期】从设计师的角度看 Redux](https://mp.weixin.qq.com/s/PlOdvq3Xe4Heo8Xxf2gEfg)
￼
“设计不仅仅是外观感受。设计关乎于工作原理。”— 史蒂夫·乔布斯<br>关键词：状态管理、记录回放、白话文。<br>概要：<br>1. 什么才是应用状态（数据获取、分发、变化）管理？<br>2. 记录回放，天机（时间旅行、秋后算账制约随心所欲、偶现Bug必现重现……）无限。阳光普照，成长快乐。<br>3. 作为工具，Redux 自身也需要去权衡。（Last but not the least）<br>评：<br>1. 推荐理由：电脑爱好者科普，老少皆宜，童叟无欺。<br>2. 里面真的没有一行代码[强]<br>3. 不了解技术的设计师不是一个好产品经理[偷笑]
{:.success}

# ￼[4 张动图解释为什么（什么时候）使用 Redux](https://juejin.im/post/5a1791316fb9a0451238a0aa)

一言以蔽之：一分钟动图科普Redux “What、Why、When”<br>关键词：React单向数据流、共享状态、Redux<br>小结：<br>1. 过早优化是万恶之源。适合你的，才是最好的。<br>2. 当提升状态来共享变得逻辑杂乱，是时候了祭出Redux。
{:.success}

# [深入理解JavaScriptCore](https://mp.weixin.qq.com/s/H5wBNAm93uPJDvCQCg0_cg)

关键词：JavaScriptCore、Webkit、iOS<br>小结：<br>1. 软件= 界面（渲染引擎，如WebCore） + 逻辑（逻辑处理引擎，如JavaScriptCore）。界面是静态图片（小人书），通过逻辑操作界面元素动态放映（动画片）。<br>2. 计算机知识（编译原理、汇编、虚拟机……）串串香。<br>3. JavaScript一桶天下。<br>评：<br>1. 终于把一行打点换成多行展开[悠闲]<br>2. 用电话（电子计算机程序语言）写软件跟用人话（人类语言）写文章是相似的。<br>3. 我团大🐮 高作，深入浅出，电脑爱好者科普进阶☝ 
{:.success}
￼








