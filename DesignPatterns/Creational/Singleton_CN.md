# 单例（Singleton）模式 - 创建型模式（Creational）

单例模式确保类在应用程序中只有一个实例，并提供全局访问点来访问该实例。

## 代码示例

### Kotlin

```kt
// 单例
object Singleton {

    fun info() {
        println(this.hashCode())
    }

}
```

### Java

```java
// 枚举单例
public enum Singleton {
    INSTANCE;

    public void info() {
        System.out.println(INSTANCE.hashCode());
    }

}
```
