
**静态软件分析（Static Software Analysis）是指在不实际运行程序**的情况下，通过检查代码（源代码、字节码或二进制代码）来推断程序行为、查找缺陷或验证安全性的技术

通过分析数据流向可以知道在程序的某一点上，某一个变量可能包含的抽象数据，或者说所指向的对象，在面对代码量巨大的情况下，能够快速的做自动化分析






## 编译过程

一个程序进入编译器之后会经过一下几个过程：
- 词法分析器（Scanner），结合正则表达式，通过词法分析（Lexical Analysis）将 source code 翻译为 token。
- 语法分析器（Parser），结合上下文无关文法（Context-Free Grammar），通过语法分析（Syntax Analysis），将 token 解析为抽象语法树（Abstract Syntax Tree, AST）
- 语义分析器（Type Checker），结合属性文法（Attribute Grammar），通过语义分析（Semantic Analysis），将 AST 解析为 decorated AST
- Translator，将 decorated AST 翻译为生成三地址码这样的中间表示形式（Intermediate Representation, IR），并**基于 IR 做静态分析**（例如代码优化这样的工作）。
- Code Generator，将 IR 转换为机器代码。

使用 IR 前必须经过上面三步， 其目的是为了我们进行的分析是正确的，即没有很基础的错误（trivial mistake）

## 三地址码（Three Address Code）

三地址码最多有三个“地址”，其中“地址”可以是 `普通名称` ， `常量`，  `编译器生成的临时变量`


## Basic Block

基础块代表是满足以下性质的3AC块：
- 只能从块的第一条指令进入。
- 只能从块的最后一条指令离开。
![[Pasted image 20260418202140.png]]
而程序控制流 CFG 则是由BB作为节点构成的一张图，描述程序控制流在不同BB之间的流动方向
![[image 2.avif]]

CFG 通常会有一个统一的入口 Entry 和统一的出口 Exit
