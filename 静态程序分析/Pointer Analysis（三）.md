处理函数调用的指针分析

![[Pasted image 20260428152538.png]]`this` 变量到底是什么？
在静态分析的建模中，`this` 被视为一个**特殊的局部变量（Special Local Variable）**。
- **作用域**：每一个实例方法（Instance Method）都有其专属的 `this` 变量，记作 $m_{this}$
- **唯一性**：它是该方法中所有实例字段访问（如 `this.f`）的基准指针（Base Pointer）。
- **来源**：它的值不是通过常规的 `Assign` 语句（`x = y`）得到的，而是由函数调用（Call）时，由调用者的 `receiver` 指针精准传递过来的。

整体的调用算法为：
![[Pasted image 20260428154113.png]]
* S里的statements就是RM里methods的statements（语句）
- Call Graph和指针集作为最后的输出。

