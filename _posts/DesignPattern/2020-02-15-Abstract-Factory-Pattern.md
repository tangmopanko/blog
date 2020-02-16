---
title: "Abstract Factory Pattern"
layout: posts
excerpt: "추상팩토리 패턴"
search: true
categories:
  - DesignPattern
tags:
  - DesignPattern
--- 
##### 디자인패턴

### abstract-factory 

###### Creational (생성)

**서로 관련된 객체를 묶고 팩토리클래스를 만들고, 조건에 맞는경우 각 팩토리 클래스를 생성한다**

###### *concrete class -> 구상, 구현클래스을 의미하며, 추상클래스*(abstract class)를 상속을 받아 실질적으로 operation을 정의하는 클래스를 말한다.

```java
//interface 작성
public interface Eat {
    String getEat();
}
public interface Exercise {
    String getBehavior();
}
```

```java
//interface을 상속받고 concrete class(구상)로. 
public class Eat_ChikenBeast implements Eat{
    private String MUSCULAR_EAT = "Eat Chicken breast";
    public String getEat() {
        return MUSCULAR_EAT;
    }
}
public class Eat_Fastfood implements Eat{
    private String FATMAN_EAT = "Eat fastfood";
    public String getEat() {
        return FATMAN_EAT;
    }
}
public class Exercise_Hard implements Exercise{
    private String MUSCULAR_BEHAVIOR = "Play exercise";
    public String getBehavior() {
        return MUSCULAR_BEHAVIOR;
    }
}
public class Exercise_No implements Exercise {
    private String FATMAN_BEHAVIOR = "Do not exercise.";
    public String getBehavior() {
        return FATMAN_BEHAVIOR;
    }
}

```

```java
//factory interface생성. 
public interface TrainerFactory {
    Eat eat();
    Exercise exercise();
}
//factory interface을 상속받고 각 클래스형태 별로 객체를 생성. 
public class GoodTrainer implements TrainerFactory {
    public Eat eat() {
        return new Eat_ChikenBeast();
    }
    public Exercise exercise() {
        return new Exercise_Hard();
    }
}
public class BadTrainer implements TrainerFactory {
    public Eat eat() {
        return new Eat_Fastfood();
    }
    public Exercise exercise() {
        return new Exercise_No();
    }
}
```

```java
public class App {
    private Eat eat;
    private Exercise exercise;

    public void createTrainer(TrainerFactory trainerFactory) {
        setEat(trainerFactory.eat());
        setExercise(trainerFactory.exercise());
    }

    public static class factoryMaker {
        public enum TrainerType {
            BAD, GOOD
        }
      
        public static TrainerFactory createTrainer(TrainerType type) {
            switch (type) {
                case GOOD:
                    return new GoodTrainer();
                case BAD:
                    return new BadTrainer();
                default:
                    throw new IllegalArgumentException("TrainerType not supported.");
            }
        }
    }

    public static void main(String... args) {
        App app = new App();
        app.createTrainer(factoryMaker.createTrainer(factoryMaker.TrainerType.GOOD));
        app.createTrainer(factoryMaker.createTrainer(factoryMaker.TrainerType.BAD));
    }
}
```

![abstact_factory](/blog/assets/images/dsnptn/abstract-factory.png)


