# 中介者（Mediator）模式 - 行为模式（Behavioral）

中介者模式通过引入一个中介者对象来集中处理多个对象之间的交互，从而减少对象之间的直接耦合。

在中介者模式中，有以下几种角色：

***抽象中介者*** ： 定义了对各个相关对象之间进行通信和协调的接口。

***具体中介者***

***抽象相关对象*** ： 定义相关对象之间的通用行为。

***具体相关对象***

## 代码示例

### Java

```java
    public interface Mediator {
        void sendMessage(Colleague colleague, String message);
    }

    public static class ConcreteMediator implements Mediator {
        private Colleague colleague1;
        private Colleague colleague2;

        public void setColleague1(Colleague colleague) {
            this.colleague1 = colleague;
        }

        public void setColleague2(Colleague colleague) {
            this.colleague2 = colleague;
        }

        @Override
        public void sendMessage(Colleague colleague, String message) {
            if (colleague == colleague1) {
                colleague2.receiveMessage(colleague, message);
            } else if (colleague == colleague2) {
                colleague1.receiveMessage(colleague, message);
            }
        }
    }

    public static abstract class Colleague {

        public final String name;

        protected Mediator mediator;

        public Colleague(String name) {
            this.name = name;
        }

        public void setMediator(Mediator mediator) {
            this.mediator = mediator;
        }

        public abstract void send(String message);

        public abstract void receiveMessage(Colleague colleague, String message);
    }

    public static class ConcreteColleague extends Colleague {
        public ConcreteColleague(String name) {
            super(name);
        }

        @Override
        public void send(String message) {
            mediator.sendMessage(this, message);
        }

        @Override
        public void receiveMessage(Colleague colleague, String message) {
            System.out.println("Received message from " + colleague.name + ", message: " + message);
        }
    }

    public static void main(String[] args) {
        ConcreteMediator mediator = new ConcreteMediator();

        ConcreteColleague colleague1 = new ConcreteColleague("colleague1");
        ConcreteColleague colleague2 = new ConcreteColleague("colleague2");
        mediator.setColleague1(colleague1);
        mediator.setColleague2(colleague2);
        colleague1.setMediator(mediator);
        colleague2.setMediator(mediator);

        colleague1.send("Hello");
        colleague2.send("Hi");
    }
```
