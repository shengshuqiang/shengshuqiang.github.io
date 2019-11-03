<!-----
layout: post
title: 进击ReactNative-FlatList源码解析
key: 20190717
tags:
  - 蜻蜓切
  - ReactNative
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
**FlatList的性能高在哪里?**
{:.info}

<!--more-->

☞[阅读原文](https://shengshuqiang.github.io/2019/07/17/%E8%BF%9B%E5%87%BBReactNative-FlatList%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90.html)-->

# 考考你

1. <blockquote>问：在数据项高度不确定情况下，js侧不具备直接计算组件大小的能力，是怎么知道首屏展示几个数据项？<br>答：js侧首屏几个不是直接计算出来的，而是先通过设置的属性估算出几个数据项，同时设置数据项和列表的布局监听回调onLayout，回调中修正数据项个数（如果还有数据项并且屏幕还有空间，则继续添加数据项）。</blockquote>
2. <blockquote>问：FlatList只展示屏幕中的个数，那怎么还能快速滚动，Native底层对应的可是普通的滚动容器ScrollView？<br>答：FlatList中数据项个数不止屏幕中显示的个数，会有超出屏幕外的多屏数据项（通过windowSize属性设置）。假如实际数据项100，FlatList一屏能展示10个，此时FlatList中的个数可能是30个，如果你快速滑动，只能滑出30个就到底了，需要过一会才发现还能接着滑动。</blockquote>
3. <blockquote>问：FlatList中空白项和实际未显示的数据项个数对应么，空白项高度怎么计算？<br>答：空白项和实际未显示个数不对应，最多只有两个空白项（顶部空白项和底部空白项）。如果未显示数据项高度已经计算过（之前在界面显示过），则直接累加即可算出，如果位置，则通过估算（已知数据项高度的平均值）。</blockquote>
4. <blockquote>问：FlatList滑动起来一点都不卡，必要的计算还是有的，这是怎么做到的？<br>答：除少数情况下（用户滑动到的页面是空白），计算工作大部分在InteractionManager.runAfterInteractions（将一些耗时较长的工作安排到所有互动或动画完成之后再进行）执行。有效的避免了和用户交互抢占CPU，而且是用户交互停下来了才触发，避免了跟随用户动作频繁计算。不过弊端就是上面的少数情况下会卡顿（预设的缓冲用完了，只能强制在交互中同步计算）或者看起来像彩蛋（快速滑动还没有显示完所有数据就到底滑不动了、快速滑动下白屏）。</blockquote>
5. <blockquote>问：网上说FlatList的原理是“不在屏幕中的组件会被移除，通过空白替代”，和没说一样？<br>答：<br>1. 长列表最大的硬伤是随着列表不断滑动，数据项越来越多，内存越来越大，然后就OOM了。通过将屏幕外组件移除是解决该硬伤的核心思想（其实核心思想就这么几种，大家都能想到，困惑的其实是怎么做到的）。<br>2. FlatList首先会预缓冲很多屏数据，这样不会影响正常显示和滑动功能。其次就是在互动或动画结束后再刷新缓冲区域，这样不会卡。再次通过key保证了缓冲区是增量刷新，并且限制增量大小，确保不会卡。</blockquote>
6. <blockquote>问：getItemLayout真能提高性能么？<br>答：getItemLayout能直接获取数据项控件位置和大小，无需借助onLayout回调，可以提高位置计算效率。</blockquote>
7. <blockquote>问：多级吸顶怎么做的？<br>答：实际使用的是ScrollView.js中自带的吸顶功能（通过位移动画实现）。FlatList虽然声明的属性没有说支持吸顶，但通过设置隐藏属性stickyHeaderIndices（这个属性在VirtualizedList.js里面用到，但是没有显示声明，FlatList会将自身所有属性直接赋值给VirtualizedList）能支持吸顶。</blockquote>


# 怎么看

1. 直接看源码，功力不够，也没有这个耐心，这和看英文文章一样，看着看着就(～﹃～)~zZ。
2. 直接打断点一步步Debug，随便一个操作会让你Debug到停不下来，直到怀疑人生。
3. 首先网上搜一下相关文章，熟悉一下大概。其次从核心入口render方法大概看看，找到一些关键函数。再次就是直接打日志（js代码就是这个好，依赖文件直接加日志就可以跑），串一下思路。再再次就是日志串不起来再再回头断点看看，来来回回你就懂了。

# 剖析

1. 整个Demo

```
export default class App extends Component<Props> {
    renderItem = (item) => {
        var txt = '第' + item.index + '个' + ' title=' + item.item.title;
        var bgColor = item.index % 2 == 0 ? 'red' : 'blue';
        return <Text style={[{flex: 1, height: 100, backgroundColor: bgColor}, styles.txt]}>{txt}</Text>
    }

    render() {
        var data = [];
        for (var i = 0; i < 1000; i++) {
            data.push({key: i, title: i + ''});
        }

        return (
            <View style={{flex: 1}}>
                <View style={{flex: 1}}>
                    <FlatList
                        initialNumToRender={1}
                        windowSize={2}
                        renderItem={this.renderItem}
                        data={data}>
                    </FlatList>
                </View>

            </View>
        );
    }
}
```

2. 长这样<br>![demo](https://shengshuqiang.github.io/assets/flatlist-demo.png)
3. 加点日志
	4. VirtualizedList.js
		5. console.log('SSU', '\n\nVirtualizedList#render(){列表开始渲染}\n\n', this.state);
		5. console.log('SSU', 'VirtualizedList#render()$lead_spacer{列表添加头部空白块}',{lastInitialIndex, initBlock, first, firstBlock}, {[spacerKey]: firstSpace});
		6. console.log('SSU', 'VirtualizedList#render()$tail_spacer{列表添加尾部空白块}', {last, lastFrame, end, endFrame},{[spacerKey]: tailSpacerLength})
		7. console.log('SSU', 'VirtualizedList#_pushCells(){填充列表项}',{first, last}, cells);
		8. console.log('SSU', 'VirtualizedList#_onCellLayout(){开始列表项布局回调}', cellKey, index);
		9. console.log('SSU', 'VirtualizedList#_onCellLayout(){列表项布局回调结束}', next, this._frames);
		10. console.log('SSU', 'VirtualizedList#_onLayout(){列表布局回调}', e.nativeEvent.layout);
		11. console.log('SSU', 'VirtualizedList#_onLayout(){列表布局回调修正滚动参数}this._scrollMetrics.visibleLength=', this._scrollMetrics.visibleLength);
		12. console.log('SSU', 'VirtualizedList#_onContentSizeChange(){列表内容请大小变化回调}', width, height, this._scrollMetrics);
		13. console.log('SSU', 'VirtualizedList#_onScroll(){滚动开始}', e.nativeEvent.layoutMeasurement, e.nativeEvent.contentSize, e.nativeEvent.contentOffset);
		14. console.log('SSU', 'VirtualizedList#_onScroll(){滚动修正滚动参数}', this._scrollMetrics);
		15. console.log('SSU', 'VirtualizedList#_onScroll(){滚动结束}');
		12. console.log('SSU', 'VirtualizedList#_scheduleCellsToRenderUpdate(){安排列表项更新优先级}', {first, last}, {offset, visibleLength, velocity}, itemCount);
		13. console.log('SSU', 'VirtualizedList#\_scheduleCellsToRenderUpdate(){优先级高，直接更新列表项}', {hiPri, \_averageCellLength: this._averageCellLength, _hiPriInProgress: this._hiPriInProgress});
		14. console.log('SSU', 'VirtualizedList#\_scheduleCellsToRenderUpdate(){优先级低，等空闲再更新列表项}', {hiPri, \_averageCellLength: this._averageCellLength, _hiPriInProgress: this._hiPriInProgress});
	8. Batchinator.js
		9. console.log('SSU', 'Batchinator#schedule(){安排列表项更新}');
		10. console.log('SSU', 'Batchinator#schedule()InteractionManager.runAfterInteractions(){开始执行列表项更新}');
		11. console.log('SSU', 'Batchinator#schedule()InteractionManager.runAfterInteractions(){结束执行列表项更新}');
	10. VirtualizeUtils.js
		11. console.log('SSU', 'VirtualizeUtils#computeWindowedRenderLimits(){开始计算屏幕渲染列表项区域}', {maxToRenderPerBatch, windowSize}, prev, scrollMetrics);
		12. console.log('SSU', 'VirtualizeUtils#computeWindowedRenderLimits(){计算屏幕渲染列表项区域}', {visibleBegin, visibleEnd}, {overscanLength, fillPreference, overscanBegin, overscanEnd});
		13. console.log('SSU', 'VirtualizeUtils#computeWindowedRenderLimits(){完成计算屏幕渲染列表项区域}', prev, {first, last});
		14. console.log('SSU', 'VirtualizeUtils#computeWindowedRenderLimits(){完成计算屏幕渲染列表项区域}', prev, {first, last}, {overscanFirst, overscanLast, newCellCount});
	15. 初始化显示
		16. 根据设置参数预估显示数据项区[0，1]（不用担心是否显示一屏，上述Demo初始数据项个数为1）<br>![](https://shengshuqiang.github.io/assets/flatlist-init_1.png)<br>![](https://shengshuqiang.github.io/assets/flatlist-init_default_state.png)
		17. 数据项布局变化、列表布局变化、列表内容区大小变化均会安排列表项更新（显示数据项个数）优先级
		18. 根据数据项返回高度、列表高度、列表内容区大小计算出显示不满一屏，直接更新列表项
		19. 计算出当前状态下计算出显示列表项区间[0，8]，通过setState触发重新render
		20. render出9个数据项和1个尾部空白区，接着走2，因为此时一屏可以显示下，优先级低，所以等待空闲触发更新列表项<br>![](https://shengshuqiang.github.io/assets/flatlist-init_2.png)
		21. 计算出当前状态下不需要继续添加数据项，setState没有变化，更新停止，页面状态稳定<br>![](https://shengshuqiang.github.io/assets/flatlist-init_2_end.png)
	22. 滚动显示（用力向下滑动，发现滑动到“第8个 title=8”就再也划不动了，过一会又可以接着滑）<br>![](https://shengshuqiang.github.io/assets/flatlist-demo.gif)
		23. 滚动回调(_onScroll)会安排列表项更新（显示数据项个数）优先级，优先级低，所以等待空闲触发更新列表项
		24. 等到滑动到最后一个列表项时，滑不动了，此时空闲，触发更新列表项
		25. 根据当前状态，计算出显示列表项区间[0，11]
		26. render出12个数据项和1个尾部空白区，等待空闲更新列表项
		27. 列表项计算区间没有变化，更新停止，页面状态稳定
	28. 不断向下滑动，找到一个中间状态（比如第一项显示“第62个 title=62”发现有顶部空白和底部空白）<br>![](https://shengshuqiang.github.io/assets/flatlist-scroll_in_middle.png)
	29. 快速向上滑动，发现会有空白块闪烁一下后再显示出对应内容，滑动流畅<br>![](https://shengshuqiang.github.io/assets/flatlist-scroll_back_middle.png)
	30. 尝试设置getItemLayout属性，发现不再有数据项的布局回调，而且空白区的高度准确(不会出现滑动到一定位置就滑不动的彩蛋)。这么看好像性能也没有提高多少。<br>![](https://shengshuqiang.github.io/assets/flatlist-scroll_getitemlayout.gif)
	31. ![](https://shengshuqiang.github.io/assets/flatlist-scroll_getitemloayout.png)

# 原理

2. 整个过程就是多render几次，然后就达到平衡态了。<br>![](https://shengshuqiang.github.io/assets/flatlist-work_flow.svg)
3. 整个过程有个窗口的概念，通过一通计算，得到当前状态一个合适的窗口大小。<br>![](https://shengshuqiang.github.io/assets/flatlist-completeArea.svg)
4. 各个文件的依赖关系。整体看一下基本就拼接成现在的功能，核心在VirtualizedList。<br>![](https://shengshuqiang.github.io/assets/flatlist-class.svg)










