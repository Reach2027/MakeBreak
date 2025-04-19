# 数据层（Data Layer）

数据层包含数据及业务逻辑，主要由数据模型（Model）、数据仓库（Repository）和数据源（DataSource）组成。

<img 
    src="/Android/Architecture/assets/mad-arch-overview-data.png"
    alt="Architecture Data"
    width="666">

## 职责

* 为界面层提供数据
* 处理来自界面层的事件

## 设计

* 数据层对外仅暴露**数据仓库抽象**和**必要的数据模型**，隐藏实现细节
* 使用依赖注入，对外提供依赖
* 对外通过不可变类型暴露数据（如 StateFlow），遵循单一事实来源
* 对外暴露的方法必须是主线程安全的，内部处理线程切换
* 使用 Kotlin Coroutine 和 Flow
* 使用自动依赖注入框架（如Koin）处理数据层内实例的生命周期
  * 创建单例数据仓库类，整个应用共享
  * 创建特定界面之间共享的数据仓库

## 数据层角色

### 数据模型（Model）

#### 职责

* 定义应用程序的数据

#### 设计

* 数据模型分离，当应用需要的数据与数据源提供的数据不一致时，创建新的数据模型
  * 比如一个接口返回了很多的参数，但是应用其实不需要那么多参数或者需要的参数类型不一致（如接口返回时间戳，期望日期字符串）

### 数据仓库（Repository）

#### 职责

* 解决多个数据源之间的冲突，对外暴露数据，集中更改数据
* 应用程序业务逻辑

#### 设计

* 定义数据仓库抽象，数据仓库实现类不对外暴露
* 一个数据仓库可包含零个或多个数据源
  * 当一个数据仓库仅依赖一个数据源时，可以在数据仓库内直接实现数据源
  * 依赖多个数据源时，数据仓库内则不应存在数据源的实现
* 需要聚合多个数据仓库时，专门建立一个聚合多个数据仓库的类

<img 
    src="/Android/Architecture/assets/mad-arch-data-multiple-repos.png"
    alt="Architecture Data"
    width="666">

#### 命名规范

`type of data` + Repo

示例：NewsRepo、MoviesRepo

### 数据源（DataSource）

#### 职责

* 使用系统能力获取数据

#### 设计

* 数据源类不对外暴露
* 每个类型的数据源都实现对应的数据源类（远程、数据库、文件等），遵循单一职责原则

#### 命名规范

`type of data` + `type of source` + DataSource

示例：NewsRemoteDataSource、NewsLocalDataSource
