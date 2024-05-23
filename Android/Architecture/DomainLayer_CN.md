# 域层（Domain Layer）

域层是可选层，主要职责是承接界面层和数据层之间的部分业务逻辑，域层仅包含用例类（UseCase）。

<img 
    src="/Android/Architecture/assets/mad-arch-overview-domain.png"
    alt="Architecture Domain"
    width="666">

## 职责

* 提取界面层、数据层内复杂的业务逻辑
* 提取多个 ViewModel 重复使用的简单业务逻辑

## 设计

* 保持域层用例类的简单，**每个用例类应只负责一个功能，而且不应包含可变数据流**
* 方法必须是主线程安全的，内部处理线程切换
* 有需要时再去实现

## 命名规范

`verb in present tense` + `noun/what (optional)` + UseCase

示例：LogOutUseCase、GetLatestNewsUseCase
