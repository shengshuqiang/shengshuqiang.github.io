---
layout: post
title: 进击ReactNative-积水（JavaScript）渊
key: 20190224
tags:
  - 拈花指
  - JavaScript  
---
<!-- 添加目录 http://blog.csdn.net/hengwei_vc/article/details/47122103 -->
<script src="/javascripts/jquery-2.1.4.min.js" type="text/javascript"></script>
<script src="/javascripts/toc.js" type="text/javascript"></script>
<script type="text/javascript">
$(document).ready(function() {
    $('#toc').toc();
}); </script>
<div id="toc"></div>
**一个小小的雪球，在漫长的雪坡上往下滚，越滚越大，加速度、冲击力，规模都是呈现出加速度的扩张，只是需要每天的往下滚，日积月累。**
{:.info}

<!--more-->

# [【译】JavaScript 完整手册](https://juejin.im/post/5bff57fee51d45021a167991)
一✐肯定看不完的手册<br>启✵从视觉上来讲，它更简单了，也是受欢迎的改变<br>渔✐慢慢看<br>鱼✎<br>⑴模板字符串可以执行更复杂的表达式<br>⑵扩展运算符...扩展数组，对象或者是字符串<br>⑶只在需要时注释：不要添加不能帮助理解的注释。如果代码自身有良好的变量、函数命名和函数注释，不要添加注释<br>评✎<br>⑴这本 JavaScript 完整手册遵循二八定律（the 80/20 rule）：在 20% 的时间里了解 80% 的 JavaScript 知识<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积水（JavaScript）渊-190221.jpg)
{:.success}

# [JavaScript 常见设计模式解析](https://juejin.im/post/58f4c702a0bb9f006aa80f25)
一✐无处不设计<br>启✵站在巨人的肩膀上<br>渔✐概念✧例子✧对比<br>鱼✎<br>⑴设计模式☞代码可重用、约定俗成、工程化<br>⑵观察者模式☞被观察者状态改变主动通知观察者<br>⑶中介者模式☞统一管理<br>⑷代理模式☞访问控制<br>⑸单例模式☞全局访问<br>⑹工厂模式☞子类实例化<br>⑺装饰者模式☞添加额外职责<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积水（JavaScript）渊-190220.jpg)
{:.success}

# [JavaScript 代码简洁之道](https://juejin.im/post/5c24b7a851882509a76875e8)
一✐干净的代码<br>启✵因为专注,所以专业<br>渔✐好坏对比<br>鱼✎<br>⑴SOLID☞<br>Single Responsibility单一功能<br>Open Close开【扩展】闭【修改】<br>Liskov Substitution里氏替换【子类替换父类】<br>Interface Segregation接口隔离<br>Dependency Inversion依赖反转【依赖抽象而不依赖具体】<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积水（JavaScript）渊-190219.jpg)
{:.success}

# [你不知道的js中关于this绑定机制的解析[看完还不懂算我输]](https://juejin.im/post/5b3715def265da59af40a630)
一✐你不知道的this绑定机制<br>启✵看完不懂算我输<br>渔✐相互独立，完全穷尽<br>鱼✎<br>⑴绑定规则☞显示/new>隐式>默认<br>⑵new绑定☞全新对象、绑this、返回新或其他对象<br>⑶箭头函数☞继承于它外面第一个非箭头函数的函数this指向；绑定后不会被改变<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积水（JavaScript）渊-190218.jpg)
{:.success}

# [JavaScript专题之跟着underscore学防抖](https://github.com/mqyqingfeng/Blog/issues/22)
一✐JS频繁的事件触发Say No<br>启✵不塞不流，不止不行<br>渔✐为了鲁棒Robust，再思考一个新的需求<br>鱼✎<br>⑴防抖debounce☞连续触发间隔小于n，则最后一次触发生效【定时器实现】<br>⑵节流throttle☞固定间隔内多次触发只执行一次，依次循环【时间戳或定时器实现】<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积水（JavaScript）渊-190215.jpg)
{:.success}

# [JavaScript深入之创建对象的多种方式以及优缺点](https://github.com/mqyqingfeng/Blog/issues/15)
一✐JS创建和继承花式玩法<br>启✵I dont't care but root<br>渔✐精益求精<br>鱼✎<br>⑴创建对象方式☞工厂【对象无法识别】➢构造函数【方法每次被创建】➢原型【无私有属性】➢组合【双剑合璧】<br>⑵继承方式☞原型链➢借用构造函数➢组合<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积水（JavaScript）渊-190214.jpg)
{:.success}

