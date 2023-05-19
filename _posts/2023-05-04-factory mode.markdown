---
layout: post
title:  "FactoryMode"
date:   2023-05-08 17:16:50 +0800
categories: jekyll update

---

## 工厂模式

### 1 意图
工厂模式是一种创建者模式，将类的创建方法和类的功能进行解耦，利于扩展，也符合编程的直觉

### 2 问题

当我们已经实现一种产品的一种功能，现在要扩展为同类产品同样实现此功能。

### 3 解决方法

可以将同类产品定义为interface，不同产品实现interface的接口。实现各自的特性功能。定义abstract工厂，定义产品创建方法，并利用创建方法创建出的对象及进行公共功能的实现。

### 4 工厂模式组件
工厂中存在产品，以及产品抽象出的功能。
1. 抽象类: 包含抽象方法创建抽象产品，以及子类工厂的功能特性。
2. 具体类: 实现抽象类的构造方法，生产具体产品
3. 产品接口: 定义抽象产品的特性及其功能
4. 具体产品: 实现产品接口，重写产品接口方法

### 5 工厂方法模式结构

![工厂方法模式结构](https://p.ipic.vip/5mjaj2.png)

### 6 实现步骤

1. 定义抽象产品对象，抽象产品特性方法
2. 定义具体产品对象，并重写具体产品特性方法
3. 将工厂定义为抽象类，抽象类中将产品的特性抽象为统一的方法，不同的产品的特性调用方法会产生不同的效果
4. 定义具体工厂类实现工厂，并重写构造方法

### 7 实现效果
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
