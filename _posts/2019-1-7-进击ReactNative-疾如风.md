---
layout: post
title: 进击ReactNative-疾如风
key: 20190107
tags:
  - 蜻蜓切
  - ReactNative
  - JNI
  - V8
  - 跨平台（Java、C、JavaScript）
---
<!-- 添加目录 http://blog.csdn.net/hengwei_vc/article/details/47122103 -->
<script src="/javascripts/jquery-2.1.4.min.js" type="text/javascript"></script>
<script src="/javascripts/toc.js" type="text/javascript"></script>
<script type="text/javascript">
$(document).ready(function() {
    $('#toc').toc();
}); </script>
<div id="toc"></div>

![进击ReactNative疾如风]({{ site.url }}/assets/进击ReactNative疾如风.png)<br>目前没有找到让我能达到Native开发熟练度的ReactNative文章，在@胡朝旭大神点拨“ReactNative涉及Native(Android/iOS)、C和JavaScript，业界大部分只会其一，少数会其二，全会的不是一般的人，而且他们也没有时间写这个”下，<br>**我的机会来了**<img src="http://www.linglingfa.net/new/face/233.gif"/><br>ReactNative涉及技术栈包含前端、客户端、跨平台通信，语言包含Java/OC、C、JavaScript。<br>直接看源码肯定是一头雾水【大神当我这句话没说】，<br>我尝试先从**“原理+实践，现学现做”**的角度**手写ReactNative**，加深理解。<br>目的是先学会怎么用，再去想为什么！
{:.success}
<!--more-->

# 情境(Situation)

## 约法三章

