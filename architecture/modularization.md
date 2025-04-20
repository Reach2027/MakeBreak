# 模块化

## 模块化的意义

* 提供合适的位置放置不同的代码，通用逻辑下沉至底层模块，提升代码的可复用性，减少项目内的重复代码
* 提升代码的可维护性
* 提升代码质量，代码可测试性（模块化后模块间耦合度低）、可扩展性（模块化后模块间耦合度低）等
* 提升构建性能（如模块化可充分利用Gradle的增量构建、构建缓存、并行构建等能力）
* 更方便的控制代码的可见性
* 更方便的进行代码封装
* 更方便的项目管理（一个模块指定一个负责人）

## 模块化方案

**说明**：
颜色代表理想更新频率，颜色越深代表更新越频繁

### 第一阶段 按照功能来划分模块

<img src="/architecture/res/modularization_feature.png" alt="Modularization" width="777">

### 第二阶段 关注点分离，分层架构，数据层复用

**注意**：没有复用需求且简单的数据模块可以不和功能模块分离，只留一个功能模块就行。

<img src="/architecture/res/modularization_feature_data.png" alt="Modularization" width="777">

### 第三阶段 业务通用逻辑下沉

<img src="/architecture/res/modularization_feature_core.png" alt="Modularization" width="777">

### 第四阶段 业务无关通用逻辑再下沉

<img src="/architecture/res/modularization_feature_base.png" alt="Modularization" width="777">

## 模块化规范

* 模块内高内聚
* 模块间低耦合
* 对外尽可能少暴露
* 对外尽量暴露接口（抽象），不对外暴露实现
* 模块只能向下依赖，不允许依赖同级模块
* 业务模块是拆分出数据模块
  * 是否有复用需求，有则拆分
  * 数据部分是否够简单，简单且不需要需用可以不拆分

## 常见陷阱

* 模块化粒度过细
* 模块化粒度过大
* 模块之间引用过于复杂

当设计模块化代码库时，需要考虑模块粒度及相对复杂性，粒度过细会让模块化成为一个负担，粒度过大会降低模块化的收益，模块之间引用过于复杂同样也会降低模块化的收益。

## 参考链接

<https://developer.android.google.cn/topic/modularization>
