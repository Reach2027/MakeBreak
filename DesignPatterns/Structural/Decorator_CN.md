# 装饰器（Decorator）模式 - 结构模式（Structural）

装饰器模式允许向现有对象添加新的功能，同时又不改变其结构。装饰器模式通过包装（装饰）原始对象，以透明的方式动态地扩展其功能。

在装饰器模式中，有以下几种角色：

***抽象组件*** ：定义了被装饰对象和装饰器对象的共同接口。

***具体组件*** ：实现了抽象组件的接口，是被装饰的原始对象。

***抽象装饰器*** ：继承自抽象组件，持有一个抽象组件的引用，并定义了与抽象组件一致的接口。

***具体装饰器*** ：继承自抽象装饰器，扩展了抽象装饰器的功能，并在其中包装具体组件。

## 代码示例

### Java

```java
    // 抽象组件
    public interface Component {
        void operation();
    }

    // 具体组件
    public static class ConcreteComponent implements Component {
        @Override
        public void operation() {
            // 原始对象的操作
        }
    }

    // 抽象装饰器
    public static abstract class Decorator implements Component {
        protected Component component;

        public Decorator(Component component) {
            this.component = component;
        }

        @Override
        public void operation() {
            component.operation();
        }
    }

    // 具体装饰器
    public static class ConcreteDecoratorA extends Decorator {
        public ConcreteDecoratorA(Component component) {
            super(component);
        }

        @Override
        public void operation() {
            // 执行装饰器A的功能
            // ...
            super.operation();
            // ...
        }
    }

    public static class ConcreteDecoratorB extends Decorator {
        public ConcreteDecoratorB(Component component) {
            super(component);
        }

        @Override
        public void operation() {
            // 执行装饰器B的功能
            // ...
            super.operation();
            // ...
        }
    }

    public static void main(String[] args) {
        Component component = new ConcreteComponent();

        Component decoratorA = new ConcreteDecoratorA(component);
        decoratorA.operation();

        Component decoratorB = new ConcreteDecoratorB(component);
        decoratorB.operation();

        Component decoratorAB = new ConcreteDecoratorA(new ConcreteDecoratorB(component));
        decoratorAB.operation();
    }
```
