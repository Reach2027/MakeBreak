# 模块化

## 参考链接

<https://developer.android.google.cn/topic/modularization>

## 为什么需要模块化

在规模不断增长的代码库中，如果不采取积极的措施去防劣化，整体的代码质量会不断的下降，越来越难以维护。

模块化可有效的将代码库结构化，是一个防止代码库劣化的手段，好的模块化可有效提升整体的代码质量。

### 模块化的收益

* 提升代码的可维护性
* 提升代码的可复用性
* 提升代码质量，代码可测试性（模块化后模块间耦合度低）、可扩展性（模块化后模块间耦合度低）等
* 提升构建性能（好的模块化可充分利用Gradle的增量构建、构建缓存、并行构建等能力）
* 更方便的控制代码的可见性
* 更方便的进行代码封装
* 更方便的项目管理（一个模块指定一个负责人）

## 模块设计原则

### 模块内高内聚，模块间低耦合

### 对外尽可能少暴露，对外尽量暴露接口，使用依赖注入暴露接口，对外不暴露接口实现类

### 尽量是与 Android 无关的 Java 或 Kotlin 模块

## 常见陷阱

* 模块化粒度过细
* 模块化粒度过大
* 模块之间引用过于复杂

当设计模块化代码库时，需要考虑模块粒度及相对复杂性，粒度过细会让模块化成为一个负担，粒度过大会降低模块化的收益，模块之间引用过于复杂同样也会降低模块化的收益。

## 模块化方案

### 特别简单的项目，无需模块化仅一个 app 模块即可

### 较多功能项目

按照功能来划分模块

<img src="/CLient/Modularization/assets/modularization-feature.drawio.png" alt="Modularization" width="777">

### 数据层存在复用的多功能项目

关注点分离，分层架构，数据层复用

<img src="/CLient/Modularization/assets/modularization-feature-data.drawio.png" alt="Modularization" width="777">

### 复杂多功能项目

业务通用逻辑下沉

<img src="/CLient/Modularization/assets/modularization-feature-core.drawio.png" alt="Modularization" width="777">

### 跨项目模块化

业务无关通用逻辑再下沉

<img src="/CLient/Modularization/assets/modularization-feature-base.drawio.png" alt="Modularization" width="777">
