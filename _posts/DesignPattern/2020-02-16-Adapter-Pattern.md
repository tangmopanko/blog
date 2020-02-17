---
title: "Adapter Pattern"
layout: posts
excerpt: "어댑터 패턴"
search: true
categories:

  - DesignPattern
tags:
  - DesignPattern
---
##### 디자인패턴
###### Structural (구조)

****

###### 인터페이스를 사용하는 클라이언트에 대해 다른 인터페이스를 사용시 호환성을 위해 어댑터패턴을 사용한다. 


```java
//스파클링은 톡톡튀는 탄산감을 가지고있다.
public interface Sparkling {
    void feelToktok();
}
//스파클링을 주입 실행한다. 
public class Drink {
    private Sparkling sparkling;
  
    public void setSparkling(Sparkling sparkling) {
        this.sparkling = sparkling;
    }
  
    public Drink(Sparkling sparkling) {
        this.sparkling = sparkling;
    }
    
    public void feelToktok() { sparkling.feelToktok(); }

}
```

```java
//텁텁한맛 음료이다. 
public class Dry {
    void feelDry() {
        System.out.println("feel Dry");
    }
}
//텁텁한맛 어댑터용 객체는 스파클링을 상속받아 구현시 feeToktok객체에 dry의 기능을 주입한다.
//DryAdapter는 Sparkling interface의 Sub이며 feeToktok을 override하여 내부에 기능을 주입한다. (dry)
public class DryAdapter implements Sparkling {
    private Dry dry;

    public DryAdapter() {
        this.dry = new Dry();
    }

    public final void feelToktok() {
        dry.feelDry();
    }
}
```

```java
//텁텁한맛 어댑터 객체는 스파클링인터페이스를 상속하였고, 기능상 구현된 오버라이딩 메소드도 존재하여 이상없이 동작한다. 
//다만 구현된 메소드의 기능을 교체하였기에 텁텁한맛으로 구현되었다. 
public class App {
    public static void main(String... args) {
        Drink drink = new Drink(new DryAdapter());
    }
}
```

![adapter](/blog/assets/images/dsnptn/adapter.png)


