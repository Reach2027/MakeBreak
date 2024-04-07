# 建造者（Builder）模式 - 创建型模式（Creational）

建造者模式允许按照步骤构建复杂对象，同时隐藏了构建过程的细节。将复杂对象的构建逻辑与对象实例分离，使得同样的构建过程可以创建不同的实例。

在建造者模式中，通常有以下几种角色：

***产品（Product）*** ：复杂对象。

***建造者（Builder）*** ：负责复杂对象的构建。

## 代码示例

### Kotlin

Kotlin因默认参数的存在，没有必要使用建造者模式。

### Java

```java
    // 产品
    static class Computer {
        private String cpu;

        private String gpu;

        private String memory;

        private String hardDisk;

        public Computer() {
        }

        public String getCpu() {
            return cpu;
        }

        public String getGpu() {
            return gpu;
        }

        public String getMemory() {
            return memory;
        }

        public String getHardDisk() {
            return hardDisk;
        }

        @Override
        public String toString() {
            return "Computer{" +
                    "cpu='" + cpu + '\'' +
                    ", gpu='" + gpu + '\'' +
                    ", memory='" + memory + '\'' +
                    ", hardDisk='" + hardDisk + '\'' +
                    '}';
        }

        // 建造者
        static class Builder {
            private final Computer computer;

            public Builder() {
                computer = new Computer();
                computer.cpu = "AMD Ryzen 9";
                computer.gpu = "NVIDIA GeForce RTX 4090";
                computer.memory = "64GB DDR5";
                computer.hardDisk = "2TB SSD";
            }

            public Builder setCpu(String cpu) {
                computer.cpu = cpu;
                return this;
            }

            public Builder setGpu(String gpu) {
                computer.gpu = gpu;
                return this;
            }

            public Builder setMemory(String memory) {
                computer.memory = memory;
                return this;
            }

            public Builder setHardDisk(String hardDisk) {
                computer.hardDisk = hardDisk;
                return this;
            }

            public Computer build() {
                return computer;
            }

        }
    }

    public static void main(String[] args) {
        Computer computer1 = new Computer.Builder().build();
        System.out.println(computer1.toString());
        Computer computer2 = new Computer.Builder()
                .setCpu("Intel Core i9")
                .setMemory("16GB DDR5")
                .build();
        System.out.println(computer2.toString());
    }
```
