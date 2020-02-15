---
title: "[Java8] Optional"
layout: categories
permalink: /categories/
excerpt: "자바8 옵션널에 대해서"
search: true
categories:
  - Java
tags:
  - 자바
last_modified_at: 2018-07-14T09:24:00+09:00
---
## JAVA8, null 대신 Optional

##### Optional 이란?  	

JDK8에서 포함된 라이브러리이며, NPE가 일어나지 않도록 한번 더 객체를 감싸주어 Optional.empty()를 반환하도록 해준다. 

*무분별하게 사용은 바람직하지 않으며 **있을 수도 아닐 수도(maybe)일 경우** 사용해야한다.*

```java
public class Person {
    private Optional<Car> car; // 차를 소유하고 있을 수도 없을 수도 일경우 Optional 
    private Optional<Car> getCar () { return car; }
}
public class Car  {
    private Optional<Insurance> insurance; // 보험이 가입되어 있을 수도 없을 수도 일경우 Optional 
    private Optional<Insurance> getInsurance () { return insurance; }
}
public class Insurance {
    private String name; // 보험회사 이름은 반드시 존재 Optional X 
    private String getName() { return name; }
}
```



##### Optional 적용 패턴

1. Empty Optional

   ```java
   Optional<String> optStr = Optional.empty();
   ```

2. null이 아닌 값으로 Optional 만들기

   ```java
   Optional<String> optStr1 = Optional.of("not empty");
   //정상작동
   Optional<String> optStrIsNull = Optional.of(null);
   //NPE 
   ```

3. null값으로 Optional 만들기 

   ```java
   Optional<String> optSt1 = Optional.ofNullable("StringWord..");
   optSt1.isPresent(); -> // true
   optSt1 -> // Optional[StringWord..]
   
   Optional<String> optSt2 = Optional.ofNullable(null);
   optSt2.isPresent() -> // false
   optSt2 -> // Optional.empty 
   ```



##### Map으로 Optional 값을 추출및 변환하기

```java
// 기존
String name = null;
	if(insurance != null) {
  	name = insurance.getName();
  }
// Optional사용 
Optional<Insurance> optInsurance = Optional.ofNullable(insurance);
Optional<String> strInsurance = optInsurance.map(Insurance::getName);
strInsurance;
```



... 정리중



##### 참조

Java8 in action 
