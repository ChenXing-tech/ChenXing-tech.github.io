---
layout: post
title:  "抽象工厂模式"
date:   2023-05-08 17:16:50 +0800
categories: jekyll update


---

## 

## 抽象工厂模式

### 1 意图

抽象工厂模式是对工厂模式的进一步延伸，可以将杂乱的对象按照特性进行分类，并将具有相同属性的对象生成工厂。

### 2 问题

确保同一工厂生产出的产品内容是一致的

### 3 解决方法

将同种变种的产品作为同一工厂进行生产。

### 4 如何对抽象工厂进行分类

is a， have a维度杂乱对象进行归类。同样的组件各有各的实现的时候使用工厂方法。

is a是指对象映射物品的实质。

have a是指对象映射物品的属性，

eg: 电脑，属性是mac， 所以is a computer ， have a Mac feature，这样就可以将对象进行分组，建立computerFactory，抽象工厂为macComputerFactory。

### 5 实现步骤

1. 需求抽象化
2. 创建抽象产品类
3. 创建具体产品继承抽象产品
4. 创建抽象工厂（产品的某一属性的集合） eg: mac , windows都属于computer。
5. 创建具体工厂 

### 6 抽象工厂模式结构

![抽象工厂设计模式](/Users/mac/workspace/project/github/ChenXing-tech.github.io/_posts/structure-20230516150147300.png)

### 7 实现效果

```java
/**
* 抽象产品类
*/
public interface Chair {

    boolean hasLeg();

    boolean sitOn();
}

```

```java
/**
* 创建具体产品类
*/
public class ModernChair implements Chair{
    @Override
    public boolean hasLeg() {
        return false;
    }

    @Override
    public boolean sitOn() {
        return false;
    }
}
```

```java
/**
* 抽象产品工厂
*/
public interface FurnitureFactory {

    Chair createChair();

    CoffeeTable createCoffeeTable();

    Sofa createSofa();
}

```

```java
/**
* 具体产品工厂
*/
public class ModernFurnitureFactory implements FurnitureFactory{

    @Override
    public Chair createChair() {
        return new ModernChair();
    }

    @Override
    public CoffeeTable createCoffeeTable() {
        return new ModernCoffeeTable();
    }

    @Override
    public Sofa createSofa() {
        return new ModernSofa();
    }
}
```

> 抽象工厂模式: https://refactoringguru.cn/design-patterns/abstract-factory

## 