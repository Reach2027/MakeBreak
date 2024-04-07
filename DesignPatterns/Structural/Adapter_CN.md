# 适配器（Adapter）模式 - 结构模式（Structural）

适配器模式允许将一个类的接口转换成客户端所期望的另一个接口。适配器模式解决了不兼容接口之间的问题，通过包装一个已有的类，将其接口转换成客户端所期望的接口。

在适配器模式中，有以下几种角色：

***目标接口*** ：期望的接口，适配器将会实现这个接口。

***被适配者*** ：已有的类或接口，需要被适配以满足目标接口的要求。

***适配器*** ：实现目标接口，并包装被适配者，将客户端的请求转发给被适配者进行处理。

## 代码示例

### Java

```java
public interface Target {
        void request();
    }

    public static class Adaptee {
        public void specificRequest() {
            // 已有的功能实现
        }
    }

    public static class Adapter implements Target {
        private final Adaptee adaptee;

        public Adapter(Adaptee adaptee) {
            this.adaptee = adaptee;
        }

        @Override
        public void request() {
            // 在适配器中调用被适配者的方法
            adaptee.specificRequest();
        }
    }

    public static void main(String[] args) {
        Adaptee adaptee = new Adaptee();
        Target adapter = new Adapter(adaptee);

        adapter.request();
    }
```
