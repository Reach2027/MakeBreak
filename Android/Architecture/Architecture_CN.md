# 现代 Android 架构

## 核心架构原则

### 关注点分离

界面、数据分层

### 数据模型驱动用户界面

从数据模型（最好是持久化数据模型）驱动用户界面

### 全局响应式编程

推荐项目全局使用 Kotlin Flow

### 单一事实源（Single source of truth - SSOT）

数据所有者通过不可变类型暴露数据（如 StateFlow）,修改、更改数据逻辑必须在数据所有者内。

### 单向数据流（Unidirectional data flow - UDF）

状态只朝一个方向流动，修改数据的事件向相反方向流动。

### 依赖注入

使用依赖注入处理类之间的依赖关系

![UDF](/Android/Architecture/assets/mad-arch-ui-udf.png)

## 现代应用架构

现代应用架构最少有两层，界面层及数据层。

![ArchitectureOverview](/Android/Architecture/assets/mad-arch-overview.png)

### 界面层（UI layer）

![ArchitectureUI](/Android/Architecture/assets/mad-arch-overview-ui.png)

### 域层（Domain layer）

域层负责**封装复杂的业务逻辑或多个 ViewModels 重复使用的简单业务逻辑**。这一层是可选的，因为并非所有应用程序都有这些要求。只有在需要时，例如，为了处理复杂性或提高可重用性，才应该使用它。

为了保持域层用例类的简单和轻量级，**每个用例类应只负责一个功能，而且不应包含可变数据**。

![ArchitectureDomain](/Android/Architecture/assets/mad-arch-overview-domain.png)

### 数据层（Data layer）

![ArchitectureData](/Android/Architecture/assets/mad-arch-overview-data.png)

## 最佳实践

* 不要在 Android 组件（Activity、Service）中存储数据
* 减少对 Android 系统类的依赖（Context）
* 为各个模块建立明确的责任界限
* 尽可能减少每个模块的暴露
* 使代码每个部分都可独立测试
* 逻辑代码自己负责转移到正确的线程中运行
* 尽可能多的本地持久化新拉取的数据
