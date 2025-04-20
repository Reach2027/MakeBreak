# App 架构设计

## 架构设计的意义

* 提升团队成员设计、开发功能思路的一致性，提升团队协作效率、开发效率
* 提升代码可扩展性
* 提升代码可维护性
* 提升代码可测试性

## 架构设计原则

### 关注点分离，将一个功能拆分为多个组成部分

UI 与数据分离

### 职责单一原则，定义好每个组成部分的职责及边界

界面层负责界面显示及界面逻辑，数据层负责应用数据及业务逻辑

### 数据模型驱动用户界面

UI 行为由数据模型驱动，设计好数据模型

### 单向数据流（Unidirectional data flow - UDF）

状态只朝一个方向流动，修改数据的事件向相反方向流动

### 单一事实源（Single source of truth - SSOT）

数据持有者通过不可变类型暴露数据（如 StateFlow），暴露更改数据的方法，外部调用数据持有者内的更改数据方法去更改数据

### 全局响应式编程

全局使用数据流，如使用 Kotlin Flow

### 依赖注入

使用依赖注入处理类之间的依赖关系，可以选择自动依赖注入框架

## 推荐架构

* 界面层（UI Layer）：界面层负责显示应用数据及接收用户操作，一般由用户界面(UI)、用户界面状态（UiState）和状态持有者（StateHolder）组成
* 数据层（Data Layer）：数据层负责提供数据及处理来自界面层的请求，一般由数据模型（Model）、数据仓库（Repository）和数据源（DataSource）组成
* 域层（Domain Layer）：域层是可选层，职责是承接界面层和数据层之间的部分逻辑，域层仅包含用例类（UseCase）

<img 
    src="/architecture/res/arch_overview.png"
    alt="Architecture Overview"
    width="666">

## 界面层（UI Layer）

<img
    src="/architecture/res/arch_ui_udf.png"
    alt="Architecture UI"
    width="666">

### 职责

* 显示应用数据
* 接收用户操作

### 界面层规范

* 职责单一原则
* 单向数据流（Unidirectional data flow - UDF）
* 单一事实源（Single source of truth - SSOT）
* 响应式编程
* 依赖注入

### 界面层组成

* 界面 UI（Compose）
* 状态持有者 StateHolder（ViewModel）
* 界面状态 UiState

#### 界面 UI

##### 界面职责

* 收集状态持有者内的界面状态数据流，并根据界面状态显示界面
* 接收并分发用户操作
  * 业务逻辑抛给状态持有者处理
  * 不涉及界面状态的界面逻辑内部处理

*说明*：
业务逻辑，如查询用户信息。
界面逻辑，如界面跳转，界面显示隐藏

##### 界面命名（Compose）

有逻辑的屏幕级 Compose 函数

*functionality* + Route

无状态的屏幕级 Compose 函数

*functionality* + Screen

示例：NewsRoute、NewsScreen

##### 界面规范

* 使用生命周期方法收集界面状态数据流
* 业务逻辑必须抛给状态持有者处理
* 设计修改界面状态的逻辑必须抛给状态持有者处理
* 不涉及界面状态的界面逻辑内部处理

#### 状态持有者 StateHolder

##### 状态持有者职责

* 管理界面状态
  * 从数据层获取数据并转换为界面状态
  * 暴露界面状态数据流给界面使用
* 处理界面传过来的请求
  * 涉及数据的业务逻辑抛给数据层处理

##### 状态持有者命名

*functionality* + VM

示例：NewsVM

##### 状态持有者规范

* 单一事实源（Single source of truth - SSOT）
* 响应式编程
* 依赖注入
* 对外暴露不可变数据流
* 对外暴露方法必须是主线程安全的
* 涉及数据的业务逻辑抛给数据层处理
* 生命周期比界面长，在界面因屏幕配置变更（如屏幕旋转）销毁重建时不会被销毁

#### 界面状态 UiState

##### 界面状态职责

* 定义界面显示

##### 界面状态命名

*functionality* + UiState

示例：NewsUiState

##### 界面状态规范

* 属性不可变且有默认值，通过 data class 的 copy 方法更新
* 一个界面对应一个还是多个界面状态类，可从以下两方面考虑：
  * 状态是否相关
  * 更新频率差异大不大

## 数据层（Data Layer）

<img 
    src="/architecture/res/arch_data_overview.png"
    alt="Architecture Data"
    width="666">

### 数据层职责

* 为界面层提供数据
* 处理来自界面层的请求

### 数据层规范

* 职责单一原则
* 单向数据流（Unidirectional data flow - UDF）
* 单一事实源（Single source of truth - SSOT）
* 响应式编程
* 依赖注入
* 对外仅暴露必要的数据模型及数据仓库抽象
* 对外暴露不可变数据类型

### 数据层组成

* 数据仓库 Repository
* 数据源 DataSource
* 数据模型 Model

#### 数据仓库 Repository

##### 数据仓库职责

* 数据仓库是数据层的入口，为界面层提供数据
* 集中管理应用数据
  * 合并多个数据源
  * 合并多种类型数据
  * 更改数据
  * 内存缓存数据
* 处理应用业务逻辑

##### 数据仓库命名

*type of data* + Repo

示例：NewsRepo

##### 数据仓库规范

* 职责单一原则
  * 为每个数据类型实现对应数据仓库
  * 需要聚合多个数据仓库时，专门建立一个聚合多个数据仓库的类
* 为每个数据仓库类定义接口（抽象），数据仓库实现类不对外暴露
* 一个数据仓库可包含零个或多个数据源
  * 当一个数据仓库仅依赖一个数据源时，可以在数据仓库内直接实现数据源
  * 依赖多个数据源时，数据仓库内则不应存在数据源的实现
    <img 
    src="/architecture/res/arch_data_multiple_repos.png"
    alt="Architecture Data"
    width="666">
* 对外暴露的方法必须是主线程安全的

#### 数据源 DataSource

##### 数据源职责

* 使用某一系统能力（网络请求、数据库、文件等）获取数据

##### 数据源命名

*type of data* + *type of source* + Source

示例：NewsRemoteSource、NewsLocalSource

##### 数据源规范

* 遵循单一职责原则，为每个类型的数据源都实现对应的数据源类（网络、数据库、文件等）
* 为每个数据源类定义接口（抽象），数据源抽象及实现类不对外暴露

#### 数据模型 Model

##### 数据模型职责

* 定义应用程序的数据

##### 数据模型命名

*type of data* + Model

示例：NewsModel

##### 数据模型规范

* 属性为不可变类型

## 域层（Domain Layer）

<img 
    src="/architecture/res/arch_domain_overview.png"
    alt="Architecture Domain"
    width="666">

### 域层组成

域层仅包含用例类

#### 用例类（UseCase）

##### 用例类职责

* 封装复杂逻辑，避免界面层、数据层膨胀，比如处理多个数据仓库
  <img 
    src="/architecture/res/arch_domain_multiple_repos.png"
    alt="Architecture Domain"
    width="666">
* 封装由多个界面层重用的逻辑

##### 用例类命名规范

*verb in present tense* + *noun/what (optional)* + UC

示例：LogOutUC、GetLatestNewsUC

##### 用例类规范

* 职责单一原则，每个用例类应只负责一个功能
* 不能持有可变数据流
* 方法必须是主线程安全的
