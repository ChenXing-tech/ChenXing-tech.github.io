---
layout: post
title:  "设计模式之创建型模式"
date:   2023-05-08 17:16:50 +0800
categories: jekyll update

---

## 1. 工厂模式

### 1.1 意图
工厂模式是一种创建者模式，将类的创建方法和类的功能进行解耦，利于扩展，也符合编程的直觉

### 1.2 问题

当我们已经实现一种产品的一种功能，现在要扩展为同类产品同样实现此功能。

### 1.3 解决方法

可以将同类产品定义为interface，不同产品实现interface的接口。实现各自的特性功能。定义abstract工厂，定义产品创建方法，并利用创建方法创建出的对象及进行公共功能的实现。

### 1.4 工厂模式组件
工厂中存在产品，以及产品抽象出的功能。
1. 抽象类: 包含抽象方法创建抽象产品，以及子类工厂的功能特性。
2. 具体类: 实现抽象类的构造方法，生产具体产品
3. 产品接口: 定义抽象产品的特性及其功能
4. 具体产品: 实现产品接口，重写产品接口方法

### 1.5 工厂方法模式结构

![工厂方法模式结构](/Users/mac/workspace/project/github/ChenXing-tech.github.io/_posts/structure-20230516150217975.png)

### 1.6 实现步骤

1. 定义抽象产品对象，抽象产品特性方法
2. 定义具体产品对象，并重写具体产品特性方法
3. 将工厂定义为抽象类，抽象类中将产品的特性抽象为统一的方法，不同的产品的特性调用方法会产生不同的效果
4. 定义具体工厂类实现工厂，并重写构造方法

### 1.7 实现效果
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

### 2.1  意图

抽象工厂模式是对工厂模式的进一步延伸，可以将杂乱的对象按照特性进行分类，并将具有相同属性的对象生成工厂。

### 2.2 问题

确保同一工厂生产出的产品内容是一致的

### 2.3 解决方法

将同种变种的产品作为同一工厂进行生产。

### 2.4 如何对抽象工厂进行分类

is a， have a维度杂乱对象进行归类。同样的组件各有各的实现的时候使用工厂方法。

is a是指对象映射物品的实质。

have a是指对象映射物品的属性，

eg: 电脑，属性是mac， 所以is a computer ， have a Mac feature，这样就可以将对象进行分组，建立computerFactory，抽象工厂为macComputerFactory。

### 2.5 实现步骤

1. 需求抽象化
2. 创建抽象产品类
3. 创建具体产品继承抽象产品
4. 创建抽象工厂（产品的某一属性的集合） eg: mac , windows都属于computer。
5. 创建具体工厂 

### 2.6 抽象工厂模式结构

![抽象工厂设计模式](/Users/mac/workspace/project/github/ChenXing-tech.github.io/_posts/structure-20230516150147300.png)

### 2.7 实现效果

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

## 3. 生成器模式

### 3.1 意图

生成器模式是创建性模式，为了解决类与变量的耦合问题

### 3.2 问题

当一个类中变量众多，生成的对象所需变量的数量不同。需要的构造方法是杂乱无章的，。

### 3.3 解决办法

可以为每个变量单独进行赋予值，并可以将赋值的顺序和流程记录作为单独的方法

### 3.4 生成器模式组件

1. 生成器: 接口中声明所需的构造步骤
2. 具体生成器: 包含构造步骤中的变量，提供构造过程中的不同实现。
3. 产品: 最终生成的产品类。
4. 主管：类调用构造步骤的顺序
5. 客户端： 将具体生成器放入主管中，按照指定构造步骤进行构造生成最终的产品对象

### 3.5 生成器方法结构

