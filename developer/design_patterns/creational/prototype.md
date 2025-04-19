# 原型（Prototype）模式 - 创建型模式（Creational）

原型模式通过复制现有对象来生成新对象，而无需通过实例化创建。原型模式的核心概念是原型和克隆。原型是一个已经存在的对象，我们通过克隆这个原型对象来创建新的对象。原型模式要求被复制的对象实现一个克隆方法，通过这个方法来返回克隆后的新对象。

在原型模式中，有以下几种角色：

***原型接口（Prototype）*** ：定义了克隆方法的接口，所有的具体原型都必须实现这个接口。

***具体原型（Concrete Prototype）*** ：实现原型接口，实现克隆方法，通过复制自身来创建新的对象。

## 代码示例

### Kotlin

```kt
// 原型接口
interface Prototype<T> {
    fun clone(): T
}

// 具体原型
class ConcretePrototype : Prototype<ConcretePrototype> {

    var x: Int = 0

    lateinit var ys: IntArray

    override fun equals(other: Any?): Boolean {
        if (this === other) return true
        if (javaClass != other?.javaClass) return false

        other as ConcretePrototype

        if (x != other.x) return false
        return ys.contentEquals(other.ys)
    }

    override fun hashCode(): Int {
        var result = x
        result = 31 * result + ys.contentHashCode()
        return result
    }

    override fun toString(): String {
        return "ConcretePrototype(x=$x, ys=${ys.contentToString()})"
    }

    override fun clone(): ConcretePrototype {
        val prototype = ConcretePrototype()
        prototype.x = this.x
        prototype.ys = this.ys.copyOf()
        return prototype
    }

}

fun main() {
    val prototype = ConcretePrototype()
    prototype.x = 100
    prototype.ys = intArrayOf(100)
    println(prototype.toString())

    val clone = prototype.clone()
    println(clone.toString())
    println(prototype.ys === clone.ys)
}
```

### Java

```java
    // 原型接口
    public interface Prototype<T> {
        T clone();
    }

    // 具体原型
    public static class ConcretePrototype implements Prototype<ConcretePrototype> {

        public int x;
        public int[] ys;

        public ConcretePrototype() {
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (o == null || getClass() != o.getClass()) return false;

            ConcretePrototype prototype = (ConcretePrototype) o;

            if (x != prototype.x) return false;
            return Arrays.equals(ys, prototype.ys);
        }

        @Override
        public int hashCode() {
            int result = x;
            result = 31 * result + Arrays.hashCode(ys);
            return result;
        }

        @Override
        public String toString() {
            return "ConcretePrototype{" +
                    "x=" + x +
                    ", ys=" + Arrays.toString(ys) +
                    '}';
        }

        @Override
        public ConcretePrototype clone() {
            ConcretePrototype prototype = new ConcretePrototype();
            prototype.x = this.x;
            int len = this.ys.length;
            prototype.ys = Arrays.copyOf(this.ys, len);
            return prototype;
        }
    }

    public static void main(String[] args) {
        ConcretePrototype prototype = new ConcretePrototype();
        prototype.x = 100;
        prototype.ys = new int[]{100};
        System.out.println(prototype.toString());
        
        ConcretePrototype clone = prototype.clone();
        System.out.println(clone.toString());
        System.out.println(prototype.ys == clone.ys);
    }
```
