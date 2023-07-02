# 命令（Command）模式 - 行为模式（Behavioral）

命令模式将请求封装成一个对象，从而可以使用不同的请求对接收者进行参数化。命令模式还可以支持请求的排队和日志记录，以及支持撤销操作。

在命令模式中，有以下几种角色：

***抽象命令*** ： 定义执行操作的方法。

***具体命令*** ： 实现抽象命令，并封装了执行操作的对象。

***接收者*** ： 实际执行操作。

***调用者*** ： 调用命令对象执行操作

## 代码示例

### Java

```java
    // 抽象命令
    public interface Command {
        void execute();
    }

    // 具体命令
    public static class ConcreteCommand implements Command {
        private final Receiver receiver;

        public ConcreteCommand(Receiver receiver) {
            this.receiver = receiver;
        }

        public void execute() {
            receiver.action();
        }
    }

    // 接收者
    public static class Receiver {
        public void action() {
            System.out.println("Do Action");
        }
    }

    // 调用者
    public static class Invoker {
        private Command command;

        public void setCommand(Command command) {
            this.command = command;
        }

        public void executeCommand() {
            command.execute();
        }
    }

    public static void main(String[] args) {
        Receiver receiver = new Receiver();
        Command command = new ConcreteCommand(receiver);

        Invoker invoker = new Invoker();
        invoker.setCommand(command);
        invoker.executeCommand();
    }
```
