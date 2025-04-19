# 桥接（Bridge）模式 - 结构模式（Structural）

桥接模式将抽象部分和实现部分分离，使它们可以独立地变化。桥接模式通过组合的方式，而不是继承的方式，实现了抽象和实现的解耦。

在桥接模式中，有以下几种角色：

***抽象部分*** ：定义抽象部分的接口，并包含对实现部分的引用。

***具体抽象部分*** ：扩展抽象部分的接口，实现更多的功能。

***实现部分*** ：定义实现部分的接口。

***具体实现部分*** ：实现实现部分的接口，并提供具体的实现。

## 代码示例

### Java

```java
    // 实现部分
    public interface Implementor {
        void operationImpl();
    }

    public static class ConcreteImplementorA implements Implementor {
        @Override
        public void operationImpl() {
            // 具体实现 A
            System.out.println("operator ConcreteImplementorA");
        }
    }

    public static class ConcreteImplementorB implements Implementor {
        @Override
        public void operationImpl() {
            // 具体实现 B
            System.out.println("operator ConcreteImplementorB");
        }
    }

    // 抽象部分
    public static abstract class Abstraction {
        protected Implementor implementor;

        public Abstraction(Implementor implementor) {
            this.implementor = implementor;
        }

        public abstract void operation();
    }

    // 具体抽象部分
    public static class RefinedAbstraction extends Abstraction {
        public RefinedAbstraction(Implementor implementor) {
            super(implementor);
        }

        @Override
        public void operation() {
            // 执行更多功能
            // ...
            implementor.operationImpl();
            // ...
        }
    }

    public static void main(String[] args) {
        Implementor implementorA = new ConcreteImplementorA();
        Abstraction abstractionA = new RefinedAbstraction(implementorA);
        abstractionA.operation();

        Implementor implementorB = new ConcreteImplementorB();
        Abstraction abstractionB = new RefinedAbstraction(implementorB);
        abstractionB.operation();
    }
```