# [JavaScript深入之call和apply的模拟实现](https://github.com/mqyqingfeng/Blog/issues/11)
一✐JS函数操作模拟实现<br>启✵不做不知道<br>渔✐功能➢实现➢优化<br>鱼✎<br>⑴call和apply（改变执行函数this指向）模拟☞函数设为对象属性，执行，删除属性<br>⑵bind（返回新函数，this为创建首参数）模拟☞调用原函数apply实现，this为闭包持有的首参数<br>⑶new（创建对象）模拟☞调用构造函数apply实现，this为新建空对象<br>评✎<br>⑴bind返回的函数作为构造函数时，绑定的this值失效<br>⑵new的构造函数返回值如果是一个对象，则该对象为返回值，否则，返回本函数对象<br>⑶说了你又不听，听了你又不懂，懂了你又不做，做了你又做错，错了你又不认，认了你又不改，改了你又不服，不服你又不说！你要我怎么说你呢？<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积水（JavaScript）渊-190213.jpg)
{:.success}

# [JavaScript深入之词法作用域和动态作用域](https://github.com/mqyqingfeng/Blog/issues/3)
一✐慢镜头JS解析运行<br>启✵一步步来<br>渔✐思考实验解答<br>鱼✎<br>⑴JS是词法【静态】作用域，作用域基于创建的代码位置<br>⑵执行上下文☞<br>属性【变量对象、作用域链、this】<br>阶段【进入执行上下文、代码执行】<br>⑶变量对象☞函数形参、函数声明、变量声明<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积水（JavaScript）渊-190212.jpg)
{:.success}

# [JavaScript深入之从原型到原型链](https://github.com/mqyqingfeng/Blog/issues/2)
一✐顺藤【原型链】摸瓜【属性】<br>启✵图话<br>渔✐画图<br>鱼✎<br>⑴prototype☞函数的属性，构造函数.prototype=实例原型
constructor☞原型的属性，实例原型.constructor=构造函数
\_\_proto__☞对象的属性，实例.__proto\_\_=实例原型<br>⑵原型链☞原型的原型，实例原型.__proto\_\_=Object.prototype
原型☞每个JS对象(null除外)在创建时就与之关联的对象<br>⑶Object.prototype.__proto__=null<br>![](https://github.com/mqyqingfeng/Blog/raw/master/Images/prototype5.png)<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积水（JavaScript）渊-190211.jpg)
{:.success}

# [[译] 在你学习 React 之前必备的 JavaScript 基础](https://mp.weixin.qq.com/s?__biz=MzA3MjA4NjE3NQ==&mid=2652105094&idx=1&sn=d956af1ba67d511135cc7365692921c4&chksm=84c4ee76b3b36760d13052d9a9247d7ad4a85bd3fa4212d83a4d9a2fa94b48a07502bf87dc08&mpshare=1&scene=2&srcid=0201qaE8Ikqmj6LZey31ET6V&from=timeline&ascene=2&devicetype=android-26&version=2700003d&nettype=WIFI&abtest_cookie=BgABAAgACgALABIAEwAUAAYAnYYeACOXHgBamR4Am5keAMuZHgDUmR4AAAA%3D&lang=zh_CN&pass_ticket=6jub%2FQnBqW5crQE0cOl%2FWp9%2FVy94oj0QXeeW%2B8oboUKHFtymzUx9wGFfwrRE%2B1dX&wx_header=1)
一✐万丈高楼【RN】平地【JS】起<br>启✵人若无名，便可专心练剑<br>渔✐上手React知识精要【80％的时间用到的20％知识】<br>鱼✎<br>⑴Class☞定义任意需要的方法、可继承扩展<br>⑵let和const☞代码块变量声明<br>⑶更短的语法☞箭头函数<br>⑷map和filter☞数组处理<br>⑸解构赋值☞复制对象或数组部分数据<br>⑹import和export☞文件导入和导出<br>评✎<br>⑴为了测试美国、日本、中国三地警察的实力，联合国将三只兔子放在三个森林中，看三地警察谁先找出兔子。任务：找出兔子。美国和日本警察都兴师动众地在森林里找，结果都是到了晚上无功而返。最后是中国警察，先是玩了一天王者荣耀，黄昏时一个拿着一根警棍进入森林，没五分钟，听到森林里传来一阵动物的惨叫，中国警察抽着烟有说有笑的出来，后面拖着一只鼻青脸肿的熊，熊奄奄一息的用兔子的叫声说：“不要再打了，我就是兔子！”<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积水（JavaScript）渊-190201.jpg)
{:.success}

# [【第45期】JavaScript 箭头函数：适用与不适用场景](https://mp.weixin.qq.com/s?__biz=Mzg5MDAyNjIxOQ==&mid=2247483940&idx=1&sn=d6cedb859f943c9982c0ee4cfbe7ef82&chksm=cfe3a3a0f8942ab6c1799bb82ad98b5e856ee32b295b47ab14d1d76497df93d3900752aea0d6&mpshare=1&scene=2&srcid=0131J69QKNMKPwwgvAikjv7G&from=timeline&ascene=2&devicetype=android-26&version=2700003d&nettype=WIFI&abtest_cookie=BgABAAgACgALABIAEwAUAAYAnYYeACOXHgBamR4Am5keAMuZHgDUmR4AAAA%3D&lang=zh_CN&pass_ticket=6jub%2FQnBqW5crQE0cOl%2FWp9%2FVy94oj0QXeeW%2B8oboUKHFtymzUx9wGFfwrRE%2B1dX&wx_header=1)
一✐箭头函数试试看<br>启✵权衡才是关键<br>渔✐辩证地看<br>鱼✎<br>⑴优点☞语法简洁、作用域直观、this绑定<br>⑵特色☞没有自己的执行期上下文，this和arguments均从继承父函数<br>⑶赞成☞遍历列表、管理异步、对象转换<br>反对☞对象的方法、深层调用链、动态上下文<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积水（JavaScript）渊-190131.jpg)
{:.success}

