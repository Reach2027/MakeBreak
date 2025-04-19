# 模板方法（Template Method）模式 - 行为模式（Behavioral）

模板方法模式定义了一个算法的骨架，将一些步骤延迟到子类中实现。模板方法模式使得子类可以在不改变算法结构的情况下重新定义算法的某些步骤。

在模板方法中，有以下几种角色：

***模板*** ： ：定义了算法的骨架，其中包含一个或多个抽象方法，用于延迟到子类中实现的步骤，以及具体方法，用于定义算法中的固定步骤。

***模板实现类*** ： 实现抽象方法，完成具体步骤的实现。

## 代码示例

### Java

```java
    // 抽象模板
    public static abstract class AbstractClass {
        public final void templateMethod() {
            // 固定步骤1
            step1();
            // 可选步骤2
            if (hookMethod()) {
                step2();
            }
            // 固定步骤3
            step3();
        }

        protected abstract void step1();

        protected abstract void step2();

        protected abstract void step3();

        protected boolean hookMethod() {
            return true;
        }
    }

    public static class ConcreteClassA extends AbstractClass {
        @Override
        protected void step1() {
            System.out.println("ConcreteClassA - Step 1");
        }

        @Override
        protected void step2() {
            System.out.println("ConcreteClassA - Step 2");
        }

        @Override
        protected void step3() {
            System.out.println("ConcreteClassA - Step 3");
        }
    }

    public static class ConcreteClassB extends AbstractClass {
        @Override
        protected void step1() {
            System.out.println("ConcreteClassB - Step 1");
        }

        @Override
        protected void step2() {
            System.out.println("ConcreteClassB - Step 2");
        }

        @Override
        protected void step3() {
            System.out.println("ConcreteClassB - Step 3");
        }

        @Override
        protected boolean hookMethod() {
            return false;
        }
    }

    public static void main(String[] args) {
        AbstractClass classA = new ConcreteClassA();
        classA.templateMethod();

        AbstractClass classB = new ConcreteClassB();
        classB.templateMethod();
    }
```
