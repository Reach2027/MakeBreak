# 界面层（UI layer）

界面层包含界面相关状态及界面逻辑，由用户界面(UI)、用户界面状态（UiState）和状态持有者（StateHolder）组成。

<img 
    src="/Android/Architecture/assets/mad-arch-overview-ui.png"
    alt="Architecture UI"
    width="666">

## 设计

* 为每个界面定义界面状态类，状态持有者对界面暴露界面状态
* 感知生命周期
* 使用单向数据流管理界面状态
<img 
    src="/Android/Architecture/assets/mad-arch-ui-udf.png"
    alt="Architecture UI"
    width="666">

### 界面状态类设计

* 属性全是不可变且有默认值，通过 data class 的 copy 方法更新
* 一个界面对应一个还是多个界面状态类，可从以下两方面考虑：
  * 状态是否相关
  * 更新频率差异大

### 状态持有者类设计

* 生命周期比界面要长一点，在配置更改界面重建时不会被销毁
  * Android ViewModel 是个很好的状态持有者
* 单一事实来源
* 对外暴露的方法必须是主线程安全的，内部处理线程切换

#### 状态持有者类职责

* 暴露界面状态，界面仅需关注暴露的状态
* 处理界面逻辑，界面仅负责根据状态渲染界面

## 命名规范

用户界面（Compose）：

有逻辑的根 Compose 函数

`functionality` + Route

无状态的根 Compose 函数

`functionality` + Screen

示例：NewsRoute、NewsScreen

用户界面状态类：

`functionality` + UiState

示例：NewsUiState

状态持有者类：

`functionality` + ViewModel

示例：NewsViewModel
