# 工厂方法（Factory Method）模式 - 创建型模式（Creational）

工厂方法模式提供了一种将对象的创建委托给子类的方式。在工厂方法模式中，通过定义一个创建对象的接口，但具体的对象创建逻辑由子类来实现。

在工厂方法模式中，有以下几种角色：

***抽象产品（Product）*** ：定义了产品的接口，是所有具体产品的共同基类。

***具体产品（Concrete Product）*** ：实现抽象产品接口，具体的产品类。

***抽象工厂（Factory）*** ：定义了创建产品的接口，包含一个抽象的工厂方法，用于返回具体产品的实例。

***具体工厂（Concrete Factory）*** ：实现抽象工厂接口，具体的工厂类。负责实际创建具体产品的对象。

**工厂方法模式与抽象工厂模式的区别是前者是一个工厂对应一个产品，后者是一个工厂对应多个产品。**

## 代码示例

### Kotlin

```kt
// 抽象产品
interface Product {
    fun operation()
}

// 具体产品A
class ConcreteProductA : Product {
    override fun operation() = println("ConcreteProductA operation")
}

// 具体产品B
class ConcreteProductB : Product {
    override fun operation() = println("ConcreteProductB operation")
}

// 抽象工厂
interface Factory {
    fun createProduct(): Product
}

// 具体工厂A
class ConcreteFactoryA : Factory {
    override fun createProduct() = ConcreteProductA()
}

// 具体工厂B
class ConcreteFactoryB : Factory {
    override fun createProduct() = ConcreteProductB()
}

// 使用示例
fun main() {
    val factoryA = ConcreteFactoryA()
    val productA = factoryA.createProduct()
    productA.operation()

    val factoryB = ConcreteFactoryB()
    val productB = factoryB.createProduct()
    productB.operation()
}
```

### Java

```java
    // 抽象产品
    public interface Product {
        void operation();
    }

    // 具体产品 A
    public static class ConcreteProductA implements Product {
        @Override
        public void operation() {
            System.out.println("ConcreteProductA operation");
        }
    }

    // 具体产品 B
    public static class ConcreteProductB implements Product {
        @Override
        public void operation() {
            System.out.println("ConcreteProductB operation");
        }
    }

    // 抽象工厂
    public interface Factory {
        Product createProduct();
    }

    // 具体工厂 A
    public static class ConcreteFactoryA implements Factory {
        @Override
        public Product createProduct() {
            return new ConcreteProductA();
        }
    }

    // 具体工厂 B
    public static class ConcreteFactoryB implements Factory {
        @Override
        public Product createProduct() {
            return new ConcreteProductB();
        }
    }

    // 使用示例
    public static void main(String[] args) {
        Factory factoryA = new ConcreteFactoryA();
        Product productA = factoryA.createProduct();
        productA.operation();

        Factory factoryB = new ConcreteFactoryB();
        Product productB = factoryB.createProduct();
        productB.operation();
    }
```
