# 观察者（Observer）模式 - 行为模式（Behavioral）

观察者模式定义了对象之间的一种一对多依赖关系，当一个对象的状态发生变化时，其所有依赖对象都会得到通知并自动更新。

在观察者模式中，有以下几种角色：

***被观察者*** ： 维护一组观察者对象，并在状态发生变化时通知观察者。

***具体被观察者***

***观察者*** ： 定义了观察者接口，包含了主题状态变化时需要执行的方法。

***具体观察者***

## 代码示例

### Java

```java
    // 被观察者
    public interface Subject {
        void attach(Observer observer);
        void detach(Observer observer);
        void notifyObservers();
    }

    // 具体被观察者
    public static class ConcreteSubject implements Subject {
        private final List<Observer> observers = new ArrayList<>();
        private String state;

        public String getState() {
            return state;
        }

        public void setState(String state) {
            this.state = state;
            notifyObservers();
        }

        @Override
        public void attach(Observer observer) {
            observers.add(observer);
        }

        @Override
        public void detach(Observer observer) {
            observers.remove(observer);
        }

        @Override
        public void notifyObservers() {
            for (Observer observer : observers) {
                observer.update();
            }
        }
    }

    // 观察者
    public interface Observer {
        void update();
    }

    // 具体观察者
    public static class ConcreteObserver implements Observer {
        private final String name;
        private final ConcreteSubject subject;

        public ConcreteObserver(String name, ConcreteSubject subject) {
            this.name = name;
            this.subject = subject;
        }

        @Override
        public void update() {
            System.out.println(name + " received notification. New state: " + subject.getState());
        }
    }

    public static void main(String[] args) {
        ConcreteSubject subject = new ConcreteSubject();

        ConcreteObserver observer1 = new ConcreteObserver("Observer 1", subject);
        ConcreteObserver observer2 = new ConcreteObserver("Observer 2", subject);

        subject.attach(observer1);
        subject.attach(observer2);

        subject.setState("State 1");
        subject.setState("State 2");

        subject.detach(observer1);

        subject.setState("State 3");
    }
```
