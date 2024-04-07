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

让依赖注入更有意义

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

结合上面的代码示例来看，从常规写法到依赖注入在到依赖注入+抽象，原本的出发点是降低类之间的耦合度，但是这样一来类的可复用性、可测试性、可重构、可维护性都得到提升了。这说明衡量代码质量的指标低耦合、高内聚、可测试性、可维护性、可读性等这些指标都是互相关联的，其中一个指标好的话，其他的也就不会差。

在代码质量指标中，可测试性是最好去检查和衡量的，要判断代码的可测试性，去写这段代码的单元测试就行了。如果单元测试不能很容易的写出或者不能有效的测试代码，那么这段代码的可测试性就不高，就应该改进这段代码了。利用可测试性的提高，可以方便的编写单元测试、集成测试，进一步提升代码质量。

我认为编写单元测试是一种有效检查代码并提升代码质量的手段。编写完代码后，编写单元测试就像学生时期做完数学题后在重新推导一次来检查结果是否正确一样。一组合格的单元测试写出来就表明这段代码的逻辑清晰、低耦合、健壮性好，Review代码时也可通过查看其单元测试来了解这段代码逻辑及判断这段代码的质量。当每个方法单元测试都通过都没问题，把它们组合起来也就不会有什么问题。

综上所诉，使用依赖注入+抽象可有效提升代码可测试性，利用其带来的可测试性提升，编写有效的单元测试，可有效提升代码质量，我觉得有必要在架构规范中引入依赖注入。

## Android 常见自动依赖框架

|       | Hilt | Koin |
| :---: | :---: | :---: |
| 编写语言 | Java | Kotlin |
| 依赖注入方式 | 注解 | 支持 DSL 和 注解 |
| 依赖注入检查 | 编译时检查 | 使用DSL时，需通过本地单元测试检查 <br> 使用注解时编译时检查 |
| 优点 | 可直接在Android studio内查看依赖树 | 1、支持KMP、Compose MultiPlatform <br> 2、使用比hilt简单 <br> |

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

首先得在Application中设置koin

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

scoped {} // 支持在Activity、Fragment、Activity ViewModel内保持同一个实例

single {} // 单例

viewModel {} // 创建Android ViewModel

构造函数DSL
factoryOf

scopedOf

singleOf

viewModelOf
```

Android Context 注入

```kt
androidApplication() // Application
```

示例

```kt
val appInfoDataModule = module {

    // 包含依赖项模块
    includes(dispatcherModule, scopeModule, databaseModule)

    // 使用常规DSL定义依赖
    // 当要使用限定符（qualifier）获取依赖时，只能使用这种方式创建依赖
    factory<AppInfoSource> {
        DefaultAppInfoSource(
            androidApplication(),
            get(),
            get(qualifier = DispatcherQualifier.IO.qualifier),
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

Koin Scope

```kt

activityScope() // Activity 使用，Activity 的生命周期

activityRetainedScope() // Activity 使用，ViewModel 的生命周期

fragmentScope() // Fragment 内使用，Fragment 的生命周期

// Activity 或 Fragment 继承 AndroidScopeComponent 接口
class MainActivity : ComponentActivity(), AndroidScopeComponent {

    override val scope: Scope by activityScope()
}

scope<MainActivity> {
    scopedOf(::ClassA)
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
by viewModel() // 懒加载

getViewModel() // 立马获取

// Fragment 获取 Activity ViewModel
by activityViewModel() // 懒加载

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

在模块build.gradle内导入koin test 依赖，然后在test目录下新建单元测试，可以检查每个 Module 的依赖的正确性，也可建立一个总的 Module 检查整个软件的依赖注入正确性（此功能目前为实验性功能，不稳定）。

```kt
class DiTest {

    @Test
    fun diTest() {
        appModule.verify(
            extraTypes = listOf(Application::class, SavedStateHandle::class),
        )
    }
}
```

### 使用 Koin 编写 AndroidTest

AndroidTest 是从 Application 开始的，要重写其中的依赖关系，得先从替换 Application 开始。

```kt
class TestApplication : Application()

class InstrumentationTestRunner : AndroidJUnitRunner() {
    override fun newApplication(
        classLoader: ClassLoader?,
        className: String?,
        context: Context?
    ): Application {
        return super.newApplication(classLoader, TestApplication::class.java.name, context)
    }
}

android{
    defaultConfig {
        applicationId = "com.example.kotlinexample"
        minSdk = 21
        targetSdk = 34
        versionCode = 1
        versionName = "0.0.1-alpha"

        testInstrumentationRunner = "com.example.kotlinexample.InstrumentationTestRunner"
        vectorDrawables {
            useSupportLibrary = true
        }
    }
}
```

在TestApplication中替换依赖

```kt
class TestApplication : Application() {
    override fun onCreate() {
        super.onCreate()
        startKoin {
            modules(productionModule, instrumentedTestModule)
        }
    }
}
```

使用test rule 替换依赖

```kt
class KoinTestRule(
    private val modules: List<Module>
) : TestWatcher() {

    override fun starting(description: Description) {
        startKoin {
            androidLogger()
            androidContext(InstrumentationRegistry.getInstrumentation().targetContext.applicationContext)
            modules(modules)
        }
    }

    override fun finished(description: Description) {
        stopKoin()
    }
}

// 使用rule替换依赖
@get:Rule
val koinTestRule = KoinTestRule(
    modules = listOf(fakeAppInfoUiModule)
)
```
