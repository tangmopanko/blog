---
title: "[자료구조] Linkedlist"
layout: posts
excerpt: "Linkedlist"
search: true
categories:
  - DataStructure
tags:
  - 자바
  - 자료구조
---
## Linkedlist

- LinkedList는 요소의 삽입 순서를 유지한다.
- LinkedList는 중복 값과 널값을 가질 수 있습니다.중복 및 널 값을 허용한다.
- LinkedList 클래스는 구현 Queue및 Deque인터페이스입니다. 따라서, 그것은 또한로 사용할 수 있습니다 Queue, Deque또는 Stack.
- Java LinkedList는 스레드로부터 안전하지 않습니다. 멀티 스레드 환경에서 LinkedList에 대한 동시 수정을 명시 적으로 동기화해야한다.
- 별도의 데이터 저장위치 정보가 있어야 하기에 저장공간효율이 좋지 않음.
- 이중 연결 목록은 노드 모음으로 구성되며 각 노드에는 세 개의 필드가 있다.
  - 노드의 데이터
  - 다음 노드에 대한 포인터 / 참조
  - 이전 노드에 대한 포인터 / 참조
- 기본 구조와 용어 
  - 노드 : 데이터값, 포인터 / 데이터 저장단위로 구성
  - 포인터 : 노드와 노드의 연결정보를 가지고 있는 공간 

```java
public class LinkedListTest {
    public static void main(String... args) {
        LinkedList<Country> datas = new LinkedList<>();
        datas.add(new Country("중국", 13368,1420060));
        datas.add(new Country("대한민국", 1720,51709));
        datas.add(new Country("미국", 20580, 329676));
        datas.add(new Country("독일", 3951,82379));
        datas.addAll(Arrays.asList(new Country("영국",2828, 66830)
                , new Country("프랑스", 2780, 65712))
        );

        datas.forEach(Nation -> System.out.print(Nation.name));
        //중국대한민국미국독일영국프랑스

        Collections.sort(datas, new Comparator<Country>() {
            @Override
            public int compare(Country o1, Country o2) {
                int avg = o1.getPopulation() / o1.getGdp();
                int avg1 = o2.getPopulation() / o2.getGdp();
                if (avg > avg1)
                    return +1;
                else
                    return -1;
            }
        });
        datas.forEach(Nation -> System.out.print(Nation.name));
        //미국독일영국프랑스대한민국중국

        datas.remove();// elements 0 index deleted..
        System.out.print(datas.get(0)); // 독일
        
    }

    static class Country {
        private final String name;
        private final int gdp;
        private final int Population;

        public Country(String name, int gdp, int population) {
            this.name = name;
            this.gdp = gdp;
            Population = population;
        }
        public String getName() {
            return name;
        }
        public int getGdp() {
            return gdp;
        }
        public int getPopulation() {
            return Population;
        }
        @Override
        public String toString() {
            return "Country{" +
                    "name='" + name + '\'' +
                    ", gdp=" + gdp +
                    ", Population=" + Population +
                    '}';
        }
    }
    
}
```



