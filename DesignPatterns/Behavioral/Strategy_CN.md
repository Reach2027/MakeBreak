# 策略（Strategy）模式 - 行为模式（Behavioral）

策略模式定义了一系列算法，并将每个算法封装成独立的策略类。在使用策略模式时，可以根据需要选择不同的策略类来进行算法的动态切换。

在策略模式中，有以下几种角色：

***环境*** ： 定义客户端所感兴趣的接口，维护一个对策略对象的引用，将具体的算法委托给策略对象进行执行。

***抽象策略*** ： 定义策略类公共接口

***具体策略*** ： 实现了抽象策略，封装具体的算法。

## 代码示例

### Java

```java
    // 抽象策略
    interface Strategy {
        void execute();
    }

    public static class ConcreteStrategyA implements Strategy {
        @Override
        public void execute() {
            System.out.println("Executing strategy A...");
        }
    }

    public static class ConcreteStrategyB implements Strategy {
        @Override
        public void execute() {
            System.out.println("Executing strategy B...");
        }
    }

    public static class Context {
        private Strategy strategy;

        public void setStrategy(Strategy strategy) {
            this.strategy = strategy;
        }

        public void executeStrategy() {
            strategy.execute();
        }
    }

    public static void main(String[] args) {
        Context context = new Context();

        Strategy strategyA = new ConcreteStrategyA();
        Strategy strategyB = new ConcreteStrategyB();

        context.setStrategy(strategyA);
        context.executeStrategy();

        context.setStrategy(strategyB);
        context.executeStrategy();
    }

```
