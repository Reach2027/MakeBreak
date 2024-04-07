# 状态（State）模式 - 行为模式（Behavioral）

状态模式允许对象在内部状态改变时改变其行为，看起来就像是对象的类发生了改变。状态模式将对象的行为包装在不同的状态类中，使得状态的变化可以直接影响对象的行为。

在状态模式中，有以下几种角色：

***环境*** ： 定义客户端所感兴趣的接口，维护一个当前状态对象，并将客户端请求委托给当前状态对象处理。

***抽象状态*** ： 用于封装与环境的特定状态相关的行为。

***具体状态*** ： 实现抽象状态的具体子类，每个具体状态类对应环境的一个具体状态，实现与该状态相关的行为。

## 代码示例

### Java

```java
    // 抽象状态
    interface State {
        void handle();
    }

    public static class ConcreteStateA implements State {
        @Override
        public void handle() {
            System.out.println("Handling state A...");
        }
    }

    public static class ConcreteStateB implements State {
        @Override
        public void handle() {
            System.out.println("Handling state B...");
        }
    }

    public static class Context {
        private State currentState;

        public void setCurrentState(State state) {
            this.currentState = state;
        }

        public void request() {
            currentState.handle();
        }
    }

    public static void main(String[] args) {
        Context context = new Context();

        State stateA = new ConcreteStateA();
        State stateB = new ConcreteStateB();

        context.setCurrentState(stateA);
        context.request();

        context.setCurrentState(stateB);
        context.request();
    }
```
