# 现代 Android 架构

## 为什么需要架构

### 收益

## 架构原则

### 关注点分离

界面、数据分层

### 依赖注入

使用依赖注入处理类之间的依赖关系

### 数据模型驱动用户界面

从数据模型（最好是持久化数据模型）驱动用户界面

### 全局响应式编程

推荐项目全局使用 Kotlin Flow

### 单一事实源（Single source of truth - SSOT）

数据持有者通过不可变类型暴露数据（如 StateFlow），暴露更改数据的方法，外部调用数据持有者内的更改数据方法去更改数据。

### 单向数据流（Unidirectional data flow - UDF）

状态只朝一个方向流动，修改数据的事件向相反方向流动。

<img 
    src="/Android/Architecture/assets/mad-arch-ui-udf.png"
    alt="UDF"
    width="666">

## 架构概览

现代应用架构最少有两层:

* [界面层（UI layer）](/Android/Architecture/UILayer_CN.md)：包含界面相关状态及界面逻辑
* [域层（Domain layer）](/Android/Architecture/DomainLayer_CN.md)：负责封装复杂的业务逻辑或多个 ViewModel 重复使用的简单业务逻辑，域层是可选的，有需求时再去实现。
* [数据层（Data layer）](/Android/Architecture/DataLayer_CN.md)：包含应用数据及业务逻辑

<img 
    src="/Android/Architecture/assets/mad-arch-overview.png"
    alt="Architecture Overview"
    width="666">

## 架构最佳实践

* 不要在 Android 组件（Activity、Service）中存储数据
* 减少对 Android 系统类的依赖（Context）
* 为各个模块建立明确的责任界限
* 尽可能减少暴露
* 使代码每个部分都可独立测试
* 逻辑代码自己负责转移到正确的线程中运行
* 尽可能多的本地持久化新拉取的数据
  