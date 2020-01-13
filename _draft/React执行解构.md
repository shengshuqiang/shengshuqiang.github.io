1. ReactNativeRenderer.render
	2. 传入注册element {$$typeof: Symbol(react.element), type: ƒ HelloSSUTextFunc(), props: {rootTag: 11}}
	3. 创建root: {current: {id: 0, tag: 3, constructor: ƒ FiberNode} }, finishedWork: null}
	4. scheduleRootUpdate: {id: 0, tag: 3, constructor: ƒ FiberNode, updateQueue: {firstUpdate:{payload:{element: {$$typeof: Symbol(react.element), type: ƒ HelloSSUTextFunc(), props: {rootTag: 11}}}}}}
		5. renderRoot
			6. nextUnitOfWork: {id: 0, tag: 3, constructor: ƒ FiberNode, alternate: FiberNode {id: 0}}
			7. workLoop: while (nextUnitOfWork !== null) { nextUnitOfWork = performUnitOfWork(nextUnitOfWork); }
				8. performUnitOfWork
					9. assignFiberPropertiesInDEV 新增了一个fiber节点2，先忽略。
					10. next = beginWork(current$$1, workInProgress, nextRenderExpirationTime);
						11. HostRoot: updateHostRoot(current$$1, workInProgress, renderExpirationTime);
							12. processUpdateQueue( workInProgress,updateQueue,nextProps,null,renderExpirationTime);
								13. resultState = getStateFromUpdate(workInProgress, queue, update, resultState, props, instance);
							14. nextState: {element: {$$typeof: Symbol(react.element), type: ƒ HelloSSUTextFunc(), props: {rootTag: 11}}}
							15. var nextChildren = nextState.element;
								16. nextChildren: {$$typeof: Symbol(react.element), type: ƒ HelloSSUTextFunc(), props: {rootTag: 11}}
							17. reconcileChildren(current$$1, workInProgress, nextChildren, renderExpirationTime);
								18. workInProgress.child = reconcileChildFibers(workInProgress,current$$1.child,nextChildren,renderExpirationTime);
									19. reconcileSingleElement(returnFiber, currentFirstChild, newChild, expirationTime)
									20. workInProgress.child: {id: 3, tag: 2, constructor: ƒ FiberNode, type: ƒ HelloSSUTextFunc(), return: {id: 0 } }
					21. next: {id: 3, tag: 2, constructor: ƒ FiberNode, type: ƒ HelloSSUTextFunc(), return: {id: 0 } }
					22. next = beginWork(current$$1, workInProgress, nextRenderExpirationTime);
						23. IndeterminateComponent: mountIndeterminateComponent(current$$1, workInProgress, elementType, renderExpirationTime);
							24. var children = Component(props, refOrContext);
								25. function HelloSSUTextFunc () {const RCTText = 'RCTText';return <RCTText>{`Hello SSU!`}</RCTText>;};
							25. children: {$$typeof: Symbol(react.element), type: "RCTText",props: {children: "Hello SSU!"}}
							26. reconcileChildren(null, workInProgress, value, renderExpirationTime);
					27. next: {id: 4, tag: 5, constructor: ƒ FiberNode, type: "RCTText", return: {id: 3 }, pendingProps: {children: "Hello SSU!"} }
					28. next = beginWork(current$$1, workInProgress, nextRenderExpirationTime);
						29. HostComponent: updateHostComponent(current$$1, workInProgress, renderExpirationTime)
						30. var nextChildren = nextProps.children;
						31. nextChildren: "Hello SSU!"
						32. reconcileChildren(current$$1, workInProgress, nextChildren, renderExpirationTime);
							33. workInProgress.child = mountChildFibers(workInProgress, null, nextChildren, renderExpirationTime);
								34. reconcileSingleTextNode(returnFiber, currentFirstChild, "" + newChild, expirationTime)
									35. var created = createFiberFromText(textContent, returnFiber.mode, expirationTime);
					36. next: {id: 5, tag: 6, constructor: ƒ FiberNode, type: null, return: {id: 4 }, pendingProps: "Hello SSU!", alternate: null }
					37. next = beginWork(current$$1, workInProgress, nextRenderExpirationTime);
						38. HostText: updateHostText(current$$1, workInProgress);
					39. next: null
					40. next = completeUnitOfWork(workInProgress);
						41. (workInProgress.effectTag & Incomplete) === NoEffect
						42. nextUnitOfWork = workInProgress;
						43. nextUnitOfWork = completeWork(current$$1, workInProgress, nextRenderExpirationTime);
							44. HostText:
								45. workInProgress.stateNode = createTextInstance(newText, _rootContainerInstance,  _currentHostContext, workInProgress);
									46. UIManager.createView(tag, "RCTRawText", rootContainerInstance, {text: text});
										47. 指令：UIManager.createView([3,"RCTRawText",1,{"text":"Hello SSU!"}])
						47. nextUnitOfWork: null
						48. workInProgress = returnFiber;
						49. workInProgress: { id: 4 }
						50. nextUnitOfWork = workInProgress;
						51. nextUnitOfWork = completeWork(current$$1, workInProgress, nextRenderExpirationTime);
							52. HostComponent:
								53. var instance = createInstance(type, newProps, rootContainerInstance, currentHostContext, workInProgress);
								54. UIManager.createView(tag, viewConfig.uiViewClassName, rootContainerInstance, updatePayload);
									55. 指令：UIManager.createView([5,"RCTText",1,null])
								55. instance: { _nativeTag: 5, _children: [], constructor: ƒ ReactNativeFiberHostComponent}
								56. appendAllChildren(instance, workInProgress, false, false);
									57. appendInitialChild(parent, node.stateNode);
									58. instance: { _nativeTag: 5, _children: [3], constructor: ƒ ReactNativeFiberHostComponent}
								59. finalizeInitialChildren(instance, type, newProps, rootContainerInstance, currentHostContext)
									60. UIManager.setChildren(parentInstance._nativeTag, nativeTags);
										61. parentInstance._nativeTag: 5
										62. nativeTags: [3]
										63. 指令：UIManager.setChildren([5,[3]])
								63. workInProgress.stateNode = instance;
						64. nextUnitOfWork: null
						65. workInProgress = returnFiber;
						66. workInProgress: {id: 3}
						67. nextUnitOfWork = workInProgress;
						68. nextUnitOfWork: {id: 3}
						69. nextUnitOfWork = completeWork(current$$1, workInProgress, nextRenderExpirationTime);
							70. FunctionComponent: break;
						71. nextUnitOfWork: null
						72. workInProgress = returnFiber;
						73. workInProgress: {id: 1}
						73. nextUnitOfWork = workInProgress;
						74. nextUnitOfWork = completeWork(current$$1, workInProgress, nextRenderExpirationTime);
							75. HostRoot: updateHostContainer(workInProgress);
								76. Noop
					77. next: null
	78. onComplete(root, rootWorkInProgress, expirationTime);
		79. root.finishedWork = finishedWork;
	80. finishedWork = root.finishedWork;
	81. finishedWork: {id: 1}
	82. completeRoot(root, finishedWork, expirationTime);
		83. // Commit the root.
		84. root.finishedWork = null;
		85. commitRoot(root, finishedWork)
			86. firstEffect = finishedWork.firstEffect;
			87. firstEffect: {id: 3}
			87. nextEffect = firstEffect;
			88. while (nextEffect !== null) { invokeGuardedCallback(null, commitBeforeMutationLifecycles, null); }
				89. commitBeforeMutationLifecycles()
					90. while (nextEffect !== null) { commitBeforeMutationLifeCycles(current$$1, nextEffect); nextEffect = nextEffect.nextEffect; }
			91. nextEffect = firstEffect;
			92. while (nextEffect !== null) { invokeGuardedCallback(null, commitAllHostEffects, null); }
				93. commitAllHostEffects()
					94. while (nextEffect !== null) { ...; nextEffect = nextEffect.nextEffect; }
						95. Placement: commitPlacement(nextEffect);
							96. HostRoot:
								97. appendChildToContainer(parent, node.stateNode);
									98. UIManager.setChildren(parentInstance, [childTag]);
										99. parentInstance: 1
										100. childTag: 5
										101. 指令：UIManager.setChildren([1,[5]])
			101. root.current = finishedWork;
			102. nextEffect = firstEffect;
			103. while (nextEffect !== null) { invokeGuardedCallback(null, commitAllLifeCycles, null, root, committedExpirationTime); }
				104. commitAllLifeCycles(finishedRoot, committedExpirationTime);
					105. while (nextEffect !== null) { commitLifeCycles(finishedRoot, current$$1, nextEffect, committedExpirationTime); nextEffect = nextEffect.nextEffect; }
			106. onCommit(root, expirationTime)
				107. root.finishedWork = null;

2. renderRoot：root节点0克隆出workInProgress节点1
2. workloop大循环
	2. performUnitOfWork：新增了一个fiber节点2，先忽略。
	3. beginwork：
		4. root节点1 + element，生成子节点3
			5. HostRoot：updateHostRoot
				6. nextChildren = nextState.element
		5. 节点3
			6. IndeterminateComponent：mountIndeterminateComponent
				7. children = Component(props, refOrContext);
				8. { $$typeof: Symbol(react.element), type: "RCTText", props: {children: "Hello SSU!"}}
		2. 