![生成器设计模式结构](https://refactoringguru.cn/images/patterns/diagrams/builder/structure.png)

### 3.6 实现步骤

1. 设计所有需要的组件
2. 生成器中定义构造方法
3. 定义具体生成器进行实现
4. 主管类构造组装方法
5. 客户端创建主管类创造所需对象

### 3.7 实现效果

```java
package com.ichenxing.builder.components;

/**
 * @date 2023/5/16
 * @description 引擎
 */

public class Engine {
    private final double volume;
    private double mileage;
    private boolean started;

    public Engine(double volume, double mileage) {

        this.volume = volume;
        this.mileage = mileage;
    }

    public void on() {
        started = true;
    }

    public void off() {
        started = false;
    }

    public boolean isStarted() {
        return started;
    }

    public void go(double mileage) {
        if (started) {
            this.mileage += mileage;
        } else {
            System.err.println("Cannot go(), you must start engine first!");
        }
    }

    public double getVolume() {
        return volume;
    }

    public double getMileage() {
        return mileage;
    }
}

```



```java
package com.ichenxing.builder.components;

/**
 * @date 2023/5/16
 * @description GPS定位
 */
public class GPSNavigator {
    private String route;

    public GPSNavigator() {
        this.route = "221b, Baker Street, London  to Scotland Yard, 8-10 Broadway, London";
    }

    public GPSNavigator(String manualRoute) {
        this.route = manualRoute;
    }

    public String getRoute() {
        return route;
    }
}

```

```java

/**
 * @date 2023/5/16
 * @description
 */
public enum Transmission {
    SINGLE_SPEED, MANUAL, AUTOMATIC, SEMI_AUTOMATIC
}

```

```java
package com.ichenxing.builder.components;

import com.ichenxing.builder.Car;

/**
 * @date 2023/5/16
 * @description
 */
public class TripComputer {
    private Car car;

    public void setCar(Car car) {
        this.car = car;
    }

    public void showFuelLevel() {
        System.out.println("Fuel level: " + car.getFuel());
    }

    public void showStatus() {
        if (this.car.getEngine().isStarted()) {
            System.out.println("Car is started");
        } else {
            System.out.println("Car isn't started");
        }
    }
}

```

```java
public interface Builder {
    void setCarType(CarType type);
    void setSeats(int seats);
    void setEngine(Engine engine);
    void setTransmission(Transmission transmission);
    void setTripComputer(TripComputer tripComputer);
    void setGPSNavigator(GPSNavigator gpsNavigator);
}
```

```java
public class Car {

    private final CarType carType;
    private final int seats;
    private final Engine engine;
    private final Transmission transmission;
    private final TripComputer tripComputer;
    private final GPSNavigator gpsNavigator;
    private double fuel = 0;

    public Car(CarType carType, int seats, Engine engine, Transmission transmission,
               TripComputer tripComputer, GPSNavigator gpsNavigator) {
        this.carType = carType;
        this.seats = seats;
        this.engine = engine;
        this.transmission = transmission;
        this.tripComputer = tripComputer;
        if (this.tripComputer != null) {
            this.tripComputer.setCar(this);
        }
        this.gpsNavigator = gpsNavigator;
    }

    public CarType getCarType() {
        return carType;
    }

    public double getFuel() {
        return fuel;
    }

    public void setFuel(double fuel) {
        this.fuel = fuel;
    }

    public int getSeats() {
        return seats;
    }

    public Engine getEngine() {
        return engine;
    }

    public Transmission getTransmission() {
        return transmission;
    }

    public TripComputer getTripComputer() {
        return tripComputer;
    }

    public GPSNavigator getGpsNavigator() {
        return gpsNavigator;
    }
}
```

```java
public enum CarType {
    CITY_CAR, SPORTS_CAR, SUV
}
```

```java
public class Manual {
    private final CarType carType;
    private final int seats;
    private final Engine engine;
    private final Transmission transmission;
    private final TripComputer tripComputer;
    private final GPSNavigator gpsNavigator;

    public Manual(CarType carType, int seats, Engine engine, Transmission transmission,
                  TripComputer tripComputer, GPSNavigator gpsNavigator) {
        this.carType = carType;
        this.seats = seats;
        this.engine = engine;
        this.transmission = transmission;
        this.tripComputer = tripComputer;
        this.gpsNavigator = gpsNavigator;
    }

    public String print() {
        String info = "";
        info += "Type of car: " + carType + "\n";
        info += "Count of seats: " + seats + "\n";
        info += "Engine: volume - " + engine.getVolume() + "; mileage - " + engine.getMileage() + "\n";
        info += "Transmission: " + transmission + "\n";
        if (this.tripComputer != null) {
            info += "Trip Computer: Functional" + "\n";
        } else {
            info += "Trip Computer: N/A" + "\n";
        }
        if (this.gpsNavigator != null) {
            info += "GPS Navigator: Functional" + "\n";
        } else {
            info += "GPS Navigator: N/A" + "\n";
        }
        return info;
    }
}
```

```java
public class CarBuilder implements Builder{

    private CarType type;
    private int seats;
    private Engine engine;
    private Transmission transmission;
    private TripComputer tripComputer;
    private GPSNavigator gpsNavigator;

    @Override
    public void setCarType(CarType type) {
        this.type = type;
    }

    @Override
    public void setSeats(int seats) {
        this.seats = seats;
    }

    @Override
    public void setEngine(Engine engine) {
        this.engine = engine;
    }

    @Override
    public void setTransmission(Transmission transmission) {
        this.transmission = transmission;
    }

    @Override
    public void setTripComputer(TripComputer tripComputer) {
        this.tripComputer = tripComputer;
    }

    @Override
    public void setGPSNavigator(GPSNavigator gpsNavigator) {
        this.gpsNavigator = gpsNavigator;
    }

    public Car getResult() {
        return new Car(type, seats, engine, transmission, tripComputer, gpsNavigator);
    }
}

```

```java
public class CarManualBuilder implements Builder{

    private CarType type;
    private int seats;
    private Engine engine;
    private Transmission transmission;
    private TripComputer tripComputer;
    private GPSNavigator gpsNavigator;

    @Override
    public void setCarType(CarType type) {
        this.type = type;
    }

    @Override
    public void setSeats(int seats) {
        this.seats = seats;
    }

    @Override
    public void setEngine(Engine engine) {
        this.engine = engine;
    }

    @Override
    public void setTransmission(Transmission transmission) {
        this.transmission = transmission;
    }

    @Override
    public void setTripComputer(TripComputer tripComputer) {
        this.tripComputer = tripComputer;
    }

    @Override
    public void setGPSNavigator(GPSNavigator gpsNavigator) {
        this.gpsNavigator = gpsNavigator;
    }

    public Manual getResult() {
        return new Manual(type, seats, engine, transmission, tripComputer, gpsNavigator);
    }
}

```

```java
public class Director {

    public void constructSportsCar(Builder builder) {
        builder.setCarType(CarType.SPORTS_CAR);
        builder.setSeats(2);
        builder.setEngine(new Engine(3.0, 0));
        builder.setTransmission(Transmission.SEMI_AUTOMATIC);
        builder.setTripComputer(new TripComputer());
        builder.setGPSNavigator(new GPSNavigator());
    }

    public void constructCityCar(Builder builder) {
        builder.setCarType(CarType.CITY_CAR);
        builder.setSeats(2);
        builder.setEngine(new Engine(1.2, 0));
        builder.setTransmission(Transmission.AUTOMATIC);
        builder.setTripComputer(new TripComputer());
        builder.setGPSNavigator(new GPSNavigator());
    }

    public void constructSUV(Builder builder) {
        builder.setCarType(CarType.SUV);
        builder.setSeats(4);
        builder.setEngine(new Engine(2.5, 0));
        builder.setTransmission(Transmission.MANUAL);
        builder.setGPSNavigator(new GPSNavigator());
    }
}

```

```java
public class BuilderMain {

    public static void main(String[] args) {
        CarBuilder carBuilder = new CarBuilder();
        Director director = new Director();
        director.constructCityCar(carBuilder);
        Car car = carBuilder.getResult();
        System.out.println(car.getCarType());

        CarManualBuilder carManualBuilder = new CarManualBuilder();
        Director manualDirector = new Director();
        manualDirector.constructSportsCar(carManualBuilder);
        Manual manual = carManualBuilder.getResult();
        System.out.println(manual.print());
    }
}
```

> 生成器模式: https://refactoringguru.cn/design-patterns/builder
