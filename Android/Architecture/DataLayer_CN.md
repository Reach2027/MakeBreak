# 数据层（Data layer）

数据层包含应用数据及业务逻辑，由数据仓库（Repository）和数据源（Data Source）组成。

<img 
    src="/Android/Architecture/assets/mad-arch-overview-data.png"
    alt="Architecture Data"
    width="666">

## 设计原则

* 单一职责原则
* 控制反转（依赖注入）
* 隐藏实现细节
* 单一事实来源

## 数据层设计

* 数据层对外仅暴露**数据仓库抽象**和**数据模型**，隐藏实现细节
* 使用依赖注入，对外提供依赖
* 对外通过不可变类型暴露数据（如 StateFlow），遵循单一事实来源
* 对外暴露的方法必须是主线程安全的，内部处理线程切换逻辑
* 使用 Kotlin Coroutine 和 Flow
* 使用自动依赖注入框架（如Koin）处理数据层内实例的生命周期，单例、多个类之间共享数据等情况

### 数据仓库类设计

* 定义数据仓库抽象，数据仓库实现类不对外暴露
* 一个数据仓库可包含零个或多个数据源
* 当一个数据仓库仅依赖一个数据源时，可以在数据仓库内直接实现数据源；依赖多个数据源时，数据仓库内则不应存在数据源的实现

### 数据源类设计

* 数据源类不对外暴露

## 命名规范

数据仓库类：

`type of data` + Repo

示例：NewsRepo、MoviesRepo

数据源类：

`type of data` + `type of source` + DataSource

示例：NewsRemoteDataSource、NewsLocalDataSource