2. 本文还没有触及到ReactNative源码，属于我单方面的臆想，中间杂糅一些段子和啰嗦的三观，纯粹为博一笑。
3. 本文是在[AdvanceOnReactNative Demo](https://github.com/shengshuqiang/AdvanceOnReactNative)运行通过（环境为Mac电脑）的基础上写作，Native端从Java出发，目前不包含Object-C【因为我是Android开发，不会iOS】。
4. 本文是我解决问题的一种思路，会因为我目前水平局限，如果有其他更优解，望大神不吝赐教。

## 三问

1. **我是谁**：专注于移动互联网的Android野生程序猿。
2. **从哪来**：隶属于大前端（FontEnd、iOS、Android）战斗序列，和全栈小伙伴们（产品、研发（前端、后端）、测试）并肩作战。
3. **到哪去**：全栈工程师。

![三问](https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=859306704,3538571102&fm=26&gp=0.jpg)
![不管是谁](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1542416334451&di=3538ddd1b4c9af54fc91fa6dde0d88f2&imgtype=0&src=http%3A%2F%2Fn.sinaimg.cn%2Ftranslate%2F20170819%2FJF98-fykcypq0000579.jpg)
   
## 三看

### 向后看

2. 互联网时代，浏览器一统天下，大部分需求（聊天、购物、看剧、查资料等）用浏览器“即开即用”。只有少量工具级软件使用Native（Word、PPT、Excel、翻译词典等）开发。
3. 移动互联网时代，大部分需求有独立应用（美团、手机百度、天猫、京东、头条等），但是一定也有匹配的网页版，否则你无法用微信分享出去，而且用户手机一般会预装上面的巨无霸应用，基本涵盖吃喝玩乐，所以不大可能再安装其他同质应用，实在不行先在网页版里面体验一下。
4. 下载安装一个应用弊端：下载花时间、浪费流量、占空间、影响手机性能、泄露隐私等。
5. 用户是上帝，你能让上帝乖乖听你的么？

### 向外看

1. 业界领袖Facebook 15年推出ReactNative、目前已席卷全球，蒸蒸日上；Google 17年推出Flutter，炙手可热。国内巨擘微信小程序、阿里Weex、大众点评Picasso等。无一不是时代洪流的弄潮儿。<br>![](http://5b0988e595225.cdn.sohucs.com/images/20180809/8747c6b10f79405790cefec2e6271ca7.jpeg)
2. 客户端动态化方案技术演进：Web➢Hybrid➢插件化➢热更新➢ReactNative、Weex、小程序、Fultter。
3. 公司前端人数近年来快速增长，过去App中应用主要是Native实现（H5、Android、iOS三足鼎立），现在大部分被H5取代（H5和Android&iOS平分秋色），特别是创新性需求，H5快速试错、随时上线造就其终端王者地位。
4. 移动互联网瞬息万变（H5抢占一波又一波风口，根本不用等Native后援），手机硬件性能提升、通信网络不断升级、各种前端方案百花齐放（离线化、[AMP（ Accelerated Mobile Pages ，移动页面加速）](https://baike.baidu.com/item/amp/20623731?fr=aladdin)、[PWA（Progressive Web App，渐进式WEB应用）](https://segmentfault.com/a/1190000012353473)），为前端一统天下提供了物质基础。

### 向前看

1. “科学技术是第一生产力。”-- 邓爷爷
2. “劳动生产力是随着科学和技术的不断进步而不断发展的。” -- 马克思
2. 总之，动态化方案的出现，不是为了替代谁，更多是为了给用户更好的体验，同时让业务可以更快的迭代，并在不断的尝试中，给用户带来更好的产品。-- [再谈移动端动态化方案](http://www.sohu.com/a/246063323_505779)
2. “时势不可尽倚，贫穷不可尽欺，世事翻来覆去，须当周而复始。”-- [寒窑赋](https://baike.baidu.com/item/%E7%A0%B4%E7%AA%91%E8%B5%8B/7791451?fr=aladdin)

<br>![](https://pic4.zhimg.com/80/v2-2bd8921d36259b4687e7de4fe3f24d27_hd.png)
<br>△ -- 叶俊星 [美团旅行前端技术体系的思考与实践](https://zhuanlan.zhihu.com/p/29373613)

## 三思

### 思危

1. 前端工程师利用ReactNative、Fultter等技术开发应用，一个（前端工程师）顶两（Android和iOS工程师）。
2. 公司不会花钱养闲人（一个前端工程师就能完成的工作，干嘛要雇两个客户端工程师）。
3. JavaScript大行其道，一种语言，能跨端开发（前端浏览器应用和后端NodeJS服务）。前端工程师快速向全栈工程师演进。
4. 充分挖掘客户端计算力，对公司运营成本是巨大诱惑（想一想1亿个手机的计算都在服务端，要多少服务端机器，还要解决各种网络传输和负载均衡问题，这个钱能省干嘛不省）。

### 思退

1. 开放心态，拥抱变化。局限于客户端开发，下岗是早晚的事，除非你足够牛逼。
2. 知己知彼，是时候该停下客户端脚步，去了解前端技术栈了。

### 思变

1. 这是个客户端向大前端转型的机会，是迈向全栈工程师的起点。
2. 客户端工程师转大前端工程师是富有挑战性的工作，但好处不言而喻。
3. 客户端较前端有无法逾越的性能优势，单纯的前端开发无法发挥极致的用户体验，所以，天下终究是全栈工程师（客户端、前端、服务端）的天下。相较于前端工程师学习客户端技术，客户端工程师学前端门槛会低很多。
4. 技术殊途同归，原理算法如出一辙，技术始终是工程师的核心竞争力之一。 -- 我的导师@乔璞大神
5. “天下武功，无招不破，唯快不破。”-- 星爷的《功夫》
6. “假如祸事不可免的话，朕情愿它早点来。”-- 《康熙王朝》中康熙回答周培公[“万一吴三桂造反，皇帝的心思是什么”](https://v.qq.com/x/page/e0622kd63z0.html)

<br>![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1541914179348&di=051d28fb0214ccb408f02e2b9798fb23&imgtype=0&src=http%3A%2F%2Ffsv.cmoney.tw%2Fcmstatic%2Fnotes%2Fcapture%2F470430%2F20170217160601521.jpg)
<br>△ -- [《大明王朝1566》](http://www.sohu.com/a/153324482_497602)

## 三愿

3. 寻门而入：ReactNative道理我都懂，但是仍无从下手？
4. 闭门合辙：手工自作简易版ReactNative，深入分析工业机器ReactNative实现原理（驱动Virtual DOM和Native通信原理），ReactNative文章、高阶组件。
4. 破门而出：ReactNative是前端的一条沟，掉进去了，叫挫折，爬出来了，叫成长。

# 冲突(Complication)

1. 我是专注于移动互联网的北漂打工仔，前端的活我看到，但没有学过，不会做，局促于Android客户端开发。
2. 我团大面积铺开ReactNative，作为老油条，我不入地狱谁入地狱。如其束手就擒，不如奋起一搏。
2. 短兵相接半月左右，一度凭借自己多年累积的方法论和人生观，直面杀入ReactNative源码，硬着头皮看了3遍大牛郭孝星的[ReactNative源码篇](https://github.com/guoxiaoxing/react-native/tree/6f3c65c62e9fc30826f1fa3ac907c6bcaadf9995/doc/ReactNative%E6%BA%90%E7%A0%81%E7%AF%87)，JavaScript、C++、Android来回切换，各种高能预警，磕得我一脸狗血，整得我晕头转向、稀里糊涂，最终身体不适，差点扑街。
3. 直接debug源码，JavaScript层代码没法看，一个套着一个，看了半天都没有找到门在哪里；Android层算找到了消息通信的往来入口。
2. 卒子的命运，注定是身不由己。“今亡亦死,举大计亦死,等死,死国可乎?”-- 《陈涉世家》
3. 最朴实的方法，敢于直面人生，我开始努力切入ReactNative源码了，目标是达到客户端熟练度。

<br>![理想VS现实](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1541954176177&di=6659cd539fe2660894fb4c820186bd60&imgtype=0&src=http%3A%2F%2Fimg.mp.itc.cn%2Fupload%2F20161021%2F5479e3439be145e7b925a0644e3d13a7_th.jpeg)

# 疑问(Question)

## [5W2H分析法](https://baike.baidu.com/item/5W2H%E5%88%86%E6%9E%90%E6%B3%95/8111597?fr=aladdin)

1. What：目标是什么，核心问题是什么？
2. Why：为什么要这么做，业界的现状是什么？
3. Who：都有哪些角色，谁是甲方，谁是乙方，受益者是谁？
4. When：什么时候的事，什么时机切入？
5. Where：场景是什么？
6. How：怎么做到的（实现原理）？
7. How Much：有什么价值，投入产出比是多少，怎么衡量？

<br>![5W2H](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1546775431818&di=bbede93b18b3018b73eaf9f7d52639d2&imgtype=0&src=http%3A%2F%2Fimg3.doubanio.com%2Fview%2Fnote%2Flarge%2Fpublic%2Fp36908353.jpg)
<br> ![灵魂拷问三连]({{ site.url }}/assets/灵魂拷问三连.jpg) 

# 答案(Answer)

## 三原则

[“有理、有利、有节”是毛泽东在《目前抗日统一战线中的策略问题》中提出的对敌战争“三原则”](https://www.douban.com/note/687869850/)。

### 有理

2. **价值**：不忘初心，打破客户端工程师边界，超越自我，强化核心竞争力。<br>“[开拓万里之波涛，布国威于四方。](http://jz.chinamil.com.cn/n2014/tp/content_5987779.htm)”--明治天皇
1. **目标**：以ReactNative作为演进全栈工程师的契机，深入理解背后的原理，扩大自我生存空间。<br>“设计不仅仅是外观感受。设计关乎于工作原理。”--史蒂夫·乔帮主
2. **问题**：
    * **成本大**：双端（Android和iOS）独立开发一份，关键是功能还一样，能不能一个人干两个人的活？
    * **周期长**：依赖发版（一般一个月一版，跟不上移动互联网的风口），需要用户升级（没事瞎升级，整一堆根本用不上的乱七八糟功能，除了浪费我流量，就是让手机更卡了）。
    * **代价高**：热修复不成熟，难以及时止损，好羡慕后台和前端就可以“偷偷地、悄悄地”把bug改了，客户端即使发现了问题，等到发版修复，再小的问题也闹得满城风雨了。<br>“且欲[防微杜渐，忧在未萌](https://baike.baidu.com/item/%E9%98%B2%E5%BE%AE%E6%9D%9C%E6%B8%90/601280?fr=aladdin)。”-- 宋书·吴喜传
    * **体重胖**：各家应用的布局都是构建生态平台，打造巨无霸应用，实现一桶天下。随着业务迭代和时间这把杀猪刀，应用体重无可奈何的飙升。让我减肥?开玩笑!你知道我为这身材花了多少钱吗?

<br>![站在巨人的肩膀上](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1541952301987&di=4bcc3298ef66ff8424dbf1d206f96261&imgtype=0&src=http%3A%2F%2F5b0988e595225.cdn.sohucs.com%2Fimages%2F20180829%2Fc640fda97a5147c78f38a6e505ed335a.jpeg)

2. **途径**：MECE分析法（Mutually Exclusive Collectively Exhaustive，“相互独立，完全穷尽”）。穷举所有的可能性，比方我拿起杀猪刀，假装庖丁解牛，手工制作一个简易版ReactNative。

<br>![MECE](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1541955092281&di=b3af2ad5edfe6e7daa51cb0d777ab8fe&imgtype=0&src=http%3A%2F%2Fs6.sinaimg.cn%2Fmw690%2F003GHWbUzy78T5mxcbj65%26690)

### 有利

![升职加薪走上人生巅峰](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1542542981&di=d8788816752e07f582032420ae21fb52&imgtype=jpg&er=1&src=http%3A%2F%2F5b0988e595225.cdn.sohucs.com%2Fimages%2F20171208%2F21211fa948284d8d98a7ffb3bf149021.jpeg)

1. **时间**：洞悉ReactNative底层原理，有助于更快更好地完成工作，节约时间。是不是面对ReactNative红屏不知所措？遇到问题没法深入，不得不肤浅的换一种解决方案试试？网上资料太少，知其然不知其所以然，没法随心所欲、游刃有余？
1. **加薪**：一分耕耘一分收获。
1. **升职**：公司不会亏待你的。
1. **信心**：看，XXX哥果然不是盖的；我感觉我能搞定XXX。
2. **荣誉**：🐂🐝、💯、👏、🌹、🎖。

### 有节

1. 站在巨人的肩膀上，不要重复造轮子。<br>![站在巨人的肩膀上](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1541949456499&di=e3c64252bb027cd5911741a7aea6a304&imgtype=0&src=http%3A%2F%2Fimgsrc.baidu.com%2Fimage%2Fc0%253Dshijue1%252C0%252C0%252C294%252C40%2Fsign%3D0fd751a79fcad1c8c4b6f46417570d7c%2Fa686c9177f3e6709f6a4047731c79f3df9dc5595.jpg)
2. 战线不要太长，明天的你很轻松做到的，现在做可能有点吃力，毕竟我们还在成长，还有明天。
3. 把握分寸，毕竟精力有限。<br>![把握分寸]({{ site.url }}/assets/把握分寸.jpg)

## 庖丁解牛

### 磨刀霍霍

1. 模仿最小可用产品（Minimum Viable Product， MVP）理念，手工制作一个对角棋AI程序，JSX控制AI算法和UI，Java控制台输出棋谱和对弈交互。<br>“世事如棋盘，世人若棋子。这年头，谁利用谁还不一定！”-- 《画江湖》李克用
<br>![最小可用产品（Minimum Viable Product， MVP）](http://a.36krcnd.com/nil_class/0b33832f-1dbc-4471-a550-f90f2b26d00f/19DA.tmp.jpg)

### 一刀两断

1. 软件 = 逻辑（逻辑处理引擎，如JavaScriptCore） + 界面（渲染引擎，如WebCore） 。界面是静态图片（像连环画），通过逻辑操作界面元素动态放映（像动画片）。
2. 逻辑处理引擎跟进用户输入进行逻辑处理，输出界面描述文本给渲染引擎。
2. 渲染引擎解析界面描述文本（Html、JSX、布局代码等）和图片资源，调用系统图形API（OpenGL等），刷新界面。

### 吞刀吐火

1. ReactNative中JavaScript代码处理逻辑，生成界面描述文本给到Native，Native根据界面描述文本协议，解析成界面对象，调用图形API刷新界面。
2. Java如何运行JavaScript？

#### Java和JavaScript直接通信

☞**上网搜索个Demo，拿来主义**
{:.info}

1. Java可以通过ScriptEngine直接调用知悉JavaScript代码。 
2. 参考[java与js通信](https://blog.csdn.net/f6991/article/details/9312791)、[java ScriptEngine 使用](https://www.cnblogs.com/zhangtan/p/8110210.html)实现Java运行JS代码HelloScriptEngine.java
3. 核心代码示例如下（完整代码详见[HelloScriptEngine.java](https://github.com/shengshuqiang/AdvanceOnReactNative/blob/master/learn/java/HelloScriptEngine.java)）

 ```
public void testJSCore2() {
    // 初始化JS脚本引擎
    ScriptEngineManager factory = new ScriptEngineManager();
    ScriptEngine engine = factory.getEngineByName("JavaScript");
    // 注入全局变量，a = 4， b = 6
    engine.put("a", 4);
    engine.put("b", 6);
    try {
        // 运行JS代码字符串
        // 定义求最大值函数max_num
        // 执行max_num(a,b)，a 和 b对应上面设置的全局变量
        Object maxNum = engine.eval("function max_num(a,b){return (a>b)?a:b;}max_num(a,b);");
        System.out.println("max_num:" + maxNum);
    }
    catch (Exception e) {
        e.printStackTrace();
    }
}
// 运行命令行：
shengshuqiangdeMacBook-Pro:java shengshuqiang$ javac HelloScriptEngine.java 
shengshuqiangdeMacBook-Pro:java shengshuqiang$ java HelloScriptEngine
hello,SSU
2
./hello.js
hello js
19
handleMsg:我服SSU,一个牛逼💯的人
我服SSU,一个牛逼💯的人
max_num:6
 ```
 
 本来想简化点复杂度，避开C++这一层。<br>但是无法做到双端通信和共享上下文，无奈ㄟ( ▔, ▔ )ㄏ此路不通，只能回到起点 。<br> **躲不了，Just Do C++** ！
 {:.warning}
 
#### Java和C++通信

☞**JNI**（JAVA Native Interface）：Java本地接口,它是一种协议,提供一套编程框架,让Java和本地语言(C/C++)能相互调用。{:.info}

6. 通过[JNI初探](https://mp.weixin.qq.com/s/xu1wM2c7mCNRK8xI1M7P4A)，跑通第一个Java和C++调用示例HelloJNI.java。
3. 核心代码示例如下（完整代码详见[HelloJNI.java](https://github.com/shengshuqiang/AdvanceOnReactNative/blob/94449015ded88b774976227f333b74c51a99caf0/learn/java/HelloJNI.java)）

 ```
 // HelloJNI.java
 public class HelloJNI {
    static {
        System.loadLibrary("NativeLibrary");
    }
    public static native void sendMsg(String msg);
    public static void main(String[] args) {
        System.out.println("Hello SSU!");
        sendMsg("SSU");
    }
}
// HelloJNI.cpp
JNIEXPORT void JNICALL Java_HelloJNI_sendMsg(JNIEnv * env, jclass cls, jstring jstr) {
    const char* str = env->GetStringUTFChars(jstr, NULL);
    printf("Java_HelloJNI_sendMsg:%s\n", str);
    env->ReleaseStringUTFChars(jstr, str);
}
 // 运行命令行：
shengshuqiangdeMacBook-Pro:java shengshuqiang$ javac HelloJNI.java
shengshuqiangdeMacBook-Pro:java shengshuqiang$ javah HelloJNI
shengshuqiangdeMacBook-Pro:java shengshuqiang$ gcc -dynamiclib HelloJNI.cpp -o libNativeLibrary.jnilib -I ./include/
shengshuqiangdeMacBook-Pro:java shengshuqiang$ java HelloJNI
Hello SSU!
Java_HelloJNI_sendMsg:SSU
 ```
 
**JNI进阶**

7. [JNI规范](https://blog.csdn.net/a345017062/article/category/1256568)
8. [JNI规范配套代码示例JNI Example](https://github.com/leeowenowen/jni-examples)

![JNI允许JAVA和本地代码间的双向交互](https://img-my.csdn.net/uploads/201210/14/1350177645_3728.gif)<br>△ -- 农场老马[JNI规范](https://blog.csdn.net/a345017062/article/category/1256568)

#### JavaScript和C++通信

☞**JS引擎**有JSCore和V8，我选择<img src="https://v8.dev/_img/v8.svg" alt="V8" width="5%" height="5%" />。
{:.info}

1. [V8源码](https://github.com/v8/v8)，当然，我是看不懂的。
2. 先找几篇文章学习一下，知其然
    3. [使用 Google V8 引擎开发可定制的应用程序](https://www.ibm.com/developerworks/cn/opensource/os-cn-v8engine/index.html)
    4. [认识 V8 引擎](https://zhuanlan.zhihu.com/p/27628685)
    4. [浅读V8——强大的JavaScript引擎](https://www.jianshu.com/p/332c15fd7c7d)
    5. [V8引擎在C++程序中使用简介](https://www.cnblogs.com/wolfx/p/5920141.html)
2. 这里，我掉到一个坑里面了，就是自己尝试[编译V8源码](https://v8.dev/docs/build)，发现gclient根本下载不下来，一度泪奔┭┮﹏┭┮
2. 纠缠了一段时间后，我抖了个小机灵，github上面搜索可运行的V8Demo代码。
3. 人间只有真情在，还真给我找到一个可用的 依赖V8动态库运行JS代码[coderzh/v8-demo](https://github.com/coderzh/v8-demo)，里面是直接引用编译好的v8动态链接库libv8.dylib、libv8_libplatform.dylib。
4. 开启了我盲人摸象的第一步。里面用到了CMake和Make编译运行代码，我又是第一次用到。

```
// main.cc
{
    // 初始化上下文
    v8::Isolate::Scope isolate_scope(isolate);
    v8::HandleScope handle_scope(isolate);
    v8::Local<v8::Context> context = v8::Context::New(isolate);
    v8::Context::Scope context_scope(context);

    // 传入简单JS字符串拼接代码"Hello, World!"
    v8::Local<v8::String> source = v8::String::NewFromUtf8(isolate, "'Hello' + ', World!'", v8::NewStringType::kNormal).ToLocalChecked();

    // Compile the source code.
    v8::Local<v8::Script> script = v8::Script::Compile(context, source).ToLocalChecked();

    // Run the script to get the result.
    v8::Local<v8::Value> result = script->Run(context).ToLocalChecked();

    // Convert the result to an UTF8 string and print it.
    v8::String::Utf8Value utf8(isolate, result);
    printf("%s\n", *utf8);
}
// 命令行
// 直接运行会有个小问题，运行会报错“dyld: Library not loaded: /usr/local/lib/libv8.dylib”，找不到v8动态链接库
// 解决方案是把libs下动态链接库文件直接复制到/usr/local/lib即可（cp ../libs/* /usr/local/lib） 。
shengshuqiangdeMacBook-Pro:AdvanceOnReactNative shengshuqiang$ mkdir build
shengshuqiangdeMacBook-Pro:AdvanceOnReactNative shengshuqiang$ cd build/
shengshuqiangdeMacBook-Pro:build shengshuqiang$ cmake ..
-- The C compiler identification is AppleClang 10.0.0.10001145
-- The CXX compiler identification is AppleClang 10.0.0.10001145
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /Users/shengshuqiang/ideal/HelloJSCore/AdvanceOnReactNative/build
shengshuqiangdeMacBook-Pro:build shengshuqiang$ make
Scanning dependencies of target v8_demo
[ 50%] Building CXX object CMakeFiles/v8_demo.dir/src/main.cc.o
[100%] Linking CXX executable v8_demo
[100%] Built target v8_demo
shengshuqiangdeMacBook-Pro:build shengshuqiang$ ./v8_demo
dyld: Library not loaded: /usr/local/lib/libv8.dylib
  Referenced from: /Users/shengshuqiang/ideal/HelloJSCore/AdvanceOnReactNative/build/./v8_demo
  Reason: image not found
Abort trap: 6
shengshuqiangdeMacBook-Pro:build shengshuqiang$ cp ../libs/* /usr/local/lib
shengshuqiangdeMacBook-Pro:build shengshuqiang$ ./v8_demo
我服SSU,一个牛逼💯的人
```
![v8-say-hello]({{ site.url }}/assets/v8-say-hello.png)

5. coderzh/v8-demo只能简单运行JS字符串拼接代码，虽然看到了曙光，但距离旭日还遥不可及。
6. 再一次被命运女神眷顾，[嵌入V8的核心概念](https://www.jianshu.com/p/8cd3cc2a1630)和[嵌入V8的核心概念1](https://www.jianshu.com/p/76a44522549a)助我跑通了[v8源码中有三个例子](http://github.com/v8/v8/tree/master/samples)（hello-world.cc、process.cc、shell.cc），这下前进了关键性的一大步，这是一个里程碑。
    7. hello-world.cc：简单打印出“hello world”。
    8. process.cc：演示C++如何调用JavaScript函数。
    9. shell.cc：演示如何暴露native function到JavaScript。

```
// hello.js
// 对角棋程序
// 初始化，主要配置棋局输出字符
init = () => {
	RedArmy = '🔥'
	BlackArmy = '💧'
	EmptyArmy = '🎄'
	Board = [EmptyArmy, BlackArmy, BlackArmy, BlackArmy, EmptyArmy, EmptyArmy, EmptyArmy, RedArmy, RedArmy, RedArmy]
	Location = ['⑩','①','②','③','④','⑤','⑥','⑦','⑧','⑨']
	Left2Right = '一'
	Top2Bottom = '｜'
	LeftBottom2RightTop = '╱'
	LeftTop2RightBottom = '╲'
	Space = '\t'
	stepIndex = 0

	printBoard()
	printMsg("请🌴🐑");
}

// 构建棋子字符
buildChess = index => (Location[index] + Board[index])

// 打印棋盘字符
printBoard = () => {
	let str = buildChess(1) + Space + Left2Right + Space + buildChess(2) + Space + Left2Right + Space + buildChess(3) + '\n'
	+ Top2Bottom + Space + LeftTop2RightBottom + Space + Top2Bottom + Space + LeftBottom2RightTop  + Space + Top2Bottom + '\n'
	+ buildChess(4) + Space+ Left2Right + Space + buildChess(5) + Space + Left2Right + Space + buildChess(6) + '\n'
	+ Top2Bottom + Space + LeftBottom2RightTop + Space + Top2Bottom + Space + LeftTop2RightBottom  + Space + Top2Bottom + '\n'
	+ buildChess(7) + Space + Left2Right + Space + buildChess(8) + Space + Left2Right + Space + buildChess(9) + '\n'
	printMsg(str)
}

// 打印接口，因为console.log在V8中无法直接使用，这里调用C++打印函数print
printMsg = msg => {
	// console.log(msg)
	print(msg)
}

// 接收输入下棋指令
// 指令字符串为两位整数，第一位表示源位置，第二位表示目标位置
receiveOrderStr = orderStr => {
	if (orderStr && orderStr.length == 2) {
		const srcIndex = parseInt(orderStr.charAt(0))
		const destIndex = parseInt(orderStr.charAt(1))
		// 输入指令有效性判断
		if (srcIndex >= 1 && srcIndex <= 9 && destIndex >= 1 && destIndex <= 9) {
			handleOrder(srcIndex, destIndex)
		} else {
			printMsg('🈲🚫犯规⛔️输入指令【'+ orderStr + '】错误，应该为两位1-9的数字，源位置目标位置')
		}
	} else {
		printMsg('🈲🚫犯规⛔️输入指令【'+ orderStr + '】错误，应该为两位1-9的数字，源位置目标位置')
	}
}

// 处理下期指令
handleOrder = (srcIndex, destIndex) => {
	printMsg('handleOrder源位置' + srcIndex + '棋子' + Board[srcIndex] + '移动到目标位置' + destIndex + '棋子' + Board[destIndex])
	// 目标位置为空
	if (Board[destIndex] === EmptyArmy) {
		// 双方轮流下棋
		const army = (stepIndex % 2 === 0 ? BlackArmy : RedArmy)
		if (Board[srcIndex] === army) {
			Board[destIndex] = army
			Board[srcIndex] = EmptyArmy
			printMsg('Step' + (stepIndex++) + ': ' + srcIndex + '➡️' + destIndex)
		} else {
			printMsg('🈲🚫犯规⛔️源位置' + srcIndex + '不是你的棋子' + Board[srcIndex])
		}
	} else {
		printMsg('🈲🚫犯规⛔️目标位置' + destIndex + '已有棋子' + Board[destIndex])
	}
	printBoard()
}
shengshuqiangdeMacBook-Pro:c shengshuqiang$ mkdir build
shengshuqiangdeMacBook-Pro:c shengshuqiang$ cd build
shengshuqiangdeMacBook-Pro:build shengshuqiang$ cmake ..
-- The C compiler identification is AppleClang 10.0.0.10001145
-- The CXX compiler identification is AppleClang 10.0.0.10001145
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /Users/shengshuqiang/ideal/HelloJSCore/AdvanceOnReactNative/learn/c/build
shengshuqiangdeMacBook-Pro:build shengshuqiang$ make
Scanning dependencies of target learn
[ 50%] Building CXX object CMakeFiles/learn.dir/shell.cc.o
[100%] Linking CXX executable learn
ld: warning: directory not found for option '-L/Users/shengshuqiang/ideal/HelloJSCore/AdvanceOnReactNative/learn/c/libs'
[100%] Built target learn
shengshuqiangdeMacBook-Pro:build shengshuqiang$ ./learn
> load('../../js/hello.js')
> init()
①💧   一      ②💧   一      ③💧
｜      ╲       ｜      ╱       ｜
④🎄   一      ⑤🎄   一      ⑥🎄
｜      ╱       ｜      ╲       ｜
⑦🔥   一      ⑧🔥   一      ⑨🔥

请🌴🐑
> handleOrder(1,5)
handleOrder源位置1棋子💧移动到目标位置5棋子🎄
Step0: 1➡️5
①🎄   一      ②💧   一      ③💧
｜      ╲       ｜      ╱       ｜
④🎄   一      ⑤💧   一      ⑥🎄
｜      ╱       ｜      ╲       ｜
⑦🔥   一      ⑧🔥   一      ⑨🔥

> quit()
```
### 操刀必割（Do It）

☞**思路**：<br>1. java端启动js初始化运行环境<br>2. js端输出游戏规则和棋局，等待java端输入<br>3. java端输入指令，js刷新棋盘并且对弈，直至高下立判<br>4. java端只是用户输入和棋盘展示终端，c端只负责透传消息，js端负责逻辑处理
{:.info}

因为CMake和Make不懂，在使用CMake将引入V8的HelloJNI.cc打成动态链接库始终过不去。<br>在我进击ReactNative最黑暗的时刻，是@罗佳妮女神、@雷地球和@张千一大牛指点迷津，我才能够更上一层楼，O(∩_∩)O谢谢你们的帮助
 {:.warning}
 
1. 剩下的只是花点时间和三个键：Ctrl、C、V
2. 成功构建手写[ReactNative简易版](https://github.com/shengshuqiang/AdvanceOnReactNative/blob/94449015ded88b774976227f333b74c51a99caf0/do/HelloJNI.java)，虽然没有React，但是已经初具的跨平台交互能力。

 ```
 // HelloJNI.java
 public class HelloJNI {
    private Scanner scanner = new Scanner(System.in);

    static {
        System.loadLibrary("HelloJNI");
    }

    public HelloJNI() {
        scanner.useDelimiter(System.getProperty("line.separator"));
    }

    public native void load(String jsBundle);
    public native void sendOrder(String orderStr);

    public static void receiveMsg(String msg) {
        System.out.println("receiveMsg:\t" + msg);
    }
    public static void main(String[] args) {
        System.out.println("Hello SSU!");
        HelloJNI helloJni = new HelloJNI();

        String jsBundle = null;
        try {
            jsBundle = readFile("../hello.js");
        } catch(IOException exception) {
        }
        if (null != jsBundle) {
            helloJni.load(jsBundle);
        }
    }

    /**
     * 通过字符流读取文件中的数据
     * @throws IOException
     */
     public static String readFile(String path) throws IOException{
         // 注意这里的不同，File.separator是分隔符，这里指明绝对路径，即D盘根目录下的abc.txt文件
         File file = new File(path);
         // 如果文件不存在则创建文件
         if (!file.exists()) {
             file.createNewFile();
         }
         InputStream inputStream = new FileInputStream(file);
         // 这里也有不同，可以根据文件的大小来声明byte数组的大小，确保能把文件读完
         byte[] bs = new byte[(int)file.length()];
         // read()方法每次只能读一个byte的内容
         inputStream.read(bs);
         inputStream.close();
         String fileStr = new String(bs);
         // System.out.println("##JAVA##\n" + fileStr);
         return fileStr;
     }

     public String waitForInputOrder() {
         if (scanner.hasNext()) {
             String order = scanner.next();
             // System.out.println("##JAVA##\n" + "waitForInputOrder:\t" + order);
             return order;
         }
         return null;
     }
}
 // HelloJNI.cc
 void Init(JNIEnv* env, jobject jobj, const char* str) {
    // 初始化v8引擎
    char* argv = "hello";
    int argc = 1;
    v8::V8::InitializeICUDefaultLocation(argv);
    v8::V8::InitializeExternalStartupData(argv);
    std::unique_ptr<v8::Platform> platform = v8::platform::NewDefaultPlatform();
    platform = v8::platform::NewDefaultPlatform();
    v8::V8::InitializePlatform(platform.get());
    v8::V8::Initialize();
    v8::V8::SetFlagsFromCommandLine(&argc, &argv, true);
    v8::Isolate* isolate;
    v8::Isolate::CreateParams create_params;
    create_params.array_buffer_allocator = v8::ArrayBuffer::Allocator::NewDefaultAllocator();
    isolate = v8::Isolate::New(create_params);
    {
        // 初始化js上下文
        v8::Isolate::Scope isolate_scope(isolate);
        v8::HandleScope handle_scope(isolate);
        // 初始化注入可供js调用c方法上下文
        context = CreateShellContext(isolate);
        if (context.IsEmpty()) {
            fprintf(stderr, "Error creating context\n");
            return;
        }
        // printf("##C##\nInit:isolate=%p\n",isolate);
        ExecuteString(isolate, str);
        ExecuteString(isolate, "init()");

        static const int kBufferSize = 256;
        // Enter the execution environment before evaluating any code.
        v8::Context::Scope context_scope(context);
        v8::Local<v8::String> name(v8::String::NewFromUtf8(isolate, "(shell)", v8::NewStringType::kNormal).ToLocalChecked());
        while (true) {
            jclass cls = env->GetObjectClass(jobj);
            jmethodID mid = env->GetMethodID(cls, "waitForInputOrder", "()Ljava/lang/String;");
            if (mid == NULL) {
                return;
            }
            jstring jstr = static_cast<jstring>(env->CallObjectMethod(jobj, mid));
            const char* iputOrder = env->GetStringUTFChars(jstr, NULL);
            // printf("##C##\nJava_HelloJNI_load:iputOrder= %s\n", iputOrder);
            ExecuteString(isolate, iputOrder);
        }
    }
    // 释放v8引擎
    isolate->Dispose();
    v8::V8::Dispose();
    v8::V8::ShutdownPlatform();
    delete create_params.array_buffer_allocator;
}
 shengshuqiangdeMacBook-Pro:do shengshuqiang$ pwd
/Users/shengshuqiang/ideal/HelloJSCore/AdvanceOnReactNative/do
shengshuqiangdeMacBook-Pro:do shengshuqiang$ mkdir build
shengshuqiangdeMacBook-Pro:do shengshuqiang$ cd build
shengshuqiangdeMacBook-Pro:build shengshuqiang$ cmake ..
-- The C compiler identification is AppleClang 10.0.0.10001145
-- The CXX compiler identification is AppleClang 10.0.0.10001145
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
CMake Warning (dev):
  Policy CMP0042 is not set: MACOSX_RPATH is enabled by default.  Run "cmake
  --help-policy CMP0042" for policy details.  Use the cmake_policy command to
  set the policy and suppress this warning.
  MACOSX_RPATH is not specified for the following targets:
   HelloJNI
This warning is for project developers.  Use -Wno-dev to suppress it.
-- Generating done
-- Build files have been written to: /Users/shengshuqiang/ideal/HelloJSCore/AdvanceOnReactNative/do/build
shengshuqiangdeMacBook-Pro:build shengshuqiang$ make
Scanning dependencies of target HelloJNI
[ 50%] Building CXX object CMakeFiles/HelloJNI.dir/HelloJNI.cc.o
In file included from /Users/shengshuqiang/ideal/HelloJSCore/AdvanceOnReactNative/do/HelloJNI.cc:1:
/Users/shengshuqiang/ideal/HelloJSCore/AdvanceOnReactNative/do/HelloJNI.h:19:24: warning: 'CreateShellContext' has C-linkage specified, but returns user-defined type 'v8::Local<v8::Context>' which
      is incompatible with C [-Wreturn-type-c-linkage]
v8::Local<v8::Context> CreateShellContext(v8::Isolate* isolate);
                       ^
/Users/shengshuqiang/ideal/HelloJSCore/AdvanceOnReactNative/do/HelloJNI.cc:20:18: warning: ISO C++11 does not allow conversion from string literal to 'char *' [-Wwritable-strings]
    char* argv = "hello";
                 ^
2 warnings generated.
[100%] Linking CXX shared library libHelloJNI.dylib
ld: warning: directory not found for option '-L/Users/shengshuqiang/ideal/HelloJSCore/AdvanceOnReactNative/do/libs'
[100%] Built target HelloJNI
shengshuqiangdeMacBook-Pro:build shengshuqiang$ javac ../HelloJNI.java -d .
shengshuqiangdeMacBook-Pro:build shengshuqiang$ java HelloJNI
Hello SSU!
①♠︎   一      ②♠︎   一      ③♠︎
｜      ╲       ｜      ╱       ｜
④♢    一      ⑤♢    一      ⑥♢
｜      ╱       ｜      ╲       ｜
⑦♤    一      ⑧♤    一      ⑨♤
请
handleOrder(1,5)
handleOrder源位置1棋子♠︎移动到目标位置5棋子♢
Step0: 1➡️5
①♢    一      ②♠︎   一      ③♠︎
｜      ╲       ｜      ╱       ｜
④♢    一      ⑤♠︎   一      ⑥♢
｜      ╱       ｜      ╲       ｜
⑦♤    一      ⑧♤    一      ⑨♤
handleOrder(7,4)
handleOrder源位置7棋子♤移动到目标位置4棋子♢
Step1: 7➡️4
①♢    一      ②♠︎   一      ③♠︎
｜      ╲       ｜      ╱       ｜
④♤    一      ⑤♠︎   一      ⑥♢
｜      ╱       ｜      ╲       ｜
⑦♢    一      ⑧♤    一      ⑨♤
quit()
shengshuqiangdeMacBook-Pro:build shengshuqiang$ 
 ```
 ![对角棋博弈]({{ site.url }}/assets/对角棋博弈.png)

## 一张图

![手写ReactNative架构图]({{ site.url }}/assets/手写ReactNative架构图.png)

## 渔

1. 每天看一篇技术文章，发朋友圈。我先抛个砖：[进击ReactNative-纳百川](https://shengshuqiang.github.io/2018/12/15/%E8%BF%9B%E5%87%BBReactNative-%E7%BA%B3%E7%99%BE%E5%B7%9D.html)
2. Just Do It。

## 鱼

1. 可运行的跨平台（Java、C、JavaScript）Demo [AdvanceOnReactNative](https://github.com/shengshuqiang/AdvanceOnReactNative)
2. 一些有利于北漂混下去的理由

## 进阶

1. [进击ReactNative-纳百川](https://shengshuqiang.github.io/2018/12/15/%E8%BF%9B%E5%87%BBReactNative-%E7%BA%B3%E7%99%BE%E5%B7%9D.html)

## 参考

1. [深入理解JSCore](https://my.oschina.net/meituantech/blog/1933335)
2. 郭孝星[ReactNative源码篇](https://github.com/guoxiaoxing/react-native/tree/6f3c65c62e9fc30826f1fa3ac907c6bcaadf9995/doc/ReactNative%E6%BA%90%E7%A0%81%E7%AF%87)
2. [再谈移动端动态化方案](http://www.sohu.com/a/246063323_505779)
2.  [美团旅行前端技术体系的思考与实践](https://zhuanlan.zhihu.com/p/29373613)
3. [每个架构师都应该研究下康威定律](https://36kr.com/p/5042735.html)
4. [java ScriptEngine 使用](https://www.cnblogs.com/zhangtan/p/8110210.html)
5. [java中执行js代码](https://www.cnblogs.com/huzi007/p/4702851.html)
6. [如何在java中调用js方法](https://www.cnblogs.com/huangjingzhou/articles/2049043.html)
7. [JavaScriptCore全面解析 （上篇）](https://cloud.tencent.com/developer/article/1004875)
8. [JavaScriptCore全面解析 （下篇）](https://cloud.tencent.com/developer/article/1004876)

## 长歌

### 寒窑赋
**北宋大臣吕蒙正**

天有不测风云，人有旦夕祸福。
蜈蚣百足，行不及蛇；雄鸡两翼，飞不过鸦。
马有千里之程，无骑不能自往；人有冲天之志，非运不能自通。

盖闻：人生在世，富贵不能淫，贫贱不能移。
文章盖世，孔子厄于陈邦；武略超群，太公钓于渭水。
颜渊命短，殊非凶恶之徒；盗跖年长，岂是善良之辈。
尧帝明圣，却生不肖之儿；瞽叟愚顽，反生大孝之子。
张良原是布衣，萧何称谓县吏。
晏子身无五尺，封作齐国宰相；孔明卧居草庐，能作蜀汉军师。
楚霸虽雄，败于乌江自刎；汉王虽弱，竟有万里江山。
李广有射虎之威，到老无封；冯唐有乘龙之才，一生不遇。
韩信未遇之时，无一日三餐，及至遇行，腰悬三尺玉印，一旦时衰，死于阴人之手。

有先贫而后富，有老壮而少衰。
满腹文章，白发竟然不中；才疏学浅，少年及第登科。
深院宫娥，运退反为妓妾；风流妓女，时来配作夫人。

青春美女，却招愚蠢之夫；俊秀郎君，反配粗丑之妇。
蛟龙未遇，潜水于鱼鳖之间；君子失时，拱手于小人之下。
衣服虽破，常存仪礼之容；面带忧愁，每抱怀安之量。
时遭不遇，只宜安贫守份；心若不欺，必然扬眉吐气。
初贫君子，天然骨骼生成；乍富小人，不脱贫寒肌体。

天不得时，日月无光；地不得时，草木不生；水不得时，风浪不平；人不得时，利运不通。
注福注禄，命里已安排定，富贵谁不欲？
人若不依根基八字，岂能为卿为相？

吾昔寓居洛阳，朝求僧餐，暮宿破窖，思衣不可遮其体，思食不可济其饥，
上人憎，下人厌，人道我贱，非我不弃也。
今居朝堂，官至极品，位置三公，身虽鞠躬于一人之下，而列职于千万人之上，
有挞百僚之杖，有斩鄙吝之剑，思衣而有罗锦千箱，思食而有珍馐百味，
出则壮士执鞭，入则佳人捧觞，上人宠，下人拥。
人道我贵，非我之能也，此乃时也、运也、命也。

嗟呼！人生在世，富贵不可尽用，贫贱不可自欺，听由天地循环，周而复始焉。
