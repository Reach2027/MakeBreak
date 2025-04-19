# 访问者（Visitor）模式 - 行为模式（Behavioral）

访问者模式允许在不改变被访问对象的类的前提下，定义新的操作（访问者）并将其应用于对象的元素上。通过使用访问者模式，可以在不修改对象结构的情况下，对对象结构中的元素进行新的操作。

在访问者模式中，有以下几种角色：

***抽象访问者*** ： ：定义了对每个元素的访问方法，通过这些访问方法可以访问具体元素，并在具体元素上执行相应的操作。

***具体访问者***

***抽象元素*** ： 定义了接受访问者对象的接口，通常包含一个接受访问者的方法。

***具体元素***

***对象结构*** ： 包含一组具体元素的集合，提供了遍历元素的方法，可以接受访问者对象进行访问。

## 代码示例

### Java

```java
    interface Visitor {
        void visitConcreteElementA(ConcreteElementA element);
        void visitConcreteElementB(ConcreteElementB element);
    }

    public static class ConcreteVisitor implements Visitor {
        @Override
        public void visitConcreteElementA(ConcreteElementA element) {
            System.out.println("Visit ConcreteElementA");
        }

        @Override
        public void visitConcreteElementB(ConcreteElementB element) {
            System.out.println("Visit ConcreteElementB");
        }
    }

    interface Element {
        void accept(Visitor visitor);
    }

    public static class ConcreteElementA implements Element {
        @Override
        public void accept(Visitor visitor) {
            visitor.visitConcreteElementA(this);
        }
    }

    public static class ConcreteElementB implements Element {
        @Override
        public void accept(Visitor visitor) {
            visitor.visitConcreteElementB(this);
        }
    }

    public static class ObjectStructure {
        private final List<Element> elements = new ArrayList<>();

        public void addElement(Element element) {
            elements.add(element);
        }

        public void removeElement(Element element) {
            elements.remove(element);
        }

        public void accept(Visitor visitor) {
            for (Element element : elements) {
                element.accept(visitor);
            }
        }
    }

    public static void main(String[] args) {
        ObjectStructure objectStructure = new ObjectStructure();
        objectStructure.addElement(new ConcreteElementA());
        objectStructure.addElement(new ConcreteElementB());

        Visitor visitor = new ConcreteVisitor();
        objectStructure.accept(visitor);
    }
```
