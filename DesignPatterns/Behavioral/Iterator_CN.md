# 迭代器（Iterator）模式 - 行为模式（Behavioral）

迭代器模式提供了一种在不暴露集合内部表示的情况下，顺序访问集合中元素的方法。

迭代器模式的核心思想是将遍历集合的责任从集合对象中分离出来，交给一个独立的迭代器对象来完成。这样可以使得集合对象可以独立地变化，而不会影响到遍历算法。

在迭代器模式中，有以下几种角色：

***迭代器（Iterator）*** ： 定义遍历集合的接口，包括获取下一个元素、是否还有下一个元素等方法。

***具体迭代器*** ： 实现迭代器接口。

***集合*** ： 定义集合接口，包括获取迭代器的方法。

***具体集合*** ： 实现集合接口。

## 代码示例

### Java

```java
    // 迭代器接口
    interface MyIterator<E> {
        boolean hasNext();
        E next();
    }

    // 具体迭代器
    public static class ListMyIterator<E> implements MyIterator<E> {
        private final MyList<E> list;
        private int index;

        public ListMyIterator(MyList<E> list) {
            this.list = list;
            this.index = 0;
        }

        public boolean hasNext() {
            return index < list.size();
        }

        public E next() {
            if (hasNext()) {
                E item = list.get(index);
                index++;
                return item;
            }
            return null;
        }
    }

    // 集合接口
    interface MyList<E> {
        MyIterator<E> createIterator();

        int size();

        E get(int index);
    }

    // 具体集合
    public static class MyMyList<E> implements MyList<E> {
        private final E[] items;

        public MyMyList(E[] items) {
            this.items = items;
        }

        public MyIterator<E> createIterator() {
            return new ListMyIterator<>(this);
        }

        public int size() {
            return items.length;
        }

        public E get(int index) {
            return items[index];
        }
    }

    public static void main(String[] args) {
        Integer[] numbers = {1, 2, 3, 4, 5};
        MyList<Integer> myList = new MyMyList<>(numbers);

        MyIterator<Integer> iterator = myList.createIterator();
        while (iterator.hasNext()) {
            int number = iterator.next();
            System.out.println(number);
        }
    }
```