# [JavaScript高级程序设计——上下文环境和作用域](http://www.cduyzh.com/js-context/)
一✐大环境【上下文】和小趋势【作用域】<br>启✵万法唯识，识外无境<br>渔✐原理探究<br>鱼✎<br>⑴上下文☞变量和函数表达式声明赋值undefined、this和函数声明赋值<br>⑵上下文栈☞执行全局代码产生栈，执行函数产生栈，调用完成后回退到上一个栈<br>⑶函数创建时确定作用域，运行时确定变量值<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积水（JavaScript）渊-190130.jpg)
{:.success}

# [JavaScript高级程序设计——原型和原型链](http://www.cduyzh.com/js-prototype/)
一✐谈对象<br>启✵换个姿势阐述<br>渔✐概念+Demo层层递进<br>鱼✎<br>⑴值类型【undefined, number, string, boolean】不是对象，引用类型【函数、数组、对象、null】都是对象<br>⑵函数创建对象，对象里的一切皆属性【包括方法，可任意扩展】<br>⑶每个函数都有一个原型【prototype】，每个对象都有一个隐式原型【proto，指向创建它的构造函数的原型对象】<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积水（JavaScript）渊-190129.jpg)
{:.success}

# [JavaScript 的 this 原理](http://www.ruanyifeng.com/blog/2018/06/javascript-this.html)
一✐What this<br>启✵这种解释没错，但是教科书往往不告诉你，为什么会这样<br>渔✐问➢数据结构➢函数➢环境<br>鱼✎<br>⑴懂JS☞两种写法，可能有不一样的结果<br>⑵函数单独保存在内存中，通过函数地址访问，能在不同环境执行<br>⑶this☞指代函数当前的运行环境【函数地址的宿主】<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积水（JavaScript）渊-190128.jpg)
{:.success}

# [JavaScript高级程序设计——闭包](http://www.cduyzh.com/js-closure/)
一✐闭包听起来不好吃<br>启✵让生活更美好的小知识点<br>渔✐定义+举例<br>鱼✎<br>⑴闭包☞有权访问另一个函数作用域中的变量的函数，外部函数调用完成后上下文不会被销毁<br>⑵条件☞内部函数使用外部函数的变量、外部函数已经退出、内部函数可以访问<br>⑶应用☞函数作为返回值，函数作为参数传递<br>评✎<br>⑴匿名函数的执行环境具有全局性，因此this对象通常指向window(在通过call或apply函数改变函数执行环境的情况下，会指向其他对象)<br>⑵潜规则☞声明提前【函数内的声明在函数体内始终可见】、全局变量优先级低于局部变量<br>⑶JS没有块级作用域，只有函数级<br>⑷立即执行函数☞(function(){}) ()<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积水（JavaScript）渊-190125.jpg)
{:.success}

# [JavaScript 内存机制（前端同学进阶必备）](https://juejin.im/post/5b10ba336fb9a01e66164346)
一✐自己JS垃圾自己带走<br>启✵不要满足于写出能运行的程序，也不要认为机器的升级就能解决一切<br>渔✐模型➢生命周期➢泄露<br>鱼✎<br>⑴机制☞内存基元在变量创建时分配，在不用时“自动”释放<br>⑵模型☞栈【变量、池【常量】】、堆【对象】<br>⑶分代☞新生代【短命、复制】、老年代【长命、标记】<br>评✎<br>⑴基础数据类型： Number String Null Undefined Boolean<br>⑵拥有超多BUG并依然占有大量市场的IE是前端开发一生之敌！亲，没有买卖就没有杀害。
<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积水（JavaScript）渊-190123.jpg)
{:.success}

# [10分钟理解JS引擎的执行机制](https://segmentfault.com/a/1190000012806637)
一✐一眼看穿JS引擎<br>启✵十分钟<br>渔✐层层递进<br>鱼✎<br>⑴JS单线程☞天然支持互斥<br>异步☞避免卡死<br>机制☞Event Loop<br>⑵宏任务☞代码段、setTimeout、setInterval
微任务☞Promise、process.nextTick<br>⑶宏任务➢所有微任务➢新的宏任务循环<br>![](https://shengshuqiang.github.io/assets/进击ReactNative-积水（JavaScript）渊-190122.jpg)
{:.success}
