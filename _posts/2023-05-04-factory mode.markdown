---
layout: post
title:  "设计模式之创建型模式"
date:   2023-05-08 17:16:50 +0800
categories: jekyll update

---

## 1. 工厂模式

### 1.1 什么是工厂模式
工厂模式是一种创建者模式，将类的创建方法和类的功能进行解耦，利于扩展，也符合编程的直觉

### 1.2 工厂模式组件
工厂中存在产品，以及产品抽象出的功能。
1. 抽象类: 包含抽象方法创建抽象产品，以及子类工厂的功能特性。
2. 具体类: 实现抽象类的构造方法，生产具体产品
3. 产品接口: 定义抽象产品的特性及其功能
4. 具体产品: 实现产品接口，重写产品接口方法

### 1.3 工厂方法模式结构

![image-20230512154252022](https://p.ipic.vip/8a1z0e.png)

### 1.3 实现步骤

1. 定义抽象产品对象，抽象产品特性方法
2. 定义具体产品对象，并重写具体产品特性方法
3. 将工厂定义为抽象类，抽象类中将产品的特性抽象为统一的方法，不同的产品的特性调用方法会产生不同的效果
4. 定义具体工厂类实现工厂，并重写构造方法

### 1.4 实现效果
通过使用工厂父类指向子类对象的方式，可以实现不同子类对象的不同特性
```java
/**
 * desc：定义抽象产品
 */
public interface Button {

    void render();

    void onClick();
}
```

```java
/**
 * desc: 定义工厂，并抽离出特性功能
 */
public abstract class Dialog {

    public void render(){
        Button button = createButton();
        button.click();
    }

    abstract Button createButton();
}
```

```java
/**
 * desc: 实现具体产品，并定义具体产品具体功能方法
 */
public class HTMLButton implements Button{

    @Override
    public void render() {
        System.out.println("htmlButton");
    }

    @Override
    public void onClick() {
        System.out.println("htmlButton");
    }
}
```

```java
/**
 * desc: 继承工厂类，并实现抽象方法，创建具体产品对象
 */
public class HTMLDialog extends Dialog {


    @Override
    public Button createButton() {
        return new HTMLButton();
    }
}

```

```java
/**
 * desc: 具体使用方式
 */
public class Main(){
    public static void main(String[] args){
        Dialog dialog = new HTMLDialog();
        dialog.render();
    }
}
```

> 工厂模式：https://refactoringguru.cn/design-patterns/factory-method

## 2. 抽象工厂模式

### 2.1  什么是抽象工厂模式

抽象工厂模式是对工厂模式的进一步延伸，可以将杂乱的对象按照特性进行分类，并将具有相同属性的对象生成工厂。

### 2.2 如何对抽象工厂进行分类

is a， have a维度杂乱对象进行归类。

is a是指对象映射物品的实质。

have a是指对象映射物品的属性，

eg: 电脑，属性是mac， 所以is a computer ， have a Mac feature，这样就可以将对象进行分组，建立computerFactory，抽象工厂为macComputerFactory。

### 2.3 实现步骤

1. 需求抽象化
2. 创建抽象产品类
3. 创建具体产品继承抽象产品
4. 创建抽象工厂（产品的某一属性的集合） eg: mac , windows都属于computer。
5. 创建具体工厂 

### 2.4 抽象工厂模式结构

![image-20230512162611164](https://p.ipic.vip/3dania.png)

### 2.5 实现效果

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
