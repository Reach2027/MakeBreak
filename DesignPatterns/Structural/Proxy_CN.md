# 代理（Proxy）模式 - 结构模式（Structural）

代理模式通过代理对象控制对其他对象的访问。代理模式在客户端和目标对象之间引入了一个代理对象，以间接地访问目标对象，从而可以在访问过程中添加额外的逻辑。

在代理模式中，有以下几种角色：

***抽象主题*** ：定义了目标对象和代理对象共同的接口，这样在任何使用目标对象的地方都可以使用代理对象。

***具体主题*** ：实现了抽象主题的接口，是真正的目标对象。

***代理*** ：持有一个对具体主题的引用，通过实现抽象主题的接口，可以在访问具体主题之前或之后添加额外的逻辑。

## 代码示例

### Java

```java
    // 抽象目标
    public interface Subject {
        void request();
    }

    // 具体目标
    public static class RealSubject implements Subject {
        @Override
        public void request() {
            System.out.println("Finished");
        }
    }

    // 代理
    public static class Proxy implements Subject {
        private final RealSubject realSubject;

        public Proxy() {
            realSubject = new RealSubject();
        }

        @Override
        public void request() {
            // 在访问具体主题之前可以添加额外的逻辑
            // ...

            realSubject.request();

            // 在访问具体主题之后可以添加额外的逻辑
            // ...
        }
    }

    public static void main(String[] args) {
        Proxy proxy = new Proxy();
        proxy.request();
    }
```
