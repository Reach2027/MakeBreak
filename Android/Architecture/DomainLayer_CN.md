# 域层（Domain layer）

**域层负责封装复杂的业务逻辑或多个 ViewModel 重复使用的简单业务逻辑**。域层是可选的，有需求时再去实现。

<img 
    src="/Android/Architecture/assets/mad-arch-overview-domain.png"
    alt="Architecture Domain"
    width="666">

## 域层设计

* 保持域层用例类的简单和轻量级，**每个用例类应只负责一个功能，而且不应包含可变数据**
* 方法必须是主线程安全的，内部处理线程切换

## 命名规范

`verb in present tense` + `noun/what (optional)` + UseCase

示例：LogOutUseCase、GetLatestNewsUseCase
