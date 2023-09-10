# 依赖注入（Dependency Injection）

## 什么是依赖注入

先看以下代码

```kt
class Engine() {
    fun start() {
        ...
    }
}

class Car1 {

    private val engine = Engine()

    fun start() {
        engine.start()
    }
}

// 构造函数注入
class Car2(private val engine: Engine) {
    fun start() {
        engine.start()
    }
}

// 字段注入
class Car3() {
    lateinit var engine: Engine
    
    fun start() {
        engine.start()
    }
}

fun main() {
    val car1 = Car1()
    car1.start()

    val engine = Engine()
    val car2 = Car2(engine)
    car2.start()
    val car3 = Car3()
    car3.engine = engine
    car3.start()
}
```

有意义的依赖注入

```kt
interface Engine {
    fun start()
}

class Car(private val engine: Engine) {
    fun start() {
        engine.start()
    }
}

fun provideEngine() = BenzEngine()

internal class BenzEngine: Engine {
    override fun start() {
        ...
    }
}

fun main() {
    val car = Car(provideEngine())
    car.start()
}
```

依赖注入是一种设计模式（因提出时间较晚，所以不在gof设计模式里），依赖注入是控制反转（Inversion of Control）设计原则的实现方式之一。

定义：依赖注入将类依赖关系的创建与类自身的行为分离。

最佳实践：依赖注入一般和抽象一起使用。

## 依赖注入的意义

* 降低类之间的耦合度
* 提升类的可复用性
* 易于重构
* 提升类的可测试性

## Android 常见自动依赖框架

|       | Hilt | Koin |
| :---: | :---: | :---: |
| 静态依赖注入 | 是 | 是 |
| 编写语言 | Java | Kotlin |
| 依赖注入方式 | 注解 | 支持 DSL 和 注解 |
| 优点 | 可直接在Android studio内查看依赖树 | 支持KMP，使用比hilt简单，可不使用注解避免增加编译时间 |

## 自动依赖注入框架Koin

GitHub地址 https://github.com/InsertKoinIO/koin

Gradle设置

```kts
mavenCentral()
```

```kts
 implementation("io.insert-koin:koin-android:3.4.3")
 implementation("io.insert-koin:koin-androidx-compose:3.4.6")
 implementation("io.insert-koin:koin-androidx-compose-navigation:3.4.6")
```
