<!--# 进击ReactNative-->

# 方向

1. 通过上面的介绍，可以发现这三组概念有很多重叠部分。这三组概念都体现了关注点分离的思想：UI展现和数据逻辑的分离。函数组件、无状态组件和展示型组件主要关注UI展现，类组件、有状态组件和容器型组件主要关注数据逻辑。但由于它们的划分依据不同，它们并非完全等价的概念。它们之间的关联关系可以归纳为：函数组件一定是无状态组件，展示型组件一般是无状态组件；类组件既可以是有状态组件，又可以是无状态组件，容器型组件一般是有状态组件。