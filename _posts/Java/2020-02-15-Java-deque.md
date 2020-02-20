---
title: "[자료구조] Deque"
layout: posts
excerpt: "deque in Java"
search: true
categories:
  - DataStructure
tags:
  - 자바
  - 자료구조
---
## Deque란?

Deque 또는 Double Ended Queue 는 일반화 된 버전의 Queue 데이터 구조 로 양쪽 끝에 삽입 및 삭제할 수 있습니다..

```java
      Deque<String> test = new ArrayDeque<String>();
        // add first
        test.add("1"); // first
        test.addFirst("0"); // first
        test.addLast("10"); // last
        test.addLast("11"); // last
        // [0, 1, 10, 11]

        test.remove(); // first
        // [1, 10, 11]
        test.removeLast(); // last
        // [1, 10]
        test.offer("11"); // last
        test.offerFirst("-1"); // first
        test.offerLast("12"); // last
        test.offerLast("13"); // last

        System.out.println(test.toString());

        // [-1, 1, 10, 11, 12, 13]
        test.pop(); // first
        System.out.println(test.toString());

        // [1, 10, 11, 12]
        System.out.println(test.poll()); // 1
        System.out.println(test.pop()); // 10
        System.out.println(test.poll()); // 11
        System.out.println(test.element()); // 12  deque head 데이터 가지고오지만 삭제하지는 않는다.
        System.out.println(test.toString()); // [12, 13]
        System.out.println(test.getFirst()); // 12 deque head 데이터 가지고오지만 삭제하지는 않는다.
        System.out.println(test.getLast()); // 13 deque Rear 데이터 가지고오지만 삭제하지는 않는다.
        System.out.println(test.poll()); // 12
        System.out.println(test.poll()); // 13
        System.out.println(test.peek()); // null    deque head 데이터 가지고오지만 삭제하지는 않는다. deque가 비워있을경우 null을 반환한다.
        test.element();                 // NoSuchElementException
```



#### 참고

https://www.javatpoint.com/java-deque-arraydeque

