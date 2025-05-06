# Compose 规范

## Composable 函数

### Composeable 函数命名

* 显示界面的 Composeable 函数，没有返回值，使用大驼峰命名
* 有返回值的 Composeable 函数，使用小驼峰命名

```kt
@Composable
fun TopAppBar(
    ...
）

@Composable
fun topAppBarColors(
    ...
): TopAppBarColors
```

### Composeable 函数参数排序

* 强制参数在前面，有默认值的参数在后面
* Modifier 在有默认值的参数第一位

```kt
@Composable
fun TopAppBar(
    title: @Composable () -> Unit,
    modifier: Modifier = Modifier,
    navigationIcon: @Composable () -> Unit = {},
    actions: @Composable RowScope.() -> Unit = {},
    expandedHeight: Dp = TopAppBarDefaults.TopAppBarExpandedHeight,
    windowInsets: WindowInsets = TopAppBarDefaults.windowInsets,
    colors: TopAppBarColors = TopAppBarDefaults.topAppBarColors(),
    scrollBehavior: TopAppBarScrollBehavior? = null
)
```

### Composeable 函数的依赖放在参数里

```kt
// 反例
@Composable
private fun MyComposable() {
    val viewModel = viewModel<MyViewModel>()
    // ...
}

// 正例
@Composable
private fun MyComposable(
    viewModel: MyViewModel = viewModel(),
) {
    // ...
}
```

### Composeable 函数应显示界面或返回值，不可同时兼顾两者

Composeable 函数有返回值则不应该包含显示界面逻辑，显示界面就不能有返回值

### Composeable 函数之间不要传递可变类型

反例：在Composeable 函数之间传递  ```ArrayList<T> MutableState<T>```

### Composeable 函数里内容的正确性不能依赖调用方

```kt
// 反例
Column {
    InnerContent()
}

@Composable
private fun InnerContent() {
    Text(...)
    Image(...)
    Button(...)
}

// 正例
@Composable
private fun InnerContent() {
    Column {
        Text(...)
        Image(...)
        Button(...)
    }
}
```

### CompositionLocal 命名应以 Local 前缀

Local + *type of data*

```kt
val LocalDensity = staticCompositionLocalOf<Density> {
    noLocalProvidedFor("LocalDensity")
}
```

### Composeable 预览函数命名

* 单个预览使用 Preview 后缀
* 多个预览使用 Previews 后缀

### Composeable 预览函数必须是 private

## Compose State

### 上升所有逻辑

Composeable 函数需尽量是无状态的（stateless），即只负责显示不负责逻辑。无状态的 Composeable 函数逻辑清晰、可测试性高。

* 一个页面至少有两个 Composeable 函数，一个负责处理逻辑，一个负责显示，状态持有者实例（ViewModel）、状态数据流实例（Compose state、Kotlin flow）只允许处理逻辑的 Composeable 函数持有，不允许向下传递
* 负责显示的 Composeable 函数事件（如点击事件）需要上升到负责处理逻辑的 Composeable 函数内

```kt
@Composable
private fun LearnRoute(
    navController: NavController,
    viewModel: LearnVM = koinViewModel(),
) {
    val text by viewModel.text.collectAsStateWithLifecycle()

    LearnScreen(
        onBackClick = { navController.navigateUp() },
        text = text,
    )
}

@Composable
private fun LearnScreen(
    onBackClick: () -> Unit,
    text: String,
) {
    TitleBarWithBack(onBackClick = onBackClick) {
        Box(modifier = Modifier.fillMaxSize()) {
            Text(
                text = text,
                modifier = Modifier.align(Alignment.Center),
            )
        }
    }
}
```

### Compose state 在 Composeable 函数内必须使用 remember

Composeable 函数在刷新时会多次执行，不使用 remember 则 Compose state 会多次赋值引发异常。

```kt
var isSuccess by remember { mutableStateOf(false) }
```

### 能使用 @Immutable 注解类就使用

### 避免使用不稳定的集合

Compose 编译器会将 `List<T> Map<T> Set<T>` 识别为不稳定类型

```kt
val list: List<String> = mutableListOf<String>()
```

推荐使用 [Kotlinx Immutable](https://github.com/Kotlin/kotlinx.collections.immutable) 集合

可选使用 @Immutable 注解类包装普通集合

```kt
  @Immutable
    data class StringList(val items: List<String>)
    // ...
    val list: StringList = StringList(yourList)
```

## Modifier 修饰符

### 自定义公共组件暴露 Modifier 参数

### 不要重复使用 Modifier

一个 Compose 组件对应一个 Moidifer，多个 Compose 不能公用一个 Modifier

```kt
@Composable
private fun InnerContent(modifier: Modifier = Modifier) {
    Column(modifier) {
        Text(Modifier.clickable(), ...)
        Image(Modifier.size(), ...)
        Button(Modifier, ...)
    }
}
```

### Modifier 参数需是有默认值的参数

### 不要使用 Modifier 扩展函数，使用 Modifier.composed

Modifier.composed 能限制重组仅发生在修饰的实例，Modifier 扩展函数可能会影响整个 Composeable 函数

## 参考链接

<https://twitter.github.io/compose-rules/rules/>
