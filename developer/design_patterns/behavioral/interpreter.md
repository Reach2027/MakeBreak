# 解释器（Interpreter）模式 - 行为模式（Behavioral）

解释器模式给定一种语言，为其语法定义一个表示法，同时定义一个解释器，使用该表示法来解释语言中的句子。

在解释器模式中，有以下几种角色：

***抽象表达式*** ： 定义解释器操作的方法。

***终结符表达式*** ： 实现抽象表达式，表示终结符（即不能再分解的最小单元）。

***非终结符表达式*** ：  实现抽象表达式，表示非终结符（即可以继续分解的复合元素）。

***上下文*** ： 包含解释器需要的全局信息或状态，供解释器进行解释时使用。

## 代码示例

### Java

```java
    // 抽象表达式
    interface Expression {
        int interpret(Context context);
    }

    // 终结符表达式
    public static class NumberExpression implements Expression {
        private final int number;

        public NumberExpression(int number) {
            this.number = number;
        }

        public int interpret(Context context) {
            return number;
        }
    }

    // 非终结符表达式
    public static class AddExpression implements Expression {
        private final Expression left;
        private final Expression right;

        public AddExpression(Expression left, Expression right) {
            this.left = left;
            this.right = right;
        }

        public int interpret(Context context) {
            return left.interpret(context) + right.interpret(context);
        }
    }

    // 上下文
    public static class Context {
        // 可以在上下文中保存一些全局信息或状态
    }

    public static void main(String[] args) {
        // 构建语法树：5 + 3
        Expression expression = new AddExpression(
                new NumberExpression(5),
                new NumberExpression(3)
        );

        // 创建上下文
        Context context = new Context();

        // 解释并计算表达式的结果
        int result = expression.interpret(context);

        System.out.println("结果：" + result); // 输出结果：8
    }
```
