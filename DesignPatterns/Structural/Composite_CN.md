# 组合（Composite）模式 - 结构模式（Structural）

组合模式允许将对象组合成树形结构以表示"整体-部分"的层次结构。

在组合模式中，有以下三种角色：

***组件（Component）*** ：组合对象和叶节点对象的通用接口，可以包含子组件的操作方法。它可以是抽象类或接口。

***叶节点（Leaf）*** ：表示组合的叶节点对象，没有子组件。实现了组件接口的方法。

***组合节点（Composite）*** ：表示包含子组件的组合对象。它实现了组件接口，并且可以包含其他组件或叶节点作为子组件。

组合模式的目的是通过统一对待单个对象和组合对象，简化调用者的代码。调用者可以像处理单个对象一样处理组合对象，而无需关心对象的具体类型。

## 代码示例

### Kotlin

```kt
// 组件接口
interface Component {
    fun operation();
}

// 叶节点
class Leaf : Component {
    override fun operation() {
        println("执行叶节点操作")
    }
}

// 组合节点
class Composite : Component {
    private val children = mutableListOf<Component>()

    fun add(component: Component) {
        children.add(component)
    }

    fun remove(component: Component) {
        children.remove(component)
    }

    override fun operation() {
        println("执行组合节点操作")
        for (component in children) {
            component.operation()
        }
    }
}

// 客户端代码
fun main() {
    val leaf1 = Leaf()
    val leaf2 = Leaf()

    val composite = Composite()
    composite.add(leaf1)
    composite.add(leaf2)

    leaf1.operation()
    composite.operation()
}
```

### Java

```java
// 组件接口
interface Component {
    void operation();
}

// 叶节点
class Leaf implements Component {
    @Override
    public void operation() {
        System.out.println("执行叶节点操作");
    }
}

// 组合节点
class Composite implements Component {
    private List<Component> children = new ArrayList<>();

    public void add(Component component) {
        children.add(component);
    }

    public void remove(Component component) {
        children.remove(component);
    }

    @Override
    public void operation() {
        System.out.println("执行组合节点操作");
        for (Component component : children) {
            component.operation();
        }
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        Component leaf1 = new Leaf();
        Component leaf2 = new Leaf();

        Composite composite = new Composite();
        composite.add(leaf1);
        composite.add(leaf2);

        leaf1.operation();
        composite.operation();
    }
}
```
