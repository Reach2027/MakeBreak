# 责任链（Chain of responsibility）模式 - 行为模式（Behavioral）

责任链模式允许将请求沿着处理链进行传递，直到有一个处理者能够处理该请求为止。责任链模式解耦了发送者和接收者之间的关系，并允许多个对象都有机会处理请求。

在责任链模式中，有以下两种角色：

***抽象处理者（Handler）*** ：定义了处理请求的接口，并持有下一个处理者的引用。通常是一个抽象类或接口。

***具体处理者（Concrete Handler）*** ：实现了抽象处理者的接口，并负责处理请求。如果它能够处理请求，则处理；否则将请求传递给下一个处理者。

责任链模式的工作原理如下：

1. 当一个请求到达时，首先由第一个处理者处理。

2. 如果该处理者能够处理该请求，则处理请求并结束。

3. 如果该处理者无法处理该请求，则将请求传递给下一个处理者。

4. 重复步骤2和步骤3，直到有一个处理者能够处理请求为止。

## 代码示例

### Kotlin

```kt
// 抽象处理者
abstract class Handler {
    var nextHandler: Handler? = null

    abstract fun handleRequest(request: Int)
}

// 具体处理者A
class ConcreteHandlerA : Handler() {
    override fun handleRequest(request: Int) {
        if (request <= 10) {
            println("ConcreteHandlerA 处理请求: $request")
        } else nextHandler?.handleRequest(request)
    }
}

// 具体处理者B
class ConcreteHandlerB : Handler() {
    override fun handleRequest(request: Int) {
        if (request in 11..20) {
            println("ConcreteHandlerB 处理请求: $request")
        } else nextHandler?.handleRequest(request)
    }
}

// 具体处理者C
class ConcreteHandlerC : Handler(){
    override fun handleRequest(request: Int) {
        if (request > 20) {
            println("ConcreteHandlerC 处理请求: $request")
        } else nextHandler?.handleRequest(request)
    }
}

// 客户端代码
fun main() {
    val handlerA = ConcreteHandlerA()
    val handlerB = ConcreteHandlerB()
    val handlerC = ConcreteHandlerC()

    handlerA.nextHandler = handlerB
    handlerB.nextHandler = handlerC

    handlerA.handleRequest(5)
    handlerA.handleRequest(15)
    handlerA.handleRequest(25)
}
```

### Java

```java
// 抽象处理者
abstract class Handler {
    protected Handler nextHandler;

    public void setNextHandler(Handler nextHandler) {
        this.nextHandler = nextHandler;
    }

    public abstract void handleRequest(int request);
}

// 具体处理者A
class ConcreteHandlerA extends Handler {
    @Override
    public void handleRequest(int request) {
        if (request <= 10) {
            System.out.println("ConcreteHandlerA 处理请求: " + request);
        } else if (nextHandler != null) {
            nextHandler.handleRequest(request);
        }
    }
}

// 具体处理者B
class ConcreteHandlerB extends Handler {
    @Override
    public void handleRequest(int request) {
        if (request > 10 && request <= 20) {
            System.out.println("ConcreteHandlerB 处理请求: " + request);
        } else if (nextHandler != null) {
            nextHandler.handleRequest(request);
        }
    }
}

// 具体处理者C
class ConcreteHandlerC extends Handler {
    @Override
    public void handleRequest(int request) {
        if (request > 20) {
            System.out.println("ConcreteHandlerC 处理请求: " + request);
        } else if (nextHandler != null) {
            nextHandler.handleRequest(request);
        }
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        Handler handlerA = new ConcreteHandlerA();
        Handler handlerB = new ConcreteHandlerB();
        Handler handlerC = new ConcreteHandlerC();

        handlerA.setNextHandler(handlerB);
        handlerB.setNextHandler(handlerC);

        handlerA.handleRequest(5);
        handlerA.handleRequest(15);
        handlerA.handleRequest(25);
    }
}
```
