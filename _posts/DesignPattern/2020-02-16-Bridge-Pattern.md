---
title: "Bridge Pattern"
layout: posts
excerpt: "브릿지 패턴"
search: true
categories:

  - DesignPattern
tags:
  - DesignPattern
---
##### 디자인패턴
###### Structural (구조)

****

###### 구현부와 구상층을 분리하여 확장하기 편하도록 설계


```java
public interface Amp {

    void increaseVolume();
    void decreaseVolume();
}
public class BadAmp implements Amp {
    @Override
    public void increaseVolume() { System.out.println("MaximumSound 5/...");}

    @Override
    public void decreaseVolume() { System.out.println("DecreaseSound...."); }
}
public class GoodAmp implements Amp{
    @Override
    public void increaseVolume() { System.out.println("MaximumSound 10/..."); }

    @Override
    public void decreaseVolume() { System.out.println("DecreaseSound...."); }
}

```

```java
public interface DacChip {

    void clearSound();

    void chip();

    Amp getAmp();
}
public class DAC_V1 implements DacChip {

    private final Amp amp;

    public DAC_V1(Amp amp) {
        this.amp = amp;
    }

    @Override
    public void clearSound() { System.out.println("V1 ClearSound."); }

    @Override
    public void chip() { System.out.println("setting V1 Chip"); }

    @Override
    public Amp getAmp() {
        return amp;
    }
}
public class DAC_V2 implements DacChip {

    private final Amp amp;

    public DAC_V2(Amp amp) {
        this.amp = amp;
    }

    @Override
    public void clearSound() { System.out.println("V2 ClearSound."); }

    @Override
    public void chip() { System.out.println("setting V2 Chip"); }

    @Override
    public Amp getAmp() {
        return amp;
    }
}

```

```java

public class App {
    public static void main(String... args) {
        GoodAmp goodAmp = new GoodAmp();
        BadAmp badAmp = new BadAmp();

        DAC_V1 dac_v1 = new DAC_V1(goodAmp);
        dac_v1.chip();
        dac_v1.clearSound();
        dac_v1.getAmp().increaseVolume();
        dac_v1.getAmp().decreaseVolume();
// setting V1 Chip
// V1 ClearSound.
// MaximumSound 10/...
// DecreaseSound....
        DAC_V2 dac_v2 = new DAC_V2(badAmp);
        dac_v2.chip();
        dac_v2.clearSound();
        dac_v2.getAmp().increaseVolume();
        dac_v2.getAmp().decreaseVolume();
// setting V2 Chip
// V2 ClearSound.
// MaximumSound 5/...
// DecreaseSound....
    }
}
```

![adapter](/blog/assets/images/dsnptn/bridge.png)


