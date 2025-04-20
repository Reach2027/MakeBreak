# 享元（Flyweight）模式 - 结构模式（Structural）

享元模式通过共享对象来有效地支持大量细粒度的对象。享元模式通过将对象的状态分为内部状态（Intrinsic State）和外部状态（Extrinsic State），并将内部状态共享，来减少内存使用和提高性能。

在享元模式中，有以下几种角色：

***享元接口*** ：定义了享元对象的接口，包含对内部状态的操作方法。

***具体享元类*** ：实现了享元接口，表示可以共享的具体享元对象。

***享元工厂*** ：负责创建和管理享元对象，提供对享元对象的获取方法。

## 代码示例

### Java

```java
// 享元接口
    public interface Flyweight {
        void operation(String extrinsicState);
    }

    // 享元实现
    public static class ConcreteFlyweight implements Flyweight {
        private final String intrinsicState;

        public ConcreteFlyweight(String intrinsicState) {
            this.intrinsicState = intrinsicState;
        }

        @Override
        public void operation(String extrinsicState) {
            System.out.println(intrinsicState + " " + extrinsicState);
        }
    }

    // 享元工厂
    public static class FlyweightFactory {
        private final Map<String, Flyweight> flyweights = new HashMap<>();

        public Flyweight getFlyweight(String key) {
            if (flyweights.containsKey(key)) {
                return flyweights.get(key);
            } else {
                Flyweight flyweight = new ConcreteFlyweight(key);
                flyweights.put(key, flyweight);
                return flyweight;
            }
        }
    }

    public static void main(String[] args) {
        FlyweightFactory factory = new FlyweightFactory();

        Flyweight flyweight1 = factory.getFlyweight("key1");
        flyweight1.operation("extrinsicState1");

        Flyweight flyweight2 = factory.getFlyweight("key1");
        flyweight2.operation("extrinsicState2");

        Flyweight flyweight3 = factory.getFlyweight("key2");
        flyweight3.operation("extrinsicState3");
    }
```
