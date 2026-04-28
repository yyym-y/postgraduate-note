1. Why use interprocedural analysis
2. Call graph
3. Java 三种调用方式（Virtual 可能会有多种结果）
4. dispatch 函数（dispatch(c, m) 从c开始不断寻找父类包含m函数）
	> 实际上是找出这个类调用某个方法具体会落实到哪里
5. CHA 算法
	> 对于 virtual call 实际上是对所有的可能情况进行dispatch
6. Call Graph 的构建
	> ![[Pasted image 20260428140630.png]]
7. ICFG
	> CFG + Call/Return edge
	> ![[Pasted image 20260428141235.png]]

