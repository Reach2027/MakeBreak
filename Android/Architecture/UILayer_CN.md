# 界面层（UI Layer）

界面层包含界面显示及界面逻辑，主要由用户界面(UI)、用户界面状态（UiState）和状态持有者（StateHolder）组成。

<img
    src="/Android/Architecture/assets/mad-arch-overview-ui.png"
    alt="Architecture UI"
    width="666">

## 职责

* 显示数据
* 响应用户交互

## 设计

* 为每个界面定义界面状态类，状态持有者对界面暴露界面状态
* 感知生命周期
* 使用单向数据流管理界面状态
* 使用 Kotlin Coroutine 和 Flow

<img
    src="/Android/Architecture/assets/mad-arch-ui-udf.png"
    alt="Architecture UDF"
    width="666">

## 界面层角色

### 用户界面（UI）

#### 职责

* 显示数据
* 响应用户操作，向状态持有者传递用户操作

#### 设计

* 根据界面状态来显示界面
* 仅负责显示，不处理任何逻辑

#### 命名规范

有逻辑的根 Compose 函数

`functionality` + Route

无状态的根 Compose 函数

`functionality` + Screen

示例：NewsRoute、NewsScreen

### 状态持有者(StateHolder)

#### 职责

* 管理界面状态，将数据层的数据转换为界面状态
* 处理界面传递过来的用户操作，根据用户操作来调用数据层能力、更新界面状态

#### 设计

* 生命周期比界面要长一点，在配置更改界面重建时不会被销毁
  * Android ViewModel 是个很好的状态持有者
* 单一事实来源
* 对外暴露的方法必须是主线程安全的，内部处理线程切换

#### 命名规范

`functionality` + ViewModel

示例：NewsViewModel

### 界面状态（UiState）

#### 职责

* 定义界面显示的数据

#### 设计

* 属性全是不可变且有默认值，通过 data class 的 copy 方法更新
* 一个界面对应一个还是多个界面状态类，可从以下两方面考虑：
  * 状态是否相关
  * 更新频率差异大
  
#### 命名规范

`functionality` + UiState

示例：NewsUiState
