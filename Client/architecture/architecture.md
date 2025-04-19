# 架构设计

## 参考链接

<https://developer.android.google.cn/topic/architecture>

## 为什么需要设计应用架构

随着应用的不断迭代，功能会越来越多，相应的代码也会越来越多，越来越复杂。如果团队不去规范团队成员如何去组织代码实现功能，整体代码质量会越来越低，越来越难以维护。

### 收益

* 使代码更简单，提升代码可维护性
* 提升代码质量，代码可扩展性、可测试性等
* 提升开发效率，多个开发者可更好的协作开发

## 架构设计原则

### 关注点分离

界面、数据分层

### 定义好每个部分（架构层及其内的类）的边界及职责，遵循职责单一原则

界面层负责界面显示及界面逻辑，数据层负责应用数据及业务逻辑

### 依赖注入

使用依赖注入处理类之间的依赖关系

### 全局响应式编程

推荐项目全局使用 Kotlin Flow

### 数据模型驱动用户界面

从数据模型（最好是持久化数据模型）驱动用户界面

### 单向数据流（Unidirectional data flow - UDF）

状态只朝一个方向流动，修改数据的事件向相反方向流动

### 单一事实源（Single source of truth - SSOT）

数据持有者通过不可变类型暴露数据（如 StateFlow），暴露更改数据的方法，外部调用数据持有者内的更改数据方法去更改数据

<img 
    src="/Android/Architecture/assets/mad-arch-ui-udf.png"
    alt="UDF"
    width="666">

## 推荐架构

现代应用架构最少有两层:

* [界面层（UI layer）](/Android/Architecture/UILayer_CN.md)：界面层包含界面显示及界面逻辑，主要由用户界面(UI)、用户界面状态（UiState）和状态持有者（StateHolder）组成
* [域层（Domain layer）](/Android/Architecture/DomainLayer_CN.md)：域层是可选层，主要职责是承接界面层和数据层之间的部分业务逻辑，域层仅包含用例类（UseCase）
* [数据层（Data layer）](/Android/Architecture/DataLayer_CN.md)：数据层包含数据及业务逻辑，主要由数据模型（Model）、数据仓库（Repository）和数据源（DataSource）组成

<img 
    src="/Android/Architecture/assets/mad-arch-overview.png"
    alt="Architecture Overview"
    width="666">

## 架构最佳实践

* 不要在 Android 组件（Activity、Service）中存储数据
* 减少对 Android 系统类的依赖（Context）
* 尽可能减少暴露
* 使代码每个部分都可独立测试
* 逻辑代码自己负责转移到正确的线程中运行
* 尽可能多的本地持久化新拉取的数据
  