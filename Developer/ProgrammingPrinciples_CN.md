# 软件开发者应知的原则

原文： https://java-design-patterns.com/principles/

## 编程原则

### KISS (Keep it Simple Stupid) 保持简单

### YAGNI (You aren't gonna need it) 仅在需要的时候去实现

### Do The Simplest Thing That Could Possibly Work 做最简单却可能有效的事

### DRY (Don't Repeat Yourself) 不要做重复的事情

### Code For The Maintainer 维护者友好代码

### Avoid Premature Optimization 避免过早优化

Make it wotk -> Make it right -> Make it fast
使其工作 -> 使其正确 —> 使其更快

### Minimise Coupling 低耦合

### Maximise Cohesion 高内聚

### Orthogonality 正交性

在概念上不相关的事情就不应该在系统中相关。

## 编码原则

### Separation of Concerns 关注点分离

界面、数据分离。

### Law of Demeter

```kt
// bad
val a = A()
a.b.test()

// good
val a = A()
val b = a.b
b.test()
```

### Composition Over Inheritance 组合优于继承

### Robustness Principle 稳健性原则

be conservative in what you do, be liberal in what you accept from others.

对自己做的事情严格，对从别人那接收的事情宽容.

输出严格，输入宽容。

### Inversion of Control 控制反转

使用依赖注入。

### Liskov Substitution Principle(LSP) 置换原则

程序中的对象可以使用其子类来替换并且不改变程序的正确性。

### Open/Closed Principle 开闭原则

对扩展开放，对修改关闭。

### Single Responsibility Principle 单一职责原则

模块、类、方法都应该遵循。

### Curly's Law : Do One Thing 只做一件事

模块、类、方法都应该遵循。

### Hide Implementation Details 隐藏实现细节

当接口实现发生变化时，接口调用不需要变化。

### Encapsulate What Changes 封装变化

识别有可能发生变化的地方，使用接口将其封装。

### Interface Segregation Principle 接口隔离原则

避免胖接口，遵循单一职责原则。

### Command Query Separation 命令查询分离

查询方法不会改变状态，需要保持其可以在任何地方以任何顺序使用；命令方法会改变状态，调用时需谨慎。

## 项目管理原则

### Boy-Scout Rule 童子军规则

保证每次提交的质量，避免形成技术债。

### Murphy's Law 墨菲定律

Anything that can go wrong will go wrong.
任何可能出错的事情都会出错。

软件开发者需要做的是 **多思考、多测试**。

### Brooks's Law

该定律与软件项目管理有关，由 Fred Brooks 在其名著《The Mythical Man-Month》中提出。该定律的精髓在于，**在软件项目中增加新的开发人员并不会立即提高他们的工作效率，相反，由于沟通的开销，会占用团队其他成员的时间**。

### Linus's Law

该定律源于 埃里克-雷蒙德（Eric S. Raymond）所著的《The Cathedral and the Bazaar》一书，是为了纪念著名的芬兰 Linux 操作系统发明者 林纳斯-托瓦兹（Linus Torvalds） 而命名的。它是对软件审查过程的一种赞美，即 **在代码被接受和合并之前，由多个开发人员对其进行检查** 。
