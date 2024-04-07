# 抽象工厂（Abstract Factory）模式 - 创建型模式（Creational）

抽象工厂模式提供了一种接口来创建一系列相关或相互依赖的对象，而无需指定它们具体的类。通过引入抽象工厂和具体工厂的概念，以及抽象产品和具体产品的概念，实现了对象的创建和使用的分离。

在抽象工厂模式中，有以下几种角色：

***抽象产品（Product）*** ：定义了产品的接口，是所有具体产品的共同基类。

***具体产品（Concrete Product）*** ：实现抽象产品接口，具体的产品类。

***抽象工厂（Factory）*** ：定义了创建产品的接口，包含一个抽象的工厂方法，用于返回具体产品的实例。

***具体工厂（Concrete Factory）*** ：实现抽象工厂接口，具体的工厂类。负责实际创建具体产品的对象。

**工厂方法模式与抽象工厂模式的区别是前者是一个工厂对应一个产品，后者是一个工厂对应多个产品。**

## 代码示例

### Kotlin

```kt
// 抽象产品A
interface AbstractProductA {
    fun operationA()
}

// 具体产品A1
class ConcreteProductA1 : AbstractProductA {
    override fun operationA() = println("ConcreteProductA1 operation")
}

// 具体产品A2
class ConcreteProductA2 : AbstractProductA {
    override fun operationA() = println("ConcreteProductA2 operation")
}

interface AbstractProductB {
    fun operationB()
}

// 具体产品B1
class ConcreteProductB1 : AbstractProductB {
    override fun operationB() = println("ConcreteProductB1 operation")
}

// 具体产品B2
class ConcreteProductB2 : AbstractProductB {
    override fun operationB() = println("ConcreteProductB2 operation")
}

// 抽象工厂
interface AbstractFactory {
    fun createProductA(): AbstractProductA

    fun createProductB(): AbstractProductB
}

// 具体工厂1
class ConcreteFactory1 : AbstractFactory {
    override fun createProductA() = ConcreteProductA1()

    override fun createProductB() = ConcreteProductB1()
}

// 具体工厂2
class ConcreteFactory2 : AbstractFactory {
    override fun createProductA() = ConcreteProductA2()

    override fun createProductB() = ConcreteProductB2()
}

// 使用示例
fun main() {
    val factory1 = ConcreteFactory1()
    val productA1 = factory1.createProductA()
    val productB1 = factory1.createProductB()
    productA1.operationA()
    productB1.operationB()

    val factory2 = ConcreteFactory2()
    val productA2 = factory2.createProductA()
    val productB2 = factory2.createProductB()
    productA2.operationA()
    productB2.operationB()
}
```

### Java

```java
    // 抽象产品 A
    public interface AbstractProductA {
        void operationA();
    }

    // 具体产品 A1
    public static class ConcreteProductA1 implements AbstractProductA {
        @Override
        public void operationA() {
            System.out.println("ConcreteProductA1 operationA");
        }
    }

    // 具体产品 A2
    public static class ConcreteProductA2 implements AbstractProductA {
        @Override
        public void operationA() {
            System.out.println("ConcreteProductA2 operationA");
        }
    }

    // 抽象产品 B
    public interface AbstractProductB {
        void operationB();
    }

    // 具体产品 B1
    public static class ConcreteProductB1 implements AbstractProductB {
        @Override
        public void operationB() {
            System.out.println("ConcreteProductB1 operationB");
        }
    }

    // 具体产品 B2
    public static class ConcreteProductB2 implements AbstractProductB {
        @Override
        public void operationB() {
            System.out.println("ConcreteProductB2 operationB");
        }
    }

    // 抽象工厂
    public interface AbstractFactory {
        AbstractProductA createProductA();
        AbstractProductB createProductB();
    }

    // 具体工厂 1
    public static class ConcreteFactory1 implements AbstractFactory {
        @Override
        public AbstractProductA createProductA() {
            return new ConcreteProductA1();
        }

        @Override
        public AbstractProductB createProductB() {
            return new ConcreteProductB1();
        }
    }

    // 具体工厂 2
    public static class ConcreteFactory2 implements AbstractFactory {
        @Override
        public AbstractProductA createProductA() {
            return new ConcreteProductA2();
        }

        @Override
        public AbstractProductB createProductB() {
            return new ConcreteProductB2();
        }
    }

    // 使用示例
    public static void main(String[] args) {
        AbstractFactory factory1 = new ConcreteFactory1();
        AbstractProductA productA1 = factory1.createProductA();
        AbstractProductB productB1 = factory1.createProductB();

        productA1.operationA();
        productB1.operationB();

        AbstractFactory factory2 = new ConcreteFactory2();
        AbstractProductA productA2 = factory2.createProductA();
        AbstractProductB productB2 = factory2.createProductB();

        productA2.operationA();
        productB2.operationB();
    }
```
