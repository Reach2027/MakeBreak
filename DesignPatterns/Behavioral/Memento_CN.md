# 备忘录（Memento）模式 - 行为模式（Behavioral）

备忘录模式允许在不破坏封装性的前提下捕获和恢复一个对象的内部状态。

备忘录模式通过引入备忘录对象，用于保存和恢复对象的状态，实现了对对象状态的保存与恢复操作，同时使得对象本身不需要关心状态的保存和恢复过程。

在备忘录模式中，有以下几种角色：

***源发器（Originator）*** ：需要保存和恢复状态的对象，可以创建备忘录对象，并通过备忘录对象保存和恢复自身状态。

***备忘录（Memento）*** ：用于存储源发器对象的内部状态，并提供给源发器对象恢复状态的方法。

***管理者（Caretaker）*** ：负责保存备忘录对象，但不对备忘录对象的内容进行操作或检查。

## 代码示例

### Java

```java
    // 备忘录
    public static class Memento {
        private final String state;

        public Memento(String state) {
            this.state = state;
        }

        public String getState() {
            return state;
        }
    }

    // 源发器
    public static class Originator {
        private String state;

        public void setState(String state) {
            this.state = state;
        }

        public String getState() {
            return state;
        }

        public Memento createMemento() {
            return new Memento(state);
        }

        public void restoreMemento(Memento memento) {
            this.state = memento.getState();
        }
    }

    // 管理者
    public static class Caretaker {
        private Memento memento;

        public void setMemento(Memento memento) {
            this.memento = memento;
        }

        public Memento getMemento() {
            return memento;
        }
    }

    public static void main(String[] args) {
        Originator originator = new Originator();
        Caretaker caretaker = new Caretaker();

        // 设置初始状态
        originator.setState("State 1");
        // 创建备忘录并保存状态
        caretaker.setMemento(originator.createMemento());
        // 修改状态
        originator.setState("State 2");
        // 恢复到备忘录保存的状态
        originator.restoreMemento(caretaker.getMemento());

        System.out.println("Current state: " + originator.getState());
    }
```
