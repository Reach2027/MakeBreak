# 外观（Facade）模式 - 结构模式（Structural）

外观模式供了一个统一的接口，用于访问子系统中的一组接口。外观模式隐藏了子系统的复杂性，为客户端提供了一个简化的接口，使得客户端与子系统之间的交互更加方便。

在外观模式中，有以下几种角色：

***外观*** ：提供了一个统一的接口，封装了子系统中的一组接口，对客户端隐藏了子系统的复杂性。

***子系统*** ：由一组类或对象组成的子系统，实现了具体的功能。

## 代码示例

### Java

```java
    // 外观
    public static class Facade {
        private SubsystemA subsystemA;
        private SubsystemB subsystemB;
        private SubsystemC subsystemC;

        public Facade() {
            subsystemA = new SubsystemA();
            subsystemB = new SubsystemB();
            subsystemC = new SubsystemC();
        }

        public void operation() {
            subsystemA.operationA();
            subsystemB.operationB();
            subsystemC.operationC();
        }
    }

    // 子系统
    public static class SubsystemA {
        public void operationA() {
            // 实现子系统A的功能
        }
    }

    public static class SubsystemB {
        public void operationB() {
            // 实现子系统B的功能
        }
    }

    public static class SubsystemC {
        public void operationC() {
            // 实现子系统C的功能
        }
    }

    public static void main(String[] args) {
        Facade facade = new Facade();
        facade.operation();
    }
```
