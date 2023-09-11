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

### Gradle设置

```kts
mavenCentral()
```

```kts
implementation("io.insert-koin:koin-android:3.4.3")
implementation("io.insert-koin:koin-androidx-compose:3.4.6")
implementation("io.insert-koin:koin-androidx-compose-navigation:3.4.6")

testimplementation("io.insert-koin:koin-test:3.4.3")
```
### Koin的使用

在Application中设置koin

```kt
class AppApplication : Application() {

    override fun onCreate() {
        super.onCreate()
        startKoin {
            androidLogger()
            androidContext(this@AppApplication)
            modules(appModule)
        }
    }
}
```

使用Koin DSL创建依赖

```kt
常规DSL
factory {} // 每次都 new

scope {} // 

single {} // 单例

viewModel {} // 创建Android ViewModel

构造函数DSL
factoryOf

scopedOf

singleOf

viewModelOf
```

示例
```kt
val appInfoDataModule = module {

    // 包含前置依赖项模块
    includes(dispatcherModule, scopeModule, databaseModule)

    // 使用常规DSL定义依赖
    // 当要使用限定符（qualifier）获取依赖时，只能使用这种方式创建依赖
    factory<AppInfoSource> {
        DefaultAppInfoSource(
            get(),
            get(),
            get(qualifier = qualifier(DispatcherQualifier.IO)),
        )
    }

    // 使用构造函数DSL定义依赖
    // 能使用这个就使用这个
    factoryOf(::DefaultAppInfoRepository) {
        // 绑定类型
        bind<AppInfoRepository>()
    }
}
```

使用限定符创建相同类型的依赖

```kt
// 限定符可以是 String 或 enum 
enum class DispatcherQualifier {
    IO, DEFAULT
}

val dispatcherModule = module {
    single<CoroutineDispatcher>(qualifier(DispatcherQualifier.IO)) { Dispatchers.IO }
    single<CoroutineDispatcher>(qualifier(DispatcherQualifier.DEFAULT)) { Dispatchers.Default }
}

// 外部通过限定符获取依赖项示例
get(qualifier = qualifier(DispatcherQualifier.IO))
```

创建ViewModel

```kt
val appInfoUiModule = module {
    includes(appInfoDataModule)

    viewModelOf(::AppInfoViewModel)
}
```

在Activity、Fragment内注入ViewModel

```kt
by viewModel() // 延迟获取

getViewModel() // 立马获取

// Fragment 获取 Activity ViewModel
by activityViewModel() // 延迟获取

getActivityViewModel() // 立马获取

// 获取某个导航图生命周期内的ViewModel，需提供导航图id
val mainViewModel: NavViewModel by koinNavGraphViewModel(R.id.my_graph)
```

在Compose内注入ViewModel

```kt
// 未使用 Jetpack Navigation
val viewModel = koinViewModel<AppInfoViewModel>()

// 使用了 Jetpack Navigation
val viewModel = koinNavViewModel<AppInfoViewModel>()
```

### 检查依赖注入的正确性

导入koin test 包，然后在test目录下新建单元测试，可以检查每个 Module 的依赖的正确性，也可建立建立一个总的 Module 检查整个软件的依赖注入正确性（此功能目前为实验性功能，不稳定）。

```kt
class DiTest {

    @Test
    fun diTest() {
        appModule.verify(
            extraTypes = listOf(Application::class),
        )
    }
}
```

